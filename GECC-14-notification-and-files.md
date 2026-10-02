Sprint 013 approval recorded.

GECC-14-notifications-and-files.md
GECC Sales Back Office System — Sprint 14
Bundle Metadata
Bundle name: GECC-14-notifications-and-files.md
Project: GECC Sales Back Office System
Company: Go Ecco Climate Control
Sprint: 14
Sprint name: Notifications, Email, and File Management
Baseline: `GECC-00-project-baseline.md
Previous bundle: `GECC-13-dashboard-and-search.md
Status: Implementation bundle
Date: 2026-09-20
Next planned bundle: GECC-15-testing-and-integrity.md
Primary database: Relational database
System of record: GECC database
Google Sheets: Export or reporting destination only
1. Sprint Objective
Implement in-app notifications, email delivery, customer and project file management, document metadata, delivery tracking, retention controls, and file-access auditing.

Sprint 14 must ensure that:

Users are notified of tasks and exceptions relevant to their roles.
Email messages are linked to the correct project, customer, document, or report.
Generated and signed documents are retained in the appropriate customer/project file.
Original and superseded document versions remain preserved.
File access follows record and role permissions.
Email delivery status is recorded.
Failed deliveries create actionable exceptions.
File downloads, emails, and document access are audited.
The annual contractor-report email process is supported.
Notification activity does not bypass the controlled workflow.
2. Sprint Scope
2.1 Included
In-app notifications
Email notifications
Notification events
Notification recipients
Notification status
Notification preferences
Customer folders
Project folders
Document metadata
File references
Generated-document storage
Signed-document storage
Document version links
Superseded-document handling
Email delivery tracking
Failed-delivery handling
Signature-related reminders
Expiration notifications
Approval notifications
Payment-verification notifications
Commission-report distribution
Annual contractor-report email process
File-access permissions
File download auditing
Retention metadata
File and notification search integration
2.2 Excluded
Replacing the electronic-signature system
Replacing the relational database
Automatic bank notifications
Customer self-service portal
General marketing email
Bulk unsolicited email
Employee human-resources notifications
External document collaboration
Automatic legal-retention classification without approved policy
Full enterprise content-management functionality
3. Notification Architecture
3.1 Notification Event
A notification event must be created from a recognized system event, including:

Workflow task assignment
Approval request
Approval completion
Approval rejection
Signature request
Signature reminder
Signature expiration
Payment verification required
Payment verified
Payment rejected
Adjustment approval required
Payable approval required
Commission report ready for review
Commission report approved
Annual contractor summary ready for delivery
Financial report ready for approval
Financial period locked
Exception created
Failed email delivery
File upload completed
File processing failed
The system must not create notifications from unsupported or unaudited event types.

3.2 Notification Record
Required fields:

Notification ID
Notification type
Event type
Recipient user ID or external recipient reference
Customer ID, if applicable
Project ID, if applicable
Document ID, if applicable
Report ID, if applicable
Related task ID, if applicable
Subject
Message body or template reference
Priority
Delivery channel
Status
Created date and time
Sent date and time
Read date and time
Expiration date, if applicable
Failure reason
Retry count
Related audit reference
3.3 Notification Channels
Initial channels:

In-app notification
Email
The system must distinguish between:

Notification created
Notification queued
Notification sent
Notification delivered
Notification opened or read
Notification failed
Notification dismissed
Notification expired
A notification may be delivered through more than one channel.

3.4 Notification Priorities
Permitted values:

