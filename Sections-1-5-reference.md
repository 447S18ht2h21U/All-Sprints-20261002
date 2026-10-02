GECC Sales Back Office System
Reference Document — Sections 1–5
Coverage: GECC-01 through GECC-05, with applicable GECC-00 baseline requirements carried forward.
Purpose: Compact source reference for use in a subsequent chat.
System status: GECC-01 is in design execution; GECC-02 through GECC-05 requirements and decisions are carried forward unless superseded by a later approved bundle.

1. Section Index and Source References
Section	Bundle	Subject	Primary references
1	GECC-01	Data Model and Accounting Foundation	GECC-01 §§1–10; source PDF pp. 12–18
2	GECC-02	Authentication, Roles, and Permissions	GECC-02 §§1–18; source PDF pp. 19–32
3	GECC-03	Tamper-Evident Audit Logging	GECC-03 §§1–16; source PDF pp. 33–45
4	GECC-04	Customers, Contacts, Locations, and Projects	GECC-04 requirements; baseline §§8.1–8.4, 9, 10, 22–23; Sprint 04 deliverables
5	GECC-05	Master Sales Record	GECC-05 requirements; baseline §§8.5–8.6, 10–15, 27, 29; Sprint 05 deliverables
Reference convention: GECC-0x §n identifies the relevant bundle section. Baseline references use GECC-00 §n.

2. Project Architecture Carried Forward
2.1 System of record
PostgreSQL relational database is the authoritative system of record.
PDFs, emails, exports, and Google Sheets are generated outputs or copies.
Google Sheets must not be used as the primary database.
The business timezone is America/New_York.
Persistent entities use numeric BIGINT identity primary keys.
Business entities use database identifiers and stable business keys where required.
Approved, signed, paid, published, or audited records must not be hard-deleted.
References: GECC-00 §§1, 6.1, 29; GECC-01 metadata and §§1, 3, 5.

2.2 Core architectural controls
The system is organized around:

Master Sales Record as the project source of truth
Relational referential integrity
Controlled workflow and status transitions
Immutable approved versions
Versioned documents
Role-, record-, field-, and action-level authorization
Append-only, hash-chained audit logging
Effective-dated configuration
Transactional multi-record operations
Searchable, reproducible records and outputs
2.3 Versioning model
A project has an authoritative Master Sales Record.
Approved MSR versions are immutable.
Authorized reopening or scope change creates a new version.
Each new version references its predecessor.
Every revision records:
Revision reason
Revising user
Date and time
Previous version
Current or superseded status
Original records, documents, signatures, and audit references remain preserved.
A document must remain linked to the exact MSR version used to generate it.
References: GECC-00 §§6.3, 6.4, 8.5, 13; GECC-01 §§3.2–3.4; GECC-05.

2.4 Security architecture
Authorization combines:

Role-based access control
Record-scope access
Field-level restrictions
Action-level authorization
Authorization must be enforced in:

Application screens and routes
Service/business-operation layer
Record-scope queries
Field read/write filters
Database transactions
Exports
Downloads
Printing
A hidden or disabled user-interface control is not sufficient protection.

References: GECC-02 §§1–3, 10; decision D-02-001.

2.5 Audit architecture
Audit records are separate from ordinary business data.
Audit events are append-only and permanently retained.
Each event references the previous event hash.
Initial hash algorithm: SHA-256.
Any alteration, deletion, insertion, omission, or reordering must cause integrity verification failure.
Audit-log access, searches, exports, and verification operations are themselves audited.
Audit failure must fail closed for required auditable operations unless an approved emergency mode exists.
References: GECC-03 §§1–6, 10–15; decisions D-03-001 through D-03-004.

