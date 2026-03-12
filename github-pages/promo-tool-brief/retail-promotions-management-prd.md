# Retail Promotions Management Tool — Detailed Product Requirements Document

## Document purpose
This PRD translates Nathan’s voice-note briefing into a detailed build specification for Aden.

The goal is to define the required workflows, commercial rules, user roles, data entities, UI modules, system behaviours, integrations, and implementation scope for a supplier-facing retail promotions management application inside Paramount’s Adobe environment.

This document is intentionally detailed. The app described is not just a form builder. It is a rules-driven workflow engine for managing supplier promotional submissions, buyer approvals, competitive pricing discipline, slot allocation, and downstream in-store execution.

---

## 1. Product objective

Build an integrated supplier promotions management application that:
- sits behind the existing Paramount supplier login
- is accessed only by authorised supplier users
- redirects into the promotions app through SSO / OAuth
- allows suppliers to submit promotions into a structured promotional program
- enforces commercial rules before a buyer ever reviews the submission
- allows Paramount buyers to approve or decline offers efficiently
- automatically pushes approved promotions into Shopfront
- supports promo-period planning, slot allocation, supplier weighting, and release notifications

This product should reduce inbox-based back and forth, improve supplier submission quality, protect margin, and create a more disciplined and scalable promotional planning process.

---

## 2. Product philosophy

The system should behave like a guided commercial engine, not a passive intake form.

It should do the following before a buyer becomes involved:
- constrain suppliers to only valid promo opportunities
- show only valid products
- enforce slot access rules
- block repeat consecutive promotions
- guide suppliers toward market-competitive promo pricing
- force suppliers to meet Paramount margin thresholds
- make weak commercial submissions psychologically harder to submit

The system should encode key buying logic directly into the supplier experience.

---

## 3. Target users and roles

### 3.1 Supplier user
A supplier user is an external user who logs in through the Paramount website.

They should only be allowed into the promotions app if they have the correct permission/status attached to their supplier account.

Capabilities:
- access the promotions app through the existing supplier portal
- view open and historical promotional periods
- view available slots by category/subcategory
- view already approved products and prices in filled slots
- see how many slots remain available to them by category
- submit eligible products only
- select pack format and enter commercial terms
- upload supporting promo materials
- receive release and outcome notifications

### 3.2 Buyer
A buyer is an internal Paramount user responsible for reviewing submissions in one or more categories.

Capabilities:
- receive daily summary emails of new submissions
- log into a buyer-facing admin workflow
- view submissions relevant to their categories
- approve or decline promotions
- provide decline reason codes and optional notes
- see slot status and filled-board progress
- trigger downstream Shopfront loading via approval

### 3.3 Admin
An admin is an internal Paramount user responsible for system configuration.

Capabilities:
- configure periods
- configure categories and subcategories
- configure slot structures for each period
- configure supplier access permissions
- configure product eligibility mappings
- configure margin rules
- configure supplier slot weighting rules
- release periods individually or in batches
- manage release windows and countdown logic
- monitor system state and notifications

---

## 4. Technical environment and architecture direction

The app should be built inside:
- **Adobe App Builder**
- using **Adobe API Mesh**

The promotions tool should feel like a native extension of the Paramount supplier environment.

### Integration entry pattern
1. Supplier logs into the Paramount supplier portal.
2. If the user has the right retail-specific permission/status, they see a promotions entry point.
3. Clicking that entry point redirects them into the promotions application.
4. The handoff should use **SSO or OAuth** so the user is not forced through a second login experience.

### Architectural principle
This should not feel like a disconnected third-party mini-app.
Authentication, access control, and navigation should all feel part of the main supplier portal.

---

## 5. Promo period model

### 5.1 Promotional cadence
Promotions run on a **fortnightly cycle**.

### 5.2 Naming convention
Promotional periods should auto-generate using the calendar week number in which the promotion begins.

Examples:
- promo starts week 1 → `P1-26`
- promo starts week 3 → `P3-26`
- promo starts week 5 → `P5-26`
- and so on

This continues year over year:
- `P1-27`
- `P3-27`
- etc.

### 5.3 Promo period fields
Each period should carry at minimum:
- period code
- year
- start date
- end date
- submission open date/time
- submission close date/time
- release batch identifier (optional)
- status
- created by
- created at
- updated at

### 5.4 Promo period statuses
Recommended statuses:
- draft
- scheduled
- open
- closed for submission
- under buyer review
- approved / locked
- completed
- archived

