Sprint 011 approval recorded.

GECC-12-financial-reporting.md
GECC Sales Back Office System — Sprint 12
Bundle Metadata
Bundle name: GECC-12-financial-reporting.md
Project: GECC Sales Back Office System
Company: Go Ecco Climate Control
Sprint: 12
Sprint name: Financial Ledger and Reporting
Baseline: GECC-00-project-baseline.md
Previous bundle: GECC-11-inventory-assets-and-loans.md
Status: Implementation bundle
Date: 2026-09-20
Next planned bundle: GECC-13-dashboard-and-search.md
Primary database: Relational database
System of record: GECC database
Google Sheets: Export or reporting destination only
1. Sprint Objective
Implement the internal double-entry accounting ledger, cash-basis financial reporting, monthly reporting periods, report approval and publication, period locking, formal adjustment entries, and required financial reports.

Sprint 12 must ensure that:

Financial events originate from controlled operational records.
Every posted ledger entry balances.
Financial reports are reproducible from source transactions.
Cash-basis revenue and expenses are reported consistently.
Balance-sheet accounts continue to reflect assets, liabilities, and equity.
Published periods are locked.
Corrections use formal adjustment entries.
Published financial statements remain permanently preserved.
The Sales Manager and Comptroller approve and sign published financial statements.
Financial reports remain auditable and permission-controlled.
2. Sprint Scope
2.1 Included
Chart of accounts
Double-entry journal
Journal entries
Journal-entry lines
Source-event references
Cash receipts
Cash disbursements
Accounts receivable
Accounts payable
Inventory
Fixed assets
Depreciation
Loans
Retained earnings
Financial-report periods
Period close
Period publication
Period locking
Formal adjustments
Balance sheet
Cash-flow statement
Statement of operations
Accounts-receivable report
Accounts-payable report
Inventory report
Depreciation report
Loan report
Fixed-asset report
Retained-earnings report
Working Capital Requirement report
Customer profitability report
Project profitability report
Financial signature workflow
Financial-report audit logging
Report exports and preservation
2.2 Excluded
Automatic bank-statement import
Automatic bank reconciliation
Automatic transaction matching
Tax-return preparation
Tax-basis depreciation
Payroll tax calculation
Contractor tax reporting
External audit management
Budgeting and forecasting
Consolidated reporting for multiple legal entities
Automatic lender integration
3. Financial Architecture
3.1 Ledger as Internal Accounting Foundation
The system must use a double-entry ledger internally.

Each posted journal entry must satisfy:

text


Total Debits = Total Credits
Operational records remain the source of business events. The ledger records the accounting effect of those events.

Examples of source events include:

Verified customer payment
Approved customer refund
Vendor payment
Payable creation
Inventory receipt
Inventory issuance
Fixed-asset acquisition
Depreciation
Loan origination
Loan payment
Retained-earnings closing entry
Formal period adjustment
3.2 Source-Event Traceability
Every journal entry must reference:

Source event type
Source record ID
Project, when applicable
Customer, when applicable
Vendor, when applicable
Reporting period
User or system process creating the entry
Creation date and time
Reversal or adjustment reference, if applicable
A journal entry must not be posted without a valid source event or an authorized formal adjustment.

3.3 Posted Entries
Posted journal entries are immutable.

Corrections require:

Reversing entry
Replacement entry
Formal adjustment entry
Reason
Authorized user
Audit record
Link to the original entry
The system must not permit ordinary users to edit or delete posted entries.

4. Chart of Accounts
The system must support a configurable chart of accounts with at least these account classes:

Assets
Liabilities
Equity
Revenue
Expenses
Contra accounts
Cash-flow classification references
4.1 Initial Account Groups
The initial configuration must support accounts for:

Assets
Cash
Accounts receivable
Inventory
Fixed assets
Accumulated depreciation
Other approved assets
Liabilities
Accounts payable
Loans payable
Other approved liabilities
Equity
Owner or company equity
Retained earnings
Current-period earnings, if used
Revenue
Cash-basis project revenue
Other approved revenue categories
Expenses
Equipment cost
Material cost
Dealer fees
Lead cost
Technician cost
Damon cost
Jerry cost
Contractor commissions
Depreciation expense
Interest expense
Overhead expenses
Other approved expense categories
The exact account numbers and final account names are configurable.

