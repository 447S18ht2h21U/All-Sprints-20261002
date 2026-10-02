Bundle name: `GECC-10-costs-and-commissions.md
Project: GECC Sales Back Office System
Company: Go Ecco Climate Control
Sprint: 10
Sprint name: Costs, Profitability, Commissions, and Contractor Reports
Baseline: GECC-00-project-baseline.md
Previous bundle: `GECC-09-payments-and-payables.md
Status: Implementation bundle
Date: 2026-09-20
Next planned bundle: GECC-11-inventory-assets-and-loans.md
Primary database: Relational database
System of record: GECC database
Google Sheets: Export or reporting destination only
1. Sprint Objective
Implement project-cost tracking, gross-profit calculations, net-profit calculations, fixed compensation, variable commissions, commission eligibility, commission approvals, biweekly contractor reports, annual contractor summaries, and commission adjustments.

Sprint 10 must ensure that:

Project profitability is calculated from controlled cost records.
Approved costs and corrected costs remain historically traceable.
Commissions are not earned before project completion.
Commissions are not paid on zero-profit or negative-profit projects.
Fixed compensation is calculated according to approved project rules.
Variable commissions use the approved Gross Profit and Net Profit formulas.
Commission reports are generated only for eligible completed projects.
Contractor reports contain only information authorized for the applicable contractor.
Post-payroll corrections are handled through adjustments rather than silent overwrites.
2. Sprint Scope
2.1 Included
Project-cost records
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
Biweekly commission periods
Commission report generation
Commission report approval
Contractor report distribution
Annual commission summaries
Commission corrections
Post-payroll adjustments
Cost and commission permissions
Cost and commission audit logging
Profitability dashboard indicators
Project and customer profitability outputs
2.2 Excluded
Inventory valuation and FIFO implementation
Fixed-asset depreciation
Loan accounting
Full payroll processing
Contractor tax calculations
Bank-statement import
Automatic payment processing
Final financial-statement publication
Vendor payment processing, except integration with Sprint 09
General ledger account configuration, except required integration events
3. Source Records and Dependencies
Sprint 10 depends on:

Approved Master Sales Record versions
Approved retail price
Approved project scope
Equipment records
Material records
Dealer-fee records
Technician cost records
Lead, Damon, and Jerry cost records
Verified customer payments
Approved credits, refunds, and adjustments
Project completion status
Contractor records
User and role permissions
Tamper-evident audit logging
Approved electronic-signature status
Sprint 09 accounts-receivable and payment status
A commission must not be calculated as earned from an unapproved or superseded project version.

4. Approved Cost Categories
The system must support the following initial project-cost categories:

Equipment Cost
Material Cost
Dealer Fee
Lead Cost
Technician Cost
Damon Cost
Jerry Cost
Each cost record must identify:

Cost ID
Project ID
Master Sales Record version
Cost category
Description
Amount
Vendor or contractor, if applicable
Source record
Cost status
Entered by
Entry date and time
Approved by, if approval is required
Approval date and time
Effective date
Revision reference
Correction or adjustment reference
Notes
Amounts must be stored using fixed-precision numeric fields. Floating-point values must not be used for financial calculations.

5. Cost Permissions
5.1 Equipment, Material, and Dealer-Fee Costs
The following roles may enter or revise permitted cost records:

Sales Manager
Installation Manager
Comptroller
5.2 Technician Cost
Technician Cost is a variable actual project cost.

It may be entered or revised by:

Sales Manager
Installation Manager
Comptroller
The system must retain the user, date, reason, and source for each Technician Cost change.

5.3 Lead, Damon, and Jerry Costs
Lead Cost, Damon Cost, and Jerry Cost may be edited for an individual project by authorized users.

The initial default values are:

Lead: $3,000
Damon: $500
Jerry: $250
Changing a cost for one project must not automatically change:

Past projects
Future projects
Other projects
The global default
Any future change to a global default requires separate administrative configuration and must not rewrite historical project costs.

5.4 Sales Associate
A Sales Associate may view limited profitability or commission information only when authorized by the applicable permission policy.

A Sales Associate may view:

Commission information related to their own completed sales
Their own approved commission reports
A Sales Associate may not:

Change approved costs
Change commission calculations
Approve commission changes
Approve commission reports
View another Sales Associate’s commission information
5.5 Commission Approvers
The Sales Manager and Comptroller may:

Review commission calculations
Approve commission reports
Review commission corrections
Approve commission adjustments according to the approved workflow
Only the Sales Manager may approve:

Early commission payment
Commission changes
Commission changes caused by corrected project costs
Commission adjustments after payroll approval
6. Profitability Calculations
6.1 Gross Profit
The system must calculate:

text


Gross Profit =
Invoice Amount
- Equipment Cost
- Material Cost
- Dealer Fee
- Lead Cost
- Technician Cost
- Damon Cost
- Jerry Cost
The invoice amount must be the applicable approved invoice amount after authorized pricing changes and before subtracting project costs.

Approved credits, refunds, and adjustments must be handled according to the accounting and profitability treatment approved for the relevant project version. The implementation must not silently substitute a different amount.

6.2 Variable Commissions
Ryan’s commission:

text


Ryan Commission = 15%
Doug’s commission:

text


Doug Commission = 5% × Gross Profit
The system must store the commission rate used for each calculation so that a future rate change does not alter historical calculations.

If Gross Profit is zero or negative:

No variable commission is paid.
Negative profitability remains visible.
The zero or negative calculation is retained in the commission record.
6.3 Net Profit
The system must calculate:

text


Net Profit =
Gross Profit
-​
If Net Profit is zero or negative:

No variable commission is paid.
Negative profitability remains visible.
The negative result is preserved in reports.
6.4 Profit Split
Net Profit is split:

50% to the Sales Associate who created and owns the Master Sales Record
50% to GECC
The system must identify the Sales Associate from the project ownership record, not from a manually entered report value.

If the Sales Associate assignment changes after approval, the change must follow the controlled revision process and must not silently change an already approved commission report.

6.5 Fixed Compensation
Fixed compensation is paid once per completed project to:

Lead
Damon
Jerry
The initial fixed amounts are:

Lead: $3,000
Damon: $500
Jerry: $250
These amounts are project-level deductions and compensation values. They are not dependent on whether Damon or Jerry actively participated in the project, unless a later approved decision supersedes the baseline.

Fixed compensation becomes earned only after project completion.

7. Commission Eligibility
A project is commission-eligible only when all of the following conditions are true:

Required scope of work is complete.
Contract has all required signatures.
Invoice has all required signatures.
Certificate has all required signatures.
Invoice is fully paid after approved adjustments.
Payment verification is complete.
Project status is COMPLETE.
No unresolved payment reversal, refund, or completion-blocking exception exists.
A project that is merely invoiced, signed, installed, or partially paid is not commission-eligible.

7.1 Commission Eligibility Event
When the final eligibility condition is met, the system must:

Create a commission-eligibility record.
Record the eligibility date and time.
Identify the applicable project and Master Sales Record version.
Capture the approved financial inputs.
Calculate fixed compensation.
Calculate variable commissions.
Calculate Net Profit.
Assign the project to the applicable biweekly commission period.
Create a commission-report task.
Record the event in the audit log.
7.2 Eligibility Reversal
If a project later becomes financially incomplete because of:

Payment reversal
Refund
Approved adjustment
Corrected cost
Reopened project
Invalid completion status
the system must not delete the original eligibility record.

Instead, it must create:

Eligibility-reversal record
Reason
Date and time
Approver, when required
Related payment, cost, or adjustment record
Required commission correction task
8. Commission Data Model
8.1 Commission Record
Required fields:

Commission ID
Project ID
Customer ID
Project identification code
Master Sales Record version
Sales Associate ID
Contractor ID
Commission type
Commission rate
Commission basis
Gross Profit
Net Profit
Fixed compensation amount
Variable commission amount
Sales Associate share
GECC share
Eligibility status
Approval status
Payroll status
Report ID
Pay-period ID
Created date and time
Approved date and time
Paid date, if applicable
Correction reference
Adjustment reference
Notes
8.2 Commission Types
Initial permitted values:

FIXED_LEAD
FIXED_DAMON
FIXED_JERRY
VARIABLE_RYAN
VARIABLE_DOUG
SALES_ASSOCIATE_PROFIT_SHARE
GECC_PROFIT_SHARE
COMMISSION_ADJUSTMENT
8.3 Commission Statuses
Permitted statuses:

NOT_ELIGIBLE
ELIGIBLE
`CALCULATED
PENDING_APPROVAL
APPROVED
`REPORTED
PAID
CORRECTION_PENDING
ADJUSTED
VOIDED
A commission record must not be marked PAID unless the applicable commission report has been approved.

