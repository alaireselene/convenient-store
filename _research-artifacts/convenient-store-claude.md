# Convenience Store Operations: A Complete Software Design Reference

**Building software for convenience store operations—particularly those with made-to-order food service—requires deep understanding of interconnected workflows spanning inventory, POS, kitchen production, food safety, and cash management.** This reference document provides granular operational procedures drawn from 7-Eleven, Circle K, FamilyMart, and Wawa models, with specific attention to Vietnamese market requirements. The workflows documented here define the functional specifications necessary for a comprehensive retail/food service management system.

---

## Core operational workflows form the backbone of convenience store software

Modern convenience stores operate through five tightly integrated operational domains: inventory management, point-of-sale transactions, shift handover, cash reconciliation, and staff scheduling. Each domain requires specific system capabilities and follows standardized procedures that have been refined across major chains globally.

### Inventory management follows a receiving-to-replenishment cycle

**Receiving procedures** establish the foundation for accurate inventory tracking. When deliveries arrive, staff must verify items against purchase orders, inspect for damage, check expiration dates on perishables, and verify temperature for cold-chain products (refrigerated items must arrive at ≤41°F, frozen items frozen solid). The system must capture: delivery timestamp, supplier information, item quantities with variances noted, and batch numbers for perishables. Japanese convenience stores like FamilyMart receive fresh deliveries **3 times daily** for temperature-sensitive items—a benchmark for systems supporting high-turnover fresh food operations.

**Stocking follows FIFO/FEFO principles** (First-In-First-Out / First-Expired-First-Out). Staff pull items from backroom storage, verify expiration dates before stocking, position newer items at back with older items at front, ensure products face forward, and verify pricing labels. Software must enforce FIFO through date-tracking and generate alerts when rotation may be compromised.

**Expiry management requires daily systematic checks.** The system should generate expiry alert reports, trigger sales-prevention blocks at the register for expired items, enable markdown application for near-expiry products, and track all disposals for shrinkage reporting. Cycle counting uses ABC analysis: A-items (high-value/high-turnover) counted weekly, B-items monthly, C-items quarterly.

**Reorder point management** calculates minimum stock thresholds based on lead time, average daily sales, and safety stock buffers. Modern chains like Circle K use AI-based supply chain solutions (Relex) for automated ordering and demand forecasting. The system should monitor inventory in real-time, generate alerts at reorder points, calculate suggested order quantities, and require manager approval before transmission to suppliers.

### Point-of-sale operations handle transactions and compliance

The standard checkout workflow proceeds through: greeting → scanning items → quantity entry → discount/promotion application → tax calculation → loyalty program capture → payment processing → receipt generation. The system must support hotkey shortcuts for frequent items, touch-screen optimization, barcode/SKU/PLU search, modifier prompts for customizable items, and offline capability during internet outages.

**Age-restricted item verification** is legally mandated and system-enforced. When restricted items (tobacco, alcohol) scan, the system must prompt for ID verification. Staff check government-issued ID for authenticity (holograms, UV features), verify photo match, confirm current expiration, and scan the ID barcode or manually enter date of birth. The system then confirms/denies legal age—**only supervisors can override verification failures**. Circle K stores must achieve 90%+ on quarterly compliance evaluations including tobacco verification.

**Returns and refunds** follow a controlled workflow: verify receipt and return timeframe (typically 7-30 days), inspect product condition, process through POS return function, refund to original payment method. Cash refunds above threshold require manager approval. The system must log return reasons and either restore items to inventory or mark as damaged.

### Shift handover ensures operational continuity

The shift handover process transfers accountability between outgoing and incoming staff through systematic documentation. **Cash drawer handover** requires the outgoing cashier to count all cash by denomination, record totals on a cash count sheet, compare to POS expected total, document variances, and obtain dual signatures from both cashiers. The incoming cashier re-counts to confirm before accepting the drawer.

**Pending task communication** covers: completed tasks, outstanding work, issues encountered and resolution status, customer requests, equipment malfunctions, expected deliveries, and promotional changes to implement. This information should be captured in a standardized shift handover report with date/time, employee names/signatures, and notes for the next shift.