LOW
`NORMAL
HIGH
URGENT
Urgent notifications may include:

Payment verification failure
Expiring signature request
Expired signature request
Closed-period adjustment issue
Security or permission exception
Failed required document delivery
4. Notification Rules
4.1 Assignment Notifications
Notify an assigned user when:

A project is assigned.
A project task is assigned.
A task is reassigned.
A task becomes overdue.
A task is cancelled or superseded.
The notification must include:

Project identification code
Customer or project reference permitted for the recipient
Task
Due date
Required action
Link to the authorized record
4.2 Approval Notifications
Notify authorized approvers when:

A Master Sales Record awaits approval.
A scope change awaits approval.
A payment adjustment awaits approval.
A refund awaits approval.
A commission report awaits review or approval.
A payable awaits approval.
A financial report awaits approval.
A formal adjustment awaits approval.
Approval notifications must not grant approval authority. They only direct the user to the authorized workflow.

4.3 Signature Notifications
Integrate with Sprint 08 for:

Signature request creation
Signature reminders
Signature delivery failure
Signature expiration
Signature completion
Required substitute signer action
The notification system must not create a second, conflicting signature request.

4.4 Payment Notifications
Notify authorized users when:

A payment is entered.
A payment requires verification.
A cashier’s-check verification is rejected.
A bank wire lacks required evidence.
A payment is verified.
A payment is reversed.
A refund changes project payment status.
A project remains blocked by missing verified payment.
Customer notifications must be sent only through approved templates and only when the applicable workflow authorizes customer communication.

4.5 Commission Notifications
Notify authorized users when:

A project becomes commission-eligible.
A project is added to a commission period.
A commission report is ready for review.
A commission report is approved.
A commission report is rejected.
A commission adjustment is required.
An annual contractor summary is generated.
Annual-report delivery fails.
Contractor notifications must contain only that contractor’s authorized commission information.

4.6 Financial-Report Notifications
Notify the Sales Manager and Comptroller when:

A financial period is ready for preparation.
Financial reports are ready for review.
A report is awaiting a signature.
A report is published.
A period is locked.
A formal adjustment is requested.
A ledger or reconciliation exception blocks publication.
5. Notification Preferences
Users may configure permitted preferences for:

In-app notifications
Email notifications
Reminder frequency
Non-urgent notification delivery
Daily summary notifications, if enabled
The system must not allow users to disable required notifications for:

Approval responsibilities
Payment verification responsibilities
Signature expiration actions
Financial statement approvals
Security events
Required legal or operational retention events
Notification preference changes must be audited.

6. Email Delivery
6.1 Email Message Record
Each outgoing email must retain:

Email message ID
Recipient
Sender identity or configured sender
Subject
Template ID and version
Related customer, project, document, report, or task
Attachment references
Created date and time
Queued date and time
Sent date and time
Delivery status
Failure reason
Retry count
User or system process initiating the email
Audit reference
6.2 Email Statuses
Permitted statuses:

`DRAFT
QUEUED
SENDING
SENT
DELIVERED
BOUNCED
FAILED
CANCELLED
EXPIRED
6.3 Email Templates
Templates must be versioned.

A template must include:

Template ID
Template name
Purpose
Recipient type
Subject template
Body template
Supported variables
Language, if applicable
Active status
Effective date
Retirement date
Approved by
Approval date
Version number
Templates must use controlled project and document data.

Users must not be able to insert unauthorized financial or personal information into a template through ordinary free-text fields.

6.4 Attachments
Attachments must reference stored document records rather than unmanaged local files.

The system must verify:

Recipient authorization
Document status
Document version
File availability
File integrity
Attachment size
Attachment type
A superseded or unauthorized document must not be attached automatically.

6.5 Failed Delivery
When an email fails:

Record the failure status.
Preserve the message and attachment references.
Record the failure reason.
Create an exception notification.
Create a retry task when permitted.
Avoid duplicate messages during retry.
Record every retry attempt.
Notify the responsible authorized user when the message is operationally required.
7. File and Folder Architecture
7.1 Customer Folder
Each customer may have a customer-level file area containing:

Customer agreements
General correspondence
Customer identity or contact documents
Customer-level reports
Archived files
Approved supporting records
Access must follow customer-level authorization.

7.2 Project Folder
Each project must have a project-level file area containing:

Master Sales Record versions
Invoice versions
Contract versions
Certificate versions
Signed-document certificates
Payment evidence
Adjustment documents
Scope-change documents
Equipment records or supporting files
Commission-report references
Project correspondence
Financial-supporting records
Audit-linked file references
Project documents must remain associated with the customer and project.

