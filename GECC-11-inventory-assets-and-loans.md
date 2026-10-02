GECC-11-inventory-assets-and-loans.md
GECC Sales Back Office System — Sprint 11
Bundle Metadata
Bundle name: `GECC-11-inventory-assets-and-loans.md
Project: GECC Sales Back Office System
Company: Go Ecco Climate Control
Sprint: 11
Sprint name: Inventory, Assets, Loans, and Depreciation
Baseline: GECC-00-project-baseline.md
Previous bundle: GECC-10-costs-and-commissions.md
Status: Implementation bundle
Date: 2026-09-20
Next planned bundle: GECC-12-financial-reporting.md
Primary database: Relational database
System of record: GECC database
Google Sheets: Export or reporting destination only
1. Sprint Objective
Implement inventory tracking, serial-number tracking, FIFO cost layers, fixed-asset records, straight-line depreciation, loans, and related accounting integration.

Sprint 11 must ensure that:

Inventory is tracked by product identity and cost layer.
Serialized equipment remains individually identifiable.
Inventory issued to projects is traceable to the project and source cost layer.
Equipment costs used in profitability calculations are reproducible.
Fixed assets are recorded by category and identification details.
Depreciation uses the straight-line method.
Depreciation begins on the placed-in-service date.
Loans track principal, interest, payments, and outstanding balances.
Inventory, asset, depreciation, and loan records are auditable.
Historical project profitability is not silently recalculated after later corrections.
2. Sprint Scope
2.1 Included
Inventory master records
Product identity records
Serialized equipment
Nonserialized materials
Inventory receipts
Inventory cost layers
FIFO issuance
Inventory reservations
Project allocation
Inventory returns
Inventory adjustments
Fixed-asset records
Vehicles
Computers
Offices
Additional asset categories
Asset acquisition
Asset placement in service
Straight-line depreciation
Accumulated depreciation
Net book value
Asset disposal
Loan records
Loan payment records
Principal and interest tracking
Inventory reports
Fixed-asset reports
Depreciation reports
Loan reports
Permissions
Audit logging
Accounting integration events
2.2 Excluded
Automatic vendor purchase-order integration
Automatic bank-statement import
Automatic bank reconciliation
Automatic loan-servicer integration
Accelerated depreciation
Declining-balance depreciation
Section 179 or tax depreciation calculations
Tax-return preparation
Asset maintenance scheduling
Fleet GPS tracking
Technician time tracking
Full procurement management
3. Source Records and Dependencies
Sprint 11 depends on:

Sprint 01 relational database and accounting foundation
Sprint 03 tamper-evident audit logging
Sprint 04 customer, project, and location records
Sprint 05 Master Sales Record and equipment records
Sprint 06 controlled workflow
Sprint 09 payment and payable records
Sprint 10 project-cost and profitability records
Sprint 11 must integrate with:

Equipment records
Project records
Vendor records
Accounts payable
Project-cost records
General-ledger integration events
Financial reporting periods
Audit records
4. Inventory Data Model
4.1 Product Identity
A product identity represents a product type or model.

Required fields:

Product identity ID
Brand
Model
Function
Product type
Description
Unit of measure
Serialized indicator
Active or inactive status
Default vendor, if applicable
Creation date
Last modified date
Examples of product types include:

Condenser coil
Evaporator coil
Refrigerant
Air handler
Furnace
Thermostat
Air ducts
Vents
4.2 Inventory Item
An inventory item represents an individually received or tracked unit.

Required fields:

Inventory item ID
Product identity ID
Serial number, when applicable
Brand
Model
Function
Product type
Acquisition date
Acquisition cost
Vendor
Source payable
Cost-layer ID
Inventory status
Project allocation
Date received
Date issued
Date returned, if applicable
Notes
Created by
Created date and time
Last modified date and time
4.3 Serialized Inventory
Serialized products must retain individual records.

The system must prevent:

Duplicate serial numbers for the same product identity
Issuing the same serial-numbered item to two projects
Changing a serial number without a controlled correction
Deleting a serialized item with historical activity
Reusing a retired or disposed serial number
A serial-number correction must preserve:

Original serial number
Corrected serial number
Reason
User
Date and time
Approval, when required
Related project and document references
4.4 Nonserialized Inventory
Nonserialized materials may be tracked by:

Product identity
Quantity
Unit of measure
Acquisition cost
Cost layer
Vendor
Project allocation
The system must retain transaction-level quantity changes and must not rely solely on a manually edited balance.

5. Inventory Statuses
Permitted inventory statuses:

ORDERED
RECEIVED
AVAILABLE
RESERVED
ISSUED
INSTALLED
`RETURNED
`DAMAGED
SCRAPPED
ADJUSTED
`TRANSFERRED
CANCELLED
5.1 Status Rules
ORDERED items are not available for project allocation.
RECEIVED items have arrived but may require receiving review.
AVAILABLE items may be reserved or issued.
RESERVED items are assigned to a project but have not been issued.
ISSUED items are allocated to a project and included in project cost.
INSTALLED items are confirmed as used in the completed scope.
RETURNED items are removed from project allocation and require condition assessment.
DAMAGED items may not be issued without authorized override.
SCRAPPED items may not be returned to available inventory.
CANCELLED items are retained historically but excluded from available quantities.
6. Inventory Receiving
When inventory is received, the authorized user must record:

Vendor
Receipt date
Product identity
Quantity
Unit cost
Total cost
Serial numbers, when applicable
Related payable or vendor document
Receiving notes
The system must:

Validate the vendor.
Validate product identity.
Validate quantity and cost.
Create or update the applicable inventory records.
Create a FIFO cost layer.
Link the inventory receipt to the payable, when available.
Record the receiving event in the audit log.
Inventory may be received before the related payable is paid.

7. FIFO Cost Layers
7.1 Cost-Layer Requirements
Each inventory receipt creates a cost layer containing:

Cost-layer ID
Product identity ID
Receipt date
Quantity received
Quantity remaining
Unit cost
Total layer cost
Vendor
Source payable
Source receipt
Status
7.2 FIFO Issuance
When inventory is issued, the system must consume the oldest available cost layer first for the applicable product identity.

The system must:

Identify available cost layers.
Order layers by receipt date and cost-layer sequence.
Consume the oldest layer first.
Split the issuance across layers when necessary.
Record the quantity and cost consumed from each layer.
Calculate the total issued cost.
Link the issued cost to the project and Master Sales Record version.
Preserve the cost-layer history.
7.3 Serial-Numbered Equipment
For serial-numbered equipment, the individual serial-number record controls identity and allocation.

FIFO cost-layer tracking must still retain the acquisition cost and receipt sequence, but the system must not substitute one serial-numbered unit for another without a controlled change.

7.4 Cost Corrections
A cost-layer correction must not overwrite the original layer.

The system must create:

Original layer reference
Corrected layer reference
Correction reason
Correcting user
Date and time
Approval status
Affected project references
Accounting adjustment reference
If the affected inventory was already issued to a project, the correction must create a project-cost adjustment rather than silently changing historical project profitability.

8. Project Allocation and Issuance
Inventory may be allocated to a project only when:

The project exists.
The user is authorized.
The product is available or reserved.
The quantity is sufficient.
The project has an approved or otherwise authorized scope requiring the item.
The item is not already allocated to another project.
The allocation record must include:

Allocation ID
Project ID
Master Sales Record version
Product identity
Inventory item or cost layer
Quantity
Unit cost
Total cost
Allocation status
Reserved date
Issued date
Installed date
Returned date, if applicable
User performing each action
Notes
Inventory issued to a project must flow to the project’s Material Cost or Equipment Cost according to the item classification.

The system must preserve the distinction between:

Estimated cost
Reserved cost
Issued actual cost
Installed actual cost
Returned cost
Corrected cost
9. Inventory Returns and Adjustments
9.1 Returns
A return must identify:

Project
Product or serial number
Original issue transaction
Return date
Quantity
Condition
Return reason
User
Destination status
Returned inventory may become:

Available
Damaged
Scrapped
Pending inspection
The system must not automatically return an item to available inventory without a condition decision.

9.2 Adjustments
Inventory adjustments require:

Adjustment reason
Quantity or cost difference
Supporting note
Authorized user
Date and time
Approval where required
Related product, serial number, cost layer, or project
Adjustments must be recorded as transactions and must not overwrite the original inventory history.

10. Fixed-Asset Data Model
10.1 Common Fixed-Asset Fields
Each fixed asset must include:

Asset ID
Asset category
Identifying code
Description
Acquisition date
Acquisition cost
Vendor
Source payable
Placed-in-service date
Useful life
Depreciation method
Accumulated depreciation
Net book value
Disposal status
Disposal date
Disposal proceeds
Disposal reason
Location
Responsible user
Created by
Created date and time
Last modified date and time
Asset status
10.2 Vehicle
Required vehicle fields:

VIN
Make
Model
Year
Description
Acquisition date
Acquisition cost
Vendor
Placed-in-service date
Useful life
Accumulated depreciation
Net book value
Location
Disposal information
10.3 Computer
Required computer fields:

Asset serial number
Brand
Model
Description
Acquisition date
Acquisition cost
Vendor
Placed-in-service date
Useful life
Accumulated depreciation
Net book value
Assigned user or location
Disposal information
10.4 Office
Required office fields:

Address
Description
Acquisition date
Acquisition cost
Vendor
Placed-in-service date
Useful life
Accumulated depreciation
Net book value
Disposal information
10.5 Additional Categories
Authorized users may create additional categories containing:

Category name
Identifying code
Description
Acquisition date
Acquisition cost
Placed-in-service date
Useful life
Depreciation data
Creation of a new asset category must be audited.

11. Fixed-Asset Lifecycle
Permitted asset statuses:

PLANNED
ACQUIRED
IN_SERVICE
SUSPENDED
FULLY_DEPRECIATED
DISPOSED
CANCELLED
11.1 Acquisition
An asset is acquired when the asset record is created and linked to the appropriate vendor, payable, invoice, or source document.

11.2 Placed in Service
Depreciation begins on the placed-in-service date.

The system must not calculate depreciation before that date.

The placed-in-service date must be validated as:

On or after acquisition date
A valid calendar date
Recorded by an authorized user
Immutable after depreciation begins except through a formal adjustment
11.3 Depreciation
The only supported depreciation method in this sprint is straight-line depreciation.

The basic calculation is:

text


Depreciable Basis =
Acquisition Cost
- Salvage Value
text


Periodic Depreciation =
Depreciable Basis
÷ Useful Life
The exact periodic convention—monthly, daily, or another approved convention—must be configured before production use.

Accumulated depreciation may not exceed depreciable basis unless a formal correction is approved.

11.4 Net Book Value
text


Net Book Value =
Acquisition Cost
- Accumulated Depreciation
- Approved Impairments or Adjustments
The system must preserve historical depreciation entries and must not overwrite prior-period depreciation.

11.5 Disposal
Disposal requires:

Disposal date
Disposal reason
Disposal status
Proceeds, if any
Authorized user
Approval, if required
Final accumulated depreciation
Final net book value
Disposal accounting event
Audit entry
A disposed asset cannot return to IN_SERVICE without a formal reinstatement process.

12. Loan Data Model
Required loan fields:

Loan ID
Lender
Loan number or reference
Loan amount
Loan date
Term
Interest rate
Payment frequency
Payment schedule
Principal balance
Interest paid
Interest balance, if applicable
Origination fees, if applicable
Maturity date
Loan status
Collateral or related asset, if applicable
Notes
Created by
Creation date and time
12.1 Loan Statuses
Permitted statuses:

PLANNED
ACTIVE
PAID_OFF
DELINQUENT
MODIFIED
DEFAULTED
CANCELLED
12.2 Loan Payments
Each loan payment must include:

Loan payment ID
Loan ID
Payment date
Payment amount
Principal portion
Interest portion
Fees
Payment method
Reference number
User recording payment
Supporting document
Notes
The system must preserve the original payment amount and the allocation between principal, interest, and fees.

12.3 Loan Balance
The system must calculate:

text


Principal Balance =
Original Principal
- Principal Paid
± Approved Principal Adjustments
Interest paid must be tracked separately from principal.

A loan may not be marked PAID_OFF until the principal balance and other required amounts are resolved.

13. Permissions
13.1 Sales Manager and Comptroller
May:

View all inventory, asset, depreciation, and loan records.
Approve inventory adjustments.
Approve asset corrections.
Enter or approve loan records.
Review accounting effects.
Resolve exceptions.
View audit history.
13.2 Installation Manager
May:

View authorized inventory.
Reserve or issue installation-related equipment and materials.
Record project allocation.
Record actual materials used.
Propose inventory corrections.
View installation-related project costs.
May not:

Modify unrelated fixed assets.
Change depreciation rules.
Modify loan records unless separately authorized.
13.3 Accounts Payable Associate
May:

View authorized inventory receipts and source payables.
Link inventory receipts to payable records.
View payable-related asset acquisition information.
May not:

Change inventory quantities without authorization.
Change acquisition costs.
Modify depreciation.
Modify loans.
13.4 Database Administrator
May:

Maintain authorized reference data.
Maintain product identities and asset categories.
Correct data through controlled administrative procedures.
View records permitted by the database-administration role.
May not automatically approve financial adjustments unless separately assigned that authority.

13.5 Sales Associates, Technicians, and Other Contractors
May view only inventory or equipment information required for their assigned work.

They may not access:

Unauthorized acquisition costs
Depreciation values
Loan records
Asset financial reports
Inventory valuation reports
14. Accounting Integration
Sprint 11 must create ledger-compatible events for:

Inventory receipt
Inventory issue
Inventory return
Inventory adjustment
Fixed-asset acquisition
Fixed-asset placement in service
Depreciation
Asset disposal
Loan origination
Loan principal payment
Loan interest payment
Loan adjustment
Final chart-of-accounts mapping remains configurable for Sprint 12.

Inventory and fixed-asset corrections must create adjustment events rather than modifying historical ledger events.

15. Required Reports
15.1 Inventory Report
Must include:

Product identity
Brand
Model
Serial number, when applicable
Quantity
Available quantity
Reserved quantity
Issued quantity
Inventory status
Vendor
Cost layer
Unit cost
Total cost
Project allocation
15.2 FIFO Cost-Layer Report
Must include:

Product identity
Cost-layer ID
Receipt date
Original quantity
Quantity consumed
Quantity remaining
Unit cost
Total layer cost
Source vendor
Source payable
Allocated projects
15.3 Fixed-Asset Report
Must include:

Asset ID
Category
Identifying code
Description
Acquisition date
Acquisition cost
Placed-in-service date
Useful life
Accumulated depreciation
Net book value
Status
Location
Disposal information
15.4 Depreciation Report
Must include:

Asset
Period
Beginning net book value
Depreciation expense
Accumulated depreciation
Ending net book value
Depreciation method
Useful life
Placed-in-service date
Adjustment references
15.5 Loan Report
Must include:

Loan
Lender
Original amount
Loan date
Interest rate
Term
Payment schedule
Principal balance
Principal paid
Interest paid
Fees
Maturity date
Status
Related asset, if applicable
16. Dashboard and Exception Requirements
Sprint 11 must add or populate exceptions for:

Inventory below configured threshold
Inventory reserved but not issued
Inventory issued but not installed
Missing serial number
Duplicate serial number
Inventory with missing cost layer
Inventory allocated to multiple projects
Negative inventory quantity
Damaged inventory awaiting disposition
Returned inventory awaiting inspection
Asset missing placed-in-service date
Asset missing useful life
Asset with depreciation exceeding depreciable basis
Asset with negative net book value
Asset past expected useful life
Asset awaiting disposal
Loan payment overdue
Loan balance mismatch
Loan missing payment allocation
Loan approaching maturity
Missing source payable or acquisition document
17. Sprint 11 Decisions
Decision 11-01 — FIFO inventory
Inventory is valued and issued using First In, First Out by product identity and cost layer.

Decision 11-02 — Serialized equipment identity
Serial-numbered equipment remains individually identifiable throughout its lifecycle.

Serial numbers cannot be reused after retirement, disposal, or cancellation.

Decision 11-03 — Inventory history
Inventory changes are recorded as transactions.

The system must not rely on manually overwriting a single quantity or value field.

Decision 11-04 — Project cost integration
Issued inventory must flow into the applicable project cost category:

Equipment Cost for equipment
Material Cost for materials
The source inventory item, serial number, cost layer, and project must remain linked.

Decision 11-05 — Historical project profitability
A later inventory or acquisition-cost correction must not silently recalculate a historical project’s profitability.

The correction must create a formal project-cost adjustment linked to the original project and calculation snapshot.

Decision 11-06 — Depreciation method
All fixed assets use straight-line depreciation.

Accelerated and declining-balance depreciation are not supported in this sprint.

Decision 11-07 — Depreciation start date
Depreciation begins on the placed-in-service date, not the acquisition date.

Decision 11-08 — Depreciation preservation
Prior-period depreciation entries remain preserved.

Corrections create adjustment entries rather than rewriting historical depreciation.

Decision 11-09 — Asset categories
The initial fixed-asset categories are:

Vehicles
Computers
Offices
Additional categories may be created through controlled administration.

Decision 11-10 — Loan separation
Principal, interest, and fees are tracked separately for every loan payment.

Decision 11-11 — Manual loan entry
Loans and loan payments are manually recorded during the initial implementation.

Automatic lender or bank imports are deferred.

Decision 11-12 — Accounting integration
Inventory, asset, depreciation, and loan activities create ledger-compatible events, with final chart-of-accounts mapping completed during Sprint 12.

Decision 11-13 — Historical asset records
Fixed assets and loans cannot be silently deleted after financial activity exists.

Corrections require formal adjustment or cancellation records.

Decision 11-14 — Security
Inventory costs, asset values, depreciation, and loan balances are financial information and are permission-controlled.

18. Open Questions
Open Question 11-01 — Inventory valuation scope
Should FIFO be applied separately by:

Product identity
Brand and model
Product identity and location
Product identity, location, and condition
Recommended default: Product identity and cost layer, with location tracked separately.

Open Question 11-02 — Inventory locations
Will GECC have one inventory location or multiple locations?

Recommended default: Support multiple locations in the data model, even if only one location is configured initially.

Open Question 11-03 — Serialized material rules
Which product categories require serial-number tracking?

Recommended default: Make serialization configurable by product identity. Equipment requiring individual traceability must be serialized; consumable materials may remain quantity-based.

Open Question 11-04 — Inventory reservation expiration
How long may inventory remain reserved for a project before it is released?

Recommended default: No automatic release initially. Create an exception for stale reservations and require authorized review.

Open Question 11-05 — Depreciation frequency
Should depreciation be calculated monthly, daily, or by another convention?

Recommended default: Monthly depreciation with a documented month-end convention.

Open Question 11-06 — Partial-month depreciation
How should depreciation be handled when an asset is placed in service during a month?

Recommended default: Defer the final policy to Sprint 12 accounting configuration. The system must support the selected convention without rewriting prior entries.

Open Question 11-07 — Salvage value
Should fixed assets include salvage value?

Recommended default: Include an optional salvage-value field. Default to zero until an accounting policy is approved.

Open Question 11-08 — Asset impairment
Should impairment adjustments be supported?

Recommended default: Preserve an adjustment field and audit structure, but defer the impairment workflow until a later accounting decision.

Open Question 11-09 — Capitalization threshold
What minimum dollar amount requires a fixed-asset record rather than an expense record?

Recommended default: Make the threshold configurable and require the Comptroller to approve the initial value.

Open Question 11-10 — Useful lives
What useful lives apply to:

Vehicles
Computers
Offices
Additional asset categories
Recommended default: Store useful life per asset and require the Comptroller to approve category defaults before depreciation begins.

Open Question 11-11 — Loan amortization
Should the system calculate an amortization schedule or only record manually supplied principal and interest allocations?

Recommended default: Support manual payment allocation initially and design the model to support calculated schedules later.

Open Question 11-12 — Loan interest accrual
Should interest be accrued between payment dates?

Recommended default: Defer interest accrual until Sprint 12 unless required for the initial financial reports.

Open Question 11-13 — Asset disposal approval
Who must approve the disposal of a fixed asset?

Recommended default: Require Sales Manager or Comptroller approval, with the Comptroller responsible for financial review.

Open Question 11-14 — Inventory write-offs
Who may approve damaged, lost, or scrapped inventory?

Recommended default: Require Sales Manager or Comptroller approval and a documented reason.

19. Acceptance Criteria
Sprint 11 is accepted when:

Product identities can be created and maintained.
Serialized inventory items retain unique serial-number records.
Duplicate serial numbers are prevented.
Nonserialized materials can be tracked by quantity and cost layer.
Inventory receipts create cost layers.
FIFO issuance consumes the oldest available cost layer first.
Issuance across multiple cost layers calculates the correct total cost.
Serialized equipment remains linked to its individual serial number.
Inventory allocation is linked to the correct project and Master Sales Record version.
Inventory cannot be allocated to two active projects simultaneously.
Inventory returns require a condition or disposition decision.
Inventory adjustments preserve the original transaction.
Issued equipment and materials flow to the correct project-cost category.
Later inventory-cost corrections create formal adjustments rather than silently changing historical project profitability.
Vehicles, computers, and offices can be recorded as fixed assets.
Additional asset categories can be created through controlled administration.
Fixed assets retain acquisition cost, placed-in-service date, useful life, accumulated depreciation, and net book value.
Depreciation does not begin before the placed-in-service date.
Straight-line depreciation is calculated correctly.
Accumulated depreciation cannot exceed the depreciable basis without an approved adjustment.
Net book value is calculated correctly.
Historical depreciation entries cannot be silently overwritten.
Asset disposal preserves final book value and disposal information.
Loans can be recorded with lender, amount, date, term, rate, and payment schedule.
Loan payments separate principal, interest, and fees.
Principal balances recalculate correctly after payments.
Loan records cannot be deleted after financial activity exists.
Inventory, asset, depreciation, and loan reports are searchable and reproducible.
Unauthorized users cannot view restricted financial values.
Dashboard exceptions identify missing, inconsistent, or overdue records.
Inventory, asset, depreciation, and loan activities create accounting integration events.
All required actions are recorded in the tamper-evident audit log.
Automated Sprint 11 tests pass.
20. Automated Test Scenarios
Inventory Tests
Create a product identity.
Receive inventory from a vendor.
Create a cost layer.
Receive the same product at a different cost.
Issue inventory and confirm FIFO consumption.
Issue quantity across two cost layers.
Confirm remaining quantities in each layer.
Attempt to issue unavailable inventory.
Attempt to allocate one serialized item to two projects.
Attempt to reuse a retired serial number.
Return an issued item and require condition disposition.
Correct an inventory cost after project issuance.
Confirm the original inventory transaction remains unchanged.
Project-Cost Tests
Issue equipment to a project and confirm Equipment Cost integration.
Issue materials to a project and confirm Material Cost integration.
Confirm the Master Sales Record version is retained.
Confirm a later inventory correction creates a project-cost adjustment.
Confirm historical Sprint 10 profitability remains preserved.
Fixed-Asset Tests
Create a vehicle asset.
Create a computer asset.
Create an office asset.
Create an additional asset category.
Set acquisition date and placed-in-service date.
Attempt to calculate depreciation before placed-in-service date.
Calculate straight-line depreciation.
Confirm accumulated depreciation and net book value.
Attempt to exceed depreciable basis.
Dispose of an asset and preserve final values.
Confirm historical depreciation entries cannot be deleted.
Loan Tests
Create a loan.
Record a loan payment.
Split a payment between principal, interest, and fees.
Confirm the principal balance decreases by the principal portion only.
Mark a loan paid off only after the balance is resolved.
Correct a loan payment and preserve the original transaction.
Confirm loan activity creates accounting events.
Security and Audit Tests
Confirm a Technician cannot view acquisition costs.
Confirm an Accounts Payable Associate cannot change depreciation.
Confirm unauthorized users cannot edit loan balances.
Confirm asset-disposal approval is required.
Confirm inventory adjustments are audited.
Verify audit-chain integrity after corrections.
Confirm financial records cannot be silently deleted.
21. Sprint 11 Completion Definition
Sprint 11 is complete when:

Inventory is tracked by product identity, quantity, serial number, and FIFO cost layer.
Inventory is traceable from receipt through project allocation, installation, return, or disposal.
Project costs receive the correct inventory costs.
Fixed assets are recorded and depreciated using the approved straight-line method.
Asset disposal is controlled and auditable.
Loans and loan payments track principal, interest, and fees separately.
Reports reconcile to source transactions.
Accounting integration events are available for Sprint 12.
Permissions prevent unauthorized access or modification.
Dashboard exceptions are operational.
Audit records are complete and tamper-evident.
Acceptance tests pass.
Open questions are resolved or formally carried forward.
The Sprint 11 bundle is approved and preserved unchanged.
22. Next Markdown Bundle
The next bundle is:

text


GECC-12-financial-reporting.md
Its objective is to implement:

Double-entry financial ledger integration
Cash-basis financial reporting
Balance sheet
Cash-flow statement
Statement of operations
Accounts-receivable report
Accounts-payable report
Inventory report
Depreciation report
Loan report
Fixed-asset report
Retained earnings report
Working Capital Requirement report
Customer profitability report
Project profitability report
Monthly reporting periods
Fifth-calendar-day report preparation
Financial statement approval and signatures
Period publication
Period locking
Formal adjustment entries
Published-report preservation
Financial-report permissions
Financial-report audit logging
Reproducibility and report-integrity testing