4.2 Account Controls
Each account must include:

Account ID
Account number
Account name
Account class
Normal balance
Active or inactive status
Report classification
Cash-flow classification
Parent account, if applicable
Effective date
Deactivation date
Description
An account used by a posted journal entry may not be deleted.

5. Journal Data Model
5.1 Journal Entry
Required fields:

Journal entry ID
Journal-entry number
Entry date
Posting date
Reporting period
Entry type
Source event type
Source record ID
Description
Total debits
Total credits
Status
Created by
Approved by, if required
Created date and time
Posted date and time
Reversal reference
Adjustment reference
Notes
5.2 Journal Entry Line
Required fields:

Journal-entry line ID
Journal entry ID
Account ID
Debit amount
Credit amount
Customer reference, if applicable
Project reference, if applicable
Vendor reference, if applicable
Contractor reference, if applicable
Cost category, if applicable
Description
Source reference
A journal line must have either a debit or credit amount, not both.

5.3 Entry Statuses
Permitted statuses:

DRAFT
PENDING_REVIEW
APPROVED
POSTED
REVERSED
VOIDED
A POSTED entry cannot return to DRAFT.

6. Required Accounting Events
6.1 Verified Customer Payment
A verified customer payment must create a cash-receipt accounting event.

The event must reference:

Payment
Invoice
Project
Customer
Verification event
Payment date
Reporting period
The default accounting structure is:

Debit: Cash
Credit: Accounts receivable or cash-basis revenue account, according to the configured revenue-recognition policy
The final account mapping must be configurable without changing the source payment record.

6.2 Customer Refund
An approved refund must create a cash-disbursement or receivable-reduction event.

The event must reference:

Original payment or invoice
Refund approval
Project
Customer
Refund date
Amount
6.3 Vendor Payable Creation
When the Certificate is fully signed and required payables are created, the system must create the applicable payable obligation.

The event must reference:

Payable
Project
Vendor
Certificate
Payable category
Certificate signing date
6.4 Vendor Payment
A payable payment must create a cash-disbursement event referencing:

Payable
Vendor
Project
Payment date
Payment method
Amount
The accounting event must reduce cash and the applicable payable balance.

6.5 Inventory
The ledger must support events for:

Inventory receipt
Inventory issue
Inventory return
Inventory adjustment
Inventory write-off, if approved
6.6 Fixed Assets
The ledger must support events for:

Fixed-asset acquisition
Asset placed in service
Depreciation
Asset disposal
Asset-value adjustment
6.7 Loans
The ledger must support events for:

Loan origination
Principal payment
Interest payment
Loan fee payment
Principal adjustment
Loan payoff
Principal and interest must remain separately identifiable.

7. Cash-Basis Reporting
Financial reporting is cash-basis, subject to the final accounting policy approved by the Comptroller.

7.1 Revenue
Revenue is recognized in cash-basis reports only when a customer payment is verified and recognized as received.

An invoice or accounts-receivable balance alone does not create cash-basis revenue.

7.2 Expenses
Cash-basis operating expenses are recognized when the related cash disbursement is recorded and posted.

The system must distinguish between:

Expense obligation
Payable creation
Cash disbursement
Cash-basis expense recognition
7.3 Balance-Sheet Accounts
The balance sheet must continue to report relevant non-cash balances, including:

Cash
Accounts receivable
Inventory
Fixed assets
Accumulated depreciation
Accounts payable
Loans payable
Equity
Retained earnings
The system must not remove accounts payable merely because the primary statement of operations uses cash-basis recognition.

7.4 Non-Cash Events
Non-cash events, including depreciation and payable creation, must remain in the ledger and appear in the applicable supporting reports even when excluded from cash-basis operating results.

8. Financial-Report Periods
8.1 Period Fields
Each financial-report period must include:

Period ID
Month
Year
Period start date
Period end date
Report preparation date
Report publication date
Approval status
Signature status
Locked status
Adjustment references
Created date and time
Closed date and time
Published by
8.2 Monthly Reporting Date
Monthly reports are prepared on the fifth calendar day of the following month.

Example:

text


May reports are prepared on June 5.
The report preparation date must be stored even if preparation occurs later.

8.3 Period Statuses
Permitted statuses:

OPEN
PREPARING
PENDING_APPROVAL
APPROVED
PUBLISHED
LOCKED
ADJUSTMENT_PENDING
REOPENED_BY_AUTHORIZED_EXCEPTION
8.4 Period Locking
Once financial statements are published:

The period becomes locked.
Ordinary edits are prohibited.
Posted entries cannot be changed.
Corrections require formal adjustment entries.
Original published statements remain preserved.
Any adjustment must reference the locked period.
A locked period may not be reopened through ordinary user actions.

9. Financial Statement Workflow
Step 1 — Period preparation
The system identifies the prior month and prepares the applicable reports on the fifth calendar day of the following month.

Step 2 — Source reconciliation
The system validates:

Journal-entry balance
Payment totals
Accounts-receivable totals
Accounts-payable totals
Inventory totals
Fixed-asset totals
Depreciation totals
Loan balances
Retained earnings
Report-to-ledger consistency
Step 3 — Review
The Sales Manager and Comptroller review the reports and exceptions.

Step 4 — Corrections
Errors must be corrected through:

Unposted-entry correction, if the period remains open
Formal adjustment entry, if the period is published or locked
Step 5 — Approval and signatures
Published financial statements must be approved and signed by:

Sales Manager
Comptroller
The Installation Manager does not sign financial statements.

Step 6 — Publication
After required approval and signatures:

Reports receive a publication date.
Report versions are finalized.
Reports are retained permanently.
The period is locked.
Distribution events are recorded.
10. Required Financial Reports
10.1 Balance Sheet
The balance sheet must report, by period:

Assets
Liabilities
Equity
Retained earnings
Total assets
Total liabilities and equity
The system must validate:

text


Total Assets = Total Liabilities + Total Equity
Any imbalance must block publication.

10.2 Cash-Flow Statement
The cash-flow statement must report:

Beginning cash
Customer cash receipts
Vendor cash disbursements
Contractor or commission cash disbursements
Overhead cash disbursements
Loan proceeds
Loan principal payments
Loan interest payments
Asset purchases
Asset-disposal proceeds
Other approved cash movements
Net change in cash
Ending cash
Ending cash must reconcile to the applicable cash-ledger balance.

10.3 Statement of Operations
The statement of operations must report cash-basis results, including:

Cash-basis revenue
Cash-basis project costs
Cash-basis contractor commissions
Cash-basis overhead expenses
Depreciation, according to the approved reporting presentation
Interest expense, according to the approved reporting presentation
Net income or loss
The report must clearly identify any non-cash items included for management or financial-reporting purposes.

10.4 Accounts-Receivable Report
The report must include:

Customer
Project
Invoice
Original invoice amount
Approved adjustments
Verified payments
Remaining balance
Payment status
Verification status
Aging information
10.5 Accounts-Payable Report
The report must include:

Vendor
Project
Payable category
Amount owed
Amount paid
Outstanding balance
Certificate signing date
Due date
Payment status
Approval status
10.6 Inventory Report
The report must include:

Product identity
Serial number, if applicable
Quantity
Inventory status
Cost layer
Unit cost
Total inventory value
Project allocation
Available quantity
10.7 Depreciation Report
The report must include:

Asset
Category
Acquisition cost
Placed-in-service date
Useful life
Depreciation method
Current-period depreciation
Accumulated depreciation
Net book value
Adjustment references
10.8 Loan Report
The report must include:

Lender
Loan amount
Loan date
Interest rate
Term
Principal balance
Principal paid
Interest paid
Fees
Payment schedule
Maturity date
Loan status
10.9 Fixed-Asset Report
The report must include:

Asset ID
Asset category
Description
Acquisition date
Acquisition cost
Placed-in-service date
Accumulated depreciation
Net book value
Location
Disposal status
10.10 Retained-Earnings Report
The report must include:

Beginning retained earnings
Prior-period adjustments
Current-period net income or loss
Distributions, if applicable
Other approved equity adjustments
Ending retained earnings
10.11 Working Capital Requirement Report
The report must include configurable working-capital components, initially including:

Accounts receivable
Inventory
Other current assets
Accounts payable
Other current liabilities
Working capital
Cash available
Required working capital
Working-capital surplus or deficiency
The final formula and required operating assumptions must be approved by the Comptroller.