3. Shared Terminology
Term	Definition
GECC	Go Ecco Climate Control
MSR	Master Sales Record; authoritative project-level sales record
Project	Customer work engagement associated with a location, scope, pricing, documents, costs, payments, and workflow
Project code	Unique code in the format Sequential Number-Sales Associate Initials-YYYY-MM-DD
Current version	Approved MSR or document version governing current processing
Superseded version	Historical version replaced by a later approved version
Record scope	Restriction on which records a user may access
Field permission	Restriction on whether a user may view, edit, or export a field
Action permission	Authorization for an operation such as approve, sign, reopen, export, or verify
Effective dating	Start and optional end dates controlling when a role, permission, configuration, or version applies
Controlled transition	State change permitted only when required guards and authorizations pass
Workflow task	Action assigned to a user or role; task completion does not replace transition validation
Audit event	Permanent record of an access attempt, business action, system action, or failure
Hash chain	Audit structure in which each event includes the hash of the immediately preceding event
Restricted field	Financial, commission, payment, payroll, or audit information requiring additional authorization
Approved record	Record that passed required review and is locked against ordinary edits
Authorized reopening	Controlled process that creates a new version rather than modifying the approved version
Project location	Separate street, city, state, and ZIP data used for project identity and state artwork selection
Duplicate active location	Existing active project at the same customer location requiring warning and explanation or correction
4. GECC-01 — Data Model and Accounting Foundation
4.1 Objective and deliverables
The objective is to convert the approved baseline into a relational schema and initial accounting foundation without removing or weakening approved requirements.

Required deliverables include:

Entity-relationship model
Table and field definitions
Primary and foreign keys
Unique constraints and indexes
Controlled enumerations
Required-field rules
Status-transition structures
MSR versioning
Document and signature references
Payment, receivable, payable, cost, and commission structures
Financial-period structures
Initial accounting ledger
Audit references
Database acceptance tests
Transaction and data-integrity rules
Migration and seed-data considerations
References: GECC-01 §§1–2.

4.2 Core entity groups
The model must represent at least:

Identity
User
Role
Permission
User-role assignment
User status
Session reference
Customers and projects
Customer
Customer contact
Project location
Project
Project assignment
Project status history
Sales records
Master Sales Record
MSR version
Scope selection
Equipment
Equipment serial number
Cost component
Pricing record
Payment option
Documents and workflow
Document
Document version
Document type
Template version
Artwork version
Signature request
Signature audit certificate
Workflow state
State transition
Task
Task assignment
Approval
Exception
Required action
Payments and accounting
Payment
Payment verification
Payment proof
Adjustment
Credit
Refund
Accounts receivable structure
Vendor
Accounts payable
Payable category
Overhead expense
Payable payment
Account
Journal entry
Journal-entry line
Cash receipt
Cash disbursement
Financial-report period
Period lock
Adjusting entry
Retained-earnings reference
Contractors, inventory, and assets
Contractor or employee
Commission configuration
Commission calculation
Commission earning event
Commission report
Commission report item
Commission adjustment
Annual commission summary
Inventory item
Inventory cost layer
Inventory allocation
Fixed asset
Asset category
Depreciation record
Loan
Audit and configuration
Audit-event reference
Configuration version
State artwork reference
Effective-date configuration
References: GECC-01 §4.

4.3 Relational design rules
The database must use:

Primary keys for every persistent entity
Foreign keys for required relationships
Unique constraints for project codes and other unique values
Check constraints or lookup tables for controlled statuses
Required-field constraints
Transactional saves for multi-table operations
Indexes supporting approved search requirements
Effective and retirement dates for changing configuration
Version identifiers for historically preserved records
No hard deletion of approved, signed, paid, published, or audited records
Referential integrity across customer, project, document, payment, cost, and report records
Reference: GECC-01 §5.

4.4 Accounting foundation
The approved architectural direction is an internal double-entry ledger supporting:

Accounts
Debits
Credits
Journal entries
Journal-entry lines
Cash receipts
Cash disbursements
Accounts receivable
Accounts payable
Financial periods
Period locking
Adjusting entries
Retained earnings
The initial chart of accounts and final treatment of calculated financial values remain unresolved.