7.3 File Record
Required fields:

File ID
File name
File type
MIME type
File size
Storage reference
Checksum or integrity hash
Customer ID
Project ID
Document ID, if applicable
Document version
File category
Source system
Uploaded by
Upload date and time
Status
Supersedes file ID, if applicable
Retention classification
Retention start date
Retention end date, if applicable
Access classification
Notes
7.4 File Statuses
Permitted statuses:

UPLOADING
AVAILABLE
PROCESSING
PROCESSING_FAILED
SUPERSEDED
ARCHIVED
QUARANTINED
CANCELLED
A file that is superseded or archived remains historically available to authorized users.

7.5 File Categories
Initial categories include:

Master Sales Record
Invoice
Contract
Certificate
Signature audit certificate
Payment proof
Adjustment support
Payable support
Commission report
Annual contractor summary
Financial report
Inventory support
Fixed-asset support
Loan support
Customer correspondence
Project correspondence
Other approved document
8. Document Versioning and Preservation
Documents generated by the system must retain:

Document ID
Document type
Project ID
Customer ID
Master Sales Record version
Document version
Template version
Generation date and time
Generated by
File reference
Integrity hash
Status
Superseded-document reference
Signed documents must additionally retain:

Signature request reference
Signature audit certificate
Signer records
Signature completion dates
Final signed PDF reference
The system must:

Preserve original documents.
Preserve signed documents.
Preserve superseded documents.
Identify the current document version.
Prevent unauthorized replacement.
Prevent editing of a signed document.
Retain the document version used in each email or report.
Retain the exact attachment sent.
9. File Access Permissions
File access must be determined by:

Customer authorization
Project authorization
Document category
User role
Record status
Action type
Actions requiring permission checks include:

View
Upload
Replace
Download
Print
Email
Send for signature
Archive
Restore
Delete or cancel
Export
Share internally
Signed documents and financial reports require stricter access controls than ordinary project notes.

Files must not be exposed through predictable or guessable storage paths.

10. File Upload and Integrity Controls
The system must validate:

File type
MIME type
File size
File name
Malware or security scan result, where supported
Checksum
Source record
User authorization
A failed or quarantined file must not be available as an official project document.

The system must preserve the original uploaded file and its integrity metadata.

Replacing a file requires a new file version or a new document version. It must not overwrite the original file.

11. Retention and Archival
The baseline requires permanent retention of:

Original records
Revisions
Signed documents
Signature audit certificates
Commission reports
Approved financial statements
Audit records
Relevant payment and payable evidence
The system must support:

Retention classification
Retention start date
Retention status
Archive status
Legal or administrative hold indicator
Authorized restoration
Retention-policy version
Ordinary users must not delete permanently retained files.

Any future destruction process requires a separate approved retention policy and authorization workflow.

12. Sprint 14 Decisions
Decision 14-01 — Event-driven notifications
Notifications are created from controlled system events and workflow transitions.

Decision 14-02 — Notification channels
The initial notification channels are:

In-app notifications
Email
Decision 14-03 — Required notifications
Users may configure nonessential preferences but may not disable required workflow, security, approval, signature-expiration, or financial-report notifications.

Decision 14-04 — Template versioning
Email templates are versioned and the exact template version used for each message is retained.

Decision 14-05 — Attachment traceability
Every attachment must reference a stored document record and exact document version.

Decision 14-06 — Failed-delivery preservation
Failed emails remain preserved with their delivery status, failure reason, attachments, and retry history.

Decision 14-07 — Customer and project file structure
Each customer has a customer-level file area, and each project has a project-level file area linked to the customer.

Decision 14-08 — Document immutability
Signed documents, published financial reports, approved commission reports, and historical document versions cannot be silently replaced or edited.

Decision 14-09 — File replacement
A replacement creates a new file or document version. The original remains preserved.

Decision 14-10 — Access control
File view, download, print, email, signature, export, archive, and restore actions are permission-controlled and audited.

Decision 14-11 — Database authority
Files may be stored in an approved file-storage service, but the database remains authoritative for file metadata, relationships, versions, permissions, and integrity references.

