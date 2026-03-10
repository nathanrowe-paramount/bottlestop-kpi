# Voice AI Email Triggers

This document tracks every FAQ or script that requires the AI or workflow to trigger an internal email to a team.

## Standard trigger-email flow

Use this flow for all internal email triggers unless a task requires extra fields.

1. Ask for full name.
2. Ask for mobile number.
3. Read back the mobile number to confirm it.
4. Ask for any additional details the customer wants to add to the follow-up request.
5. Add the venue name into the email trigger if it was captured during the authentication process.

Use the initial customer query as the basis for the email subject. Do not make the customer repeat the main reason for the request if it is already clear from the interaction path. Once the required details have been captured, submit the internal trigger email.

For subject lines, the subject should reflect the initial customer query or task type. For example, Request to change email address linked to account. Use `URGENT:` at the start of the subject line for time-sensitive customer service requests that need to route to an urgent queue.

Standard data to capture:
- Full name
- Mobile number
- Mobile number confirmation
- Additional details the customer wants added
- Venue name, if available from authentication or account context

## Account credit applied to order at checkout

Customer question:
Will my account credit be applied to my order at checkout?

Trigger required:
If the customer has a terms account and wants credit applied to their order, send a request to Accounts Receivable.

Internal team:
Accounts Receivable

Internal trigger email:
accounts@paramountliquor.com.au

Trigger purpose:
Request manual allocation of available account credit to the customer’s order.


## Warehouse collection request

Customer question:
Can I collect my order from the warehouse?

Trigger required:
If the customer wants to collect an existing order from the warehouse, send a request to the Customer Experience Team.

Internal team:
Customer Experience Team

Internal trigger email:
orders@paramountliquor.com.au

Trigger purpose:
Request urgent review of an existing order for possible warehouse pickup and customer callback.

Urgency note:
This should be treated as an urgent internal email flow so the request can be reviewed straight away, especially if the customer is hoping to collect the order the same day.


## Delivery day change request

Customer question:
Can I change the delivery day for my order?

Trigger required:
If the customer wants to change the delivery day for an existing order, send a request to the Customer Experience Team.

Internal team:
Customer Experience Team

Internal trigger email:
orders@paramountliquor.com.au

Trigger purpose:
Request review of available delivery days for the customer’s area and confirm whether the delivery day can be changed.


## Change email address linked to account

Customer question:
How do I change the email address linked to my account?

Trigger required:
If the customer is not the Master User and needs help changing the email address linked to the account, send a request to the support team.

Internal team:
Customer Experience Team

Internal trigger email:
orders@paramountliquor.com.au

Trigger purpose:
Request customer callback and assistance to update account access where the caller is not the Master User.

Subject line example:
Request to change email address linked to account


## Change invoice email address

Customer question:
How do I change the email address my invoices are sent to?

Trigger required:
Send a request to Accounts Receivable so they can contact the customer and update the billing contact details.

Internal team:
Accounts Receivable

Internal trigger email:
accounts@paramountliquor.com.au

Trigger purpose:
Request customer callback to update the invoice email address linked to the account.

Subject line example:
Request to change invoice email address


## Request delivery time window

Customer question:
Can I request a specific delivery time for my order?

Trigger required:
Send a request to the Customer Experience Team to review a requested delivery time window for the customer’s account or order.

Internal team:
Customer Experience Team

Internal trigger email:
orders@paramountliquor.com.au

Trigger purpose:
Request review of a delivery time window, noting that the requested time is a guide only and cannot be guaranteed.

Subject line example:
Request for delivery time window review


## Apply for payment terms

Customer question:
How do I apply for payment terms?

Trigger required:
If the customer is an existing customer, send a request to the Accounts team to review the account for payment terms.

Internal team:
Accounts Receivable

Internal trigger email:
accounts@paramountliquor.com.au

Trigger purpose:
Request review of an existing customer account for payment terms.

Subject line example:
Request to review account for payment terms


## Same-day courier request

Customer question:
Can I get my order couriered the same day?

Trigger required:
Send an urgent request to the Customer Experience Team to review whether the order is suitable for same-day courier delivery.

Internal team:
Customer Experience Team

Internal trigger email:
orders@paramountliquor.com.au