References: GECC-00 §30; GECC-01 §§2, 6, 7, 8.

4.5 Database acceptance requirements
Acceptance testing must demonstrate that the system can:

Prevent duplicate project codes
Begin project-code sequencing at 101
Prevent reuse of cancelled codes
Preserve project codes through revisions
Preserve and supersede MSR versions without overwriting
Link documents to the correct MSR version
Support multiple customer contacts and projects
Support multiple equipment records and serial numbers
Separate equipment, dealer-fee, and material payables
Link payments to the correct project and invoice
Distinguish payment entry from payment verification
Preserve adjustment amounts, reasons, approvers, and timestamps
Link commissions to projects, contractors, reports, and adjustments
Lock financial periods
Represent post-publication corrections as adjustments
Preserve serial-number inventory identity
Represent FIFO layers
Store straight-line depreciation inputs and outputs
Store loan principal, interest, and payment information
Enforce balanced debit and credit lines
Support future audit references
Reject foreign-key and required-field violations
Commit or roll back multi-record operations transactionally
Reference: GECC-01 §6.

4.6 GECC-01 unresolved design questions
Final database platform and implementation details
Single audit-event reference versus related-event table
Lookup tables versus database enumerations
Storage of calculated financial values for reproducibility
Minimum chart of accounts
Relationship between accounting periods and publication dates
FIFO allocation for partial inventory issues
Non-deletion retirement representation
Dashboard and search indexing strategy
Spreadsheet migration staging tables
Some questions were later answered by carried-forward decisions, including PostgreSQL, BIGINT keys, Eastern timezone, effective dating, and internal double-entry accounting.

Reference: GECC-01 §7 and §8.

5. GECC-02 — Authentication, Roles, and Permissions
5.1 Objective
The authorization foundation must ensure that:

Only authenticated users access protected functionality.
Users receive only role-appropriate permissions.
Access can be restricted by screen, record, field, and action.
Sales Associates see only their own commission information.
Technicians see only assigned or authorized project information.
Financial, payroll, commission, and audit data are restricted.
Permission failures are logged.
Authorization is enforceable beyond the user interface.
Reference: GECC-02 §1.

5.2 Authentication requirements
The system must support:

Unique identity for every user
Account activation and deactivation
Password or approved external authentication
Failed-login tracking
Lockout or equivalent protective response
Session creation after successful login
Logout termination
Inactivity expiration
Re-authentication for sensitive operations where required
No shared accounts
No anonymous business-record access
No authorization based only on hidden controls
Sensitive actions requiring explicit authorization include:

Approving an MSR
Approving retail price or costs
Reopening an approved record
Approving credits, refunds, or adjustments
Signing contracts or invoices
Substitute certificate signing
Approving commission changes or reports
Publishing financial statements
Locking financial periods
Viewing or exporting audit logs
Exporting restricted financial or commission data
References: GECC-02 §4.

5.3 Permission structures
The permission model includes:

permission
Permission ID
Unique permission code
Resource type
Action code
Description
Sensitive indicator
Active/retired status
Retirement metadata
Example permission codes:

text


customer.view
customer.create
project.view
project.create
project.edit
master_sales_record.approve
master_sales_record.reopen
invoice.sign
contract.sign
certificate.sign
payment.verify
adjustment.approve
commission.view_own
commission.view_all
commission_report.approve
financial_report.publish
financial_period.lock
audit_log.view
audit_log.export
role_permission
Role
Permission
Effective dates
Granting user
Grant reason
Active/retired status
record_access_scope
User
Resource type and ID
Scope type: assignment, ownership, or authorization
Effective dates
Granting user and reason
Active/retired status
field_permission
Role
Resource type
Field name
View, edit, and export flags
Effective dates
Active status
user_session
User
Unique session identifier
Creation and activity timestamps
Expiration and logout timestamps
Termination reason
Session status
Client reference
authentication_event
User, where known
Login identifier
Event type
Timestamp
Session
Success/failure status
Failure reason
Source reference
References: GECC-02 §§5.1–5.6.