### 5.5 Batch release support
The system should support releasing:
- one promo period at a time
- several promo periods at once
- roughly 6 weeks, 12 weeks, or a quarter in advance

This needs to be configurable because Nathan specifically wants flexibility to release multiple future promotional periods in one go.

---

## 6. Category and slot model

### 6.1 Major categories
Top-level categories should include:
- Spirits
- RTD
- Beer
- Wine
- Non-alcoholic
- Other

### 6.2 Subcategory depth
Each top-level category needs support for subcategories.

Examples:
- Beer
  - mainstream beer
  - international beer
  - craft beer
- Spirits
  - entry vodka
  - mid vodka
  - premium vodka
  - scotch whisky
  - bourbon
  - international gin
  - local gin

This taxonomy must be configurable.

### 6.3 Slot-based promo planning
Each promotional period is made up of a set of **promo slots**.

A slot represents an opportunity to place a product in the promo program.

Example:
- Period P5-26
- Category: Spirits
- Subcategory: International Gin
- Slot count: 1

or
- Period P5-26
- Category: Beer
- Subcategory: Mainstream Beer
- Slot count: 6

### 6.4 Slot configuration requirements
Admins need to be able to configure for each period:
- which categories are active
- which subcategories exist in that period
- how many slots exist within each subcategory
- whether slots are visible to all suppliers with eligible products
- whether slots are locked, filled, or unavailable

### 6.5 Slot statuses
Recommended statuses:
- open
- partially claimed / submitted
- under review
- approved / filled
- closed
- unavailable
- greyed out due to no eligible products

---

## 7. Supplier dashboard and slot board UX

### 7.1 Dashboard intent
The supplier landing experience should be an interactive, visual promotional slotting board.

This is important. Nathan does not want a clunky list-based admin tool. He wants a dashboard that visually mirrors the structure of the promotional program.

### 7.2 Dashboard capabilities
The supplier should be able to:
- select a category
- see current open periods
- toggle back into previous periods
- see which slots are available
- see which slots are filled
- see what product has already been approved if the slot is filled
- see the promoted price point of previously accepted items
- see which slots they are allowed to play in
- see how many slots remain for them in that category

### 7.3 Historical toggle
Suppliers should be able to move backward through earlier periods in read-only mode.

Historical visibility should include:
- approved products
- promoted price points
- category layout
- competitive context

This is strategically useful because it helps suppliers understand how aggressive prior market offers were and what got accepted.

### 7.4 FOMO / commercial transparency effect
Nathan explicitly wants suppliers to be able to see past promoted prices because that creates pressure.

This is not accidental UX, it is a commercial design choice.
It helps suppliers infer:
- how aggressive they need to be
- what likely got accepted last time
- why they may have lost out before

---

## 8. Supplier eligibility and access logic

### 8.1 Portal access rule
Only supplier users with the appropriate retail permission/status should see the promotion app entry point.

### 8.2 Product visibility rules
A supplier should only see products that:
- belong to that supplier
- are in the approved promotable range
- map to the selected category/subcategory
- are valid for the selected period
- are not locked out by repeat-promo rules

### 8.3 Eligible product classes
Only these products should be promotable:
- core range
- tier one
- tier two

### 8.4 Product taxonomy mapping
Each product must be mapped to:
- supplier
- top-level category
- subcategory
- possibly segment/tier
- pack formats available for promo

This mapping is central to the tool.
Without it, the right products cannot be surfaced against the right slots.

---

## 9. Supplier weighting and slot allocation logic

### 9.1 Problem being solved
Nathan does not want one supplier, for example Diageo, flooding the board and blocking fair access to promotional inventory.

### 9.2 Weighting concept
Each supplier should have category-level allocation rules based on their share or agreed commercial weighting.

Example:
- Supplier: Lion
- Category: Mainstream Beer
- Market share: 30%
- Slots in period: 6
- Supplier entitlement: 2 slots

### 9.3 UX requirement
When the supplier enters a category, the UI should explicitly show their remaining allowance.

Example:
- “You have filled 0 of 2 slots in Mainstream Beer”
- “You have filled 2 of 2 slots in Mainstream Beer”

### 9.4 Enforcement behaviour
If they attempt to submit beyond their cap:
- block the action
- display message such as:

> Sorry, you have no slots available. Please speak to the buyer about increasing your slots.

### 9.5 Admin configurability
Weighting/allocation rules need to be configurable by:
- supplier
- category
- possibly subcategory if needed later
- effective date / period set

