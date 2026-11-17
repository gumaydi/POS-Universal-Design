# POS Universal — Screen & Route Inventory

This inventory documents all designed screens, modals, sheets, and operational UI states across POS Universal. All screens are natively created in FigCraft and trace directly to the approved design files:
- `POS Universal — Design Foundation.figcraft`
- `POS Universal — Phase 2 Core Screens.figcraft`
- `POS Universal — Phase 3 Business Management.figcraft`

---

## 1. Web Desktop Screens (1440 × 900 px)

| Screen Name | FigCraft Node ID | Route Path | Permitted Roles | Key UI Components & Interactions | Preview Screenshot |
| :--- | :---: | :--- | :--- | :--- | :--- |
| **Web Design Tokens** | `7` | N/A (Spec) | All | Color palette, typography scale, spacing tokens, status badges | [Preview](../design/screenshots/desktop/01-design-tokens.png) |
| **Web UI Components** | `155` | N/A (Spec) | All | Buttons, inputs, chips, tables, modals, badges, cards | [Preview](../design/screenshots/desktop/02-ui-components.png) |
| **Web App Shell** | `390` | Layout Shell | All | Left navigation sidebar (240px), top bar, store selector, user profile | [Preview](../design/screenshots/desktop/03-web-app-shell.png) |
| **Web Login Screen** | `1268` | `/login` | Public | Email/password form, show/hide password, server RBAC resolution | [Preview](../design/screenshots/desktop/04-web-login.png) |
| **Web Owner Dashboard** | `1376` | `/dashboard` | Owner, Manager | Gross sales, net sales, order volume, low stock alerts, quick links | [Preview](../design/screenshots/desktop/05-web-dashboard.png) |
| **Web Cashier & Cart** | `1686` | `/pos` | All | 65% catalog grid, category filter, 35% sticky cart, hold cart button | [Preview](../design/screenshots/desktop/06-web-cashier-cart.png) |
| **Web Checkout — Cash** | `2092` | `/pos/checkout?method=cash` | All | Total due, cash tender input, quick denomination chips, change math | [Preview](../design/screenshots/desktop/07-web-checkout-cash.png) |
| **Web Checkout — Transfer**| `2151` | `/pos/checkout?method=transfer`| All | Bank accounts (BCA/Mandiri), transfer note generator, reservation alert | [Preview](../design/screenshots/desktop/08-web-checkout-transfer.png) |
| **Web Product Catalog** | `3006` | `/products` | All | Search, category rail, multi-unit badges, HPP margin, stock counters | [Preview](../design/screenshots/desktop/09-web-products.png) |
| **Web Add/Edit Product** | `3189` | `/products/new` (Modal) | Owner, Manager | 8-unit selector, price/HPP margin math, variant builder, stock threshold| [Preview](../design/screenshots/desktop/10-web-product-modal.png) |
| **Web Category Modal** | `3265` | `/categories` (Modal) | Owner, Manager | Category list, item count, add/edit/delete category safeguards | [Preview](../design/screenshots/desktop/11-web-category-modal.png) |
| **Web Inventory Overview**| `3485` | `/inventory` | Owner, Manager | 3-tier stock table (On-Hand, Reserved, Available), dual asset valuation | [Preview](../design/screenshots/desktop/12-web-inventory.png) |
| **Web Stock Adjustment** | `3646` | `/inventory/adjust` (Modal)| Owner, Manager | Opname delta input, mandatory reason codes (RUSAK, SELISIH, etc.) | [Preview](../design/screenshots/desktop/13-web-stock-adjustment-modal.png)|
| **Web Transaction History**| `3791` | `/transactions` | All | Filter by status/date/cashier, review queue, export CSV | [Preview](../design/screenshots/desktop/14-web-transactions.png) |
| **Web Refund Modal** | `3922` | `/transactions/:id/refund` | Owner, Manager | Itemized return selector, refund reason, auto-restock toggle, notes | [Preview](../design/screenshots/desktop/15-web-refund-modal.png) |
| **Web Sales Analytics** | `4059` | `/reports` | Owner, Manager | Net sales, HPP/COGS, gross profit (50.5%), cash vs transfer reconciliation| [Preview](../design/screenshots/desktop/16-web-reports.png) |
| **Web Team & RBAC** | `4264` | `/team` | Owner Only | Staff member list, role assignment (Owner, Manager, Cashier), status | [Preview](../design/screenshots/desktop/17-web-team.png) |
| **Web Store Settings** | `4380` | `/settings` | Owner Only | Business profile, tax toggle, bank accounts list (BCA, Mandiri) | [Preview](../design/screenshots/desktop/18-web-settings.png) |