5.4 Roles
Required roles:

Sales Associate
Sales Manager
Comptroller
Installation Manager
Technician
Accounts Payable Associate
Database Administrator
Other Contractor
Users may have multiple effective-dated roles, with one optional primary role.

Important role boundaries
Sales Associates may create and edit unapproved assigned records but may not approve their own sales.
Sales Managers and Comptrollers may approve MSRs, adjustments, commissions, and financial reports as applicable.
Installation Managers manage installation operations and certificates but do not sign financial statements.
Technicians may update authorized installation fields but may not change prices, approved costs, or contract terms.
Accounts Payable Associates manage authorized payable payments.
Database Administrators do not automatically receive approval, signing, financial, payroll, commission, or audit-content rights.
Other Contractors receive only explicitly assigned projects and fields.
References: GECC-00 §§7.1–7.8; GECC-02 §§6–8.

5.5 Restricted fields
Restricted information includes:

Approved retail price
Equipment, material, dealer, lead, Damon, Jerry, and technician costs
Gross and net profit
Profitability reports
Balance-sheet, cash-flow, and operations-statement data
Accounts receivable and payable balances
Commission rates and amounts
Payroll totals
Other Sales Associates’ commission information
Bank names
Cashier’s-check numbers
Wire references
Payment proof
Verification details
Adjustment approvals
Audit hashes and integrity fields
References: GECC-02 §9.

5.6 Approval separation
Sales Associates cannot approve their own sales.
Approval must reference the exact record or document version.
Superseded versions cannot receive new approval.
Commission changes and early commission payment require Sales Manager approval.
Published financial statements require Sales Manager and Comptroller approval.
Comptroller substitute certificate signing requires Installation Manager unavailability and a reason.
Approval is rejected when the user’s role is incompatible with the action.
Reference: GECC-02 §11.

5.7 GECC-02 decisions
D-02-001: Combined RBAC, record scope, field permissions, and action permissions.
D-02-002: BIGINT identity IDs.
D-02-003: Effective-dated permissions and access scopes.
D-02-004: Default deny.
D-02-005: Database Administrator boundary.
D-02-006: Authentication and authorization events integrate with Sprint 03.
D-02-007: Sales Associates can view only commissions from their own sales, including through search, export, and reports.
D-02-008: Approvals bind to the exact version being approved.
Reference: GECC-02 §13.

5.8 GECC-02 unresolved issues
Local credentials versus external identity provider
Password and MFA requirements
Inactivity timeout and lockout thresholds
Who may grant or revoke roles
Emergency and temporary access procedures
Row-level-security implementation
Fields requiring re-authentication
Initial user and role-assignment data
Notifications after repeated permission failures
Administrative session duration
Role inheritance versus direct assignments
Reference: GECC-02 §16.

6. GECC-03 — Tamper-Evident Audit Logging
6.1 Objective
The audit system must preserve the history of:

Login, logout, session, and authentication activity
Views and searches
Creates, edits, approvals, reopenings, and revisions
Exports, prints, downloads, and emails
Signatures and signature failures
Payment and accounting activity
Permission failures and restricted-access attempts
Administrative changes
Audit searches, exports, and integrity verification
Audit data must be separate from ordinary business records.

References: GECC-03 §§1–2.

6.2 Audit principles
Append-only
Permanently retained
Protected from ordinary edits and deletion
Chained to the immediately preceding event
Successful and failed actions both recorded
Denied access attempts recorded
Audit access and export audited
Historical data searchable and reproducible
Business interpretation uses Eastern time while preserving actual event instants
Reference: GECC-03 §3.

6.3 Audit access
Audit-log access is limited to:

Sales Manager
Comptroller
Installation Manager
Database Administrator access to infrastructure does not automatically grant application-level audit-content access.

Example permissions:

text


