# GECC-03-audit-log.md

# GECC Sales Back Office System — Sprint 03 Tamper-Evident Audit Logging

## Bundle Metadata

- Bundle name: GECC-03-audit-log.md
- Project: GECC Sales Back Office System
- Company: Go Ecco Climate Control
- Company abbreviation: GECC
- Bundle type: Sprint 03 design and implementation bundle
- Status: Approved design; ready for implementation
- Created date: 2026-09-20
- Previous bundle: GECC-02-auth-and-permissions.md
- Next planned bundle: GECC-04-customers-and-projects.md
- Database: PostgreSQL
- Primary-key strategy: Numeric BIGINT identity keys
- Business timezone: America/New_York
- Audit retention: Permanent
- Audit-log model: Append-only, chained, tamper-evident

---

## 1. Sprint Objective

Implement the tamper-evident audit-log foundation required to preserve a complete history of:

- User access
- Login and logout activity
- Searches
- Record views
- Record creation
- Record edits
- Approvals
- Reopenings
- Revisions
- Exports
- Prints
- Downloads
- Emails
- Signatures
- Payment activity
- Permission failures
- Restricted-access attempts
- Administrative changes
- Audit-log access
- Audit-integrity verification

The audit log must be separate from ordinary business records and protected from ordinary application edits and deletion.

---

## 2. Sprint 03 Deliverables

Sprint 03 must produce:

1. Append-only audit-event model
2. Audit-event database table
3. Hash-chain design
4. Audit-event canonicalization rules
5. Access-event logging
6. Restricted-access logging
7. Authentication-event integration
8. Permission-failure integration
9. Business-action logging
10. Administrative-change logging
11. Audit-log search design
12. Audit-log export design
13. Integrity-verification process
14. Archive and retention process
15. Audit-log access permissions
16. Audit-log monitoring and exception behavior
17. Audit-log acceptance tests
18. Sprint 04 integration handoff

---

## 3. Audit Principles

The audit system must follow these principles:

- Audit records are separate from ordinary business records.
- Audit events are append-only.
- Ordinary users cannot edit or delete audit events.
- Audit events remain permanently retained.
- Every event references the preceding event.
- An alteration, deletion, insertion, or reordering must cause integrity verification failure.
- Audit events record both successful and failed actions.
- Restricted-access attempts are logged even when access is denied.
- Audit-log access is itself audited.
- Audit exports are audited.
- Audit verification results are audited.
- Historical records remain searchable and reproducible.
- Audit timestamps use `America/New_York` for business interpretation while preserving the actual event instant.

---

## 4. Audit Access Rules

Audit-log access is limited to:

- Sales Manager
- Comptroller
- Installation Manager

The following users do not receive audit-log access by default:

- Sales Associate
- Technician
- Accounts Payable Associate
- Other Contractor
- Database Administrator

The Database Administrator may maintain approved database infrastructure but may not automatically view business audit content through the application.

Audit-log permissions include:

```text
audit\_log.view
audit\_log.search
audit\_log.export
audit\_log.verify
audit\_log.view\_integrity\_failure
audit\_log.view\_archived
Audit-log access must be separately authorized from general database access.

5. Audit Event Data Model
5.1 audit_event
Field	Type	Requirement
id	BIGINT	Primary key
event_sequence_number	BIGINT	Required, unique
event_uuid	UUID	Required, unique event reference
occurred_at	TIMESTAMPTZ	Required
recorded_at	TIMESTAMPTZ	Required
user_id	BIGINT	Optional foreign key
user_role_snapshot	VARCHAR(100)	Required when user identified
session_id	BIGINT	Optional foreign key
event_type	VARCHAR(80)	Required
activity_type	VARCHAR(80)	Required
resource_type	VARCHAR(100)	Optional
resource_id	BIGINT	Optional
resource_business_key	VARCHAR(250)	Optional
related_project_id	BIGINT	Optional foreign key
related_customer_id	BIGINT	Optional foreign key
outcome	VARCHAR(30)	Success, failure, denied, error
failure_reason	TEXT	Required for failures where available
reason_text	TEXT	Required when business rules require a reason
request_reference	VARCHAR(200)	Optional
source_screen	VARCHAR(150)	Optional
source_operation	VARCHAR(150)	Optional
event_payload	JSONB	Required, canonicalized
previous_event_hash	VARCHAR(128)	Required except genesis event
event_hash	VARCHAR(128)	Required
hash_algorithm	VARCHAR(30)	Required
integrity_status	VARCHAR(30)	Valid, unverified, failed
archive_status	VARCHAR(30)	Hot, archived, restored
archive_segment_id	BIGINT	Optional
created_at	TIMESTAMPTZ	Required
5.2 Audit event sequence
event_sequence_number is assigned monotonically.

Requirements:

Sequence numbers cannot be reused.
Sequence numbers cannot be changed.
The next event references the hash of the immediately preceding event.
A missing sequence number is an integrity exception.
A duplicate sequence number is an integrity exception.
Out-of-order events are an integrity exception.
Concurrent transactions must serialize event-chain assignment safely.
5.3 Genesis event
The first event in the audit chain is a genesis event.

The genesis event contains:

Fixed sequence number
Fixed event type
Project identifier
Creation timestamp
Initial hash seed
Hash algorithm
System initialization metadata
The genesis event must be preserved permanently.

6. Hash-Chain Design
6.1 Hash algorithm
The initial implementation uses:

text


SHA-256
The hash_algorithm field must be stored with each event to support future algorithm migration.

6.2 Canonical event representation
The event hash is calculated from a canonical representation containing:

text


event\_sequence\_number
event\_uuid
occurred\_at
recorded\_at
user\_id
user\_role\_snapshot
session\_id
event\_type
activity\_type
resource\_type
resource\_id
resource\_business\_key
related\_project\_id
related\_customer\_id
outcome
failure\_reason
reason\_text
request\_reference
source\_screen
source\_operation
canonical\_event\_payload
previous\_event\_hash
Canonicalization requirements:

Fields are serialized in a fixed order.
Property names are normalized.
Null values are represented consistently.
Numeric values use fixed decimal formatting where applicable.
Timestamps use a consistent ISO-8601 representation.
Text encoding is UTF-8.
JSON object properties are sorted.
Arrays preserve their defined order.
Whitespace outside values is excluded.
The canonical payload is immutable after insertion.
6.3 Event-hash calculation
Conceptually:

text


event\_hash =
SHA-256(
  canonical\_event\_representation
)
The hash must include the previous event hash.

This creates a chain:

text


Event 1 → Event 2 → Event 3 → Event 4
If Event 2 is modified, removed, inserted, or reordered, later chain verification must fail.

6.4 Concurrent event creation
Audit-event creation must use a serialized sequence-allocation mechanism.

The implementation must ensure that:

Two concurrent events cannot receive the same sequence number.
Each event references the actual previous event.
A failed business transaction does not leave an invalid chain entry.
An audit event for a committed business action is committed with the action.
A failed or denied action may be logged in its own successful audit transaction when the original transaction is rolled back.
7. Event Categories
7.1 Authentication events
Login success
Login failure
Logout
Session expiration
Session revocation
Account lockout
Account activation
Account deactivation
Password or authentication-factor change
7.2 Access events
Dashboard view
Database view
Customer-record view
Project-record view
Master Sales Record view
Document view
Financial-report view
Commission-report view
Administrative-configuration view
Search
Export
Print
Download
7.3 Business-record events
Customer created
Customer edited
Contact created
Contact edited
Project created
Project edited
Master Sales Record created
Master Sales Record saved as draft
Master Sales Record approved
Master Sales Record reopened
Master Sales Record revised
Equipment added
Serial number updated
Cost entered
Cost changed
Project status changed
Task created
Task completed
Project cancelled
Project completed
7.4 Document events
Document generated
Document regenerated
Document saved
Document printed
Document emailed
Document downloaded
Document sent for signature
Signature request created
Signature request reminded
Signature request expired
Document signed
Document superseded
Document cancelled
7.5 Payment and accounting events
Payment entered
Payment proof uploaded
Cashier’s check verified
Bank wire verified
Payment rejected
Adjustment created
Adjustment approved
Refund approved
Credit approved
Accounts payable created
Payable payment entered
Financial period opened
Financial period published
Financial period locked
Adjustment entered into locked period
Journal entry posted
Journal entry reversed
7.6 Commission events
Commission calculated
Commission earned
Commission report created
Commission report approved
Commission report emailed
Commission adjustment created
Commission adjustment approved
Annual commission summary generated
Annual commission summary emailed
7.7 Security events
Permission granted
Permission revoked
Record scope granted
Record scope revoked
Permission failure
Restricted-access attempt
Unauthorized export attempt
Unauthorized download attempt
Administrative configuration changed
Audit verification started
Audit verification completed
Audit integrity failure detected
8. Event Payload Rules
The event payload must contain enough information to reconstruct what occurred without exposing unnecessary restricted data.

8.1 Permitted payload content
Examples:

Action performed
Prior status
New status
Record version
Approval reason
Revision reason
Adjustment reason
Signature role
Payment verification method
Document version
Artwork version
Permission evaluated
Authorization result
Search criteria category
Export type
Integrity-verification result
8.2 Restricted payload content
Sensitive data must be minimized.

The audit payload should not store:

Passwords
Authentication secrets
Session tokens
Private signing keys
Full payment credentials
Unnecessary personal information
Unnecessary document contents
Where a sensitive value must be referenced, store:

A masked value
A secure reference
A content hash
The related record identifier
8.3 Before-and-after values
For material changes, the event may store before-and-after values for approved fields.

Examples:

json


{
  "field": "project\_status",
  "before": "contract\_pending",
  "after": "installation\_ready"
}
For sensitive fields, store only:

Field name
Change indicator
Authorized reference
Hash of the prior and new values where necessary
9. Audit Logging Requirements by Operation
9.1 Reads and views
The system must log:

User
Role
Screen or resource
Record identifier where applicable
Timestamp
Success or failure
Session identifier
9.2 Searches
The system must log:

User
Role
Search operation
Search-resource type
Result count
Search timestamp
Whether restricted results were requested
Whether the search succeeded or failed
Search values should be minimized or masked where they contain personal information.

9.3 Creates and edits
The system must log:

Record type
Record identifier
User
Time
Operation
Version
Relevant changed fields
Result
Failure reason if applicable
9.4 Approvals
The system must log:

Approving user
Role
Exact record or document version
Approval type
Approval timestamp
Approval result
Approval reason where required
Related task or workflow state
9.5 Exports, prints, emails, and downloads
The system must log:

User
Resource type
Resource identifier
Output type
Recipient or destination where applicable
Timestamp
Success or failure
Applied authorization scope
File or document version
9.6 Permission failures
The system must log:

User identity if known
Requested resource
Requested action
Record identifier if known
User role
Session identifier
Failure reason
Timestamp
Request reference
10. Audit-Log Storage and Retention
Audit records are retained permanently.

10.1 Hot storage
Recent audit events remain in searchable PostgreSQL partitions.

Partitions should generally be organized by month using recorded_at.

Hot indexes must support:

Timestamp
User
Role
Event type
Activity type
Resource type
Resource ID
Project
Customer
Outcome
Session
Integrity status
10.2 Immutable archive
Older audit partitions may be moved to immutable archive storage.

Archive requirements:

Write-once or retention-locked storage
Permanent retention
Archive segment identifier
Segment start and end sequence numbers
Segment start and end hashes
Segment checksum
Export timestamp
Exporting process identifier
Verification status
Archiving must not change event content or sequence order.

10.3 Archive retrieval
Authorized users may request archived audit records.

Archive retrieval must:

Preserve original event content
Verify the archive segment
Verify chain continuity
Create an audit event for the retrieval
Restrict access according to Sprint 02 permissions
11. Integrity Verification
11.1 Verification scopes
The system must support verification of:

Entire audit chain
Date range
Sequence range
Archive segment
Resource-specific history
Project-specific history
User-specific history
11.2 Verification checks
The verification process must check:

Sequence continuity
Event-hash correctness
Previous-event-hash references
Genesis-event validity
Duplicate events
Missing events
Out-of-order events
Archive-segment boundaries
Archive checksums
Hash-algorithm compatibility
Canonical-payload consistency
11.3 Verification outcomes
Possible results:

Valid
Valid with archived segments
Incomplete
Failed
Unable to verify
Requires restoration
11.4 Integrity failure response
If verification fails:

Mark the affected event or segment as integrity_failure.
Create a high-priority administrative exception.
Notify Sales Manager, Comptroller, and Installation Manager.
Preserve the failed verification result.
Prevent silent repair or deletion.
Require documented investigation.
Record all investigation activity.
Preserve the original evidence.
Permit a separately authorized continuity or recovery process.
Do not rewrite historical audit events.
12. Audit Search Requirements
Authorized users must be able to search by:

Date range
User
Role
Session
Event type
Activity type
Resource type
Resource identifier
Project identification code
Customer
Outcome
Success or failure
Permission failure
Document version
Master Sales Record version
Payment
Commission report
Financial period
Integrity status
Archive status
Search results must display:

Event sequence number
Event timestamp
User
Role
Event type
Activity
Resource
Outcome
Integrity status
Archive status
Sensitive payload details require additional permission.

13. Audit Export Requirements
Authorized audit users may export audit records.

Exports must:

Apply the same access rules as search.
Include event sequence numbers.
Include event hashes.
Include previous-event hashes.
Include archive and integrity status.
Include export timestamp.
Include exporting user.
Include export criteria.
Include a content hash of the exported file.
Be recorded as audit events.
Preserve the original database records.
The export format should support:

CSV for structured review
JSON for machine processing
PDF or printable report for formal review
The final export format may be implemented in Sprint 14 if not completed earlier.

14. Database Protection Requirements
The audit-event table must be protected by:

Application-level denial of update and delete operations
Database permissions that deny ordinary application users update and delete access
Controlled insert access through an audit-writing function or service
Restricted direct database access
Trigger or procedure protection where appropriate
Separate audit schema or database role
Integrity-verification process
Backup and archive protection
The Database Administrator must not be able to alter audit history through ordinary application functions.

Database-level superuser access remains an operational risk requiring infrastructure controls, separation of duties, and administrative monitoring.

15. Audit-Event Write Behavior
Committed business operations
For a successful business operation:

Validate authorization.
Execute the business operation.
Generate the audit event.
Calculate the event hash.
Commit the business operation and audit event transactionally.
Failed or denied operations
For a denied or failed operation:

Reject or roll back the business operation.
Record the failure in a separate audit transaction.
Preserve the failure reason.
Associate the event with the session and request reference.
System failures
If audit-event creation fails during a required auditable business operation:

The business operation must fail closed unless an approved emergency mode exists.
The failure must be surfaced to the user.
The failure must create an operational alert.
No business operation may succeed silently without the required audit event.
16. Sprint 03 Decisions
D-03-001 — Audit model
Use a separate append-only audit-event store linked to business records by identifiers and references.

D-03-002 — Hash algorithm
Use SHA-256 for the initial audit-event hash chain.

D-03-003 — Hash-chain structure
Each event includes the hash of the immediately preceding event.

D-03-004 — Permanent retention
Audit records are retained permanently.

Older records may move to immutable archive storage but may not be routinely deleted.

D-03-005 — Audit access
Only Sales Manager, Comptroller, and Installation Manager receive audit-log access through the application.

D-03-006 — Denied-action logging
Permission failures and restricted-access attempts are logged even when the requested operation is denied.

D-03-007 — Audit access auditing
Viewing, searching, exporting, verifying, and retrieving audit records are themselves audited.

D-03-008 — Fail-closed behavior
A required business operation must not succeed if its required audit event cannot be created, except through a separately approved emergency procedure.

D-03-009 — Sensitive-data minimization
Audit payloads must contain enough information to reconstruct activity without storing unnecessary secrets or sensitive values.

D-03-010 — Historical immutability
Audit records cannot be repaired by editing historical rows. Corrections are recorded as new events.

17. Sprint 03 Acceptance Criteria
Sprint 03 is accepted when:

Audit records are stored separately from ordinary business records.
Audit events are append-only.
Ordinary users cannot edit or delete audit events.
Every event has a unique sequence number.
Every event has a unique event identifier.
Every event references the previous event hash except the genesis event.
Event hashes use SHA-256.
Canonical event serialization is defined and deterministic.
A changed event causes chain verification failure.
A deleted event causes chain verification failure.
An inserted event causes chain verification failure.
A reordered event causes chain verification failure.
Duplicate sequence numbers cause verification failure.
Missing sequence numbers cause verification failure.
Successful logins are logged.
Failed login attempts are logged.
Logout is logged.
Views are logged.
Searches are logged.
Creates are logged.
Edits are logged.
Approvals are logged.
Exports are logged.
Prints are logged.
Downloads are logged.
Emails are logged.
Signatures are logged.
Permission failures are logged.
Restricted-access attempts are logged.
Administrative changes are logged.
Audit-log access is restricted by role.
Audit-log searches are permission-controlled.
Audit-log exports are permission-controlled.
Audit-log verification results are preserved.
Audit records are permanently retained.
Archive segments include integrity metadata.
Archived records can be restored and verified.
Audit failures create administrative exceptions.
Required business operations fail closed if required audit logging fails.
Audit payloads do not contain passwords, session tokens, or private authentication secrets.
Audit events reference the relevant project, customer, document, payment, report, or financial period where applicable.
18. Sprint 03 Test Cases
Hash-chain tests
Create a genesis event.
Append a valid event.
Verify the complete chain.
Modify an event and confirm verification failure.
Remove an event and confirm verification failure.
Insert an event and confirm verification failure.
Reorder events and confirm verification failure.
Duplicate a sequence number and confirm verification failure.
Use an invalid previous-event hash and confirm verification failure.
Access-event tests
Successful login creates an audit event.
Failed login creates an audit event.
Logout creates an audit event.
Authorized record view creates an audit event.
Unauthorized record view creates a denied-access event.
Search creates an audit event.
Export creates an audit event.
Print creates an audit event.
Download creates an audit event.
Business-event tests
Customer creation is logged.
Project creation is logged.
Master Sales Record approval is logged.
Master Sales Record reopening is logged.
Version creation is logged.
Document generation is logged.
Signature completion is logged.
Payment verification is logged.
Adjustment approval is logged.
Commission-report approval is logged.
Financial-period locking is logged.
Protection tests
Ordinary application user cannot update an audit event.
Ordinary application user cannot delete an audit event.
Unauthorized role cannot search audit records.
Unauthorized role cannot export audit records.
Database Administrator cannot use ordinary application functions to alter audit history.
Audit-log access is itself audited.
Failed audit insertion prevents the required business action from succeeding.
Retention tests
Audit records remain available after business-record retirement.
Archived records retain their sequence range.
Archive checksums verify successfully.
Archive retrieval creates an audit event.
Chain verification works across hot and archived segments.
19. Sprint 03 Open Questions
These implementation questions do not change approved requirements:

Will the audit log reside in a separate PostgreSQL schema or a separate database?
What exact archive-storage technology will provide immutable retention locking?
What hot-storage period will be used before archival?
What audit verification schedule will be used?
What notification mechanism will report integrity failures?
What emergency audit mode, if any, will be permitted?
What infrastructure-level controls will govern PostgreSQL superuser access?
What exact export formats are required for formal compliance reviews?
What maximum payload size is permitted for an audit event?
What operational process will approve archive restoration?
What recovery-point and recovery-time objectives apply to audit data?
20. Sprint 04 Handoff
Sprint 04 will implement customers, contacts, locations, projects, duplicate detection, project-code generation, and search.

Sprint 04 must create audit events for:

Customer creation
Customer edits
Contact creation and edits
Location creation and edits
Project creation
Project-code reservation
Duplicate-location warnings
Duplicate-location overrides
Project assignment changes
Project cancellation
Project searches
Restricted project-access attempts
Sprint 04 must preserve all audit-event references defined in this bundle.
21. Historical Record
Supersedes
None.

Preserves
GECC-00-project-baseline.md
GECC-01-data-model.md
GECC-02-auth-and-permissions.md
Sprint 00 decisions
Sprint 01 decisions
Sprint 02 decisions
Approved role and permission restrictions
Permanent audit-retention requirement
Changes introduced by this bundle
Defines the Sprint 03 append-only audit-event model.
Defines SHA-256 hash chaining.
Defines canonical event serialization.
Defines audit-event categories and required payloads.
Defines audit search and export behavior.
Defines permanent retention and immutable archival.
Defines integrity verification and failure handling.
Defines Sprint 04 audit integration requirements.