**Store condition assessment** uses a walk-through checklist covering: aisle stocking and facing, floor cleanliness, restroom status, outdoor area, equipment functionality (coffee machines, slushie, coolers/freezers), security cameras, and signage.

### Cash management maintains financial control through structured procedures

**Opening float setup** involves retrieving the cash drawer from the safe, verifying starting amount (typically $100-200), counting by denomination, ensuring adequate change, obtaining dual verification, logging in POS, and signing the cash log. Standard float composition includes mixed small bills and adequate coin rolls, with no bills larger than $20.

**Cash drop procedures** activate when the drawer exceeds maximum limits (typically $200) or after receiving large bills ($50, $100). Staff count excess cash, place in a numbered envelope, record on cash log (time, amount, employee), deposit through the drop safe slot, and document the drop number. Many stores use a **two-person rule** for drops.

**End-of-day reconciliation** follows the formula: Expected Cash = Starting Float + Cash Sales - Refunds - Cash Drops. Staff print the closing POS report, count all cash by denomination, remove starting float, compare to expected total, document variance, and prepare the bank deposit bag with dual signatures. **Variance thresholds** typically operate at three levels: under $5 (document only), $5-25 (manager review), over $25 (investigation mandatory).

---

## Made-to-order food service demands integrated kitchen-to-counter workflows

Food service operations represent the **critical growth area** for convenience stores—fresh food accounts for 30%+ of sales at Japanese 7-Eleven, and U.S. chains are targeting similar percentages. Software must orchestrate the complete order-to-fulfillment cycle while enforcing food safety compliance.

### Kitchen station workflows follow assembly-line principles

**Station setup begins 30-60 minutes before service**: power on cooking equipment, verify temperatures (hot-holding at 135°F+, cold-holding at 41°F or below), calibrate thermometers, and position ingredients within arm's reach. Prep batch setup involves cutting vegetables, portioning proteins, preparing sauces, and **labeling all prep items** with: product name, prep date, use-by date, and preparer initials.

**Station types** typically include cold prep (sandwiches, salads, wraps), hot prep (grilled items, fried foods), assembly/expo (final order compilation), and beverage stations. The Wawa model positions refrigerated sandwich stations adjacent to high-speed ovens (Merrychef eikon e2s) for efficient toasted hoagie production in approximately 1 minute.

**Order flow through Kitchen Display Systems (KDS)** eliminates paper tickets entirely. Orders placed via POS, kiosk, mobile app, or third-party delivery platforms transmit instantly to the KDS server, where intelligent routing sends items to appropriate stations. Each station screen shows only relevant items, with **color-coded timing alerts**: green (on-time), yellow (approaching target), red (delayed). Cooks "bump" completed items, triggering status updates to the expo station and front counter.

### The order-to-fulfillment process coordinates multiple stations

**Order queuing and prioritization** follows FIFO by default, with items having longer prep times started earlier. The system may prioritize in-store orders over delivery based on configuration, display promised pickup times, and allow manual rush-order flagging. Similar items can be grouped for batch cooking efficiency.

**Preparation time tracking** captures: ticket time (order received → completed), station time (arrives at station → station completion), and total fulfillment time (order placed → customer pickup). **Target times** vary: fast-casual 4-7 minutes, QSR 2-4 minutes, Wawa hoagies 2-3 minutes. The system should alert when orders exceed 1.5x the average expected time.

**Order assembly at the expo station** follows this workflow: items from multiple stations arrive, the expo coordinator verifies all items are present, assembly follows brand standards, packaging is selected based on order type (dine-in, takeout, delivery), and takeout/delivery orders receive labels showing customer name, order number, contents list, and special instructions.

**Customer notification** employs multiple methods: order number displays, name calls, text/push notifications for mobile orders, pager systems, or designated pickup areas with labeled order shelving.

### Ingredient inventory differs fundamentally from finished goods tracking

**Recipe-based inventory deduction** (preferred method) links each menu item to a recipe with an ingredient list. When a hoagie sells, the system automatically deducts: 6" bread (1), turkey (3oz), cheese (2 slices), lettuce (0.5oz), etc. This enables real-time inventory tracking, automatic reorder triggers, and variance analysis comparing actual to theoretical usage.