audit_log.view
audit_log.search
audit_log.export
audit_log.verify
audit_log.view_integrity_failure
audit_log.view_archived
Reference: GECC-03 §4.

6.4 audit_event model
Required fields include:

BIGINT ID
Monotonic event sequence number
UUID event identifier
Occurred and recorded timestamps
User and role snapshot
Session
Event and activity types
Resource type, ID, and business key
Related project and customer
Outcome: success, failure, denied, or error
Failure reason
Required business reason
Request, screen, and operation references
Canonical JSON payload
Previous-event hash
Event hash
Hash algorithm
Integrity status
Archive status and segment
Creation timestamp
Sequence numbers:

Are monotonic
Cannot be reused or changed
Must not duplicate
Must not be missing or reordered
Require serialized allocation under concurrent transactions
The first event is a permanent genesis event containing the initial hash seed and system metadata.

Reference: GECC-03 §5.

6.5 Hash-chain rules
Initial algorithm: SHA-256.
Hash input includes the complete canonical event representation and previous-event hash.
Canonicalization requires:
Fixed field order
Normalized property names
Consistent null representation
Fixed numeric formatting
ISO-8601 timestamps
UTF-8 encoding
Sorted JSON object properties
Preserved array order
No non-value whitespace
Conceptually:

text


event_hash =
SHA-256(canonical_event_representation)
Concurrent event creation must ensure that each event references the actual preceding event and that failed business transactions do not leave invalid chain entries.

References: GECC-03 §§5–6.

6.6 Event categories
The audit system must support events for:

Authentication and sessions
Dashboard, database, record, document, report, search, export, print, and download access
Customer, contact, project, MSR, equipment, serial-number, cost, task, and status operations
Document generation, delivery, signature, supersession, and cancellation
Payments, verification, refunds, credits, adjustments, receivables, payables, periods, and journal entries
Commission calculations, earning, reports, adjustments, and annual summaries
Permission changes, access failures, administrative changes, and integrity verification
References: GECC-03 §§7–9.

6.7 Payload requirements
Payloads must be sufficient to reconstruct an event without storing unnecessary sensitive data.

Permitted examples:

Action
Prior and new status
Version
Approval or revision reason
Signature role
Payment-verification method
Document/artwork version
Permission result
Search category
Export type
Integrity result
The audit payload must not contain:

Passwords
Authentication secrets
Session tokens
Private signing keys
Full payment credentials
Unnecessary personal information
Unnecessary document contents
Sensitive values should use masking, secure references, or hashes.

Reference: GECC-03 §8.

6.8 Transaction behavior
For a committed business action:

Validate authorization.
Execute the operation.
Generate and hash the audit event.
Commit the business action and event transactionally.
For a failed or denied action:

Reject or roll back the business action.
Record the failure in a separate audit transaction.
Preserve the failure reason, session, and request reference.
If required audit writing fails, the business operation must fail closed unless approved emergency mode exists.

Reference: GECC-03 §15.

6.9 Storage, retention, and verification
Hot storage
Recent records remain in searchable PostgreSQL partitions, generally organized monthly by recorded_at.

Indexes should support:

Time
User and role
Event and activity type
Resource and project
Customer
Outcome
Session
Integrity status
Immutable archive
Archived records require:

Write-once or retention-locked storage
Permanent retention
Segment identifiers
Sequence and hash boundaries
Segment checksum
Export timestamp
Process identifier
Verification status
Verification
Verification must support:

Entire chain
Date range
Sequence range
Archive segment
Resource, project, or user history
Checks include sequence continuity, hashes, genesis validity, duplicates, missing events, order, archive boundaries, checksums, algorithm compatibility, and canonical payload consistency.

Possible outcomes:

Valid
Valid with archived segments
Incomplete
Failed
Unable to verify
Requires restoration
Integrity failure requires an administrative exception, notification to Sales Manager, Comptroller, and Installation Manager, preserved evidence, documented investigation, and no silent repair.

References: GECC-03 §§10–11.