10.12 Customer Profitability Report
The report must aggregate authorized project information by customer, including:

Customer
Project count
Invoice amounts
Verified cash receipts
Project costs
Commissions
Adjustments
Gross Profit
Net Profit
10.13 Project Profitability Report
The report must include:

Project identification code
Customer
Sales Associate
Invoice amount
Cost categories
Gross Profit
Variable commissions
Fixed compensation
Net Profit
Payment status
Project completion status
Adjustment references
11. Financial Signatures and Distribution
11.1 Required Signers
Published financial statements require:

Sales Manager approval and signature
Comptroller approval and signature
The Installation Manager does not sign financial statements.

11.2 Signature Records
Each signature must retain:

Signer identity
Signer role
Signature date and time
Report version
Period
Signature status
Signature certificate or equivalent audit reference
11.3 Report Distribution
The system must record:

Report recipients
Delivery method
Delivery date and time
Delivery status
Failure reason, if applicable
Report version distributed
Published reports must remain downloadable by authorized users.

12. Formal Adjustments
A formal adjustment is required when:

A published period contains an error.
A locked period requires correction.
A prior-period cost is corrected.
A prior-period payment is reversed.
A prior-period depreciation value is corrected.
A prior-period loan balance is corrected.
A prior-period inventory value is corrected.
A retained-earnings correction is required.
Each adjustment must include:

Adjustment ID
Affected period
Adjustment date
Effective period
Debit and credit accounts
Amount
Reason
Supporting source records
Requested by
Approved by
Approval date and time
Related original journal entries
Related reports
Notes
The original published statement must remain unchanged.

13. Permissions
Sales Manager
May:

View all financial reports.
Review financial statements.
Approve and sign published financial statements.
Review period exceptions.
Approve permitted adjustments.
View audit history.
Comptroller
May:

View all financial reports.
Review and approve financial statements.
Sign published financial statements.
Manage financial reporting.
Approve permitted adjustments.
Review ledger integrity.
View audit history.
Installation Manager
May view financial information only as authorized for operational duties.

The Installation Manager may not:

Sign financial statements.
Publish financial statements.
Unlock a closed period.
Approve retained-earnings adjustments.
Accounts Payable Associate
May view authorized payable and disbursement reports.

May not:

Publish financial statements.
Post arbitrary journal entries.
Unlock periods.
Approve financial statements.
Database Administrator
May maintain authorized database and reporting functions.

May not automatically:

Approve financial statements.
Publish reports.
Alter posted journal entries.
Delete financial records.
Sales Associates and Contractors
May access only authorized project profitability or commission information.

They may not access:

General financial statements
Other Sales Associates’ commission data
Retained earnings
Loan reports
Company-wide cash balances
Unauthorized accounts payable or receivable totals
14. Audit Requirements
The system must log:

Chart-of-accounts creation and changes
Journal-entry creation
Journal-entry approval
Journal-entry posting
Journal-entry reversal
Formal adjustment creation
Formal adjustment approval
Period preparation
Report generation
Report recalculation
Report approval
Report signature
Report publication
Period locking
Authorized exception reopening
Report export
Report download
Report print
Report distribution
Failed report delivery
Unauthorized financial access
Failed ledger-integrity checks
All entries must use the tamper-evident audit chain.

15. Sprint 12 Decisions
Decision 12-01 — Internal double-entry ledger
The system will maintain a double-entry ledger as the internal accounting foundation.

Decision 12-02 — Balanced journal entries
No journal entry may be posted unless total debits equal total credits.

Decision 12-03 — Source-event accounting
Operational transactions create accounting events. Users may not bypass source records to create unsupported financial activity except through an authorized formal-adjustment workflow.

Decision 12-04 — Cash-basis reporting
Revenue and expenses in the primary operating reports follow the approved cash-basis policy.

Verified customer receipts are required for cash-basis revenue recognition.

Cash disbursements are required for cash-basis expense recognition, subject to the approved presentation of non-cash items.

Decision 12-05 — Balance-sheet continuity
Accounts receivable, accounts payable, inventory, fixed assets, accumulated depreciation, loans, and equity remain available for balance-sheet reporting even when operating results are reported on a cash basis.

Decision 12-06 — Monthly reporting
Financial reporting periods are monthly.

Reports are prepared on the fifth calendar day of the following month.