9. Commission Calculation Rules
9.1 Calculation Snapshot
When commissions are calculated, the system must store a calculation snapshot containing:

Invoice amount used
Approved cost values used
Gross Profit
Ryan rate and amount
Doug rate and amount
Net Profit
Fixed compensation amounts
Sales Associate identity
Profit-share amounts
Master Sales Record version
Calculation date and time
Calculation engine or rule-set version
Future configuration changes must not recalculate historical reports automatically.

9.2 Rounding
The system must use a consistent financial rounding rule.

Recommended default:

Calculate using full stored precision.
Round display and payable amounts to two decimal places.
Apply final rounding at the individual commission-line level.
Ensure the report total equals the sum of its displayed line items.
The selected rule must be documented in the Sprint 10 configuration record.

9.3 Negative Profitability
Negative Gross Profit and Net Profit must be preserved.

The system must:

Display negative values to authorized users.
Flag negative-profit projects.
Suppress variable commissions when Gross Profit is zero or negative.
Suppress variable commissions when Net Profit is zero or negative.
Preserve fixed compensation according to the baseline unless a later approved decision changes that rule.
9.4 Missing Costs
A project must not receive a final commission calculation if required cost information is missing.

The system must create a cost-completeness exception identifying:

Missing cost category
Project
Responsible role
Date detected
Required action
A Sales Manager, Installation Manager, or Comptroller must resolve the missing cost before final commission approval.

10. Biweekly Commission Reports
10.1 Reporting Period
Contractor commission reports are created every two weeks.

Each report must identify:

Report ID
Contractor
Pay-period start date
Pay-period end date
Report date
Included projects
Included commission records
Total fixed compensation
Total variable compensation
Total adjustments
Net amount reported
Review status
Approval status
Distribution status
Retention reference
10.2 Project Inclusion
A completed project is included in the applicable report when its commission eligibility event falls within the pay period.

A project must not be included solely because:

It was sold during the period.
It was installed during the period.
Its invoice was issued during the period.
Its contract was signed during the period.
10.3 Report Review and Approval
The Sales Manager and Comptroller review commission reports according to the approved workflow.

The implementation must preserve whether:

Sales Manager reviewed
Comptroller reviewed
Sales Manager approved
Comptroller approved
Report was rejected
Rejection reason was provided
The baseline requires Sales Manager and Comptroller review and approval. Both approval events must be retained even if the workflow later permits one authorized final approval for a specific report type.

10.4 Report Distribution
Approved reports must be:

Sent by email to applicable contractors.
Permanently retained.
Linked to the relevant projects.
Linked to the relevant commission records.
Linked to the applicable pay period.
Email delivery status must be retained.

11. Annual Contractor Summaries
Every January 5, the system must create an annual summary for each contractor who was employed during the prior year.

Each summary must include:

Contractor identity
Summary year
Total commission amount
Commission payment dates
Commission amounts
Individual biweekly report references
Distribution status
Delivery date and time
Retention reference
Annual summaries must not expose another contractor’s information.

If January 5 falls on a non-business day, the implementation must use the configured delivery policy while retaining January 5 as the reporting trigger date.

12. Commission Adjustments
12.1 Adjustment Triggers
A commission adjustment may be required because of:

Corrected project cost
Payment reversal
Approved refund
Approved invoice adjustment
Incorrect commission calculation
Incorrect contractor assignment
Incorrect project ownership
Commission report correction
Post-approval accounting correction
12.2 Required Adjustment Fields
Adjustment ID
Original commission ID
Project ID
Contractor ID
Original report ID
Original pay period
Adjustment amount
Adjustment type
Reason
Source correction
Requested by
Requested date and time
Approval status
Approved by
Approval date and time
Replacement report or future-period reference
Notes
12.3 Post-Payroll Corrections
If a project cost is corrected after payroll approval:

Preserve the original commission report.
Create a commission adjustment.
Include the adjustment in the next payroll period.
Add an adjustment note.
Link the adjustment to the original project.
Link the adjustment to the original payroll period.
Require Sales Manager approval.
Preserve the original calculation snapshot.
The original approved report must never be silently rewritten.

12.4 Early Commission Payment
Early commission payment requires:

Sales Manager approval
Reason
Project reference
Amount
Date
Audit record
Early payment must not remove the project from the normal commission-report history.

13. Reporting Requirements
13.1 Project Profitability Report
The report must include:

Project identification code
Customer
Project location
Sales Associate
Invoice amount
Each cost category
Gross Profit
Ryan commission
Doug commission
Net Profit
Sales Associate share
GECC share
Commission status
Payment status
Project completion status
13.2 Customer Profitability Report
The report must aggregate authorized project records by customer and include:

Customer
Project count
Total invoice amount
Total costs
Total Gross Profit
Total Net Profit
Total commissions
Total adjustments
Total verified payments
13.3 Contractor Commission Report
Each contractor must receive only authorized information.

The report must include:

Contractor identity
Pay period
Eligible projects
Commission type
Commission amount
Approved adjustments
Total amount reported
Approval status
13.4 Negative-Profit Report
The report must identify:

Project
Customer
Sales Associate
Invoice amount
Cost categories
Gross Profit
Net Profit
Variable commission status
Required action
13.5 Commission Adjustment Report
The report must identify:

Original commission report
Original project
Original amount
Corrected amount
Adjustment amount
Reason
Approver
New reporting period
Adjustment status
14. Dashboard Requirements
Sprint 10 must add or populate the following dashboard indicators:

Projects missing required costs
Projects awaiting profitability calculation
Projects with zero Gross Profit
Projects with negative Gross Profit
Projects with zero or negative Net Profit
Projects eligible for commission
Projects awaiting commission approval
Commission reports awaiting review
Commission reports awaiting approval
Commission reports rejected
Commission adjustments pending approval
Post-payroll corrections
Annual summaries awaiting delivery
Failed commission-report emails
Financial amounts and contractor compensation must remain hidden from unauthorized users.

15. Audit Requirements
The system must log:

Cost creation
Cost edits
Cost approvals
Cost reversals
Default-cost changes
Profitability calculations
Commission-calculation events
Commission-rate changes
Eligibility creation
Eligibility reversal
Commission report creation
Commission report review
Commission report approval
Commission report rejection
Commission report distribution
Email delivery failure
Annual-summary creation
Annual-summary distribution
Commission adjustments
Early commission approvals
Unauthorized access attempts
Commission exports, downloads, and prints
Every event must retain the standard audit fields:

User identity
User role
Date and time
Activity type
Related record identifier
Success or failure
Session identifier
Reason when required
Previous audit-log entry reference
Integrity hash or equivalent value
16. Sprint 10 Decisions
Decision 10-01 — Cost categories
The initial implementation uses seven project-cost categories:

Equipment
Materials
Dealer Fee
Lead
Technician
Damon
Jerry
Decision 10-02 — Default fixed deductions
Initial project defaults are:

Lead: $3,000
Damon: $500
Jerry: $250
Defaults are copied into the project cost record when the project reaches the applicable workflow stage. Later default changes do not alter existing projects.

Decision 10-03 — Fixed compensation eligibility
Lead, Damon, and Jerry fixed compensation becomes earned only after project completion.

Decision 10-04 — Variable commission rates
Initial variable commission rates are:

Ryan: 15% of Gross Profit
Doug: 5% of Gross Profit
The rate used must be stored in each commission calculation snapshot.

Decision 10-05 — Zero and negative profitability
No variable commission is paid when:

Gross Profit is zero or negative, or
Net Profit is zero or negative
Negative profitability remains visible in reports.

Decision 10-06 — Net-profit split
Net Profit is split:

50% to the Sales Associate who created and owns the Master Sales Record
50% to GECC
Decision 10-07 — Commission eligibility
Commissions are earned only after:

Scope completion
Full Certificate signature
Full invoice signature
Full payment after approved adjustments
Verified payment
Project completion
Decision 10-08 — Commission report frequency
Contractor commission reports are created every two weeks.

Decision 10-09 — Report retention
Approved commission reports are retained permanently and linked to:

Contractor
Projects
Commission records
Pay period
Distribution events
Decision 10-10 — Annual summary date
Annual contractor summaries are generated every January 5 for contractors employed during the prior year.

Decision 10-11 — Post-payroll corrections
Cost corrections after payroll approval are handled through an adjustment in the next payroll period.

The original report remains unchanged.

Decision 10-12 — Commission approval authority
Only the Sales Manager may approve:

Early commission payment
Commission changes
Cost-correction commission changes
Commission adjustments after payroll approval
Decision 10-13 — Contractor data isolation
Each contractor receives only their own authorized commission information.

Sales Associates see only commission information related to their own sales.

Decision 10-14 — Historical calculation preservation
Historical commission calculations retain their original:

Inputs
Rates
Costs
Profitability values
Ownership
Project version
Calculation rule version
Decision 10-15 — Financial calculation precision
Financial calculations use fixed-precision numeric storage and a single documented rounding policy.

17. Open Questions
Open Question 10-01 — Definition of project invoice amount
Should Gross Profit use:

Original invoice amount, or
Adjusted invoice amount after approved credits, refunds, and adjustments?
Recommended default: Use the approved adjusted economic amount for the applicable completed project version, while retaining the original invoice amount separately. The final accounting treatment must be confirmed before production release.

Open Question 10-02 — Fixed compensation and negative profitability
Should Lead, Damon, and Jerry fixed compensation be paid when a project has zero or negative profitability?

Recommended default: Yes, because the baseline defines fixed compensation separately from variable commissions. This must be explicitly confirmed before payroll implementation.

Open Question 10-03 — Ryan and Doug identities
Are Ryan and Doug fixed contractor identities, or configurable contractor roles that may be reassigned?

Recommended default: Store them as configurable contractor records with a stable commission-role assignment. Historical records must retain the contractor identity used at calculation time.

Open Question 10-04 — Sales Associate reassignment
Who owns commissions when a project is reassigned before completion?

Recommended default: The current approved Master Sales Record owner at the time of commission eligibility receives the Sales Associate allocation. Any reassignment after approval requires a documented revision and approval.

Open Question 10-05 — Commission report approval threshold
Must both the Sales Manager and Comptroller approve every commission report, or may one authorized approver finalize a report?

Recommended default: Require review and approval by both roles because the baseline states that both review and approve commission reports.

Open Question 10-06 — Pay-period boundaries
Should commission periods be based on:

Fixed calendar periods, such as days 1–14 and 15–end of month
Rolling fourteen-day periods
A configurable biweekly calendar
Recommended default: Use configurable fixed fourteen-day periods with a defined first period and permanent period history.

Open Question 10-07 — Rounding method
Should rounding occur:

Per commission line
Per project
Per contractor report
At all three levels with reconciliation
Recommended default: Calculate at full precision, round individual payable lines to two decimals, and reconcile report totals to displayed lines.

Open Question 10-08 — Cost completeness
Which cost categories are mandatory before commission calculation?

Recommended default: All seven baseline cost categories must have a value, including zero where legitimately applicable. Missing values must not be treated as zero without an authorized explanation.

Open Question 10-09 — Commission treatment after refunds
If a refund occurs after commission payment, should the full resulting adjustment be charged to the next contractor report?

Recommended default: Calculate the correction from the affected project and apply it to the next report, subject to Sales Manager approval and applicable legal/accounting review.

Open Question 10-10 — Contractor report delivery failure
What is the required response when an approved report cannot be emailed?

Recommended default: Preserve the approved report, create a delivery exception, notify the Sales Manager and Comptroller, and permit authorized manual delivery.

Open Question 10-11 — Annual report delivery date
If January 5 is a weekend or holiday, should the report be sent:

On January 5
On the preceding business day
On the next business day
Recommended default: Create the annual report on January 5 and deliver on the next configured business day if the delivery service is unavailable.

Open Question 10-12 — Profitability treatment of payment adjustments
Should profitability be recalculated when a payment is refunded or an invoice is adjusted after project completion?

Recommended default: Preserve the original completed-project calculation and create a separate adjustment calculation linked to the original report.

18. Acceptance Criteria
Sprint 10 is accepted when:

All seven approved cost categories can be recorded.
Cost records are linked to the correct project and Master Sales Record version.
Unauthorized users cannot edit approved costs.
Default Lead, Damon, and Jerry costs are applied correctly.
A project-specific cost change does not alter other projects.
Gross Profit is calculated using the approved formula.
Ryan’s commission is calculated at 15% of eligible Gross Profit.
Doug’s commission is calculated at 5% of eligible Gross Profit.
Variable commissions are zero when Gross Profit is zero or negative.
Variable commissions are zero when Net Profit is zero or negative.
Negative Gross Profit and Net Profit remain visible in authorized reports.
Net Profit is split 50% to the Sales Associate and 50% to GECC.
Fixed compensation is calculated once per completed project.
Fixed compensation is not calculated before project completion.
A project cannot become commission-eligible before all required signatures and verified payment conditions are complete.
A project cannot be included in a commission report solely because it was sold or installed.
Each commission calculation stores its calculation inputs and rates.
Historical calculations do not change when current rates or defaults change.
Missing required costs create a blocking exception.
Biweekly commission reports include only eligible projects in the applicable pay period.
Sales Managers and Comptrollers can review commission reports.
Commission reports require the configured approval workflow.
Approved commission reports are permanently retained.
Reports are linked to the applicable projects, contractors, and commission records.
Contractors receive only their authorized commission information.
Sales Associates see only commission information related to their own sales.
Annual summaries include the required prior-year commission information.
Post-payroll cost corrections create adjustments in the next payroll period.
Original commission reports remain unchanged after corrections.
Only the Sales Manager can approve post-payroll commission adjustments.
Negative-profit projects appear on the appropriate dashboard or exception report.
All cost and commission actions are recorded in the tamper-evident audit log.
Project and customer profitability reports reconcile to underlying records.
Automated Sprint 10 tests pass.
19. Automated Test Scenarios
Cost Tests
Create all seven cost categories for a project.
Confirm unauthorized users cannot edit approved costs.
Change a cost for one project and confirm other projects are unchanged.
Change a default cost and confirm historical projects are unchanged.
Attempt to calculate commission with a missing required cost.
Confirm the missing-cost exception identifies the responsible role.
Profitability Tests
Calculate Gross Profit using positive costs.
Calculate Gross Profit when one cost is zero.
Calculate Gross Profit when total costs exceed the invoice amount.
Confirm negative Gross Profit is retained.
Calculate Ryan’s commission at 15%.
Calculate Doug’s commission at 5%.
Confirm no variable commission is paid for zero Gross Profit.
Confirm no variable commission is paid for negative Gross Profit.
Confirm Net Profit calculation.
Confirm the 50/50 Sales Associate and GECC split.
Eligibility Tests
Attempt to create a commission before contract signature.
Attempt to create a commission before Certificate signature.
Attempt to create a commission before invoice signature.
Attempt to create a commission before payment verification.
Attempt to create a commission for a partially paid project.
Complete all requirements and confirm commission eligibility.
Reverse a payment after eligibility and confirm an adjustment task is created.
Report Tests
Create a biweekly period.
Include an eligible project in the correct period.
Exclude an ineligible project.
Confirm a project is not duplicated in two reports.
Generate contractor-specific reports.
Confirm one contractor cannot view another contractor’s report.
Approve a report and confirm permanent retention.
Reject a report and require a rejection reason.
Generate an annual contractor summary.
Adjustment Tests
Correct a project cost before payroll approval.
Correct a project cost after payroll approval.
Confirm the original commission report remains unchanged.
Confirm an adjustment appears in the next payroll period.
Require Sales Manager approval for the adjustment.
Confirm the adjustment links to the original project and payroll period.
Security and Audit Tests
Confirm Sales Associates cannot view other Sales Associates’ commissions.
Confirm Technicians cannot view commission reports.
Confirm unauthorized users cannot export financial data.
Confirm rate changes are audited.
Confirm commission calculation events are audited.
Verify the audit chain after commission corrections.
Confirm financial records cannot be silently deleted.
20. Sprint 10 Completion Definition
Sprint 10 is complete when:

Cost records are controlled and versioned.
Project profitability calculations are reproducible.
Fixed and variable commissions use approved rules.
Commission eligibility is tied to completed projects and verified payment.
Biweekly reports are generated, reviewed, approved, distributed, and retained.
Annual contractor summaries are generated.
Post-payroll corrections use formal adjustments.
Role-based commission visibility is enforced.
Dashboard exceptions are operational.
Audit records are complete and tamper-evident.
Acceptance tests pass.
Open questions are resolved or formally carried forward.
The Sprint 10 bundle is approved and preserved unchanged.
21. Next Markdown Bundle
The next bundle is:

text


GECC-11-inventory-assets-and-loans.md
Its objective is to implement:

FIFO inventory
Serial-number tracking
Equipment acquisition
Equipment allocation to projects
Inventory cost layers
Inventory status transitions
Inventory adjustments
Vehicles
Computers
Offices
Additional fixed-asset categories
Straight-line depreciation
Placed-in-service dates
Accumulated depreciation
Net book value
Asset disposal
Loans
Principal balances
Interest tracking
Loan payments
Inventory, fixed-asset, depreciation, and loan reports
Asset and inventory permissions
Audit integration
Accounting integration with the internal ledger
Sprint 11 must preserve the cost and commission calculation snapshots created in Sprint 10 and must not silently recalculate historical profitability when inventory or asset values are later corrected.