**Prep batch tracking** requires all prepared items to be labeled with: product name, prep date/time, preparer name/initials, and use-by date (maximum **7 days** for TCS foods held at 41°F). Color-coded day labels are industry standard: Monday (blue), Tuesday (yellow), Wednesday (green), Thursday (red), Friday (orange), Saturday (white), Sunday (purple).

**Par level management** calculates minimum ingredient quantities based on historical sales data, day-of-week patterns, seasonal variations, and lead time for reorder. Advanced systems like PDI Foodservice Production Management use **data-driven build-to forecasts in 30-minute increments** based on 2-week sales history.

### Food safety compliance requires systematic temperature and time controls

**Critical temperature thresholds** define system parameters:

| Stage | Requirement |
|-------|-------------|
| Cold storage | ≤41°F (5°C) |
| Freezer | 0°F (-18°C) or below |
| Hot holding | ≥135°F (57°C) |
| Danger zone | 41°F - 135°F |
| Reheating | 165°F within 2 hours |
| Cooking poultry | 165°F internal |
| Cooking ground beef | 155°F internal |

**Monitoring frequency**: refrigerators/freezers minimum twice daily (AM/PM), hot-holding equipment every 2-4 hours. IoT monitoring systems (SmartSense, E-Control Systems) provide continuous recording with automatic alerts for excursions, 24/7 monitoring, and automated compliance documentation.

**Hold time limits enforce food safety**:
- Hot TCS foods: maximum **4 hours** in danger zone; discard if temperature drops below 135°F for more than 4 hours
- Cold TCS foods without refrigeration: maximum **6 hours** if started at ≤41°F and does not exceed 70°F
- Cold TCS foods with refrigeration: maximum **7 days** at ≤41°F (Day 1 = preparation day)

**HACCP compliance** implements seven principles: hazard analysis, critical control point identification, critical limit establishment (e.g., 165°F cook temp), monitoring procedures, corrective actions, verification procedures, and record keeping. Common CCPs include receiving temperature verification, cold storage maintenance, cooking temperature verification, and hot/cold holding maintenance.

### Waste management tracks losses and enables reduction strategies

**Waste categorization** distinguishes: overproduction (made too much), expired (reached use-by date), temperature abuse (out of safe range too long), quality issues, preparation errors, customer returns, and trim/spoilage. The waste log captures date/time, item name, quantity/weight, category/reason, dollar value (calculated from recipe cost), and responsible employee.

**Waste reduction strategies** include data-driven production forecasts, just-in-time cooking rather than batch cooking, smaller menus to reduce complexity, dynamic end-of-day pricing, and donation programs. Industry data indicates businesses can save **$7 for every $1 invested in waste reduction**, representing a potential $1.6 billion annual profit opportunity.

### Recipe management ensures consistency and cost control

**Standard recipe documentation** includes: recipe name and ID, yield (portions), portion size, prep/cook time, ingredient list with quantities and unit costs, step-by-step preparation instructions with temperatures and times, and nutritional/allergen information. The system should support sub-recipes (prep items linked to final menu items), yield/shrinkage calculations, and automatic cost recalculation when ingredient prices change.

**Recipe costing** tracks: raw ingredient costs (updated from invoices), yield percentages (fabrication loss), labor and overhead components, and packaging costs. Key metrics include food cost per portion, food cost percentage (cost ÷ menu price), profit margin per item, and menu engineering analysis (stars, plowhorses, puzzles, dogs).

---

## Vietnamese market context shapes regulatory and operational requirements

Vietnam's convenience store sector includes approximately **1,374 stores** representing 0.3% of retail sales, with a CAGR exceeding 18%. The market is dominated by Circle K (48% revenue share, 476+ stores), FamilyMart (18.8%), Ministop (14.3%), GS25 (14%), and 7-Eleven (7.3%). Only Circle K has achieved consistent profitability ($4M+ profit in 2022).

### Regulatory compliance requires specific certifications and documentation

**Food safety certification** (Giấy chứng nhận cơ sở đủ điều kiện an toàn thực phẩm) is governed by the Law on Food Safety 2010, Decree 15/2018/NĐ-CP, and related circulars. The provincial Department of Health (Sở Y tế) issues certificates for food service establishments, valid for **3 years** with renewal required 6 months before expiry.

