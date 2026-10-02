# GECC-06-workflow-state-machine.md

# GECC Sales Back Office System — Sprint 06 Controlled Lifecycle and Task Workflow

## Bundle Metadata

- Bundle name: GECC-06-workflow-state-machine.md
- Project: GECC Sales Back Office System
- Company: Go Ecco Climate Control
- Company abbreviation: GECC
- Bundle type: Sprint 06 design and implementation bundle
- Status: Approved design; ready for implementation
- Created date: 2026-09-20
- Previous bundle: GECC-05-master-sales-record.md
- Next planned bundle: GECC-07-document-generation.md
- Database: PostgreSQL
- Primary-key strategy: Numeric BIGINT identity keys
- Business timezone: America/New_York
- System of record: PostgreSQL relational database
- Audit model: Append-only, hash-chained, permanently retained

---

## 1. Sprint Objective

Implement the controlled project lifecycle and task workflow required to prevent users from:

- Skipping required steps
- Performing actions out of order
- Installing before the required waiting period
- Changing approved records without authorization
- Continuing changed-scope work before approval and signatures
- Marking projects complete before all required conditions are satisfied

The system must create, assign, track, complete, and audit workflow tasks.

---

## 2. Sprint 06 Deliverables

Sprint 06 must produce:

1. Project state machine
2. Master Sales Record state transitions
3. Document and signature dependency checks
4. Installation task workflow
5. Three-day installation restriction
6. Waiting-period calculation
7. Scope-change workflow
8. Authorized reopening workflow
9. Version-creation workflow
10. Approval and correction tasks
11. Task assignment and due dates
12. Exception and override handling
13. State-transition audit events
14. Workflow dashboard data
15. Workflow acceptance tests
16. Sprint 07 document-generation handoff

---

## 3. Lifecycle Principles

The workflow must preserve these approved requirements:

- Users cannot skip required steps.
- Approved records are locked.
- Authorized reopening creates a new version.
- Original versions remain preserved.
- Documents use the approved Master Sales Record.
- The customer signs the contract before GECC signs it.
- The GECC signer signs the invoice before the customer signs it.
- Installation cannot occur earlier than three days after the customer signs both the contract and invoice.
- The certificate is signed by the customer before the Installation Manager signs it.
- The Comptroller may sign the certificate only when the Installation Manager is unavailable and must record the reason.
- Payment verification is required before project completion.
- Scope changes require review, approval, revised pricing and costs where applicable, a new Master Sales Record version, revised documents, and a new signature sequence.
- Original documents and records remain permanently preserved.

---

## 4. Project State Machine

### 4.1 State list

The initial project lifecycle uses these states:

```text
draft
sales\_record\_pending\_review
sales\_record\_rejected
sales\_record\_approved
contract\_pending\_customer\_signature
contract\_pending\_gecc\_signature
contract\_signed
invoice\_pending\_gecc\_signature
invoice\_pending\_customer\_signature
invoice\_signed
installation\_waiting\_period
installation\_scheduling\_required
installation\_scheduled
installation\_in\_progress
installation\_completed
certificate\_pending\_customer\_signature
certificate\_pending\_gecc\_signature
certificate\_signed
payment\_pending
payment\_verification\_pending
adjustment\_pending\_approval
project\_ready\_for\_completion
completed
scope\_change\_pending
scope\_change\_approved
scope\_change\_rejected
revision\_pending\_approval
cancelled
exception\_review
Some document-specific states may be stored on document records while the project maintains the overall business state.

4.2 State descriptions
State	Meaning
draft	Project or Master Sales Record is being prepared
sales_record_pending_review	Record is submitted for authorized review
sales_record_rejected	Record was rejected or returned for correction
sales_record_approved	Master Sales Record is approved and locked
contract_pending_customer_signature	Contract is awaiting customer signature
contract_pending_gecc_signature	Contract is awaiting GECC signature
contract_signed	Contract has all required signatures
invoice_pending_gecc_signature	Invoice is awaiting GECC signature
invoice_pending_customer_signature	Invoice is awaiting customer signature
invoice_signed	Invoice has all required signatures
installation_waiting_period	Required three-day waiting period is active
installation_scheduling_required	Installation Manager scheduling task is open
installation_scheduled	Installation date has been assigned
installation_in_progress	Work has started
installation_completed	Technician has recorded completion
certificate_pending_customer_signature	Certificate is awaiting customer signature
certificate_pending_gecc_signature	Certificate is awaiting GECC signature
certificate_signed	Certificate has all required signatures
payment_pending	Payment has not yet been recorded
payment_verification_pending	Payment is recorded but not verified
adjustment_pending_approval	Credit, refund, or adjustment requires approval
project_ready_for_completion	All completion prerequisites are satisfied
completed	Project is complete and eligible for commissions
scope_change_pending	Technician has proposed a scope change
scope_change_approved	Scope change has been approved and revision work may proceed
scope_change_rejected	Scope change was rejected
revision_pending_approval	Revised Master Sales Record awaits approval
cancelled	Project is cancelled and cannot reuse its project code
exception_review	Authorized exception review is active
5. Allowed State Transitions
5.1 Initial sales process
text


draft
  → sales\_record\_pending\_review

sales\_record\_pending\_review
  → sales\_record\_approved
  → sales\_record\_rejected

sales\_record\_rejected
  → draft
  → cancelled

sales\_record\_approved
  → contract\_pending\_customer\_signature
5.2 Contract process
text


contract\_pending\_customer\_signature
  → contract\_pending\_gecc\_signature
  → contract\_pending\_customer\_signature
  → cancelled

contract\_pending\_gecc\_signature
  → contract\_signed
  → contract\_pending\_customer\_signature
  → cancelled
The customer must sign the contract before the GECC signer signs it.

5.3 Invoice process
text


contract\_signed
  → invoice\_pending\_gecc\_signature

invoice\_pending\_gecc\_signature
  → invoice\_pending\_customer\_signature
  → cancelled

invoice\_pending\_customer\_signature
  → invoice\_signed
  → invoice\_pending\_gecc\_signature
  → cancelled
The GECC signer must sign the invoice before the customer signs it.

5.4 Installation process
text


invoice\_signed
  → installation\_waiting\_period

installation\_waiting\_period
  → installation\_scheduling\_required
  → exception\_review

installation\_scheduling\_required
  → installation\_scheduled

installation\_scheduled
  → installation\_in\_progress
  → installation\_scheduling\_required

installation\_in\_progress
  → installation\_completed
  → scope\_change\_pending
5.5 Certificate process
text


installation\_completed
  → certificate\_pending\_customer\_signature

certificate\_pending\_customer\_signature
  → certificate\_pending\_gecc\_signature

certificate\_pending\_gecc\_signature
  → certificate\_signed
The customer must sign the certificate before the authorized GECC signer signs it.

5.6 Payment and completion process
text


certificate\_signed
  → payment\_pending
  → payment\_verification\_pending

payment\_pending
  → payment\_verification\_pending

payment\_verification\_pending
  → adjustment\_pending\_approval
  → project\_ready\_for\_completion

adjustment\_pending\_approval
  → payment\_verification\_pending
  → project\_ready\_for\_completion
A project may enter project_ready_for_completion only when all completion conditions are satisfied.

text


project\_ready\_for\_completion
  → completed
5.7 Scope-change process
text


installation\_in\_progress
  → scope\_change\_pending

scope\_change\_pending
  → scope\_change\_approved
  → scope\_change\_rejected

scope\_change\_rejected
  → installation\_in\_progress

scope\_change\_approved
  → revision\_pending\_approval

revision\_pending\_approval
  → sales\_record\_approved
  → sales\_record\_rejected
After the revised Master Sales Record is approved, revised documents must enter a new required signature sequence.

5.8 Reopening process
An approved or signed project may be reopened only by an authorized user.

text


approved\_or\_signed\_state
  → exception\_review
  → revision\_pending\_approval
The reopening action must:

Create a new Master Sales Record version.
Preserve the previous version.
Record a reason.
Record the authorized user.
Identify affected documents.
Identify whether prior signatures are invalidated for the revised scope.
Restart affected approval and signature tasks.
6. Transition Guards
Every state transition must evaluate:

Current state
Requested target state
User identity
User role
Record scope
Required permissions
Record version
Required fields
Required approvals
Required signatures
Required payment conditions
Required task completion
Waiting-period calculation
Exception status
Cancellation status
Scope-change status
Audit-event availability
A transition is rejected if any guard fails.

7. Master Sales Record Workflow
7.1 Create
A Sales Associate creates a draft Master Sales Record for an assigned project.

Required:

Customer
Project
Project location
Project code
Sales Associate
Scope
Payment option
Applicable equipment
Proposed retail price or pricing-pending status
Applicable cost information
7.2 Submit
The Sales Associate submits the record for review.

The system validates:

Required fields
Customer consistency
Project consistency
Project-code consistency
Scope consistency
Equipment requirements
Pricing
Costs
Payment option
Duplicate warnings
Required explanations
7.3 Approve
A Sales Manager or Comptroller may approve the submitted version.

Approval:

Locks the version.
Records the approver.
Records the approval time.
Identifies the exact version.
Creates the approval audit event.
Enables later document generation.
7.4 Reject or return
An authorized approver may reject or return the record for correction.

The system must require:

Rejection or correction reason
Approver identity
Date and time
Affected version
Audit event
8. Contract and Invoice Dependencies
8.1 Contract dependency
The contract cannot proceed to GECC signature until:

The customer has signed and dated the contract.
The customer signature is valid.
The contract version references the approved Master Sales Record.
The contract has not expired or been superseded.
8.2 Invoice dependency
The invoice cannot proceed to customer signature until:

The contract has all required signatures.
A GECC authorized signer has signed and dated the invoice.
The invoice references the approved Master Sales Record.
The invoice version is current.
The invoice has not expired or been superseded.
8.3 Installation dependency
An Installation Manager scheduling task cannot be created until:

Contract has all required signatures.
Invoice has all required signatures.
The relevant document versions are current.
No unresolved scope change blocks installation.
No cancellation is active.
9. Three-Day Installation Rule
9.1 Rule
Installation may not occur earlier than three days after the customer signs both:

The contract
The invoice
The waiting period begins at the later of the two customer-signature timestamps.

Conceptually:

text


installation\_eligible\_at =
later(contract\_customer\_signed\_at, invoice\_customer\_signed\_at)
+ 3 calendar days
9.2 Enforcement
The system must:

Calculate the earliest permitted installation date.
Display the date to authorized users.
Prevent scheduling before eligibility.
Prevent recording installation start before eligibility.
Prevent recording installation completion before eligibility.
Display a warning when a proposed date violates the rule.
Create an audit event for rejected early-installation attempts.
9.3 Timezone
The calculation uses:

text


America/New\_York
The system must preserve the underlying timestamp and display the business-date interpretation in Eastern time.

9.4 Exception process
The initial workflow blocks early installation.

If an authorized exception process is implemented later:

The project enters exception_review.
The exception requires an authorized approver.
The exception requires a reason.
The exception requires a date and time.
The exception does not alter the original signature timestamps.
The exception is permanently audited.
The authorized exception must be explicitly linked to the installation event.
No ordinary user may bypass the three-day rule.

10. Installation Task Workflow
10.1 Installation scheduling task
The system creates an Installation Manager task after:

Contract is fully signed.
Invoice is fully signed.
The waiting-period requirements are satisfied or an approved exception exists.
The task includes:

Project code
Customer
Project location
Assigned Sales Associate
Assigned Technician or Technicians
Approved scope
Earliest legal installation date
Current project status
Required next action
Due date
Warning indicator
10.2 Scheduling
The Installation Manager may:

Review the task
Assign or confirm technician assignment
Enter the installation date
Confirm that the date is permitted
Record scheduling notes
Reschedule before work begins
The system must reject an installation date earlier than the permitted date.

10.3 Installation start
Installation start requires:

Assigned Technician
Approved project scope
Approved current Master Sales Record
Valid installation date
No blocking scope change
Waiting period satisfied
10.4 Installation completion
The Technician may record:

Installation completion
Equipment serial numbers
Actual materials used
Completion notes
Proposed scope changes
The Technician may not:

Change retail price
Change approved costs
Change contract terms
Approve the project
Bypass required scope-change review
11. Scope-Change Workflow
11.1 Proposal
If a Technician identifies a scope change:

Technician records the proposed change.
Technician enters notes and supporting details.
The project enters scope_change_pending.
Work on the changed scope is blocked.
An approval task is assigned to the Sales Manager or Comptroller.
11.2 Review
The authorized reviewer evaluates:

Proposed scope
Reason
Equipment changes
Serial-number implications
Retail-price changes
Cost changes
Contract impact
Invoice impact
Certificate impact
Installation impact
11.3 Approval
If approved:

The scope change becomes authorized for revision.
Revised price and costs are entered or approved.
A new Master Sales Record version is created.
The original version remains preserved.
Revised documents are generated later.
A new approval and signature sequence is required.
Work cannot continue until required approvals and signatures are complete.
11.4 Rejection
If rejected:

The rejection reason is recorded.
The original approved scope remains controlling.
Work may resume only within the approved scope.
The rejected proposal remains permanently audited.
11.5 Scope-change version rules
A scope change must not:

Edit the original approved version.
Delete original documents.
Remove original signatures.
Change the original project code.
Treat the proposed change as approved before authorization.
12. Reopen and Revision Workflow
12.1 Authorized reopening
Reopening is permitted only to authorized users.

The reopening request must include:

Project
Current Master Sales Record version
Reason
Requested changes
Requesting user
Date and time
Approval or authorization
12.2 Revision creation
The system creates a new draft version by copying the prior version's controlled data.

The new version must:

Receive the next version number.
Reference the prior version.
Include a revision reason.
Remain separate from the original.
Require approval before becoming current.
Identify affected documents and signatures.
12.3 Effect on documents
If the revision affects a signed or approved document:

The original document remains preserved.
The original signature audit record remains attached.
The revised document receives a new version.
The revised document enters a new signature sequence where required.
The new document is marked current or superseding after approval.
13. Task Data Model
workflow_task
Field	Type	Requirement
id	BIGINT	Primary key
project_id	BIGINT	Required foreign key
msr_version_id	BIGINT	Optional foreign key
task_type	VARCHAR(80)	Required
task_status	VARCHAR(30)	Open, assigned, completed, cancelled, blocked, expired
assigned_user_id	BIGINT	Optional foreign key
assigned_role_code	VARCHAR(60)	Optional
created_at	TIMESTAMPTZ	Required
created_by	BIGINT	Required foreign key
assigned_at	TIMESTAMPTZ	Optional
due_at	TIMESTAMPTZ	Optional
completed_at	TIMESTAMPTZ	Optional
completed_by	BIGINT	Optional foreign key
blocked_reason	TEXT	Optional
completion_notes	TEXT	Optional
priority	VARCHAR(20)	Normal, high, urgent
related_document_id	BIGINT	Optional foreign key
related_version_id	BIGINT	Optional foreign key
audit_event_id	BIGINT	Optional foreign key
Initial task types
text


sales\_record\_review
sales\_record\_correction
contract\_customer\_signature
contract\_gecc\_signature
invoice\_gecc\_signature
invoice\_customer\_signature
installation\_scheduling
installation\_assignment
installation\_completion
scope\_change\_review
scope\_change\_revision
certificate\_customer\_signature
certificate\_gecc\_signature
payment\_entry
payment\_verification
adjustment\_approval
project\_completion\_review
exception\_review
Tasks are not a substitute for state validation. Both task status and state-transition guards must be satisfied.

14. Project Completion Rules
A project may enter completed only when all conditions are true:

Scope of work is complete.
Contract has all required signatures.
Certificate has all required signatures.
Invoice has all required signatures.
Invoice is fully paid after approved adjustments.
Payment verification is complete.
No unresolved scope change exists.
No required task remains open or blocked.
The current Master Sales Record version is approved.
The project is not cancelled.
Required audit events were created.
The system must reject completion if any condition is false.

15. Exceptions and Blocking Conditions
The system must identify and block:

Missing required approval
Missing required signature
Invalid signing order
Expired signature request
Installation before the waiting period
Missing installation assignment
Missing equipment serial numbers where required
Proposed scope change not reviewed
Revised scope not approved
Unverified payment
Unapproved adjustment
Unpaid remaining balance
Superseded Master Sales Record
Cancelled project
Missing certificate signature
Missing required completion task
A blocking condition must display:

Condition
Affected record
Required action
Assigned role or user
Date detected
Due date where applicable
16. Audit Events
Sprint 06 must create audit events for:

State transition
Transition rejection
Task creation
Task assignment
Task reassignment
Task completion
Task cancellation
Task blocking
Contract-signature dependency validation
Invoice-signature dependency validation
Earliest-installation-date calculation
Early-installation attempt
Installation scheduling
Installation rescheduling
Installation start
Installation completion
Scope-change proposal
Scope-change approval
Scope-change rejection
Scope-change work-block
Authorized reopening
New Master Sales Record version
Revised-document requirement
Exception request
Exception approval
Exception rejection
Project-completion validation
Project-completion rejection
Project completion
Each event must identify:

User
Role
Session
Project
Current state
Previous state
Requested state
Task
Master Sales Record version
Related document or payment where applicable
Outcome
Reason
Timestamp
Audit-chain references
17. Sprint 06 Decisions
D-06-001 — Controlled state machine
Project lifecycle transitions are controlled by a defined state machine.

Users cannot bypass transition guards through the user interface or ordinary application operations.

D-06-002 — Three-day waiting period
The earliest installation date is calculated from the later of the customer contract-signature timestamp and customer invoice-signature timestamp, plus three calendar days.

D-06-003 — Waiting-period timezone
Waiting-period calculations use America/New_York.

D-06-004 — Core approval authority
The Master Sales Record approval authority remains:

Sales Manager, or
Comptroller
One authorized approval is sufficient.

D-06-005 — Installation task creation
An Installation Manager scheduling task is created only after all required contract and invoice signatures are complete.

D-06-006 — Installation blocking
Installation cannot be scheduled, started, or completed before the permitted date unless a separately authorized exception is approved.

D-06-007 — Scope-change blocking
Work on changed scope is blocked while the scope change is pending review or while revised approvals and signatures are incomplete.

D-06-008 — Scope-change versioning
An approved scope change creates a new Master Sales Record version and revised document workflow.

D-06-009 — Completion gate
Project completion requires scope completion, required signatures, verified payment, full payment after approved adjustments, and no unresolved blocking task.

D-06-010 — Historical preservation
State transitions, tasks, exceptions, approvals, and rejected transitions remain permanently auditable.

18. Sprint 06 Acceptance Criteria
Sprint 06 is accepted when:

A project cannot skip required lifecycle states.
A submitted Master Sales Record requires authorized review.
An unauthorized user cannot approve a Master Sales Record.
An approved Master Sales Record becomes locked.
An approved record cannot be silently overwritten.
Authorized reopening creates a new version.
Original Master Sales Record versions remain preserved.
Contract customer signing is required before GECC contract signing.
Contract completion is required before invoice signature workflow.
GECC invoice signing is required before customer invoice signing.
Installation scheduling cannot occur before all required contract and invoice signatures.
The three-day installation rule is calculated correctly.
Installation before the permitted date is rejected.
Early-installation attempts are audited.
Authorized exceptions require a reason and approval.
Installation tasks are created for the Installation Manager.
Installation assignment and scheduling are tracked.
Technicians can update authorized completion fields.
Technicians cannot change retail price, approved costs, or contract terms.
A proposed scope change enters a blocking state.
Changed-scope work cannot continue before approval.
An approved scope change creates a new Master Sales Record version.
Revised documents are required when affected.
Original documents and signatures remain preserved.
Payment verification is required before completion.
Approved adjustments are required before adjusted payment completion.
A project cannot be completed with an unresolved scope change.
A project cannot be completed with an unverified payment.
A project cannot be completed with missing required signatures.
All transitions, tasks, exceptions, and failures are audited.
Workflow tasks expose the next required action and assigned role or user.
The implementation is ready for Sprint 07 document generation.
19. Sprint 06 Test Cases
State-transition tests
Draft transitions to review.
Review transitions to approved.
Review can return to correction.
Unapproved records cannot generate final documents.
Approved records cannot be edited directly.
Cancelled projects cannot resume ordinary processing.
Invalid transitions are rejected.
Rejected transitions are audited.
Signature-order tests
Customer can sign the contract first.
GECC contract signature is blocked until customer signature exists.
GECC invoice signature is blocked until contract is fully signed.
Customer invoice signature is blocked until GECC invoice signature exists.
Certificate customer signature is required before GECC certificate signature.
Comptroller substitute certificate signing requires a reason.
Waiting-period tests
Contract customer signature timestamp is recorded.
Invoice customer signature timestamp is recorded.
Later timestamp is selected.
Three calendar days are added in Eastern time.
Installation before the computed date is rejected.
Installation on the permitted date succeeds.
Exception approval permits only the specifically authorized exception.
Original timestamps remain unchanged.
Scope-change tests
Technician can propose a scope change.
Scope-change proposal blocks changed-scope work.
Sales Manager can approve the change.
Comptroller can approve the change.
Rejected change returns to the original approved scope.
Approved change creates a new Master Sales Record version.
Original version remains unchanged.
Revised approval is required.
Revised document signatures are required where applicable.
Completion tests
Completion is rejected without completed scope.
Completion is rejected without required signatures.
Completion is rejected without verified payment.
Completion is rejected with an outstanding adjusted balance.
Completion is rejected with an open scope change.
Completion succeeds only when every required condition is satisfied.
Completion makes the project eligible for later commission processing.
Task tests
Required task is created on the correct transition.
Task is assigned to the correct role.
Task cannot be completed by an unauthorized user.
Task completion updates the workflow state only when all guards pass.
Blocked tasks display a reason.
Overdue tasks are visible to authorized dashboard users.
20. Sprint 06 Open Questions
These implementation questions do not change approved requirements:

What exact due-date rules apply to each task type?
Are waiting periods counted as calendar days or business days? The current approved rule is three days; the precise operational interpretation requires confirmation.
What exception roles may approve early installation?
What circumstances qualify as Installation Manager unavailability?
Which document changes always require a new signature sequence?
Which scope changes require revised pricing?
Which scope changes require revised costs?
What notification channels will be used for task assignments?
What escalation rules apply to overdue tasks?
What exact state is used while an expired signature request awaits authorized action?
What cancellation rules apply after documents have been signed?
What state should be used for a project that is complete in scope but unpaid?
21. Sprint 07 Handoff
Sprint 07 will implement document generation and artwork.

Sprint 07 must use:

The approved Master Sales Record version
The current project state
The current document-generation task
Document-version references
Required approval state
Customer and project consistency checks
State-specific project location
Scope and equipment data
Retail price
Payment option
Cost data where permitted
Current revision and supersession information
Sprint 07 must not generate final controlled documents when:

The Master Sales Record is not approved.
Required fields are missing.
The record is superseded.
A required workflow approval is missing.
Customer, project, or project-code consistency fails.
Required artwork is missing.
A document must be regenerated from a newer current version.
22. Historical Record
Supersedes
None.

Preserves
GECC-00-project-baseline.md
GECC-01-data-model.md
GECC-02-auth-and-permissions.md
GECC-03-audit-log.md
GECC-04-customers-and-projects.md
GECC-05-master-sales-record.md
Sprint 00 decisions
Sprint 01 decisions
Sprint 02 decisions
Sprint 03 decisions
Sprint 04 decisions
Sprint 05 decisions
Approved lifecycle requirements
Approved waiting-period requirement
Approved scope-change requirements
Approved versioning and audit requirements
Changes introduced by this bundle
Defines the controlled project state machine.
Defines allowed lifecycle transitions.
Defines transition guards.
Defines task creation and assignment.
Defines contract and invoice dependencies.
Defines the three-day installation rule.
Defines installation scheduling and completion workflow.
Defines scope-change blocking and revision workflow.
Defines authorized reopening.
Defines project-completion gates.
Defines Sprint 06 audit events.
Defines Sprint 07 document-generation handoff.