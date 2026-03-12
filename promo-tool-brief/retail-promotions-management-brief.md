# Retail Promotions Management Tool — Build Brief

## Purpose
Build a supplier-facing promotions management tool within the Paramount Liquor website and Adobe infrastructure.

The tool should allow eligible supplier users to:
- log in via the existing Paramount supplier portal
- access a promotion submission experience through SSO / OAuth handoff
- view available promotional slots by category and period
- submit promotions against eligible slots
- receive outcome notifications after buyer review

The tool should also allow Paramount’s buying team to:
- review submitted promotions
- approve or decline them
- push approved promotions into Shopfront
- manage period releases, slot structures, supplier weighting rules, and margin rules

---

## 1. Platform and access model

### Website entry point
The promotions tool should be accessible from the existing Paramount supplier website.

### Authentication
- Supplier logs in using the standard supplier login
- If the user has the relevant retail permission/status attached to their account, they can access the promotions tool
- Access to the promotions tool should occur through seamless **OAuth / SSO** handoff from the main website

### Technical environment
The app should be built by Aden in:
- **Adobe App Builder**
- using **Adobe API Mesh**

This should feel like an integrated extension of the supplier portal, not a disconnected external tool.

---

## 2. Supplier dashboard and category navigation

### Initial supplier view
Once inside the promotions application, the supplier lands on a dashboard that lets them navigate by major category.

### Major categories
- Spirits
- RTD
- Beer
  - mainstream beer
  - international beer
  - craft beer
- Wine
- Non-alcoholic
- Other

### Promotional slotting board
The main UX should be a **visual promotional slotting board**.

This board should show:
- current promotional periods
- available slots
- already accepted promotions
- greyed-out / unavailable slots
- supplier-specific slot availability
- historical periods via backward toggle

Suppliers should be able to flick back through prior promotional periods in read-only mode.

---

## 3. Promotional periods and naming convention

### Cadence
Promotions operate on a **fortnightly cycle**.

### Naming convention
Promotional periods should be auto-created using the week number in which the promo starts.

Examples:
- P1-26
- P3-26
- P5-26
- P7-26

The same logic continues each year:
- P1-27
- P3-27
- etc.

### Required period data
Each promotional period should store:
- period code
- start date
- end date
- submission open date/time
- submission close date/time
- status
  - draft
  - open
  - closed
  - under review
  - approved / locked

### Batch release support
The team should be able to release:
- one period at a time
- multiple periods at once
- e.g. 3 periods / 6 weeks / quarter ahead

This release should trigger supplier notifications.

---

## 4. Slot structure and configurability

### Slot structure
Each period contains a configurable set of promotional slots by category/subcategory.

Example in spirits:
- vodka
  - entry
  - mid
  - premium
- scotch whisky
- bourbon
- gin
  - international gin
  - local gin

### Admin requirement
The tool must support back-end setup of:
- periods
- slots
- category/subcategory definitions
- slot counts by subcategory

This slot framework must be configurable, not hardcoded.

---

## 5. Product eligibility and visibility rules

### Eligible products
Only approved products should be promotable.

Eligible products include:
- core range
- tier one
- tier two

### Product mapping
Products must be linked internally to:
- supplier
- category
- subcategory
- pack formats where relevant

### Supplier-specific visibility
When a supplier opens a slot, they should only see products that:
- belong to that supplier
- are eligible for promotion
- match the category/subcategory of the slot
- are available for that period

### No product in consecutive periods
A product cannot be promoted in two consecutive fortnights.

If a product was promoted in the immediately previous period, it should show as **not available** for the next one.

---

## 6. Supplier weighting and slot allocation rules

A supplier should not be able to dominate the program simply by submitting first.

### Supplier weighting logic
Each supplier should have configurable weighting / allocation rules by category, based on market share or agreed commercial allocation.

Example:
- Lion has 30% market share in mainstream beer
- there are 6 mainstream beer slots in the period
- Lion gets 2 of the 6 slots

### Supplier experience
When the supplier enters a category, the dashboard should show their allowance.

Example:
- “You have filled 0 of 2 slots in mainstream beer”
- once they submit 2, the third attempt should be blocked

System message example:
> Sorry, you have no slots available. Please speak to the buyer about increasing your slots.

### Business purpose
This creates fairness, prevents slot blocking, and supports a commercial negotiation lever if a supplier wants more presence.

---

## 7. Visibility of approved promotions and historical prices

If a slot has already been accepted:
- suppliers should still be able to see the approved product
- even if it is not their product
- the board should show the promoted price point

When viewing prior periods, suppliers should also be able to see:
- products that were promoted
- the promoted price points used

This creates transparency and commercial pressure.

---

## 8. Submission flow and wizard UX

### Submission flow
When a supplier selects an available slot and then selects a valid product, the system should launch a guided **wizard-style** submission flow.

### Pack / promo format selection
For eligible products, suppliers must select one format only:
- case
- 6-pack
- single

They can only submit one format at a time for that product in that slot.

---

## 9. Commercial pricing screen

For the selected product and format, show:
- Paramount wholesale cost
- supply discount
- net retail buy price
- scan deal input
- promotional retail price point
- dollar margin
- percent margin

### Definitions
- **Supply discount** = the everyday discount already loaded through wholesale
- **Scan deal** = additional promotional funding entered by supplier

### Margin validation
The system should calculate real-time:
- dollar margin
- percentage margin

If the margin is below threshold:
- display the margin in red
- display the percentage in red
- show a message such as:

> You’ll need to increase your scan deal as you have not met the minimum promotional margin requirements.

---

## 10. Margin rules and configuration

Minimum promotional margins should be configurable by:
- category
- supplier
- pack / sale type
- possibly subcategory where needed

Example:
- 15% minimum case margin for one supplier
- 10% minimum case margin for another supplier such as CUB

Business rule priority should be:
1. supplier-specific override
2. otherwise category default

---

## 11. Market pricing benchmark integration

The pricing screen should also show market benchmark prices from Paramount’s price scraping tool.

### Show:
- lowest observed market price in the last 4 weeks
- average / most common market price in the last 4 weeks

For each benchmark show:
- retailer name
- price
- date recorded

Example:
- Dan Murphy’s — $56.00 — recorded 2 weeks ago
- Liquorland — $59.00 — average observed price

### Purpose
This is intended to influence supplier behaviour and drive stronger promotional offers.

The supplier should be forced to confront:
- what the market is doing
- whether their proposed promotional price is competitive
- whether they need to increase the scan deal while still meeting Paramount margin thresholds

---

## 12. Submission package

A supplier submission should include:
- supplier
- period
- category / subcategory / slot
- product
- pack format
- wholesale cost
- supply discount
- scan deal
- promo price
- calculated dollar margin
- calculated percent margin
- promo start / end dates
- promotional materials / assets
- submission timestamp

---

## 13. Buyer review and approvals workflow

There must be a separate buyer-side workflow / interface for Paramount’s buying team.

### Buyer capabilities
- log in
- view submissions by category / period
- review submitted products and economics
- review promotional materials
- approve or decline submissions
- see slot status and board completion

### Buyer daily summary email
Buyers should receive a daily summary email of submitted promotions relevant to their categories, with a link into the approval interface.

---

## 14. Approval action and Shopfront integration

If a promotion is approved:
- it is marked approved in the system
- the slot is locked / filled
- the promotion should automatically load into **Shopfront**

### Data pushed to Shopfront
- product
- pack / promo type
- scan deal
- pricing details
- start date
- end date
- any required promo execution fields

Approval is not just a status change, it is the operational trigger for downstream execution.

---

## 15. Decline flow

If a buyer declines a promotion:
- they must select a reason code
- they can optionally add free-text notes

### Example reason codes
- not enough margin
- price not competitive with market
- timing issue
- slot already filled
- incorrect promo structure
- other

This should create a repeatable, auditable workflow.

---

## 16. Supplier notifications

Suppliers should receive automated notifications when:
- new promo periods are released
- batches of promo periods are released
- submissions are approved
- submissions are declined

### Release notification
When a new period or batch opens, notify the qualified supplier contact with:
- period name(s)
- call to action
- deep link into the app
- support contact language

### Outcome notification
At end of day, suppliers should receive a summary of:
- approved products
- declined products
- decline reasons
- buyer notes

---

## 17. Submission window and countdown logic

Each promo period or release batch should have a controlled submission window.

Examples:
- 24 hours
- 48 hours
- 7 days

The UI should show a visible countdown timer, for example:
- “Submissions close in 48 hours”
- “This period closes in 7 days”

When the submission window closes:
- suppliers can no longer submit or edit
- the board moves into review / approval / lock phase

---

## 18. Downstream store briefing output

Once the promotion board is filled and approved, the system should support downstream generation of a promotional briefing / promotional advice output for stores.

This should likely be a separate but connected output layer, capable of compiling the approved program into a store-facing PDF.

---

## 19. Recommended MVP scope

To keep the first version practical, MVP should include:
- SSO entry from supplier portal
- supplier dashboard by category
- configurable periods and slots
- product eligibility and consecutive-period rules
- supplier weighting by category
- wizard-based promo submission flow
- scan deal and margin validation
- market benchmark pricing integration
- buyer approval / decline workflow
- supplier and buyer email notifications
- Shopfront integration on approval

### Phase 2
- richer analytics and reporting
- PDF store briefing generation
- more advanced release batching and planning tools
- buyer workload dashboards
- audit reporting
- supplier performance analytics

---

## 20. Product framing

This tool is not just a supplier submission form.

It is a **guided commercial negotiation and promotions management engine** that codifies buying rules, supplier allocation logic, pricing competitiveness, and approval workflows into one system.

Done properly, it should:
- improve promo quality before buyer review
- reduce manual back and forth
- protect margin
- improve market competitiveness
- create fair slot allocation
- accelerate program lock-in and downstream execution

---

## Short build brief for Aden

Aden,

We need to build a supplier-facing retail promotions management tool inside the Paramount website using Adobe App Builder and API Mesh.

High level:
- suppliers log in through the existing portal
- authorised users are redirected into the promotions app via SSO / OAuth
- the app presents a visual promotional slotting board by category and promo period
- suppliers can only see eligible products, available slots, and their supplier-specific slot allocation
- submissions happen through a wizard that validates commercial rules in real time
- the pricing screen must include wholesale cost, supply discount, scan deal, promo price, margin calculation, and external market price benchmarks
- minimum margin rules must be configurable by supplier/category/pack type
- products cannot run in consecutive promo periods
- buyers need a back-end approval workflow with daily summary emails
- approved promos must push directly into Shopfront with dates and pricing
- declined promos require reason codes and supplier feedback
- the system must support period creation, batch release, supplier notifications, countdown-based submission windows, and historical period visibility

The key point is this should not behave like a static form. It should behave like a guided commercial workflow that improves supplier behaviour before submissions reach the buying team.