### 9.6 Strategic value
Nathan wants this to act as both:
- a fairness control
- a commercial negotiation lever

If a supplier wants more promotional presence, that can become part of the trading terms discussion.

---

## 10. Consecutive-period lockout rule

### 10.1 Rule
A product cannot be promoted in two consecutive fortnights.

### 10.2 Example
If Asahi Super Dry ran in the previous promotional period, it must show as unavailable in the current period.

### 10.3 UX treatment
The product or slot should show as unavailable / blocked with clear messaging.

### 10.4 Implementation note
This means the system must check prior approved promo usage by SKU and pack format when building product eligibility for the next period.

---

## 11. Submission wizard design

### 11.1 Intent
Once the supplier selects a valid slot and product, the system should move them into a guided wizard.

This should not be a giant unstructured form.

### 11.2 Suggested wizard stages
1. Select slot
2. Select eligible product
3. Select pack format
4. Review current wholesale / buy economics
5. Enter scan deal and promo price
6. Review margin and market benchmarks
7. Upload promotional assets/materials
8. Submit for review

### 11.3 Pack format choice
For a given product, the supplier should be able to choose one format only:
- case
- 6-pack
- single

They cannot submit multiple pack formats at once for the same promo opportunity.

This should be enforced with mutually exclusive selection controls.

---

## 12. Commercial pricing screen

This is one of the most important screens in the system.

### 12.1 Required displayed values
When the supplier reaches the pricing stage, show:
- selected product
- selected format
- Paramount wholesale cost
- supply discount
- net retail buy price
- scan deal input
- proposed promo retail price
- dollar margin
- percentage margin

### 12.2 Commercial field definitions
**Supply discount**
- the everyday low price discount already loaded through wholesale

**Net retail buy price**
- wholesale cost less supply discount

**Scan deal**
- extra promotional funding from the supplier, e.g. $5 per case

### 12.3 Dynamic calculations
When scan deal and proposed promo price are entered, calculate in real time:
- effective buy price after scan deal
- dollar margin
- percentage margin

### 12.4 Margin threshold validation
If the proposal falls under the allowed margin threshold:
- percentage margin displays in red
- dollar margin may also display in red
- warning message appears
- submission should be blocked until corrected

Suggested validation message:
> You’ll need to increase your scan deal as you have not met the minimum promotional margin requirements.

---

## 13. Margin rule engine

### 13.1 Rule flexibility
Margin thresholds must not be one-size-fits-all.

Nathan explicitly wants margin rules configurable at:
- supplier level
- category level
- pack / sale type level
- potentially subcategory level

### 13.2 Example
- Coopers case beer might require 15%
- CUB case beer might only require 10%

Because larger suppliers with stronger market positions may justify different commercial treatment.

### 13.3 Precedence logic
Suggested rule precedence:
1. supplier + category + pack format override
2. supplier + category override
3. category + pack format default
4. category default

### 13.4 Admin controls needed
Admin users should be able to define:
- rule name
- supplier scope
- category scope
- subcategory scope (optional)
- pack type scope
- minimum percentage margin
- effective dates
- active/inactive state

---

## 14. Market price benchmark integration

### 14.1 Objective
Nathan wants the system to use competitive pricing intelligence to pressure suppliers into making stronger offers.

### 14.2 Data source
The app should integrate with Paramount’s existing **price scraping tool**.

### 14.3 Metrics to show
Nathan narrowed the required display to:
- lowest observed market price in last 4 weeks
- average / most common market price in last 4 weeks

### 14.4 Additional data shown beside benchmarks
For each benchmark, show:
- retailer name
- recorded price
- recorded date

Example:
- Dan Murphy’s — $56.00 — recorded 2 weeks ago
- Liquorland — $59.00 — average observed price

### 14.5 UX placement
These benchmark values should appear on the pricing screen, directly near the proposed promo price field.

### 14.6 Behavioural design intent
This is meant to reduce unrealistic supplier submissions.

If the supplier sees:
- Dan Murphy’s at $56
- and they are trying to submit at $62

…they should self-correct and improve their scan deal before submission.

### 14.7 Business effect
This feature effectively makes the buying team’s negotiation posture visible inside the product.

---

## 15. Submission payload

Each supplier submission should capture at minimum:
- submission id
- supplier id
- supplier user id
- period id
- slot id
- category
- subcategory
- product sku / product id
- pack format
- wholesale cost at submission time
- supply discount at submission time
- scan deal submitted
- promo retail price submitted
- calculated dollar margin
- calculated percentage margin
- benchmark market low
- benchmark market average
- promo start date
- promo end date
- uploaded assets/materials references
- submission notes (optional)
- submitted at timestamp
- status

