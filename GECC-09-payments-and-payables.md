Bundle name: GECC-09-payments-and-payables.md
Project: GECC Sales Back Office System
Company: Go Ecco Climate Control
Sprint: 09
Sprint name: Payments, Receivables, Payables, and Adjustments
Baseline: GECC-00-project-baseline.md
Status: Sprint 09 implementation bundle
Date: 2026-09-20
Previous bundle: GECC-08-electronic-signatures.md
Next bundle: GECC-10-costs-and-commissions.md
Primary database: Relational database
System of record: GECC database
Google Sheets: Export or reporting destination only
1. Sprint Objective
Implement the payment, accounts-receivable, accounts-payable, refund, credit, and adjustment functions required to support the approved GECC project lifecycle.

Sprint 09 must ensure that:

Payments are allocated to the correct customer project and invoice.
Unverified payments cannot satisfy payment requirements.
Cashier’s checks require bank confirmation.
Bank wires require a wire reference number or proof of receipt.
Unpaid invoices remain visible as accounts receivable.
Separate payables are created for equipment, dealer fees, and materials.
Payables are created when the Certificate is fully signed, regardless of when vendors are paid.
Credits, refunds, and adjustments require proper approval.
All financial actions are permission-controlled and auditable.
Payment status correctly controls project completion.
2. Sprint Scope
2.1 Included
Payment records
Cashier’s-check workflow
Bank-wire workflow
Payment verification
Payment allocation
Partial payments
Overpayment detection
Accounts-receivable balances
Credits
Refunds
Invoice adjustments
Automatic payable creation
Equipment payables
Dealer-fee payables
Materials payables
Payable approval
Payable payment recording
Payment and payable statuses
Cash-basis accounting events
Financial exception handling
Receivable and payable reports
Audit-log integration
Dashboard integration
2.2 Excluded
Automatic bank-statement import
Automatic bank reconciliation
Automatic bank-transaction matching
Credit-card processing
ACH processing
Customer payment portal
Vendor portal
Payroll processing
Commission calculations
Final financial-statement implementation
3. Sprint Deliverables
3.1 Payment Data Model
Implement database structures for:

Payment
Payment verification
Payment allocation
Payment reversal
Payment refund
Payment evidence or proof-of-receipt document
Required payment fields include:

Payment ID
Customer ID
Project ID
Invoice ID
Project identification code
Payment method
Amount
Payment date
Received date
Verification status
Payment status
Cashier’s-check number
Bank name
Bank-wire reference number
Proof-of-receipt document reference
User entering payment
User verifying payment
Verification date and time
Notes
Related adjustment reference
Created date and time
Last modified date and time
3.2 Payment Methods
The initial implementation must support:

Bank cashier’s check
Bank wire transfer
Unsupported methods must be rejected by validation and database constraints.

3.3 Payment Verification
Implement verification workflows for:

Cashier’s checks
Bank wires
A payment must remain unavailable for project completion and accounts-receivable reduction until it is verified.

Cashier’s-check verification requires:

Cashier’s-check number
Bank name
Bank confirmation
Verification user
Verification date and time
Bank-wire verification requires at least one of:

Manually entered wire reference number
Uploaded proof of receipt
3.4 Payment Allocation
Each payment must be allocated to:

One customer
One project
One invoice
One project identification code
The system must validate that all four references are consistent.

Payments may be partial, but unidentified or unallocated payments are not permitted in the operational payment workflow.

3.5 Accounts Receivable
Implement accounts-receivable calculations using:

text


Adjusted Amount Due =
Original Invoice Amount
- Approved Credits
- Approved Refunds
- Approved Adjustments
text


Remaining Balance =
Adjusted Amount Due
- Verified Amount Paid
The system must preserve the original invoice amount and each subsequent financial event separately.

3.6 Credits, Refunds, and Adjustments
Implement adjustment records for:

Credits
Refunds
Price adjustments
Other approved adjustments
Each adjustment must include:

Adjustment ID
Project ID
Invoice ID
Adjustment type
Amount
Reason
Requestor
Request date and time
Approval status
Approver
Approval date and time
Effective date
Related payment, when applicable
Related Master Sales Record version, when applicable
Notes
The Sales Manager must approve credits, refunds, and adjustments.

3.7 Accounts-Payable Data Model
Implement payable records with:

Payable ID
Project ID
Vendor ID
Payable category
Description
Amount owed
Certificate ID
Certificate signing date
Payable creation date
Due date
Payment date
Payment method
Amount paid
Outstanding balance
Payment status
Approval status
Created by
Approved by
Approval date
Notes
Related Master Sales Record version
Accounting references
Permitted payable categories:

Equipment
Dealer fee
Materials
3.8 Automatic Payable Creation
When the Certificate reaches the fully signed state, the system must:

Confirm the Certificate status.
Identify the applicable approved costs.
Create separate payables for equipment, dealer fees, and materials.
Use the Certificate signing date as the payable trigger date.
Prevent duplicate payable creation.
Link each payable to the Certificate and project.
Create an audit event.
Display the payable in the accounts-payable workflow.
Payable creation does not require GECC to have paid the vendor.

3.9 Payable Workflow
Implement:

Payable creation
Payable review
Payable approval
Payable payment recording
Partial payable payments
Payable balance calculation
Payable overdue status
Payable dispute status
The Accounts Payable Associate may record payable payments but may not alter the originating project costs, invoice, Certificate, or Master Sales Record.

3.10 Accounting Integration Events
Create ledger-compatible events for:

Verified customer payment
Approved customer refund
Approved credit
Approved invoice adjustment
Payable creation
Vendor payable payment
Payment reversal
The exact chart-of-accounts mapping remains configurable for Sprint 12.

3.11 Reports
Implement:

Accounts-receivable report
Accounts-payable report
Payment-verification report
Adjustment report
Unverified-payment report
Overpayment report
Payables-past-due report
Reports must respect role-based access restrictions.

3.12 Dashboard and Exception Integration
Add dashboard exceptions for:

Unverified cashier’s checks
Unverified bank wires
Bank wires missing reference numbers or proof
Rejected payments
Duplicate-payment warnings
Overpayments
Unpaid invoices
Partially paid invoices
Payables pending approval
Payables past due
Payables missing vendors
Refunds awaiting approval
Credits awaiting approval
Adjustments awaiting approval
Projects blocked by missing verified payment
3.13 Audit Integration
Log:

Payment creation
Payment edits
Payment verification
Payment rejection
Payment reversal
Payment refund
Payment allocation
Adjustment creation
Adjustment approval or rejection
Payable creation
Payable edits
Payable approval or rejection
Payable payment
Payable status changes
Financial-record exports
Financial-record downloads
Unauthorized access attempts
4. Approved Sprint Decisions
The following decisions are adopted for Sprint 09 based on the project baseline.

Decision 09-01 — Initial payment methods
The initial implementation supports only:

Bank cashier’s checks
Bank wire transfers
Credit cards, ACH, cash, and online payment processing are deferred.

Decision 09-02 — Verification is required
A cashier’s check or bank wire is not treated as received cash until verification is complete.

Unverified payments:

Do not reduce accounts receivable.
Do not satisfy the project-payment requirement.
Do not permit project completion.
Do not trigger earned commissions.
Decision 09-03 — Cashier’s-check verification
A cashier’s check requires bank confirmation before verification.

The confirmation event must identify:

Verifying user
Verification date and time
Bank-confirmation note or reference
Supporting document, when applicable
Decision 09-04 — Bank-wire verification
A bank wire requires either:

A manually entered wire reference number, or
Uploaded proof of receipt
The system must reject verification when neither item exists.

Decision 09-05 — Payment allocation
Every payment must be allocated to a specific project and invoice.

The system must validate consistency among:

Customer
Project
Project identification code
Invoice
Decision 09-06 — Partial payments
Partial payments are permitted.

The system must calculate remaining balance from verified transactions and approved adjustments rather than from manually edited summary values.

Decision 09-07 — Overpayments
Overpayments must be flagged for resolution.