**Food handler requirements** mandate completion of approved food safety training (Giấy xác nhận tập huấn kiến thức về an toàn thực phẩm) and annual health examinations. Employees must be free of tuberculosis, acute diarrhea, skin infections, Hepatitis A/E, typhoid, cholera, and dysentery.

**Labeling requirements** under Decree 15/2018/NĐ-CP specify: product name in Vietnamese, manufacturer name/address, net quantity, production/expiry dates, ingredients list, usage instructions, storage conditions, and origin. Small-scale establishments exempt from full food safety certification must still maintain training certificates and health examination records.

### Local payment and delivery integration is essential

**E-wallet integration** must support MoMo (30M+ users), ZaloPay, VNPay (VietQR standard with 40+ bank integration), ViettelPay, ShopeePay, and Moca/GrabPay. QR code payments are growing rapidly, though cash remains important, especially in rural areas.

**Delivery platform integration** prioritizes ShopeeFood (56% market share) and GrabMart (36%). Baemin exited Vietnam in December 2023. Convenience chains including Circle K, 7-Eleven, GS25, and WinMart+ are listed on these platforms, requiring order injection to POS/KDS, menu synchronization, and delivery status tracking.

### Labor regulations affect scheduling system requirements

Vietnamese labor law specifies: maximum **8 hours/day, 48 hours/week**; overtime requires employee consent with daily maximum of 50% of normal hours; monthly overtime cap of 40 hours; annual cap of 200 hours. **Overtime compensation** rates are 150% for weekdays, 200% for weekends, 300% for holidays. Night shift work (22:00-06:00) adds a **30% wage premium**.

The system must track: total hours against 48-hour weekly ceiling, overtime hours against daily/monthly/annual limits, night shift premium calculations, and mandatory rest periods (30-minute break after 6 hours, 45 minutes for night shifts, minimum 1 day weekly rest).

---

## Software system requirements span POS, KDS, inventory, and analytics

### POS systems require food service-specific capabilities

**Order entry interface** must include touch-screen optimization with customizable button layouts, hotkey shortcuts, item search by barcode/SKU/PLU/name, modifier prompts for customizations, split-screen for multiple orders, offline mode for internet outages, and customer-facing displays.

