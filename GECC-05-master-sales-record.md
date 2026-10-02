# GECC-05-master-sales-record.md

# GECC Sales Back Office System — Sprint 05 Master Sales Record

## Bundle Metadata

- Bundle name: GECC-05-master-sales-record.md
- Project: GECC Sales Back Office System
- Company: Go Ecco Climate Control
- Company abbreviation: GECC
- Bundle type: Sprint 05 design and implementation bundle
- Status: Approved design; ready for implementation
- Created date: 2026-09-20
- Previous bundle: GECC-04-customers-and-projects.md
- Next planned bundle: GECC-06-workflow-state-machine.md
- Database: PostgreSQL
- Primary-key strategy: Numeric BIGINT identity keys
- Business timezone: America/New_York
- System of record: PostgreSQL relational database
- Audit model: Append-only, hash-chained, permanently retained

---

## 1. Sprint Objective

Implement the Master Sales Record as the authoritative project record for:

- Customer information
- Project information
- Project location
- Sales Associate assignment
- Scope of work
- Equipment
- Serial numbers
- Retail price
- Payment option
- Project costs
- Contract data
- Invoice data
- Certificate data
- Approval status
- Document-version references
- Revision reason
- Revision history

The Master Sales Record must allow Sales Associates to enter project information once and provide the controlled source data for future invoices, contracts, certificates, commissions, payments, and financial reports.

---

## 2. Sprint 05 Deliverables

Sprint 05 must produce:

1. Master Sales Record form
2. Draft Master Sales Record behavior
3. Required-field validation
4. Customer and project consistency validation
5. Scope-of-work selections
6. Equipment records
7. Serial-number records
8. Pricing fields
9. Cost fields
10. Payment-option fields
11. Versioned Master Sales Records
12. Approval-ready records
13. Approval history
14. Revision reasons
15. Current and superseded version handling
16. Controlled access to pricing and costs
17. Master Sales Record search and display
18. Audit integration
19. Database acceptance tests
20. Sprint 06 workflow handoff

---

## 3. Requirements Carried Forward

The implementation must preserve these approved requirements:

- Each project has one Master Sales Record.
- The Master Sales Record is the authoritative source for project information.
- The Master Sales Record is linked to the customer and project.
- A project may include multiple pieces of equipment.
- A project may include multiple serial numbers.
- The Sales Associate creates the initial record.
- Unapproved information may be updated by authorized users.
- A Sales Manager or Comptroller reviews and approves the record.
- The authorized approver enters or confirms the retail price.
- One authorized approval is sufficient.
- Approved records are locked.
- Authorized reopening creates a new version.
- Original versions remain permanently preserved.
- Scope changes create a new Master Sales Record version.
- Revised documents use the revised version.
- Original documents and signatures remain preserved.
- The original project code remains unchanged through revisions.
- Documents must use the approved Master Sales Record.
- Unauthorized users may not edit approved retail price or approved costs.
- All material changes are auditable.

---

## 4. Master Sales Record Structure

### 4.1 `master_sales_record`

This table identifies the logical Master Sales Record for a project.

| Field | Type | Requirement |
|---|---|---|
| `id` | BIGINT | Primary key |
| `project_id` | BIGINT | Required foreign key, unique |
| `current_version_id` | BIGINT | Required after initial version creation |
| `record_status` | VARCHAR(30) | Draft, active, approved, superseded, cancelled |
| `created_at` | TIMESTAMPTZ | Required |
| `created_by` | BIGINT | Required foreign key |
| `updated_at` | TIMESTAMPTZ | Required |
| `updated_by` | BIGINT | Required foreign key |
| `is_active` | BOOLEAN | Required |
| `retired_at` | TIMESTAMPTZ | Optional |
| `retired_by` | BIGINT | Optional foreign key |
| `retirement_reason` | TEXT | Optional |

Constraint:

```text
UNIQUE(project\_id)
A project may have only one logical Master Sales Record.

4.2 master_sales_record_version
Field	Type	Requirement
id	BIGINT	Primary key
master_sales_record_id	BIGINT	Required foreign key
project_id	BIGINT	Required foreign key
customer_id	BIGINT	Required foreign key
version_number	INTEGER	Required
version_status	VARCHAR(30)	Draft, submitted, approved, rejected, superseded, cancelled
created_at	TIMESTAMPTZ	Required
created_by	BIGINT	Required foreign key
revision_reason	TEXT	Required for version greater than 1
supersedes_version_id	BIGINT	Optional foreign key
assigned_sales_associate_id	BIGINT	Required foreign key
retail_price	NUMERIC(19,4)	Required before approval
payment_option	VARCHAR(40)	Required
scope_summary	TEXT	Required
approval_status	VARCHAR(30)	Required
approved_by	BIGINT	Optional foreign key
approved_at	TIMESTAMPTZ	Optional
rejected_by	BIGINT	Optional foreign key
rejected_at	TIMESTAMPTZ	Optional
rejection_reason	TEXT	Optional
source_version_id	BIGINT	Optional foreign key
document_data_version	INTEGER	Required
record_hash	VARCHAR(128)	Optional integrity reference
Constraint:

text


UNIQUE(master\_sales\_record\_id, version\_number)
Only one version may be current for a logical Master Sales Record.

5. Master Sales Record Sections
The form must be divided into controlled sections.

5.1 Customer section
The form displays linked customer information:

Customer ID
Customer number
Customer name
Primary phone
Primary email
Authorized contacts
Customer information is retrieved from the Customer record.

The Master Sales Record must not create an independent conflicting copy of customer identity data.

5.2 Project section
The form displays linked project information:

Project ID
Project identification code
Project location
Assigned Sales Associate
Assigned Technician or Technicians
Current project status
Project creation date
The project identification code is read-only after creation.

5.3 Scope section
The form supports multiple scope selections:

Service
Repair
Install new
Remove old
Under Install New and Remove Old, the form supports multiple items:

Condenser coil
Evaporator coil
Refrigerant
Air handler
Furnace
Thermostat
Air ducts
Vents
A project may include more than one scope selection.

5.4 Equipment section
The form supports multiple equipment records.

Required fields may vary according to scope and equipment status:

Brand
Model
Function
Serial number
Equipment category
New, existing, removed, or installed status
Inventory reference
Notes
The form must support:

Add equipment
Edit unapproved equipment
Remove an unapproved equipment row
Record existing equipment
Record equipment to be removed
Record equipment to be installed
Update serial numbers where authorized
5.5 Pricing section
The pricing section includes:

Retail price
Payment option
Adjustment placeholder
Pricing notes where authorized
Retail price must be entered or confirmed by:

Sales Manager, or
Comptroller
The Sales Associate may enter proposed pricing where permitted but may not approve the final retail price.

5.6 Cost section
The cost section includes:

Equipment cost
Material cost
Dealer fee
Lead cost
Technician cost
Damon cost
Jerry cost
Initial default values:

text


Lead:  \$3,000
Damon: \$500
Jerry: \$250
The default values apply to the project unless an authorized user creates a project-specific change.

A change to one project must not silently alter:

Past projects
Future projects
Other projects
Global defaults
5.7 Payment-option section
Initial supported payment methods:

Bank cashier’s check
Bank wire transfer
The payment option identifies the planned method and does not constitute payment receipt or verification.

5.8 Document-data section
The Master Sales Record stores or references the controlled data needed for:

Invoice
Contract
Certificate
Document generation is implemented in Sprint 07. Sprint 05 must provide stable version references for later document generation.

6. Scope and Equipment Tables
6.1 msr_scope_selection
Field	Type	Requirement
id	BIGINT	Primary key
msr_version_id	BIGINT	Required foreign key
scope_code	VARCHAR(40)	Required
scope_group	VARCHAR(40)	Required
is_selected	BOOLEAN	Required
notes	TEXT	Optional
Standard audit columns	—	Required
6.2 msr_equipment
Field	Type	Requirement
id	BIGINT	Primary key
msr_version_id	BIGINT	Required foreign key
project_id	BIGINT	Required foreign key
equipment_sequence	INTEGER	Required
brand	VARCHAR(150)	Required where applicable
model	VARCHAR(150)	Required where applicable
function	VARCHAR(100)	Required
serial_number	VARCHAR(150)	Optional until required by workflow
equipment_category	VARCHAR(80)	Required
equipment_status	VARCHAR(40)	New, existing, removed, installed
inventory_id	BIGINT	Optional foreign key
notes	TEXT	Optional
Standard audit columns	—	Required
Constraint:

text


UNIQUE(msr\_version\_id, equipment\_sequence)
6.3 Serial-number rules
Serial numbers must remain individually identifiable.
Duplicate serial numbers must be flagged.
A serial number may not be silently reassigned to a different project.
Serial-number changes after approval require authorized revision.
The original serial-number value remains available in history.
Missing serial numbers may be permitted in draft status only where the applicable scope does not yet require them.
7. Cost Tables
7.1 msr_cost_component
Field	Type	Requirement
id	BIGINT	Primary key
msr_version_id	BIGINT	Required foreign key
cost_type	VARCHAR(40)	Required
amount	NUMERIC(19,4)	Required
default_amount	NUMERIC(19,4)	Optional
is_default_applied	BOOLEAN	Required
is_project_override	BOOLEAN	Required
entered_by	BIGINT	Required foreign key
entered_at	TIMESTAMPTZ	Required
approval_status	VARCHAR(30)	Required
approved_by	BIGINT	Optional foreign key
approved_at	TIMESTAMPTZ	Optional
notes	TEXT	Optional
7.2 Cost permissions
Equipment Cost, Material Cost, and Dealer Fee may be entered by:

Sales Manager
Installation Manager
Comptroller
Technician Cost may be entered by:

Sales Manager
Installation Manager
Comptroller
Lead Cost, Damon Cost, and Jerry Cost may be edited for an individual project by authorized users.

Sprint 05 must enforce field permissions but Sprint 06 will enforce the complete lifecycle timing.

8. Draft Behavior
8.1 Draft creation
A Sales Associate may create a draft Master Sales Record for an assigned project.

Draft creation must require:

Customer
Project
Project location
Assigned Sales Associate
Project code
Initial scope or stated scope-pending reason
8.2 Draft editing
Before approval, authorized users may edit permitted fields.

Draft edits must:

Update the draft version.
Preserve changed-field history.
Update updated_at and updated_by.
Generate an audit event.
Revalidate the complete record before submission.
8.3 Save as draft
The form must support:

Save draft
Resume draft
Cancel entry
Clear unsaved form
View validation errors
Submit for review
A draft is not an approved sale and cannot generate final controlled documents.

8.4 Draft deletion
Draft records must not be hard-deleted without an authorized retirement or cancellation action.

The system must preserve:

Creator
Creation time
Cancellation or retirement reason
Audit history
9. Submission and Approval
9.1 Submission for review
A Sales Associate may submit a complete draft for review.

Submission requires validation of:

Required customer information
Project consistency
Location consistency
Project-code consistency
Assigned Sales Associate
Scope
Equipment requirements
Pricing
Costs
Payment option
Duplicate data
Required explanations
Record version
9.2 Approval authority
The approved core lifecycle requires approval by:

Sales Manager, or
Comptroller
One authorized approval is sufficient.

The implementation must retain a flexible approval structure so the later workflow sprint can represent any approved applicable workflow distinction without weakening the core requirement.

9.3 Approval result
An approver may:

Approve
Reject
Return for correction
Approval must record:

Exact Master Sales Record version
Approver
Approver role
Date and time
Approval result
Approval notes
Related audit event
9.4 Approved record behavior
After approval:

The approved version becomes immutable.
Ordinary edits are rejected.
The current version reference is updated.
Later changes require authorized reopening or a new revision.
Documents may be generated from the approved version in later sprints.
The record cannot be silently overwritten.
10. Versioning and Revision
10.1 New version creation
A new version is required when:

An approved record is reopened.
A signed or approved project changes scope.
Approved pricing changes.
Approved costs change.
Contract data changes.
Invoice data changes.
Certificate data changes.
An authorized correction is required.
10.2 Revision requirements
Every revision must include:

New version number
Prior version reference
Revision reason
Revising user
Revision date and time
Changed fields
New status
Approval state
Related audit event
10.3 Version relationships
The system must preserve:

text


Version 1
  └── Version 2
        └── Version 3
Each version must remain independently retrievable.

10.4 Current version
Only one version may be marked current.

The current version must be:

Linked from master_sales_record.current_version_id
Linked from project.current_msr_version_id
Clearly marked in user interfaces
Used by later document generation only when its status permits generation
10.5 Superseded versions
A superseded version:

Remains read-only
Remains searchable according to permissions
Remains linked to its documents and approvals
Cannot be edited
Cannot be used as the current source for new documents
Remains available for audit and historical reporting
11. Validation Rules
Before saving a draft:

Required draft fields must be present.
Foreign-key references must be valid.
The user must have permission to create or edit the record.
The project must exist.
The customer must match the project.
The project location must match the project.
The assigned Sales Associate must be valid.
Scope values must come from controlled values.
Equipment values must be valid.
Monetary values must be non-negative unless an approved accounting rule permits otherwise.
Payment option must be supported.
Duplicate serial numbers must be flagged.
Before submission for approval:

Required fields must be complete.
Customer consistency must pass.
Project consistency must pass.
Project-code consistency must pass.
Scope consistency must pass.
Equipment and serial-number requirements must pass.
Retail price must be present.
Costs must be present or explicitly marked not applicable.
Payment option must be present.
Required duplicate explanations must be present.
No prohibited draft errors may remain.
Before approval:

The approver must be authorized.
The record must still be the current submitted version.
The record must not already be approved.
The record must not be superseded.
The record must not be cancelled.
The approval must reference the exact version being approved.
12. Access Rules
Sales Associate
May:

Create Master Sales Records for assigned projects.
Edit unapproved records within assigned projects.
View generated draft information.
Submit records for review.
Review the generated contract in later sprints.
View commission information related only to their own sales.
May not:

Approve a Master Sales Record.
Edit an approved Master Sales Record.
Change approved retail price.
Change approved costs.
Approve commission or payroll reports.
View another Sales Associate's commissions.
Sales Manager
May:

View all Master Sales Records.
Create or edit authorized records.
Enter or confirm retail price.
Enter or revise permitted costs.
Approve Master Sales Records.
Reopen approved records.
Approve applicable corrections.
Comptroller
May:

View all Master Sales Records.
Create or edit authorized records.
Enter or confirm retail price.
Enter or revise permitted costs.
Approve Master Sales Records.
Reopen approved records.
Approve applicable corrections.
Installation Manager
May:

View installation-related Master Sales Records.
Enter or approve installation-related cost information according to the applicable authorization.
View approved scope and equipment information.
The core sales-approval lifecycle remains Sales Manager or Comptroller approval unless a later approved decision explicitly changes it.

Technician
May:

View approved scope of work for assigned projects.
View required equipment information.
Update authorized serial-number and completion information in later workflow stages.
Propose scope changes.
May not:

Change retail price.
Change approved costs.
Change contract terms.
Approve the Master Sales Record.
Accounts Payable Associate
May:

View Master Sales Record fields necessary for payable duties.
View project, vendor, cost-category, and payable references where authorized.
Database Administrator
May:

Maintain approved database functions.
Search authorized records.
Perform approved data-quality maintenance.
The Database Administrator does not automatically receive Master Sales Record approval authority.

13. Audit Events
Sprint 05 must create audit events for:

Master Sales Record draft created
Draft viewed
Draft edited
Draft saved
Draft resumed
Draft cancelled
Draft submitted for review
Validation failure
Master Sales Record approved
Master Sales Record rejected
Master Sales Record returned for correction
Master Sales Record reopened
New version created
Version superseded
Revision reason entered
Scope changed
Equipment added
Equipment changed
Serial number added
Serial number changed
Retail price entered
Retail price changed
Cost entered
Cost changed
Payment option changed
Unauthorized edit attempt
Unauthorized approval attempt
Restricted Master Sales Record access attempt
Each event must include:

User
Role
Session
Project
Customer
Master Sales Record
Version
Action
Outcome
Reason where applicable
Previous and new status where applicable
Audit-chain references
14. Sprint 05 Decisions
D-05-001 — Master Sales Record authority
The Master Sales Record is the authoritative source of project information for downstream documents, payments, commissions, and reports.

D-05-002 — One logical record per project
Each project has one logical Master Sales Record with one or more preserved versions.

D-05-003 — Immutable approved versions
Approved Master Sales Record versions are immutable.

D-05-004 — Revision behavior
Changes to approved records create new versions and preserve previous versions.

D-05-005 — Core approval authority
The core approval lifecycle requires approval by either the Sales Manager or Comptroller.

One authorized approval is sufficient.

D-05-006 — Approval version binding
Approvals must reference the exact Master Sales Record version being approved.

D-05-007 — Project-code preservation
Master Sales Record revisions do not create a new project code.

D-05-008 — Default-cost isolation
Changes to Lead, Damon, Jerry, or other project costs apply only to the indicated project version unless a separately approved configuration change is made.

D-05-009 — Document-generation dependency
Sprint 05 stores stable version references for documents, but document generation itself is implemented in Sprint 07.

D-05-010 — Calculation separation
Sprint 05 stores source pricing and cost inputs. Final profitability snapshots and accounting calculations remain subject to later financial-reporting design.

15. Sprint 05 Acceptance Criteria
Sprint 05 is accepted when:

A Sales Associate can create a Master Sales Record for an assigned project.
A Master Sales Record is linked to exactly one project.
A project has only one logical Master Sales Record.
The customer and project relationships are validated.
The project code is inherited from the project and cannot be changed through the Master Sales Record.
Scope selections support service, repair, install new, and remove old.
Install-new and remove-old sub-items are supported.
Multiple equipment records can be added.
Multiple serial numbers can be stored.
Duplicate serial numbers are detected.
Retail price is stored as a fixed-precision amount.
Payment option supports cashier's check and bank wire.
Required cost categories are supported.
Initial fixed default costs are represented.
Project-specific cost overrides do not change other projects.
Draft records can be saved and resumed.
Draft records can be submitted for review.
Required fields are validated before submission.
Inconsistent customer, project, location, and project-code values are rejected.
An authorized Sales Manager or Comptroller can approve a submitted version.
One authorized approval is sufficient.
An unauthorized user cannot approve the record.
An approved version cannot be silently edited.
An approved version can be superseded only through an authorized revision process.
Revision reason, user, date, and time are preserved.
Previous versions remain retrievable.
Only one current version exists.
Superseded versions cannot be used as the current source for new documents.
Access to pricing and cost fields is role-controlled.
Sales Associates cannot view another Sales Associate's commission data.
All material Master Sales Record actions create audit events.
Failed and unauthorized actions create audit events.
The data model is ready for Sprint 06 lifecycle and state-machine enforcement.
16. Sprint 05 Test Cases
Creation and draft tests
Create a valid Master Sales Record.
Reject creation for an unauthorized project.
Save an incomplete draft where permitted.
Resume a saved draft.
Reject invalid customer-project combinations.
Reject invalid project-location combinations.
Reject unsupported payment options.
Reject invalid monetary values.
Clear a draft form without saving unsaved changes.
Scope and equipment tests
Select service.
Select repair.
Select install new.
Select remove old.
Select multiple install-new sub-items.
Select multiple remove-old sub-items.
Add multiple equipment records.
Add multiple serial numbers.
Reject duplicate serial numbers.
Preserve equipment history after revision.
Pricing and cost tests
Enter proposed retail price.
Confirm retail price as Sales Manager.
Confirm retail price as Comptroller.
Reject Sales Associate approval.
Enter permitted equipment cost.
Enter permitted material cost.
Enter permitted dealer fee.
Apply initial fixed defaults.
Override a fixed default for one project.
Confirm the override does not change another project.
Approval tests
Submit a valid record for review.
Reject submission with missing required data.
Approve as Sales Manager.
Approve as Comptroller.
Reject approval by Sales Associate.
Reject approval by Technician.
Reject approval of a superseded version.
Reject duplicate approval of the same version.
Preserve approver and timestamp.
Versioning tests
Create Version 1.
Approve Version 1.
Reopen through an authorized action.
Create Version 2.
Preserve Version 1 unchanged.
Record revision reason.
Mark Version 1 superseded.
Mark Version 2 current.
Confirm the project code remains unchanged.
Confirm Version 1 remains searchable according to permission.
Security and audit tests
Unauthorized user cannot view restricted cost fields.
Unauthorized user cannot edit approved fields.
Unauthorized user cannot approve a record.
Unauthorized user cannot reopen a record.
Draft creation is audited.
Approval is audited.
Revision is audited.
Serial-number changes are audited.
Unauthorized attempts are audited.
Failed audit creation prevents the required business action from succeeding.
17. Sprint 05 Open Questions
These implementation questions do not change approved requirements:

What exact fields are mandatory for each scope and equipment category?
Which equipment categories require serial numbers before approval?
What exact initials format will be used for Sales Associates?
What cost fields may the Installation Manager edit before installation?
What fields may be entered by the Comptroller when acting as a substitute signer later?
What exact return-for-correction workflow will Sprint 06 implement?
What approval reason values should be standardized?
What calculation-rule version should be stored with pricing and cost inputs?
What project-specific cost override process requires Sales Manager approval?
What Master Sales Record fields must be copied into document-generation staging tables?
18. Sprint 06 Handoff
Sprint 06 will implement the controlled lifecycle and task workflow.

Sprint 06 must use the following Sprint 05 structures:

Master Sales Record
Master Sales Record version
Scope selections
Equipment
Serial numbers
Pricing
Cost components
Payment option
Approval status
Revision reason
Current-version reference
Approval history
Sprint 06 must enforce:

Required state transitions
Approval sequencing
Installation task creation
Three-day installation restriction
Scope-change process
Authorized reopening
New-version creation
Revised approval and signature requirements
Sprint 06 must not overwrite Sprint 05 versions or bypass Master Sales Record immutability.

19. Historical Record
Supersedes
None.

Preserves
GECC-00-project-baseline.md
GECC-01-data-model.md
GECC-02-auth-and-permissions.md
GECC-03-audit-log.md
GECC-04-customers-and-projects.md
Sprint 00 decisions
Sprint 01 decisions
Sprint 02 decisions
Sprint 03 decisions
Sprint 04 decisions
Approved project-code requirements
Approved lifecycle requirements
Approved versioning and audit requirements
Changes introduced by this bundle
Defines the Master Sales Record structure.
Defines draft, submission, approval, and revision behavior.
Defines scope and equipment data.
Defines pricing, payment-option, and cost inputs.
Defines Master Sales Record versioning.
Defines access restrictions for pricing and cost data.
Defines Sprint 05 audit events.
Defines Sprint 06 workflow handoff.