The system must not automatically treat an overpayment as:

A new project payment
A credit
A refund
A customer account balance
An authorized user must document the resolution.

Decision 09-08 — Adjustment approval
The Sales Manager must approve credits, refunds, and adjustments.

No adjustment may affect the amount due until the approval event is complete.

Decision 09-09 — Original invoice preservation
The original invoice amount must never be overwritten.

The system must retain:

Original invoice amount
Every adjustment
Adjusted amount due
Reason
Approver
Date and time
Related records
Decision 09-10 — Payable trigger
Separate payables for equipment, dealer fees, and materials are created when the Certificate is fully signed.

The payable trigger date is the Certificate signing date.

Decision 09-11 — Payable separation
Equipment, dealer fees, and materials must be represented as separate payable records.

They must not be combined into one payable when category-specific source amounts are available.

Decision 09-12 — Payable payment permissions
The Accounts Payable Associate may enter payable payments and update payment status but may not modify the originating project cost or invoice.

Decision 09-13 — Accounting basis
Sprint 09 supports cash-basis reporting by distinguishing:

Invoice issuance
Receivable creation
Payment entry
Payment verification
Cash receipt
Revenue-recognition event
Final chart-of-accounts treatment remains configurable for Sprint 12.

Decision 09-14 — Historical preservation
Payments, refunds, credits, adjustments, payables, and payable payments cannot be silently deleted or overwritten.

Corrections require reversal, replacement, or formal adjustment records.

Decision 09-15 — Audit requirements
All payment and payable actions must use the tamper-evident audit-log system created in Sprint 03.

5. Status Definitions and Transitions
5.1 Payment Statuses
Permitted statuses:

DRAFT
PENDING_VERIFICATION
VERIFIED
PARTIALLY_ALLOCATED
FULLY_ALLOCATED
REJECTED
REVERSED
REFUNDED
CANCELLED
Primary flow:

text


DRAFT
→ PENDING_VERIFICATION
→ VERIFIED
→ PARTIALLY_ALLOCATED
→ FULLY_ALLOCATED
Exception flows:

text


PENDING_VERIFICATION → REJECTED
VERIFIED → REVERSED
VERIFIED → REFUNDED
5.2 Receivable Statuses
Permitted statuses:

NOT_DUE
OPEN
PARTIALLY_PAID
PAID
OVERPAID
DISPUTED
WRITTEN_OFF
`CANCELLED
An invoice may be marked PAID only when verified payments cover the adjusted amount due and no unresolved payment or refund issue remains.

5.3 Adjustment Statuses
Permitted statuses:

DRAFT
PENDING_APPROVAL
APPROVED
`REJECTED
REVERSED
An approved adjustment cannot be silently edited.

5.4 Payable Statuses
Permitted statuses:

DRAFT
PENDING_APPROVAL
`APPROVED
PARTIALLY_PAID
`PAID
`OVERDUE
DISPUTED
CANCELLED
6. Permission Requirements
Sales Associate
May:

View payment status for assigned projects when operationally necessary.
View whether a project is paid, unpaid, partially paid, or awaiting verification.
May not:

Verify payments.
Approve credits, refunds, or adjustments.
View unauthorized financial amounts.
Create or approve payables.
Record payable payments.
Sales Manager
May:

View all payment, receivable, and payable records.
Enter and verify payments.
Approve credits, refunds, and adjustments.
Approve payables.
Review payment exceptions.
Resolve overpayments and payment disputes.
Comptroller
May:

View all payment, receivable, and payable records.
Enter and verify payments.
Review and process financial transactions.
Review payables and payable payments.
Resolve accounting exceptions.
The Sales Manager approval requirement for credits, refunds, and adjustments remains mandatory unless a later approved decision supersedes it.

Accounts Payable Associate
May:

View authorized vendors and payables.
Enter payable payments.
Update payable payment status.
Record payment dates and methods.
View necessary project information.
May not:

Change project pricing.
Change approved project costs.
Approve customer credits or refunds.
Modify invoice terms.
Change the Certificate or Master Sales Record.
Installation Manager
May view installation-related payable information when authorized.

The Installation Manager may not approve customer credits, refunds, or invoice adjustments unless separately authorized by a later approved decision.

Technician and Other Contractors
May not view payment amounts, receivables, payables, refunds, or financial adjustments unless separately authorized.

7. Validation Rules
The system must prevent:

Payment amounts less than or equal to zero.
Unsupported payment methods.
Payments linked to nonexistent projects.
Payments linked to invoices from another project.
Mismatched project identification codes.
Cashier’s-check verification without bank confirmation.
Bank-wire verification without a reference number or proof of receipt.
Adjustments without a reason.
Adjustments without Sales Manager approval.
Payables without a project.
Payables without a Certificate reference.
Duplicate payables for the same Certificate, project, category, and source version.
Payable payments greater than the outstanding payable balance unless formally approved.
Financial records being deleted through the application.
Unauthorized access to payment and payable amounts.
Project completion based on unverified payment.
Silent changes to historical financial records.
8. Acceptance Criteria
Sprint 09 is accepted when all of the following are true:

A valid payment can be recorded against a specific project and invoice.
The project identification code is validated against the project and invoice.
Unsupported payment methods are rejected.
Cashier’s checks require bank confirmation before verification.
Bank wires require a wire reference number or proof of receipt before verification.
Unverified payments do not reduce accounts receivable.
Unverified payments do not satisfy project-completion requirements.
Partial payments calculate the remaining balance correctly.
Overpayments are detected and flagged.
Original invoice amounts remain unchanged after credits, refunds, or adjustments.
Credits, refunds, and adjustments require Sales Manager approval.
Adjustment reasons and approval history are permanently retained.
Refunds remain linked to the original payment or invoice.
Certificate completion creates separate equipment, dealer-fee, and materials payables.
Payables use the Certificate signing date as the creation trigger date.
Repeated processing of the same Certificate does not create duplicate payables.
Payable balances calculate correctly.
Accounts Payable can record payment date, method, amount, and status.
The Accounts Payable Associate cannot edit originating project costs.
Unauthorized users cannot view restricted financial amounts.
Payment and payable status transitions are enforced.
Payment and payable actions are recorded in the audit log.
Receivable and payable reports are searchable and reproducible.
Unverified payments and overdue payables appear on the exception dashboard.
Verified customer payments create accounting integration events.
Vendor payments create accounting integration events.
Payment reversals and refunds recalculate the affected receivable.
A project cannot become complete using an unverified payment.
Corrections preserve the original financial record.
All automated Sprint 09 tests pass.
9. Acceptance Test Scenarios
Payment Tests
Create a valid cashier’s-check payment.
Reject a cashier’s-check payment without a check number.
Reject cashier’s-check verification without bank confirmation.
Create a valid bank-wire payment.
Reject bank-wire verification without a wire reference or proof document.
Verify a payment and confirm that accounts receivable decreases.
Confirm that an unverified payment leaves accounts receivable unchanged.
Record a partial payment and confirm the remaining balance.
Record an overpayment and confirm the exception state.
Attempt to allocate a payment to an invoice belonging to another project.
Attempt to save a mismatched project identification code.
Attempt to create a duplicate payment and confirm a warning or controlled rejection.
Adjustment Tests
Create a credit request.
Attempt to apply a credit without Sales Manager approval.
Approve a credit and confirm the adjusted amount due.
Create a refund linked to an original payment.
Confirm that the original invoice amount remains unchanged.
Reverse an approved adjustment and confirm the historical adjustment remains visible.
Confirm that all adjustment activity appears in the audit log.
Payable Tests
Fully sign a Certificate and confirm creation of separate payables.
Confirm the equipment payable amount.
Confirm the dealer-fee payable amount.
Confirm the materials payable amount.
Trigger Certificate processing a second time and confirm no duplicates are created.
Approve a payable.
Record a partial payable payment.
Record the final payable payment and confirm PAID status.
Confirm that an overdue payable appears on the exception dashboard.
Confirm that an Accounts Payable Associate cannot edit the source project cost.
Security and Audit Tests
Confirm Sales Associates cannot view unauthorized payment amounts.
Confirm Technicians cannot access payables.
Confirm unauthorized adjustment approval is rejected.
Confirm all access failures are logged.
Confirm all financial changes are present in the tamper-evident audit chain.
Confirm historical records cannot be deleted through the application.
10. Open Questions
The following questions remain open for implementation or later approval. None may weaken the approved baseline.

Open Question 09-01 — Payment entry and verification separation
Must payment entry and payment verification always be performed by different users?

Recommended default: Enforce separation where technically practical, especially for financial control. Permit an authorized exception only with a documented reason and audit entry.

Open Question 09-02 — Overpayment resolution
Should an overpayment be resolved as:

Customer credit
Refund
Invoice adjustment
Manual exception
Recommended default: Require Sales Manager approval and an explicit resolution type. Do not automatically select a resolution.

Open Question 09-03 — Payable due-date calculation
How should payable due dates be calculated?

Possible options:

Fixed number of days after Certificate signing
Vendor-specific payment terms
Manually entered due date
Category-specific terms
Recommended default: Use vendor payment terms when available; otherwise require an authorized user to enter or approve the due date.

Open Question 09-04 — Missing vendor handling
What should happen if the Certificate is signed but a vendor is missing?

Recommended default: Create a controlled VENDOR_PENDING payable exception without permitting payment until the vendor is identified.

Open Question 09-05 — Refund payment method
Should refunds be required to use the original payment method?

Recommended default: Require the refund method to be recorded and require Sales Manager approval when the method differs from the original payment method.

Open Question 09-06 — Payment evidence file types
Which file types should be accepted for proof of receipt?

Recommended default: PDF, PNG, and JPEG, subject to the system’s standard file-size and malware-scanning controls.

Open Question 09-07 — Payable overpayment
How should an overpayment to a vendor be handled?

Recommended default: Flag it as an exception and prohibit automatic closure of the payable until corrected or approved.

Open Question 09-08 — Receivable aging intervals
Which aging intervals should be used in reports?

Recommended default: Current, 1–30 days, 31–60 days, 61–90 days, and over 90 days.

Open Question 09-09 — Write-offs
Should write-offs be included in Sprint 09?

Recommended default: Defer write-offs until a formal accounting-policy decision is approved. The data model may reserve a WRITTEN_OFF status without enabling the workflow.

Open Question 09-10 — Accounting event timing
Should the ledger event be created when a payment is verified or when the payment is fully allocated?

Recommended default: Create the cash-receipt event upon verification because the payment is then recognized as received. Allocation validation must still be complete before verification.

11. Sprint 09 Completion Definition
Sprint 09 is complete when:

Payment, verification, receivable, adjustment, payable, and payable-payment workflows operate end to end.
Cashier’s checks and bank wires follow their required verification rules.
Payment status controls project completion correctly.
Separate payables are generated from signed Certificates.
Permissions prevent unauthorized financial access and changes.
Adjustments and refunds preserve historical records.
Reports reconcile to underlying transaction records.
Dashboard exceptions are visible to authorized users.
Accounting integration events are generated.
Audit records are append-only and tamper-evident.
Acceptance tests pass.
Open questions are either resolved or formally carried forward.
This bundle is approved and preserved unchanged.
12. Next Markdown Bundle
The next bundle is:

text


GECC-10-costs-and-commissions.md
Its objective is to implement:

Equipment costs
Material costs
Dealer fees
Lead costs
Technician costs
Damon costs
Jerry costs
Gross Profit
Ryan variable commission
Doug variable commission
Net Profit
Sales Associate and GECC profit split
Fixed compensation
Commission eligibility
Biweekly contractor commission reports
Annual contractor summaries
Commission adjustments
Payroll-period corrections
Commission permissions
Commission approval workflow
Integration with completed projects and verified payments
The Sprint 10 bundle must use the verified payment and project-completion statuses established in this Sprint 09 bundle.