**Menu management** supports unlimited SKUs, time-based menus (breakfast/lunch/dinner switching), nested modifier groups (size → crust → toppings), combo/meal deal builders with automatic pricing, item availability toggles (86'd items), and seasonal/promotional item scheduling.

**Kitchen routing** implements station-based routing (grill, fryer, prep, beverage), order-type routing (dine-in, takeout, delivery), priority flagging, coursing/firing control, and split routing for items going to different stations.

**Payment processing** handles split by item/seat/amount, multiple tender types per transaction, EMV chip/NFC/contactless support, EBT/SNAP integration for eligible items, and tip adjustment. For Vietnam, integration with MoMo, ZaloPay, VNPay, and other local e-wallets is mandatory.

### Kitchen Display Systems coordinate production flow

**Order display requirements** include real-time display as orders are placed, clear item details with modifiers and special requests, order source identification (counter, online, delivery), customer name/order number, allergen alerts with visual flagging, and promised pickup time.

**Timing and routing features** encompass color-coded timers with configurable thresholds, multi-screen support for different prep stations, automatic order splitting by station, station-to-station bumping, and expo/expediter screens for assembly. **Hardware requirements** specify industrial-grade displays (IP-54 rated for grease/moisture resistance), 15-24" touchscreens, operating temperature range up to 60°C, and optional bump bars.

### Inventory modules must support recipe-based deduction

Core features include unlimited SKU capacity with multiple barcode formats, ingredient-level tracking with automatic depletion on sale, sub-recipe support for prep items, batch/lot tracking with expiration dates, FIFO enforcement, par level management with auto-alerts, actual vs. theoretical usage reports (target variance: 2-5%), and waste logging with reason codes.

**Supplier integration** enables electronic purchase order generation, invoice import/matching, price comparison across vendors, and receiving workflows with variance tracking.

### Reporting and analytics drive operational decisions

**Essential reports** include:
- **Sales**: hourly, daily, item-level/PMIX, category, employee, payment type, discount/void
- **Inventory**: stock levels, turnover analysis, waste, variance (actual vs. theoretical), reorder status, valuation
- **Labor**: hours worked, labor cost percentage, sales per labor hour (SPLH), overtime, schedule vs. actual variance
- **Food cost**: recipe cost breakdown, menu item profitability, prime cost (food + labor), theoretical vs. actual food cost

**Real-time dashboards** display live sales tickers, KDS ticket time monitoring, labor cost percentage, inventory alerts, and configurable KPIs by user role with mobile accessibility.

---

## Actor roles define system permissions and operational boundaries

### Permission matrix establishes access control

| Permission | Store Manager | Shift Supervisor | Cashier | Kitchen Staff | Inventory Staff |
|------------|:-------------:|:----------------:|:-------:|:-------------:|:---------------:|
| Process sales | ✓ | ✓ | ✓ | ✗ | ✗ |
| Manager-level discounts | ✓ | ✓ | ✗ | ✗ | ✗ |
| Void entire receipts | ✓ | ✓ | ✗ | ✗ | ✗ |
| Process refunds | ✓ | ✓ | ✗ | ✗ | ✗ |
| Safe access | ✓ | ✓ | ✗ | ✗ | ✗ |
| Adjust inventory | ✓ | Limited | ✗ | Limited | ✓ |
| Receive shipments | ✓ | ✓ | ✗ | ✗ | ✓ |
| Approve purchase orders | ✓ | ✗ | ✗ | ✗ | ✗ |
| View/bump KDS orders | ✓ | ✓ | ✗ | ✓ | ✗ |
| Edit menu items/prices | ✓ | ✗ | ✗ | ✗ | ✗ |
| Create employee accounts | ✓ | ✗ | ✗ | ✗ | ✗ |
| System configuration | ✓ | ✗ | ✗ | ✗ | ✗ |

### Role-specific responsibilities guide system design

**Store managers** hold full administrative access with P&L accountability, staff management authority, vendor relationships, and all override capabilities. They access all reports including financial data, create/modify employee profiles and permissions, and configure system settings.

**Shift supervisors** exercise limited overrides: transaction voids within thresholds, refund approval up to limits, price overrides within ranges, age verification overrides with documentation, and time clock corrections. They manage cash including safe drops and variance investigation but cannot modify schedules or employee permissions.

**Cashiers** process standard transactions, apply preset discounts, manage their assigned drawer only, perform opening/closing counts, and conduct age verification (without override authority). They cannot void finalized receipts, process refunds, or access the safe.

**Kitchen/food prep staff** view assigned station orders on KDS, bump completed items, recall recent orders, log waste for prepared items, mark items as 86'd, and view (not edit) recipes. They cannot modify order contents, prices, or receive shipments.

**Inventory staff** receive and verify shipments, perform full and cycle counts, adjust quantities with reason codes, transfer stock between locations, create draft purchase orders, and manage bin locations. They cannot approve purchases above threshold or modify item costs.

---

## Conclusion: Key specifications for software design

This operational analysis establishes the core requirements for convenience store management software. **Temperature thresholds** (cold storage ≤41°F, hot holding ≥135°F, danger zone limits) must be system-enforced with automated alerts. **Time limits** (7-day TCS hold at refrigeration, 4-hour hot-hold maximum, 6-hour cold-without-refrigeration maximum) require systematic tracking and expiry management.

**Recipe-based inventory deduction** is the preferred method for made-to-order food service, enabling real-time ingredient tracking and variance analysis. **KDS integration** must support station-specific routing, color-coded timing alerts, bump workflows, and real-time status communication to front-of-house.

For Vietnamese deployment, software must integrate local e-wallets (MoMo, ZaloPay, VNPay), delivery platforms (ShopeeFood, GrabMart), and comply with labor regulations including the 48-hour weekly ceiling, overtime caps, and night-shift premiums. Food safety documentation (training certificates, health examinations) must be tracked per Decree 15/2018/NĐ-CP requirements.

The role-permission matrix defines access boundaries that maintain operational control while enabling efficient workflows—with manager overrides requiring authentication, reason codes, and audit trails for all restricted actions.