Decision 14-12 — Permanent retention
Required historical documents, signed documents, approved financial reports, commission reports, and audit-supporting files are retained permanently.

Decision 14-13 — Annual contractor-report process
The system supports annual contractor summaries generated every January 5 and emailed to each applicable contractor.

Decision 14-14 — No duplicate signature workflows
Sprint 14 integrates with Sprint 08 and does not create a separate signature-request system.

Decision 14-15 — Notification auditability
Notification creation, delivery, failure, retry, read status, and preference changes are auditable.

13. Open Questions
Open Question 14-01 — File-storage provider
Which file-storage service will store uploaded and generated files?

Recommended default: Use an access-controlled object-storage service with encryption, versioning, checksums, and private access paths.

Open Question 14-02 — Maximum file size
What maximum file size should be permitted?

Recommended default: Establish separate limits for ordinary uploads, generated PDFs, signed documents, and supporting evidence.

Open Question 14-03 — Accepted file types
Which file types should be accepted for general project files?

Recommended default: PDF, PNG, JPEG, DOCX, XLSX, and CSV, subject to security scanning and role permissions.

Open Question 14-04 — Email sender identity
Which sender addresses should be used for:

Customer documents
Signature requests
Contractor reports
Financial reports
System alerts
Recommended default: Use separate configurable sender identities with reply handling defined for each category.

Open Question 14-05 — Email delivery provider
Which email delivery service will be used?

Recommended default: Use a transactional email service that supports delivery events, bounce handling, retries, templates, and message identifiers.

Open Question 14-06 — Customer email consent
What consent or authorization rules apply before emailing customers?

Recommended default: Permit only operational messages related to an active customer or project and retain the recipient address and delivery purpose.

Open Question 14-07 — Contractor email changes
Who may change a contractor’s report-delivery email address?

Recommended default: Require Sales Manager, Comptroller, or authorized database-administrator action with audit logging and confirmation.

Open Question 14-08 — Notification aggregation
Should multiple notifications be combined into a digest?

Recommended default: Keep urgent and required actions immediate; permit daily aggregation only for nonurgent notifications.

Open Question 14-09 — Read receipts
Should email open tracking be enabled?

Recommended default: Track provider delivery and bounce events. Enable open tracking only if approved for the applicable recipient and communication type.

Open Question 14-10 — File retention policy
What retention classifications and legal holds are required beyond the baseline permanent-retention records?

Recommended default: Define classifications in Sprint 15 and prohibit deletion until the retention policy is approved.

Open Question 14-11 — File restoration
Who may restore archived files?

Recommended default: Sales Manager, Comptroller, and authorized Database Administrator users, with a required restoration reason.

Open Question 14-12 — Customer correspondence
Should ordinary emails be copied automatically into the customer/project file?

Recommended default: Store only system-generated and explicitly recorded operational correspondence initially. Defer mailbox synchronization.

Open Question 14-13 — Notification retry policy
How many times should failed emails be retried?

Recommended default: Use configurable exponential retry for transient errors and create a manual exception after the retry limit is reached.

Open Question 14-14 — External recipient verification
Should the system require confirmation before sending documents to a newly entered external email address?

Recommended default: Require authorized confirmation for sensitive documents, including contracts, invoices, certificates, financial reports, and commission reports.

Open Question 14-15 — Customer portal links
Should customers receive links to documents through a portal rather than attachments?

Recommended default: Defer a customer portal. Use controlled document links or attachments according to the approved delivery policy.

14. Acceptance Criteria
Sprint 14 is accepted when:

Controlled system events create the required notifications.
Notifications can be delivered in-app and by email.
Notification statuses distinguish created, queued, sent, delivered, failed, and read states.
Users can configure permitted notification preferences.
Required workflow and security notifications cannot be disabled.
Approval notifications link to the correct authorized workflow.
Signature notifications integrate with Sprint 08 without creating duplicate requests.
Payment-verification notifications link to the correct payment and project.
Commission reports are emailed only to applicable contractors.
Annual contractor summaries are generated and distributed through the approved process.
Failed email delivery creates an actionable exception.
Email retries do not create uncontrolled duplicate messages.
Each email preserves its template version and attachment references.
Customer and project file areas can be created and accessed by authorized users.
Generated, signed, approved, and superseded documents are retained.
Signed documents cannot be edited or silently replaced.
Every stored file has metadata, integrity information, and a record relationship.
Unauthorized users cannot view, download, print, email, or export files.
File replacement creates a new version and preserves the original.
Quarantined or failed files are not treated as official documents.
Project and customer file searches respect role permissions.
Required historical files are retained permanently.
File access, downloads, emails, and notification events are audit logged.
Failed notification and file-processing events appear on the appropriate dashboard.
Document attachments use the correct current or explicitly selected version.
Financial and contractor reports are sent only to authorized recipients.
Notification and file records remain linked to the applicable project, customer, document, report, or task.
Automated Sprint 14 tests pass.
15. Automated Test Scenarios
Notification Tests
Create a task assignment and confirm an in-app notification.
Create an approval task and confirm the authorized approver is notified.
Confirm an unauthorized user is not notified of restricted information.
Create a signature request and confirm integration with Sprint 08.
Create a payment-verification exception and confirm notification creation.
Mark a notification read and retain the read event.
Disable a permitted nonessential notification preference.
Attempt to disable a required notification and confirm rejection.
Confirm urgent notifications are not included only in a delayed digest.
Email Tests
Queue an email from an approved template.
Confirm the template version is retained.
Attach the correct document version.
Confirm delivery status changes are recorded.
Simulate a bounced email.
Confirm a failed message creates an exception.
Retry a transient failure.
Confirm retries do not create duplicate business events.
Confirm the exact sent attachment remains linked to the email record.
File Tests
Create a customer folder.
Create a project folder.
Upload a valid file.
Reject an unsupported file type.
Quarantine a failed security scan.
Store a generated contract.
Store a signed contract and signature certificate.
Supersede a document and confirm the original remains available.
Attempt to edit a signed document.
Attempt unauthorized download.
Archive and restore a file with proper authorization.
Confirm checksums or equivalent integrity values are retained.
Distribution Tests
Send an approved commission report to the applicable contractor.
Confirm another contractor cannot receive or view it.
Generate an annual contractor summary.
Confirm delivery failure creates an exception.
Send a financial report only to authorized recipients.
Confirm the distribution event is audited.
Security and Audit Tests
Attempt to access a project file without project authorization.
Attempt to use a predictable file path.
Attempt to attach an unauthorized document.
Attempt to send a document to an unauthorized recipient.
Confirm notification preference changes are audited.
Confirm file view, download, print, email, and archive actions are audited.
Verify the tamper-evident audit chain after notification and file activity.
16. Sprint 14 Completion Definition
Sprint 14 is complete when:

Notification events and delivery channels operate end to end.
Required workflow notifications are reliable and permission-controlled.
Customer and project files are organized and searchable.
Generated, signed, approved, and superseded documents are preserved.
Email messages and attachments are fully traceable.
Failed delivery and file-processing exceptions are actionable.
File access follows role, record, field, and action permissions.
Retention and archival metadata are implemented.
Notification and file activity is audit logged.
Acceptance tests pass.
Open questions are resolved or formally carried forward.
The Sprint 14 bundle is approved and preserved unchanged.
17. Next Markdown Bundle
The next bundle is:

text


GECC-15-testing-and-integrity.md
Its objective is to implement:

End-to-end testing
Unit and integration testing
Permission testing
Workflow-state testing
Audit-log testing
Document-consistency testing
Financial-reconciliation testing
Payment and payable testing
Inventory and asset testing
Commission testing
Notification and email testing
File-integrity testing
Backup and restore testing
Data-migration validation
Security testing
Performance testing
Failure-recovery testing
Regression testing
Test-data management
Release-blocking defects
Data-integrity monitoring
Audit-chain verification
Acceptance-test evidence
Production-readiness criteria


