# GECC-08-electronic-signatures.md

# GECC Sales Back Office System — Sprint 08 Electronic Signatures and Reminders

## Bundle Metadata

- Bundle name: GECC-08-electronic-signatures.md
- Project: GECC Sales Back Office System
- Company: Go Ecco Climate Control
- Company abbreviation: GECC
- Bundle type: Sprint 08 design and implementation bundle
- Status: Approved design; ready for implementation
- Created date: 2026-09-20
- Previous bundle: GECC-07-document-generation.md
- Next planned bundle: GECC-09-payments-and-payables.md
- Database: PostgreSQL
- Primary-key strategy: Numeric BIGINT identity keys
- Business timezone: America/New_York
- System of record: PostgreSQL relational database
- Audit model: Append-only, hash-chained, permanently retained
- Signature-request expiration: 75 days
- Reminder interval: 7 days

---

## 1. Sprint Objective

Implement the built-in electronic-signature system for:

- Contracts
- Invoices
- Certificates

The system must support:

- Email delivery
- Signature links
- Signing order
- Customer authentication
- Signer authentication
- Automatic reminders
- Seven-day reminder intervals
- Seventy-five-day expiration
- Signature audit trails
- Completed-document certificates
- Downloadable signed PDFs
- Signature-status tracking
- Delivery-status tracking
- Expired-request handling
- Permanent preservation of original requests and events

---

## 2. Sprint 08 Deliverables

Sprint 08 must produce:

1. Signature-request model
2. Signature-participant model
3. Signer-order enforcement
4. Customer authentication
5. Internal-signer authentication
6. Secure signature links
7. Email-delivery integration
8. Delivery-status tracking
9. Reminder scheduling
10. Seven-day reminder behavior
11. Seventy-five-day expiration behavior
12. Signature capture
13. Signature timestamps
14. Signature audit records
15. Completed-document certificates
16. Signed-PDF generation
17. Signature-request cancellation
18. Signature-request resend
19. Signature-request revision handling
20. Signature-request extension handling
21. Expiration exception tasks
22. Electronic-signature audit events
23. Signature acceptance tests
24. Sprint 09 payment and receivable handoff

---

## 3. Requirements Carried Forward

The implementation must preserve these approved requirements:

- Documents are generated from approved Master Sales Record data.
- The document version being signed must remain identifiable.
- The contract is signed by the customer first.
- The contract is signed by GECC second.
- Either the Sales Manager or Comptroller may sign the contract for GECC.
- The GECC signer signs the invoice first.
- The customer signs the invoice second.
- The customer signs the certificate first.
- The Installation Manager signs the certificate second.
- The Comptroller may sign the certificate only when the Installation Manager is unavailable.
- A substitute Comptroller signature requires a recorded reason.
- Signature links are delivered by email.
- Signers must be authenticated.
- Automatic reminders are sent every seven days.
- Signature requests expire after 75 days.
- Expired requests create action tasks for the Sales Manager and Comptroller.
- Original signature requests remain preserved after expiration.
- Authorized users may resend, revise, cancel, or extend requests.
- Reminder and expiration events remain in the audit trail.
- Completed signed PDFs and signature audit certificates are retained.

---

## 4. Signature Data Model

### 4.1 `signature_request`

| Field | Type | Requirement |
|---|---|---|
| `id` | BIGINT | Primary key |
| `document_version_id` | BIGINT | Required foreign key |
| `project_id` | BIGINT | Required foreign key |
| `msr_version_id` | BIGINT | Required foreign key |
| `request_type` | VARCHAR(30) | Contract, invoice, certificate |
| `request_status` | VARCHAR(40) | Draft, pending, partially_signed, completed, expired, cancelled, superseded |
| `created_at` | TIMESTAMPTZ | Required |
| `created_by` | BIGINT | Required foreign key |
| `sent_at` | TIMESTAMPTZ | Optional |
| `expires_at` | TIMESTAMPTZ | Required |
| `completed_at` | TIMESTAMPTZ | Optional |
| `cancelled_at` | TIMESTAMPTZ | Optional |
| `cancelled_by` | BIGINT | Optional foreign key |
| `cancellation_reason` | TEXT | Optional |
| `supersedes_request_id` | BIGINT | Optional foreign key |
| `extended_from_request_id` | BIGINT | Optional foreign key |
| `extension_reason` | TEXT | Optional |
| `signature_audit_certificate_id` | BIGINT | Optional foreign key |
| `signed_document_version_id` | BIGINT | Optional foreign key |
| Standard audit columns | — | Required |