Decision 12-07 — Published-period locking
Published periods are locked.

Ordinary edits are prohibited after publication.

Decision 12-08 — Formal correction process
Corrections to published or locked periods require formal adjustment entries.

Original entries and published statements remain preserved.

Decision 12-09 — Required financial signers
The Sales Manager and Comptroller approve and sign published financial statements.

The Installation Manager does not sign financial statements.

Decision 12-10 — Ledger immutability
Posted journal entries cannot be edited or deleted through the application.

Corrections use reversal, replacement, or formal adjustment entries.

Decision 12-11 — Report reproducibility
Reports must be reproducible from the ledger, source records, report parameters, and approved report version.

Decision 12-12 — Financial-report access
Financial reports and ledger details are permission-controlled and cannot be exposed to unauthorized users through search, export, dashboard, or document links.

Decision 12-13 — Report preservation
Published financial reports, signatures, approval records, and distribution records are retained permanently.

16. Open Questions
Open Question 12-01 — Cash-basis statement of operations
Should depreciation appear in the primary cash-basis statement of operations or only in a supporting management presentation?

Recommended default: Include depreciation as a separately identified non-cash line when required for management reporting, while also presenting a clearly labeled cash-basis operating result.

Open Question 12-02 — Accounts-payable treatment
How should Certificate-triggered accounts payable appear in the cash-basis financial statements before vendor payment?

Recommended default: Include the payable on the balance sheet and supporting accounts-payable report, but recognize the expense in the cash-basis operating statement when paid unless the Comptroller approves another treatment.

Open Question 12-03 — Revenue amount after adjustments
Should cash-basis revenue be based on:

Gross verified customer receipts
Adjusted invoice amount
Verified receipts net of refunds
Another approved treatment
Recommended default: Use verified receipts net of approved refunds for cash-basis revenue, while preserving invoice and receivable values separately.

Open Question 12-04 — Period-end cutoff
What time zone and cutoff time determine the reporting period for payments, disbursements, and adjustments?

Recommended default: Use the company’s configured operating time zone and 11:59:59 p.m. local time on the last calendar day of the period.

Open Question 12-05 — Report preparation on non-business days
If the fifth calendar day is a weekend or holiday, should reports be prepared:

On the fifth calendar day
On the preceding business day
On the next business day
Recommended default: Preserve the fifth calendar day as the report preparation date and permit actual preparation on the next business day.

Open Question 12-06 — Retained-earnings closing
Should net income or loss close to retained earnings monthly or annually?

Recommended default: Track current-period earnings monthly and perform formal retained-earnings closing annually unless the Comptroller requires monthly closing.

Open Question 12-07 — Working Capital Requirement formula
Which operational assumptions should determine required working capital?

Recommended default: Make the formula configurable and require Comptroller approval before publication of the first official Working Capital Requirement report.

Open Question 12-08 — Journal-entry approval
Which journal entries require separate review before posting?

Recommended default: Automatically generated entries may post after validation; manual and formal-adjustment entries require review by the Comptroller or another authorized approver.

Open Question 12-09 — Report version naming
Should published reports use sequential version numbers, hash-based identifiers, or both?

Recommended default: Use a human-readable sequential report version plus an integrity hash.

Open Question 12-10 — Period reopening
Can a published period ever be reopened, or must every correction use a later-period adjustment?

Recommended default: Do not reopen published periods through ordinary workflows. Use formal adjustments in the current open period.

Open Question 12-11 — Cash-flow classification
How should loan proceeds, principal payments, interest, asset purchases, and asset-disposal proceeds be classified in the cash-flow statement?

Recommended default: Configure classifications by account and event type, subject to Comptroller approval.

Open Question 12-12 — Financial statement distribution
Who receives published financial statements and supporting reports?

Recommended default: Configure a distribution list limited to the Sales Manager, Comptroller, and explicitly authorized recipients.

17. Acceptance Criteria
Sprint 12 is accepted when:

A chart of accounts can be configured.
Journal entries contain balanced debit and credit lines.
Unbalanced journal entries cannot be posted.
Every posted entry references a valid source event or authorized formal adjustment.
Posted journal entries cannot be silently edited or deleted.
Verified customer payments create accounting events.
Vendor payments create accounting events.
Inventory events create accounting events.
Fixed-asset acquisitions create accounting events.
Depreciation creates accounting events.
Loan origination and payments create accounting events.
Accounts receivable and accounts payable reconcile to source records.
Monthly financial periods are created correctly.
Reports identify the fifth calendar day of the following month as the preparation date.
Balance sheets balance.
Ending cash on the cash-flow statement reconciles to the ledger cash balance.
Cash-basis revenue excludes unverified customer payments.
Cash-basis expenses follow the approved cash-disbursement policy.
Published financial statements require Sales Manager and Comptroller approval and signatures.
The Installation Manager cannot sign published financial statements.
Published reports receive immutable report versions.
Published periods become locked.
Ordinary edits are blocked after publication.
Corrections to locked periods require formal adjustment entries.
Original published statements remain preserved after adjustment.
Retained earnings are calculated from approved source balances.
Working Capital Requirement reports use a configured and approved formula.
Project and customer profitability reports reconcile to Sprint 10 records.
Inventory, asset, depreciation, loan, receivable, and payable reports reconcile to source records.
Unauthorized users cannot view or export restricted financial information.
Report generation, approval, signing, publication, locking, and adjustment actions are audited.
Financial reports are reproducible using stored report parameters and versions.
Automated Sprint 12 tests pass.
18. Automated Test Scenarios
Ledger Tests
Create a balanced journal entry.
Reject an unbalanced journal entry.
Reject a journal entry without a source reference.
Post a verified-payment entry.
Post a vendor-payment entry.
Reverse a posted entry.
Confirm the original entry remains unchanged.
Attempt to delete a posted entry.
Confirm deletion is blocked and audited.
Period Tests
Create a monthly reporting period.
Confirm the preparation date is the fifth calendar day of the following month.
Generate reports for an open period.
Publish reports after required approvals and signatures.
Confirm publication locks the period.
Attempt an ordinary edit after locking.
Create a formal adjustment against a locked period.
Confirm the original report remains preserved.
Statement Tests
Confirm the balance sheet balances.
Confirm ending cash reconciles to the ledger.
Confirm unverified payments are excluded from cash revenue.
Confirm verified refunds reduce the applicable cash-basis result.
Confirm accounts payable remains visible on the balance sheet before payment.
Confirm depreciation is reported according to the configured presentation.
Confirm retained earnings reconcile.
Confirm Working Capital Requirement calculations use approved configuration.
Security Tests
Confirm Sales Associates cannot view company-wide financial statements.
Confirm Contractors cannot view general-ledger details.
Confirm Accounts Payable Associates cannot publish financial statements.
Confirm the Installation Manager cannot sign financial statements.
Confirm unauthorized exports are blocked and audited.
Confirm financial report access is recorded in the audit chain.
Reproducibility Tests
Generate a report twice using the same period and parameters.
Confirm identical underlying results.
Change a source record in an open period and confirm the report version changes appropriately.
Publish a report and confirm later source changes do not alter the published version.
Create an adjustment and confirm the original and corrected versions are both available.
19. Sprint 12 Completion Definition
Sprint 12 is complete when:

The double-entry ledger is operational.
Operational source events create traceable accounting entries.
Required financial reports are generated.
Cash-basis reporting rules are implemented and documented.
Monthly reporting periods are prepared, approved, published, and locked.
Formal adjustments correct published periods without overwriting history.
Sales Manager and Comptroller signatures are captured.
Reports are reproducible and permanently preserved.
Financial permissions are enforced.
Audit records are complete and tamper-evident.
Acceptance tests pass.
Open questions are resolved or formally carried forward.
The Sprint 12 bundle is approved and preserved unchanged.
20. Next Markdown Bundle
The next bundle is:

text


GECC-13-dashboard-and-search.md
Its objective is to implement:

Activity dashboard
Role-specific dashboard views
Project-status visibility
Current-task visibility
Next-required-action visibility
Due dates
Days in current status
Last-activity tracking
Warning and exception indicators
Global project search
Customer search
Contact search
Location search
Serial-number search
Equipment search
Vendor and payable search
Document search
Date-range filters
Payment and signature filters
Negative-profitability filters
Overdue-task filters
Expiring-document filters
Role-based search results
Financial-information masking
Dashboard and search audit logging
Search-performance and access-control testing