---

## 2. Mobile Web / Android Browser Screens (390 × 844 px)

| Screen Name | FigCraft Node ID | Target Route | Permitted Roles | Key UI Components & Interactions | Preview Screenshot |
| :--- | :---: | :--- | :--- | :--- | :--- |
| **Mobile App Shell POS** | `568` | Layout Shell | All | Header with scanner button, 5-tab bottom navigation | [Preview](../design/screenshots/mobile/01-mobile-app-shell-pos.png) |
| **Mobile Lainnya Menu** | `640` | `/more` | All | Role-gated settings list, profile summary, logout action | [Preview](../design/screenshots/mobile/02-mobile-lainnya-menu.png) |
| **Mobile RBAC Matrix** | `687` | N/A (Spec) | All | Mobile visual RBAC guideline comparing Owner, Manager, Cashier | [Preview](../design/screenshots/mobile/03-mobile-rbac-matrix.png) |
| **Mobile Login Screen** | `1327` | `/login` | Public | Single column touch login form, password visibility toggle | [Preview](../design/screenshots/mobile/04-mobile-login.png) |
| **Mobile Owner Dashboard**| `1583` | `/dashboard` | Owner, Manager | KPI summary cards, today's sales graph, quick action pill buttons | [Preview](../design/screenshots/mobile/05-mobile-dashboard.png) |
| **Mobile Cashier Catalog**| `1877` | `/pos` | All | Horizontal category pills, 2-column touch cards, floating cart bar | [Preview](../design/screenshots/mobile/06-mobile-cashier-catalog.png) |
| **Mobile Cart Screen** | `2009` | `/pos/cart` | All | Full cart review, item quantity steppers, discount chips, hold cart | [Preview](../design/screenshots/mobile/07-mobile-cart.png) |
| **Mobile Checkout — Cash** | `2204` | `/pos/checkout/cash` | All | Large tender input, denomination chips, change calculator | [Preview](../design/screenshots/mobile/08-mobile-checkout-cash.png) |
| **Mobile Checkout — Transfer**| `2237` | `/pos/checkout/transfer`| All | Bank card preview, copy account number button, reservation timer | [Preview](../design/screenshots/mobile/09-mobile-checkout-transfer.png) |
| **Mobile Digital Receipt** | `2264` | `/pos/receipt/:id` | All | Receipt preview, print via Bluetooth, share via WhatsApp | [Preview](../design/screenshots/mobile/10-mobile-receipt.png) |
| **Mobile Product Catalog** | `3321` | `/products` | All | Touch search bar, category chips, stock status pills | [Preview](../design/screenshots/mobile/11-mobile-products.png) |
| **Mobile Product Detail** | `3403` | `/products/:id` | All | Stock balance, price, HPP, unit, edit button | [Preview](../design/screenshots/mobile/12-mobile-product-detail.png) |
| **Mobile Add Product** | `3446` | `/products/new` (Sheet) | Owner, Manager | Touch form bottom sheet, camera barcode scanner, unit selector | [Preview](../design/screenshots/mobile/13-mobile-add-product.png) |
| **Mobile Inventory Overview**| `3682` | `/inventory` | Owner, Manager | Searchable stock cards, On-Hand vs Available indicators | [Preview](../design/screenshots/mobile/14-mobile-inventory.png) |
| **Mobile Stock Adjustment** | `3759` | `/inventory/adjust` (Sheet)| Owner, Manager | Quick plus/minus adjustment, reason dropdown | [Preview](../design/screenshots/mobile/15-mobile-stock-adjustment.png) |
| **Mobile Transaction Feed** | `3963` | `/transactions` | All | Order cards, status badges, filter chips | [Preview](../design/screenshots/mobile/16-mobile-transactions.png) |
| **Mobile Refund Screen** | `4031` | `/transactions/:id/refund`| Owner, Manager | Touch item return checklist, restock switch, reason selector | [Preview](../design/screenshots/mobile/17-mobile-refund.png) |
| **Mobile Sales Reports** | `4199` | `/reports` | Owner, Manager | Daily sales summary, payment breakdown chart, profit margin | [Preview](../design/screenshots/mobile/18-mobile-reports.png) |
| **Mobile Team Management** | `4467` | `/team` | Owner Only | Staff cards, role badges, active/inactive toggles | [Preview](../design/screenshots/mobile/19-mobile-team.png) |
| **Mobile Settings & Bank** | `4539` | `/settings` | Owner Only | Store information, active bank accounts, notification preferences | [Preview](../design/screenshots/mobile/20-mobile-settings.png) |

