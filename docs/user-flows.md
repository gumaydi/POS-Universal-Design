# POS Universal — User Flows & State Transitions

This document details the core operational flows, state machines, and sequence interactions implemented across POS Universal.

---

## 1. Authentication & Session RBAC Flow

Unlike consumer applications, POS Universal removes role selection tabs from the login interface. The server resolves permissions upon validating credentials:

```mermaid
sequenceDiagram
    autonumber
    actor User as User (Owner / Manager / Cashier)
    participant UI as Web/Mobile Client
    participant Auth as Auth Service (Supabase)
    participant DB as PostgreSQL Database

    User->>UI: Enter Email & Password
    UI->>UI: Validate local input format
    UI->>Auth: POST /auth/v1/token (email, password)
    Auth->>DB: Query user credentials & staff role
    
    alt Credentials Valid
        DB-->>Auth: User record (id, role: OWNER|MANAGER|CASHIER, outlet_id)
        Auth-->>UI: 200 OK (JWT Token, User Profile, Role)
        UI->>UI: Store Session in secure storage
        UI->>UI: Initialize Role Guards & Feature Flags
        alt Role == CASHIER
            UI->>User: Redirect to POS Screen (/pos)
        else Role == MANAGER || OWNER
            UI->>User: Redirect to Dashboard (/dashboard)
        end
    else Credentials Invalid
        DB-->>Auth: Record not found / Hash mismatch
        Auth-->>UI: 401 Unauthorized
        UI->>User: Display "Kredensial Tidak Valid" alert banner
    end
```

---

## 2. Cash (Tunai) Checkout Flow

Cash checkout is optimized for speed, safety, and strict change calculation:

```mermaid
sequenceDiagram
    autonumber
    actor Cashier as Cashier
    participant UI as POS Workspace
    participant API as POS Backend API
    participant DB as PostgreSQL (ACID Transaction)

    Cashier->>UI: Scan barcode / Select catalog items
    UI->>UI: Update active cart & calculate Subtotal + Net Due
    Cashier->>UI: Click "Bayar Tunai"
    UI->>UI: Open Cash Tender Drawer / Modal
    Cashier->>UI: Enter Uang Diterima (or select quick chips)
    
    alt Uang Diterima < Total Tagihan
        UI->>UI: Show Insufficient Cash warning (Disable "Selesaikan Transaksi")
    else Uang Diterima >= Total Tagihan
        UI->>UI: Compute Kembalian = Uang Diterima - Total Tagihan
        Cashier->>UI: Click "Selesaikan Transaksi"
        UI->>API: POST /api/v1/transactions (Idempotency-Key, items, cash_tendered, change)
        API->>DB: BEGIN TRANSACTION
        API->>DB: Deduct physical stock: on_hand = on_hand - qty
        API->>DB: Insert transaction record (status = 'COMPLETED', payment = 'CASH')
        API->>DB: Insert stock movements (type = 'SALE')
        API->>DB: COMMIT TRANSACTION
        DB-->>API: Transaction Committed
        API-->>UI: 201 Created (Transaction ID, Receipt Data)
        UI->>Cashier: Display Digital Receipt & Cash Change Modal
    end
```

---

## 3. Manual Bank Transfer & Inventory Reservation Flow

Manual bank transfer requires an order reservation window to prevent overselling while awaiting customer transfer:

```mermaid
sequenceDiagram
    autonumber
    actor Cashier as Cashier
    actor Customer as Customer
    actor Manager as Store Manager / Owner
    participant UI as POS Client
    participant API as Backend Service
    participant DB as PostgreSQL DB
    participant Cron as Expiry Worker (pg_cron)

    Cashier->>UI: Select "Transfer Bank Manual"
    UI->>Cashier: Display Merchant Bank Details (BCA / Mandiri)
    Cashier->>Customer: Present Bank Info & Order ID note
    Cashier->>UI: Confirm "Buat Pesanan Transfer"
    UI->>API: POST /api/v1/transactions (payment = 'BANK_TRANSFER')
    API->>DB: BEGIN TRANSACTION
    API->>DB: Reserve stock: reserved = reserved + qty
    API->>DB: Create transaction (status = 'PENDING_TRANSFER', expires_at = now() + 60 mins)
    API->>DB: COMMIT TRANSACTION
    DB-->>UI: Reservation Confirmed (Order ID, 60-min timer)
    
    Customer->>Manager: Executes Bank Transfer & shows proof
    Manager->>UI: Opens /transactions queue
    Manager->>UI: Reviews proof & clicks "Verifikasi Transfer"
    UI->>API: POST /api/v1/transactions/:id/verify (Manager elevation token)
    API->>DB: BEGIN TRANSACTION
    API->>DB: on_hand = on_hand - reserved_qty
    API->>DB: reserved = reserved - reserved_qty
    API->>DB: status = 'COMPLETED'
    API->>DB: COMMIT TRANSACTION
    DB-->>UI: 200 OK (Transfer Verified)
    UI->>Manager: Show verified receipt

    opt Reservation Expired without Verification
        Cron->>API: Check expired reservations (expires_at < now() AND status = 'PENDING_TRANSFER')
        API->>DB: BEGIN TRANSACTION
        API->>DB: reserved = reserved - reserved_qty (Stock released back to Available)
        API->>DB: status = 'EXPIRED'
        API->>DB: COMMIT TRANSACTION
    end
```

---

## 4. Refund & Restock Workflow

Customer returns require manager authorization and explicit accounting:

```mermaid
sequenceDiagram
    autonumber
    actor Customer as Customer
    actor Cashier as Cashier
    actor Manager as Store Manager / Owner
    participant UI as Transaction Detail UI
    participant API as POS Backend API
    participant DB as PostgreSQL DB

    Customer->>Cashier: Request return of defective item
    Cashier->>UI: Open Transaction History & select Order
    Cashier->>UI: Click "Ajukan Refund"
    UI->>Cashier: Show "Manager Authorization Required" prompt
    Manager->>UI: Provide Manager authorization PIN / credential
    UI->>UI: Display Refund Modal with item list
    Manager->>UI: Select items to return & input Refund Reason
    Manager->>UI: Check "Kembalikan ke Stok Fisik (Restock)" toggle
    Manager->>UI: Confirm Refund
    UI->>API: POST /api/v1/transactions/:id/refunds (Manager token, items, restock: true, reason)
    API->>DB: BEGIN TRANSACTION
    API->>DB: Insert refund ledger record (negative value against Net Sales)
    opt Restock == true
        API->>DB: on_hand = on_hand + returned_qty
        API->>DB: Insert stock movement (type = 'REFUND_RESTOCK')
    end
    API->>DB: Update order refund status ('PARTIAL_REFUND' or 'REFUNDED')
    API->>DB: COMMIT TRANSACTION
    DB-->>UI: 200 OK (Refund Processed)
    UI->>Cashier: Print Refund Slip & disburse cash to customer
```

---

## 5. State Machines (FSM)

### 5.1 Transaction State Machine

```mermaid
stateDiagram-v2
    [*] --> DRAFT: Cart Active
    DRAFT --> COMPLETED: Cash Tender Confirmed
    DRAFT --> PENDING_TRANSFER: Transfer Order Created (Reserved Stock)
    DRAFT --> CANCELLED: Cashier Clears / Discards Cart

    PENDING_TRANSFER --> COMPLETED: Manager Verifies Bank Transfer
    PENDING_TRANSFER --> EXPIRED: Expiry Window Elapsed (60 Mins)
    PENDING_TRANSFER --> CANCELLED: Cashier / Customer Cancels

    COMPLETED --> PARTIALLY_REFUNDED: Authorized Partial Item Return
    COMPLETED --> REFUNDED: Authorized Full Order Return

    EXPIRED --> [*]
    CANCELLED --> [*]
    REFUNDED --> [*]
    PARTIALLY_REFUNDED --> [*]
```

### 5.2 Stock Reservation Lifecycle

```mermaid
stateDiagram-v2
    state "Available Stock (Stok Tersedia)" as Avail
    state "Reserved Stock (Stok Dipesan)" as Res
    state "Fulfilled Stock (Terjual Bersih)" as Sold

    Avail --> Res: Transfer Order Initiated (Available = OnHand - Reserved)
    Res --> Sold: Transfer Verified (OnHand = OnHand - ReservedQty, Reserved = Reserved - ReservedQty)
    Res --> Avail: Order Expired or Cancelled (Reserved released back to Available)
```