Recommended statuses:
- draft
- submitted
- under review
- approved
- declined
- sent to Shopfront
- failed Shopfront push
- archived

---

## 16. Buyer workflow

### 16.1 Buyer queue
Buyers need a dedicated admin interface where they can review submitted promotions.

### 16.2 Buyer queue capabilities
- filter by period
- filter by category/subcategory
- view supplier
- view slot
- view economics
- view market benchmarks
- view submitted assets
- approve or decline
- see if slot already filled
- see board completion progress

### 16.3 Buyer daily digest
Each buyer should receive a daily summary email containing:
- submitted promotions from that day
- grouped by relevant category where practical
- a link into the queue / review screen

Nathan described this as part of the operating rhythm, buyers logging in each day to process the submissions.

---

## 17. Approvals and decline logic

### 17.1 Approval behaviour
If buyer approves:
- submission status becomes approved
- slot becomes filled / locked
- board updates supplier-side to show approved product and price
- Shopfront integration trigger fires

### 17.2 Decline behaviour
If buyer declines:
- they must select a reason code
- they may also add notes for the supplier
- submission status becomes declined

### 17.3 Suggested decline reason codes
- not enough margin
- price not competitive with market
- timing issue
- slot already filled
- product not suitable for promo slot
- incorrect promo mechanics
- other

These should be configurable, but seeded with useful defaults.

---

## 18. Shopfront integration

### 18.1 Trigger point
Approval should trigger automatic loading of the promotion into **Shopfront**.

### 18.2 Data likely required by Shopfront
- product / SKU
- pack format
- scan deal
- promo retail price
- start date
- end date
- promo identifiers
- any store execution metadata required

### 18.3 Error handling
If Shopfront push fails:
- capture error state
- mark submission as failed Shopfront push
- notify relevant internal users
- allow retry

### 18.4 Importance
This is not optional polish. Approval only matters if the downstream execution is automated and reliable.

---

## 19. Notifications

### 19.1 Supplier release notifications
When a new promo period or release batch opens, notify the nominated retail contact at each eligible supplier.

The notification should include:
- period code(s)
- link to open the promo tool
- note that submissions are now open
- support wording for questions

### 19.2 Supplier outcome notifications
At end of day, suppliers should receive a summary including:
- products approved
- products declined
- decline reasons
- buyer notes

### 19.3 Buyer digest notifications
Buyers should receive a daily digest of new submissions to review.

### 19.4 Optional future notifications
Potential later additions:
- reminder before submission window closes
- reminder for incomplete drafts
- alert when slots are nearly filled
- alert when Shopfront integration fails

---

## 20. Submission windows and countdown timer

### 20.1 Controlled submission windows
Nathan wants periods to have a fixed submission window, for example:
- 24 hours
- 48 hours
- 7 days

### 20.2 Countdown requirement
The supplier-facing UI should clearly show the remaining time to submit.

Example:
- “This promo closes in 48 hours”
- “7 days left to submit”

### 20.3 Close behaviour
When the window expires:
- no more new submissions
- no edits to existing submitted records unless internal override exists
- status moves to closed for submission
- buyers continue in review workflow

---

## 21. Historical visibility and board transparency

### 21.1 Historical browsing
Suppliers should be able to view previous promo periods.

### 21.2 Historical detail visible
- which products were promoted
- the price points used
- category layout
- filled slots

### 21.3 Strategic reason
This is meant to create transparency and competitive pressure.
It also helps suppliers calibrate what kind of offer is needed to win next time.

---

## 22. Downstream store briefing output

Once the promo board is filled and approved, the system should support a downstream output to brief stores.

Nathan referenced a promotional advice / store briefing PDF that goes to stores.

This may be a separate phase, but the system should at minimum structure approved promo data cleanly enough to support:
- PDF generation
- store communication output
- final promo pack compilation

---

## 23. Admin modules required

Admin needs tools for:
- period management
- slot configuration
- category/subcategory management
- supplier eligibility permissions
- product mappings
- margin rule configuration
- supplier weighting configuration
- email template configuration
- release management
- countdown / submission window settings
- decline reason code management

---

## 24. Suggested UI modules