### 4.2 `signature_participant`

| Field | Type | Requirement |
|---|---|---|
| `id` | BIGINT | Primary key |
| `signature_request_id` | BIGINT | Required foreign key |
| `signer_role` | VARCHAR(40) | Customer, Sales Manager, Comptroller, Installation Manager |
| `signer_user_id` | BIGINT | Optional foreign key |
| `signer_name` | VARCHAR(200) | Required |
| `signer_email` | VARCHAR(320) | Required |
| `signing_order` | INTEGER | Required |
| `participant_status` | VARCHAR(40) | Pending, invited, viewed, authenticated, signed, declined, expired, cancelled |
| `required` | BOOLEAN | Required |
| `authentication_method` | VARCHAR(50) | Required |
| `invited_at` | TIMESTAMPTZ | Optional |
| `viewed_at` | TIMESTAMPTZ | Optional |
| `authenticated_at` | TIMESTAMPTZ | Optional |
| `signed_at` | TIMESTAMPTZ | Optional |
| `declined_at` | TIMESTAMPTZ | Optional |
| `decline_reason` | TEXT | Optional |
| `ip_reference` | VARCHAR(100) | Optional |
| `user_agent_reference` | TEXT | Optional |

Signer identity and signature evidence must be retained according to the signature-audit requirements.

### 4.3 `signature_field`

| Field | Type | Requirement |
|---|---|---|
| `id` | BIGINT | Primary key |
| `signature_request_id` | BIGINT | Required foreign key |
| `participant_id` | BIGINT | Required foreign key |
| `document_field_name` | VARCHAR(150) | Required |
| `field_type` | VARCHAR(30) | Signature, date, initials, text |
| `page_number` | INTEGER | Required |
| `field_coordinates` | JSONB | Required |
| `required` | BOOLEAN | Required |
| `field_status` | VARCHAR(30) | Pending, completed, voided |
| `completed_value_reference` | TEXT | Optional |
| `completed_at` | TIMESTAMPTZ | Optional |

### 4.4 `signature_link`

| Field | Type | Requirement |
|---|---|---|
| `id` | BIGINT | Primary key |
| `participant_id` | BIGINT | Required foreign key |
| `link_identifier` | VARCHAR(200) | Required, unique |
| `token_hash` | VARCHAR(128) | Required |
| `created_at` | TIMESTAMPTZ | Required |
| `first_used_at` | TIMESTAMPTZ | Optional |
| `last_used_at` | TIMESTAMPTZ | Optional |
| `expires_at` | TIMESTAMPTZ | Required |
| `revoked_at` | TIMESTAMPTZ | Optional |
| `link_status` | VARCHAR(30) | Active, used, expired, revoked, superseded |

The system must not store reusable signature tokens in plain text.

---

## 5. Signing-Order Rules

### 5.1 Contract

Required order:

```text
1. Customer signs contract
2. Sales Manager or Comptroller signs contract
The GECC signer cannot sign before the customer signature is complete.

5.2 Invoice
Required order:

text


1. Sales Manager or Comptroller signs invoice
2. Customer signs invoice
The customer cannot sign before the GECC signature is complete.

The invoice signature request cannot be sent until the contract has all required signatures.

5.3 Certificate
Required order:

text


1. Customer signs certificate
2. Installation Manager signs certificate
The Comptroller may substitute only when the Installation Manager is unavailable.

For a substitute Comptroller signature:

The substitute reason is required.
The reason is stored with the signature request.
The reason is stored with the certificate record.
The reason is included in the signature audit certificate.
The substitute action is audited.
5.4 Signer-order enforcement
The system must reject:

Signing before the participant's turn
Accessing an unauthorized signing action
Signing a superseded document version
Signing an expired request
Signing a cancelled request
Signing a request for which authentication has not succeeded
Completing a signature field for another participant
6. Signature Request Creation
A signature request may be created only when:

The document version is current.
The Master Sales Record version is approved.
Required workflow state permits signing.
Required document fields are complete.
Required signer identities are known.
Required signer email addresses are present.
Required signature fields are mapped.
The document content hash is recorded.
The request has not been superseded or cancelled.
The request must preserve:

Document version
Master Sales Record version
Project
Customer
Template version
Artwork version
Document content hash
Participant order
Expiration timestamp
Creation user
Creation timestamp
7. Email Delivery
7.1 Invitation email
Each signer invitation must include:

GECC identification
Document type
Project identification code where appropriate
Signer role
Secure signature link
Expiration information
Support or contact instructions
No unnecessary sensitive information
7.2 Delivery statuses
The system must track:

text


queued
sent
delivered
bounced
blocked
opened
link\_accessed
failed
Delivery events must remain attached to the signature request.

7.3 Email records
signature_delivery_event
Field	Type	Requirement
id	BIGINT	Primary key
signature_request_id	BIGINT	Required foreign key
participant_id	BIGINT	Required foreign key
delivery_type	VARCHAR(40)	Invitation, reminder, expiration, resend
recipient_email	VARCHAR(320)	Required
sent_at	TIMESTAMPTZ	Optional
delivered_at	TIMESTAMPTZ	Optional
opened_at	TIMESTAMPTZ	Optional
bounced_at	TIMESTAMPTZ	Optional
delivery_status	VARCHAR(30)	Required
provider_reference	VARCHAR(200)	Optional
failure_reason	TEXT	Optional
Every delivery event must be audited.

8. Signer Authentication
8.1 Required behavior
The system must authenticate signers before allowing signature completion.

Authentication must support:

Secure signature link
Signer identity confirmation
Email confirmation
Additional authentication where configured
Re-authentication for a sensitive signing action where required
Prevention of link reuse after completion or revocation
8.2 Customer authentication
The customer must confirm identity through the configured customer-authentication method before signing.

The exact authentication method remains an implementation configuration item.

Possible methods include:

Email link plus verification code
Email link plus known customer information
One-time code
Approved external identity verification
8.3 Internal signer authentication
Sales Manager, Comptroller, and Installation Manager signers must:

Authenticate through an active GECC user account.
Have the required role and permission.
Sign only when it is their turn.
Sign the exact document version presented.
Have the signature recorded against their user identity.
8.4 Authentication failure
After authentication failure:

Signature is denied.
The event is logged.
The request remains active unless a security rule suspends it.
Repeated failures may trigger a security review or link revocation.
The user is not shown restricted internal information.
9. Signature Capture
The system must capture:

Signer identity
Signer role
Signature image or secure signature representation
Signature timestamp
Signature date
Signature field
Document version
Document content hash
Authentication result
Signature request
Session or signing session reference
Delivery and access evidence
Applicable IP or device reference according to the final implementation
The system must prevent a signer from changing the underlying document after signing.

10. Reminder Process
10.1 Reminder interval
Automatic reminders are sent every seven days while:

The request is active.
The participant has not signed.
The request has not expired.
The request has not been cancelled.
The request has not been superseded.
10.2 Reminder schedule
The first reminder is calculated from the original invitation or prior reminder according to the configured schedule.

The system must store:

Reminder number
Scheduled time
Actual send time
Recipient
Delivery result
Failure reason where applicable
10.3 Reminder limits
The system must stop reminders when:

The participant signs.
The request expires.
The request is cancelled.
The request is superseded.
The request is revoked.
An administrator pauses reminders.
Reminder pausing must require an authorized reason and audit event.

11. Expiration
11.1 Expiration period
Unsigned contracts, invoices, and certificates expire after 75 days.

The expiration timestamp is calculated from the signature-request creation or sending time according to the approved implementation configuration.

The request must store the exact expires_at timestamp.

11.2 Expiration behavior
Upon expiration:

Mark the request as expired.
Disable active signing links.
Stop automatic reminders.
Preserve the original request.
Preserve all delivery and access events.
Create an expiration audit event.
Create an action task for the Sales Manager and Comptroller.
Display the request on the exception dashboard.
Prevent further signing until an authorized action occurs.
11.3 Authorized actions after expiration
An authorized user may:

Resend the same document version
Create a new signature request
Revise the document through the approved workflow
Cancel the request
Extend the request
Record a reason
An expired request must not be silently reactivated without a recorded action.

12. Resend, Revision, Cancellation, and Extension
12.1 Resend
Resending may be used when:

The document version remains valid.
The request is expired, failed, or delivery-blocked.
No source-data revision is required.
A resend must:

Create a new delivery event.
Preserve the original request.
Preserve the original expiration history.
Record the resending user and reason.
Use a new secure link where appropriate.
Not remove prior signer activity.
12.2 Revision
A revision is required when:

Underlying project data changes.
The Master Sales Record version changes.
The document version changes.
Scope changes.
Pricing changes.
Contract, invoice, or certificate terms change.
A revised document requires:

New document version
New signature request
New signing sequence
Preservation of the original request
Preservation of original signatures
Superseding relationship
Audit events
12.3 Cancellation
Cancellation requires:

Authorized user
Cancellation reason
Cancellation timestamp
Link revocation
Reminder cancellation
Audit event
The cancelled request remains preserved.

12.4 Extension
An extension may be granted only by an authorized user.

The extension requires:

Existing request
Extension reason
New expiration timestamp
Authorized user
Approval or authorization
Audit event
The original expiration value remains preserved.

13. Completed Signature Documents
Upon completion of all required signatures:

Verify signing order.
Verify all required fields.
Verify signer identities.
Verify document content hash.
Apply signatures to the document.
Generate the completed signed PDF.
Generate a signature audit certificate.
Store both files.
Link them to the document version.
Update signature statuses.
Update the project workflow state.
Create completion audit events.
Create the next workflow task where applicable.
13.1 signature_audit_certificate
Field	Type	Requirement
id	BIGINT	Primary key
signature_request_id	BIGINT	Required foreign key
document_version_id	BIGINT	Required foreign key
certificate_file_location	TEXT	Required
certificate_content_hash	VARCHAR(128)	Required
generated_at	TIMESTAMPTZ	Required
signer_summary	JSONB	Required
delivery_summary	JSONB	Required
authentication_summary	JSONB	Required
signature_sequence_summary	JSONB	Required
completion_status	VARCHAR(30)	Required
13.2 Certificate content
The certificate should include:

Document identity
Document version
Project identification code
Master Sales Record version
Signer names
Signer roles
Signature order
Signature timestamps
Authentication method
Delivery evidence
Reminder history
Document content hash
Completion timestamp
Substitute-signature reason where applicable
The final certificate layout remains an implementation configuration item.

14. Signature Statuses
Request statuses
text


draft
pending
partially\_signed
completed
expired
cancelled
superseded
failed
Participant statuses
text


pending
invited
delivered
viewed
authenticated
signed
declined
expired
cancelled
Field statuses
text


pending
completed
voided
Status changes must be controlled and audited.

15. Workflow Integration
Contract
When the customer signs:

Update contract participant status.
Permit GECC signing.
Create the GECC signature task.
Audit the event.
When the GECC signer signs:

Mark the contract fully signed.
Update the project workflow.
Permit invoice-signature workflow.
Invoice
When the GECC signer signs:

Permit customer invoice signing.
When the customer signs:

Mark the invoice fully signed.
Calculate the installation waiting-period start timestamp.
Permit installation-task creation when all other requirements are satisfied.
Certificate
When the customer signs:

Permit Installation Manager signing.
When the Installation Manager signs:

Mark certificate fully signed.
When the Comptroller substitutes:

Confirm Installation Manager unavailability.
Require substitute reason.
Store substitute reason.
Mark the certificate fully signed after valid signature.
Audit the substitution.
16. Access and Permission Rules
Customer
May:

Access only their own signature request.
View the document presented for signature.
Sign only when it is their turn.
Download the completed document if permitted.
Receive reminders and expiration notices.
May not:

View internal approval notes.
View internal commission or financial information.
Alter document source data.
Sign on behalf of another participant.
Sales Manager and Comptroller
May:

Sign contracts.
Sign invoices.
Approve or manage expired requests.
Resend, cancel, revise, or extend requests where authorized.
View signature status for authorized business records.
Approve certificate substitution according to applicable rules.
Installation Manager
May:

Sign certificates.
View authorized certificate status.
Manage installation-related signature tasks.
Sales Associate
May:

View signature status for assigned projects.
Review generated documents where authorized.
Send permitted signature requests.
Not sign for GECC unless separately authorized.
Technician
May:

Review and send certificates to customers where authorized.
View certificate status for assigned projects.
Not sign as GECC.
Not alter document content.
17. Audit Events
Sprint 08 must create audit events for:

Signature request created
Participant invited
Email queued
Email sent
Email delivered
Email bounced
Email opened
Signature link accessed
Authentication succeeded
Authentication failed
Document viewed
Signature field completed
Signature completed
Signature declined
Reminder scheduled
Reminder sent
Reminder failed
Signature request expired
Signature request cancelled
Signature request resent
Signature request revised
Signature request extended
Signature request superseded
Signed PDF generated
Signature audit certificate generated
Signature request downloaded
Unauthorized signature attempt
Out-of-order signing attempt
Expired-link attempt
Revoked-link attempt
Substitute certificate signature
Signature completion failure
Each event must reference:

User or external signer identity where available
Signer role
Session or signing session
Signature request
Participant
Project
Customer
Document version
Master Sales Record version
Event outcome
Failure reason where applicable
Timestamp
Audit-chain references
18. Sprint 08 Decisions
D-08-001 — Signature-request version binding
Every signature request is bound to exactly one immutable document version and one Master Sales Record version.

D-08-002 — Signing order
Signing order is enforced by document type:

Contract: customer first, GECC second
Invoice: GECC first, customer second
Certificate: customer first, Installation Manager second
D-08-003 — Comptroller substitute signing
The Comptroller may sign a certificate only when the Installation Manager is unavailable and a substitute reason is recorded.

D-08-004 — Reminder interval
Automatic reminders use a seven-day interval.

D-08-005 — Expiration period
Unsigned requests expire after 75 days.

D-08-006 — Expiration preservation
Expired requests remain permanently preserved. Expiration does not delete or overwrite the original request.

D-08-007 — Secure links
Signature links use revocable, expiring, non-reusable token references. Plain-text reusable tokens are not stored.

D-08-008 — Internal signer authentication
Internal GECC signers authenticate through active system accounts with the required role and permission.

D-08-009 — Customer authentication
Customers must authenticate through the approved customer-authentication method before signing.

D-08-010 — Revised documents
A revised document requires a new signature request and a new signing sequence where applicable.

D-08-011 — Signed-PDF preservation
Completed signed PDFs and signature audit certificates are permanently retained and linked to the original document version.

D-08-012 — Signature audit integration
All signature, delivery, authentication, reminder, expiration, and failure events are written to the tamper-evident audit log.

19. Sprint 08 Acceptance Criteria
Sprint 08 is accepted when:

A signature request can be created for a valid contract.
A signature request can be created for a valid invoice.
A signature request can be created for a valid certificate.
Each request is bound to one document version.
Each request is bound to one Master Sales Record version.
Contract signing requires customer signature before GECC signature.
Invoice signing requires GECC signature before customer signature.
Certificate signing requires customer signature before GECC signature.
Installation Manager certificate signing is supported.
Comptroller certificate substitution requires Installation Manager unavailability and a reason.
Out-of-order signing is rejected.
Unauthorized users cannot sign.
Signers are authenticated before signing.
Secure signature links are generated.
Signature links expire or are revoked as required.
Email delivery statuses are tracked.
Automatic reminders are sent at seven-day intervals.
Reminders stop after signing, cancellation, supersession, or expiration.
Requests expire after 75 days.
Expired requests cannot be signed.
Expired requests create Sales Manager and Comptroller action tasks.
Expired requests remain preserved.
Authorized users can resend requests.
Authorized users can cancel requests.
Authorized users can extend requests with a reason.
Revised documents create new signature requests.
Original signature requests remain preserved.
All required signature fields are completed before completion.
Signed PDFs are generated and stored.
Signature audit certificates are generated and stored.
Signed PDFs include the correct document version.
Signature certificates include signer and delivery evidence.
Signature completion updates the project workflow.
All signature events are audited.
All delivery events are audited.
All authentication failures are audited.
Unauthorized and out-of-order attempts are audited.
Sprint 09 can rely on completed invoice-signature state before processing payments.
20. Sprint 08 Test Cases
Request creation
Create a contract signature request from a valid document.
Create an invoice signature request only after the contract is fully signed.
Create a certificate signature request from a valid certificate.
Reject a request for an unapproved document.
Reject a request for a superseded document version.
Reject a request with missing signer data.
Reject a request with missing signature fields.
Signing order
Customer signs contract.
GECC signs contract after customer.
Reject GECC contract signing before customer.
GECC signs invoice.
Customer signs invoice after GECC.
Reject customer invoice signing before GECC.
Customer signs certificate.
Installation Manager signs certificate after customer.
Reject Installation Manager signing before customer.
Comptroller substitute signature requires a reason.
Reject unauthorized substitute signature.
Authentication
Valid internal signer authenticates.
Invalid internal signer authentication is rejected.
Valid customer authentication succeeds.
Invalid customer authentication is rejected.
Expired link is rejected.
Revoked link is rejected.
Used completed link cannot be reused.
Reminders and expiration
Invitation is sent.
Delivery failure is recorded.
Seven-day reminder is scheduled.
Reminder is sent.
Reminder stops after signing.
Request expires after 75 days.
Expired request creates action tasks.
Expired request remains preserved.
Authorized extension updates expiration and preserves original values.
Completed documents
All signature fields are completed.
Signed PDF is generated.
Signature certificate is generated.
Signed PDF content hash is recorded.
Certificate includes signer order.
Certificate includes signature timestamps.
Certificate includes delivery and authentication evidence.
Completed request updates project workflow.
Revision and preservation
Revise a signed source document.
Preserve original signed PDF.
Preserve original signature certificate.
Create a new document version.
Create a new signature request.
Require a new signing sequence.
Mark the original request superseded.
Preserve all original audit events.
Audit and security
Signature creation is audited.
Signature access is audited.
Authentication failure is audited.
Out-of-order signing is audited.
Expiration is audited.
Reminder delivery is audited.
Resend is audited.
Cancellation is audited.
Unauthorized download is rejected and audited.
21. Sprint 08 Open Questions
These implementation questions do not change approved requirements:

What exact customer-authentication method will be used?
What signature representation will be accepted?
What identity-verification level is required for customers?
What email delivery service will be used?
What exact expiration start point will be used: request creation, first delivery, or successful delivery?
What maximum number of reminders is permitted?
What exact reminder time will be used in Eastern time?
What signer notification will be sent after a failed delivery?
What constitutes Installation Manager unavailability?
Who may approve an extension beyond 75 days?
What signature-audit-certificate format is required?
What legal wording must appear on completed signature certificates?
What retention and backup requirements apply to signature evidence?
What exact customer support process handles disputed signatures?
What additional authentication is required for certificate substitute signing?
22. Sprint 09 Handoff
Sprint 09 will implement:

Cashier's-check receipt
Bank-wire receipt
Payment verification
Payment allocation
Accounts receivable
Credits
Refunds
Adjustments
Separate accounts payable records
Sprint 09 must use:

Fully signed invoice status
Invoice document version
Project identification code
Customer and project references
Approved adjustment workflow
Signature audit certificate
Payment-related task structure
Sprint 09 must not treat an invoice as payable or a project as complete unless:

Required invoice signatures are complete.
Payment is recorded.
Payment verification is complete.
Approved adjustments are properly recorded.
23. Historical Record
Supersedes
None.

Preserves
GECC-00-project-baseline.md
GECC-01-data-model.md
GECC-02-auth-and-permissions.md
GECC-03-audit-log.md
GECC-04-customers-and-projects.md
GECC-05-master-sales-record.md
GECC-06-workflow-state-machine.md
GECC-07-document-generation.md
Sprint 00 decisions
Sprint 01 decisions
Sprint 02 decisions
Sprint 03 decisions
Sprint 04 decisions
Sprint 05 decisions
Sprint 06 decisions
Sprint 07 decisions
Approved signing order
Approved reminder and expiration rules
Approved document-version and audit requirements
Changes introduced by this bundle
Defines the electronic-signature request model.
Defines participant, field, link, delivery, and audit-certificate structures.
Enforces signing order for contracts, invoices, and certificates.
Defines signer authentication behavior.
Defines seven-day reminders.
Defines 75-day expiration.
Defines resend, revision, cancellation, and extension behavior.
Defines completed signed PDFs and signature audit certificates.
Defines Sprint 08 audit events.
Defines Sprint 09 payment and receivable handoff.