6.10 GECC-03 decisions
D-03-001: Separate append-only audit-event store.
D-03-002: SHA-256.
D-03-003: Immediate previous-event hash chain.
D-03-004: Permanent retention with immutable archival permitted.
Reference: GECC-03 §16.

6.11 GECC-03 unresolved issues
Infrastructure controls for database superuser access
Exact archive technology
Archive verification and restoration operations
Monitoring and alerting implementation
Final CSV, JSON, and printable/PDF export implementation
Operational emergency mode, if permitted
Partition management and long-term performance procedures
7. GECC-04 — Customers, Contacts, Locations, and Projects
7.1 Scope
Sprint 04 establishes the customer and project foundation needed before MSR processing.

Required deliverables:

Customer records
Multiple customer contacts
Project locations
Projects
Assignments
Duplicate detection
Project-code generation
Search
References: GECC-00 §§8.1–8.4, 22–23; baseline Sprint Plan, Sprint 04.

7.2 Customer requirements
A customer may have multiple projects.

Customer data includes:

Customer ID
Customer name
Multiple phone numbers
Multiple email addresses
Mailing address, when required
Customer status
Customer-folder reference
Creation date
Last-modified date
Duplicate-check information
Customer contacts include:

Contact ID
Customer ID
Contact name
Contact type
Phone
Email
Primary-contact indicator
Preferred contact method
Active/inactive status
7.3 Project requirements
A project includes:

Project ID
Customer ID
Project identification code
Project location
Assigned Sales Associate
Assigned Technician or Technicians
Scope of work
Project status
Creation date
Completion date
Cancellation status
Current MSR version
A customer may have multiple projects. A single MSR exists for each project location and project scope.

7.4 Project-location requirements
Location must store separately:

Street address
City
State
ZIP code
The state determines default license artwork in later document generation.

7.5 Project-code requirements
The system must:

Automatically generate the sequential number
Begin at 101
Include Sales Associate initials
Use the MSR creation date
Format the code as:
text


Sequential Number-Sales Associate Initials-YYYY-MM-DD
Example:

text


101-JD-2026-09-19
Reserve the number during creation
Prevent duplicate codes
Prevent reuse of cancelled codes
Preserve the code through revisions
Warn of an existing active project at the same customer location
Require explanation or correction before another active project at that location is permitted
References: GECC-00 §9; GECC-01 §3.3.

7.6 Search requirements
Authorized searches must support:

Customer name
Phone
Email
Project code
Street address
City
State
ZIP
Sales Associate
Technician
Serial number
Brand
Model
Project status
Document statuses
Payment status
Vendor
Payable
Document type
Date range
Search results must respect record- and field-level permissions.

References: GECC-00 §23; GECC-01 §§5–6; GECC-02 §§10 and 14.

7.7 Duplicate and integrity requirements
The system must prevent or flag:

Duplicate project codes
Duplicate active project locations without explanation
Inconsistent customer/project associations
Invalid project-code formats
Unauthorized access through alternate search paths
All duplicate checks and resulting actions should be auditable.

7.8 GECC-04 dependencies
Sprint 04 depends on:

GECC-01 identity, customer, location, project, assignment, status, and audit-reference structures
GECC-02 authenticated users and record-scope enforcement
GECC-03 audit-event writing
Existing customer/project spreadsheet data for migration
Initial user list and Sales Associate assignments
Sprint 05 depends on the completed customer, project, location, assignment, and project-code foundation.

7.9 GECC-04 unresolved issues
Existing customer and project data
Duplicate-resolution policy for legacy spreadsheet records
Required customer contact rules
Exact definition of an active duplicate location
Project cancellation and reactivation behavior
Assignment and reassignment procedures
Initial migration staging and cleansing rules
Search-index strategy and performance targets
8. GECC-05 — Master Sales Record
8.1 Purpose
The MSR is the authoritative record for all controlled project information. It eliminates repetitive entry and supplies data for documents, payments, costs, commissions, and reporting.