### Supplier-facing
- Supplier dashboard / landing page
- Category selector
- Promo slot board
- Slot details panel
- Product selector
- Pack format selector
- Pricing / margin wizard step
- Asset upload step
- Review + submit step
- Historical periods viewer

### Buyer-facing
- Buyer dashboard
- Submission queue
- Submission detail view
- Approval / decline panel
- Slot board completion view

### Admin-facing
- Period management screen
- Slot builder screen
- Supplier weighting screen
- Margin rules screen
- Product taxonomy mapping screen
- Notifications config screen

---

## 25. Data model, recommended entities

Recommended entities/tables:
- suppliers
- supplier_users
- user_permissions
- promo_periods
- promo_release_batches
- promo_categories
- promo_subcategories
- promo_slots
- supplier_slot_allocations
- products
- product_category_mappings
- product_pack_formats
- supplier_product_eligibility
- margin_rules
- market_price_snapshots
- promo_submissions
- promo_submission_assets
- approval_decisions
- decline_reason_codes
- notification_events
- integration_jobs
- audit_logs

---

## 26. Auditability and operational controls

The system should keep an audit trail of:
- who created or edited slot structures
- who released promo periods
- who submitted promotions
- who approved or declined them
- which reason code was used
- what data was pushed to Shopfront
- when notification events were sent

This is important because the workflow impacts pricing, trade investment, and store execution.

---

## 27. MVP recommendation

### Must-have MVP scope
- SSO portal entry
- supplier permission gating
- promo period management
- slot configuration by category/subcategory
- product eligibility mapping
- supplier slot weighting rules
- supplier slot board UX
- one-format-only promo submission wizard
- scan deal + promo price inputs
- margin rule engine
- benchmark market pricing display
- consecutive-period lockout rule
- buyer review queue
- approve/decline workflow
- decline reason codes
- Shopfront approval trigger
- supplier and buyer notifications
- countdown-based submission windows

### Explicitly phase 2 / later
- advanced analytics dashboards
- supplier performance scoring
- negotiation insights reporting
- automated PDF store brief generation
- deeper audit/report exports
- more advanced scenario modelling
- AI-assisted buyer recommendations

---

## 28. Acceptance criteria, high level

### Access
- Given a supplier user without retail permission, they cannot access the promotions app.
- Given a supplier user with permission, they can enter via portal without separate login.

### Slot board
- Given a supplier in a category, the board shows only relevant slots and their remaining allocation.
- Given a slot where the supplier has no eligible products, it appears unavailable / greyed out.

### Product rules
- Given a product outside core range / tier one / tier two, it is not selectable.
- Given a product that ran last period, it is blocked this period.

### Commercial rules
- Given a promo below the configured margin threshold, submission is blocked.
- Given a supplier exceeds slot allocation, further submissions are blocked.
- Given market benchmark data exists, it is displayed beside the promo price input.

### Approval
- Given buyer approval, submission status updates and Shopfront integration is triggered.
- Given buyer decline, a reason code is mandatory.

### Notifications
- Given a new period release, suppliers receive a release email.
- Given review outcomes, suppliers receive an end-of-day summary.
- Given new submissions, buyers receive a digest.

---

## 29. Open questions for Aden / implementation workshop

These are not blockers to drafting, but should be confirmed during build planning:
- exact source of supplier permissions inside the current portal
- exact source of product master and supplier product mapping
- whether core range / tier one / tier two are already explicitly tagged in source systems
- Shopfront API or file-based integration mechanism
- exact format for promo assets upload and storage
- whether approval is single-stage or requires multiple internal approvers
- whether price scraping data is SKU-level clean enough for direct benchmark display
- whether weighting is always category-level or sometimes subcategory-level
- whether suppliers can edit drafts before submission close
- whether accepted promo visibility should be full-price transparent in all cases

---

## 30. Build summary for Aden

Aden, the core thing to understand is this:

This app is not just a submission portal.
It is a structured commercial workflow that should push suppliers toward stronger offers before buyers intervene.

The major mechanics you need to preserve are:
- permission-based entry from the supplier portal
- visual promo slot board by period/category
- supplier-specific slot entitlements
- product eligibility filtering
- no consecutive-period repeat promos
- single-format submission flow
- dynamic margin validation
- market benchmark visibility beside proposed promo price
- buyer approval / decline workflow
- automatic Shopfront loading on approval
- period release batching and countdown windows
- historical transparency of previously approved prices

If those mechanics are built properly, the product will do a meaningful chunk of the buyer’s commercial filtering before the buyer ever touches the submission.