---

## 3. Operational States & Edge Cases

| State / Modal Name | FigCraft Node ID | Context & Trigger | Behavior & User Action Required | Preview Screenshot |
| :--- | :---: | :--- | :--- | :--- |
| **Empty Cart Card** | `2072` | POS screen when cart has 0 items | Prompts cashier to scan barcode or select items from catalog | [Preview](../design/screenshots/states/state-web-cashier-empty-cart.png) |
| **Login: Loading** | `2345` | During credential verification | Disables inputs and shows spinner with text "Memverifikasi..." | [Preview](../design/screenshots/states/state-login-loading.png) |
| **Login: Invalid Credentials**| `2353` | Server returns 401 Unauthorized | Displays red warning banner; prompts user to check email/password | [Preview](../design/screenshots/states/state-login-invalid.png) |
| **Login: Auth Success** | `2361` | Server returns 200 OK | Displays green success banner and prepares offline cache sync | [Preview](../design/screenshots/states/state-login-success.png) |
| **Cash Insufficient** | `2289` | Tender amount < Total Due | Disables complete button; highlights missing deficit amount in red | [Preview](../design/screenshots/states/state-01-cash-insufficient.png) |
| **Hold Cart Dialog** | `2303` | Cashier holds active queue order | Requires customer identifier note; restores previous held cart | [Preview](../design/screenshots/states/state-02-hold-cart.png) |
| **Transfer Verification** | `2315` | Manager verifies bank transfer | Shows bank statement match prompt; requires Manager elevation | [Preview](../design/screenshots/states/state-03-transfer-verification.png) |
| **Stock Warnings** | `2328` | Item stock hits threshold | Amber warning badge on item; prevents adding beyond Available | [Preview](../design/screenshots/states/state-04-stock-warnings.png) |
| **Network Unavailable** | `2383` | Offline status detected | Warning banner indicating read-only catalog; disables new checkouts | [Preview](../design/screenshots/states/state-05-offline-alert.png) |
| **Checkout In-Flight** | `2391` | Server processing transaction | Disables repeat submissions; shows idempotency progress spinner | [Preview](../design/screenshots/states/state-06-checkout-processing.png) |
| **Timeout Resolution** | `2399` | Transaction state indeterminate | Provides safe check status button to prevent double-charging | [Preview](../design/screenshots/states/state-07-timeout-resolution.png) |
| **Price Updated Alert** | `2407` | Catalog price changed while in cart | Alerts cashier of price update; requires re-confirmation | [Preview](../design/screenshots/states/state-08-price-update-alert.png) |
| **Stock Decreased Warning**| `2415` | Concurrently reserved stock | Alerts cashier if available stock dropped below cart quantity | [Preview](../design/screenshots/states/state-09-stock-decrease-alert.png) |
| **Transfer Expiring** | `2423` | Reservation < 10 mins remaining | Amber countdown badge warning customer/cashier of auto-cancellation | [Preview](../design/screenshots/states/state-10-transfer-expiring.png) |
| **Session Sync Recovery** | `2431` | Network restored after disruption | Validates local session token and refreshes cart/inventory state | [Preview](../design/screenshots/states/state-11-recovery-session-sync.png) |