The MSR is the single source of truth for:

Customer and project association
Scope
Equipment
Pricing
Payment option
Costs
Contract data
Invoice data
Certificate data
Approvals
Document versions
Revisions
Profitability-related values
References: GECC-00 §§1, 6.1, 8.5, 10–15; GECC-01 §3.2; baseline Sprint Plan, Sprint 05.

8.2 Required MSR data
The MSR must support:

MSR ID
Project ID
Customer ID
Version number
Creation date
Created-by user
Assigned Sales Associate
Retail price
Payment option
Scope of work
Equipment
Serial numbers
Material costs
Equipment costs
Dealer fees
Lead cost
Technician cost
Damon cost
Jerry cost
Contract data
Invoice data
Certificate data
Approval status
Document-version references
Revision reason
Revision timestamp
Revising user
Reference: GECC-00 §8.5.

8.3 Scope and equipment
The system must support multiple scope selections:

Service
Repair
Install new
Remove old
Install/remove selections may include:

Condenser coil
Evaporator coil
Refrigerant
Air handler
Furnace
Thermostat
Air ducts
Vents
A project may contain multiple equipment records and serial numbers.

Equipment data includes:

Equipment ID
Project ID
Brand
Model
Function
Serial number
Equipment category
New, existing, removed, or installed status
Inventory reference
Notes
References: GECC-00 §§8.6 and 10; GECC-01 §4.3.

8.4 Draft and approval behavior
Sales Associates may create and update unapproved assigned records.
The MSR must be reviewed by an authorized Sales Manager or Comptroller.
Retail price must be entered or confirmed by an authorized approver.
One authorized approval is sufficient.
Approval binds to the exact MSR version.
Approved MSRs become locked.
Unauthorized users cannot edit approved records.
Reopening must be authorized and must create a new version.
The prior approved version remains preserved.
References: GECC-00 §§7.1–7.3, 11; GECC-02 §11; GECC-06 carry-forward requirements.

8.5 Source-of-truth and document consistency
Documents must use the approved current MSR version. Before document generation, the system must validate:

Required fields
Customer consistency
Project consistency
Project-code consistency
Pricing consistency
Scope consistency
Equipment and serial-number requirements
Required approvals
Document-version references
State-specific artwork requirements
Generated documents must not be independently edited to change underlying project data. Corrections must be made through the MSR or authorized template process and then regenerated.

References: GECC-00 §§6.4, 14; GECC-07 §§3, 12, and 16.

8.6 Version and revision behavior
When an approved or signed project changes:

Original MSR remains preserved.
A new MSR version is created.
The new version references the prior version.
Revision reason, user, and timestamp are mandatory.
Affected documents are identified.
Affected documents receive new versions where required.
Original signatures and signature audit records remain attached to original documents.
Revised documents enter a new approval/signature sequence where required.
Current and superseded versions are clearly identified.
Scope changes are especially controlled:

Technician records proposed change.
Project enters scope-change-pending state.
Sales Manager or Comptroller reviews.
Change is approved or rejected.
Revised price and costs are entered or approved.
New MSR version is created.
Revised documents are generated.
New signature sequence is completed before changed work continues.
References: GECC-00 §§12–13; GECC-01 §§3.4; GECC-06 scope-change requirements.

8.7 MSR relationships
The MSR must link to:

Customer
Project
Project location
Sales Associate
Assigned technicians
Scope selections
Equipment and serial numbers
Pricing
Cost components
Payment option
Invoice
Contract
Certificate
Workflow state and tasks
Approvals
Document versions
Signature requests
Payments and adjustments
Commission calculations
Audit references
8.8 MSR acceptance requirements
The implementation must prove that:

A Sales Associate can create a draft MSR for an authorized project.
Required customer, project, scope, pricing, and payment data is validated.
An authorized approver can approve the exact version.
Approval locks the version.
Unauthorized edits are rejected.
Reopening creates a new version.
Original versions remain unchanged.
Scope and equipment can be represented without duplicate or inconsistent data.
Documents can reference the correct MSR version.
Project code remains unchanged through revisions.
All creation, edits, approvals, reopening, and revision actions are audited.
Superseded versions cannot receive new approvals or generate current controlled documents.
8.9 GECC-05 dependencies
GECC-05 depends on:

GECC-01: Customer, project, MSR, equipment, cost, document, workflow, approval, version, and audit-reference structures
GECC-02: Sales Associate, Sales Manager, Comptroller, Technician, and record/field/action permissions
GECC-03: Audit events for creation, edits, approval, reopening, revision, and access
GECC-04: Customer, contact, location, assignment, project code, and duplicate-detection services
Later sprints: Document generation, electronic signatures, payments, commissions, and financial reporting
8.10 GECC-05 unresolved issues
Exact form layout and user-interface behavior
Final list of required versus optional MSR fields
Rules for incomplete or unknown equipment serial numbers
Cost-entry timing and ownership across Sales Manager, Installation Manager, Comptroller, and Technician
Exact approval task routing
Which changes require revised pricing, costs, documents, or signatures
Draft expiration and cancellation behavior
Reopening authority and emergency-revision procedure
Migration of legacy project data into MSR structures
Historical calculation snapshots for profitability and commissions
Treatment of MSR changes after partial payment or signature
Exact relationship between MSR approval and document-generation tasks
9. Cross-Section Dependencies
Dependency	Required before	Purpose
PostgreSQL schema and identifiers	GECC-02 onward	Persistent entities and relationships
User, role, permission model	Restricted operations	Authorization enforcement
Customer/project/location records	GECC-05	MSR ownership and project identity
Project-code generator	GECC-05 and documents	Stable project and invoice identity
Audit-event reference fields	GECC-02 and later	Link business actions to audit history
Full audit writer	Approvals, revisions, exports, signatures	Required auditable operations
MSR versioning	Documents, signatures, payments, commissions	Source-data consistency
Effective-dated configuration	Permissions, artwork, templates, accounting	Historical reproducibility
Transactional saves	All multi-record operations	Prevent partial business state
Search indexes and permission filters	Dashboard and search	Secure retrieval
Migration staging	Customer/project/MSR operation	Legacy spreadsheet conversion
10. Consolidated Unresolved Issues
The following remain implementation or configuration questions unless later approved decisions supersede them:

Final chart of accounts and accounting-period design.
Storage and backup technology for documents and audit archives.
Exact contract, certificate, and invoice templates.
PDF rendering or overlay technology.
Signature-field coordinates and document wording.
Customer authentication and internal MFA requirements.
Email provider, recipient rules, and delivery handling.
Password, lockout, timeout, emergency-access, and temporary-access policies.
Exact row-level-security implementation.
Initial user, role, assignment, vendor, and customer/project migration data.
Duplicate-resolution policy for legacy records.
Required customer-contact and equipment-serial-number rules.
Cost-entry authority and timing.
MSR draft, reopening, cancellation, and revision rules.
Scope-change categories requiring revised price, cost, documents, or signatures.
Audit archive technology, verification operations, and superuser controls.
Historical storage of calculated financial and commission values.
Final treatment of accounts payable and cash-basis reporting.
Asset useful lives, depreciation edge cases, and loan accounting rules.
Financial-period close, retained earnings, and adjusting-entry behavior.
11. Handoff Status
At the end of GECC-05:

The database foundation is defined.
Authentication and authorization architecture is defined.
Audit logging architecture is defined.
Customer, location, project, and project-code foundations are defined.
The MSR is established as the central source of truth.
The project is ready to proceed to controlled workflow implementation and subsequent document generation.
The next major dependency is GECC-06 — Controlled Lifecycle and Task Workflow, which must enforce approvals, signatures, installation timing, scope changes, authorized reopening, and completion gates using the structures defined in Sections 1–5.