Trigger purpose:
Request urgent review of stock availability, order size, timing, and courier suitability for same-day delivery.

Subject line example:
URGENT: Same-day courier request


## Login page shows not found

Customer question:
What should I do if the login page shows not found?

Trigger required:
If basic troubleshooting does not resolve the issue, send a request to the support team to investigate.

Internal team:
Customer Experience Team

Internal trigger email:
orders@paramountliquor.com.au

Trigger purpose:
Request investigation into login page access issues after the customer has already tried basic troubleshooting.

Subject line example:
Request to investigate login page not found issue


## 504 Gateway Timeout at checkout

Customer question:
What should I do if I get a 504 Gateway Timeout error when placing an order?

Trigger required:
If the customer is still unsure whether the order was submitted, send a request to the support team to investigate.

Internal team:
Customer Experience Team

Internal trigger email:
orders@paramountliquor.com.au

Trigger purpose:
Request investigation into possible checkout failure or uncertain order submission after a 504 Gateway Timeout error.

Subject line example:
Request to investigate 504 Gateway Timeout at checkout


## Return stock request

Customer question:
Can I return stock to Paramount Liquor?

Trigger required:
Send a request to the Credits Team to review the stock and contact the customer back regarding return eligibility.

Internal team:
Credits Team

Internal trigger email:
credits@paramountliquor.com.au

Trigger purpose:
Request review of stock return eligibility, including whether the stock is unopened, saleable, and suitable for resale.

Subject line example:
Request to review stock return


## Multiple delivery addresses or event locations

Customer question:
Can I have more than one delivery address on the same account?

Trigger required:
If the customer is a festival or events customer with changing licensed locations, send a request to the Festivals and Events team.

Internal team:
Festivals and Events team

Trigger purpose:
Request follow-up for customers who need support with changing licensed event locations or additional event addresses.

Subject line example:
Request for festival or event delivery address support


## Cancel order request

Customer question:
Can I cancel my order?

Trigger required:
Send a request to the Customer Experience Team to review whether the order can be cancelled and whether any cancellation fee applies.

Internal team:
Customer Experience Team

Internal trigger email:
orders@paramountliquor.com.au

Trigger purpose:
Request review of order status to confirm whether cancellation can occur at no charge or with a cancellation fee.

Subject line example:
Request to cancel order


## Alternative product discount request

Customer question:
Can I get a discount on an alternative product if my usual item is out of stock?

Trigger required:
Send a request to the customer’s designated sales person to review an alternative product and any available discount.

Internal team:
Designated sales person

Internal trigger note:
Route through the existing user journey that sends the request to the specific sales person.

Trigger purpose:
Request sales follow-up on alternative product options and discount support while the usual item is out of stock.

Subject line example:
Request for alternative product discount review


## Supplier contract or account discount enquiry

Customer question:
How do I check my discounts or ask about supplier contract pricing?

Trigger required:
If the customer wants help with supplier contract pricing or account-specific discount arrangements, send a request to the designated sales person.

Internal team:
Designated sales person

Internal trigger note:
Route through the existing user journey that sends the request to the specific sales person.

Trigger purpose:
Request sales follow-up on supplier contract pricing or account-specific discount arrangements.

Subject line example:
Request for supplier contract pricing review


## Replacement product after stock discrepancy

Customer question:
Why didn’t an item I ordered arrive, even though it showed as in stock?

Trigger required:
If the customer wants help choosing a replacement product or loading a discount, send a request to the designated sales representative.

Internal team:
Designated sales person

Internal trigger note:
Route through the existing user journey that sends the request to the specific sales person.

Trigger purpose:
Request sales follow-up on replacement product options and any available discount after a stock discrepancy.

Subject line example:
Request for replacement product and discount review


## Urgent next-day delivery review

Customer question:
Can I get next-day delivery if I place my order today?

Trigger required:
If the customer needs an urgent delivery request reviewed, send a request to the Customer Experience Team.

Internal team:
Customer Experience Team

Internal trigger email:
orders@paramountliquor.com.au

Trigger purpose:
Request review of urgent next-day delivery options based on location, regular delivery days, and timing.

Subject line example:
URGENT: Request for next-day delivery review

