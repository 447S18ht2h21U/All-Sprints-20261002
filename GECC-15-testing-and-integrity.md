GECC-15-testing-and-integrity.md
GECC Sales Back Office System — Sprint 15
Bundle Metadata
Bundle name: GECC-15-testing-and-integrity.md
Project: GECC Sales Back Office System
Company: Go Ecco Climate Control
Sprint: 15
Sprint name: Testing, Security, and Data Integrity
Baseline: GECC-00-project-baseline.md
Previous bundle: GECC-14-notifications-and-files.md
Status: Implementation bundle
Date: 2026-09-20
Next planned bundle: GECC-16-deployment-and-operations.md
Primary database: Relational database
System of record: GECC database
Google Sheets: Export or reporting destination only
1. Sprint Objective
Implement the testing, security validation, data-integrity verification, backup testing, recovery testing, and release-readiness controls required before deployment.

Sprint 15 must ensure that:

Core workflows operate end to end.
Permissions prevent unauthorized access and changes.
Documents remain consistent with approved database records.
Financial calculations reconcile to source records.
Audit entries remain append-only and tamper-evident.
Backups can be restored successfully.
Notifications and files remain traceable.
Data migration can be validated.
Critical defects block release.
Production deployment has documented acceptance evidence.
2. Sprint Scope
2.1 Included
Test strategy
Unit testing
Integration testing
End-to-end testing
Regression testing
Workflow-state testing
Permission testing
Field-level security testing
Audit-log testing
Document-consistency testing
Financial reconciliation testing
Payment and payable testing
Inventory and asset testing
Commission testing
Notification and email testing
File-integrity testing
Backup and restore testing
Migration validation
Security testing
Performance testing
Failure-recovery testing
Test-data management
Defect management
Release-blocking criteria
Audit-chain verification
Data-integrity monitoring
Acceptance evidence
2.2 Excluded
Production deployment execution
Final production hosting configuration
Live customer-data migration
Penetration testing by an external provider, unless separately authorized
Formal financial-statement audit
Legal certification of electronic signatures
Disaster-recovery infrastructure procurement
Ongoing operations after deployment
3. Test Strategy
3.1 Test Levels
Testing must be performed at the following levels:

Unit testing
Component testing
Integration testing
Database constraint testing
Workflow testing
End-to-end testing
Security and permission testing
Performance testing
Backup and restore testing
User acceptance testing
Regression testing
3.2 Test Environments
The project must maintain separate environments for:

Development
Test or quality assurance
User acceptance testing
Production
Production data must not be copied into lower environments without approved de-identification or masking.

Each environment must have:

Environment identifier
Application version
Database version
Configuration version
Deployment date
Responsible administrator
Test-data source
Known limitations
3.3 Test Evidence
Each test execution must retain:

Test ID
Test name
Environment
Application version
Database version
Tester
Execution date and time
Input data
Expected result
Actual result
Pass or fail status
Evidence reference
Defect reference, if failed
Retest result
Test evidence must be retained according to the project’s approved retention policy.

4. Test Data Management
4.1 Test Data Categories
Test data must include:

Customers with multiple contacts
Multiple project locations
Duplicate customer candidates
Projects in every lifecycle state
Projects with scope changes
Projects with multiple equipment records
Serialized and nonserialized inventory
Verified and unverified payments
Partial payments
Overpayments
Refunds and credits
Payables in every status
Positive-profit projects
Zero-profit projects
Negative-profit projects
Commission-eligible projects
Commission-adjustment projects
Fixed assets
Loans
Open and locked reporting periods
Signed and unsigned documents
Expired signature requests
Failed emails
Archived and superseded files
Unauthorized-access scenarios
4.2 Test Data Rules
Test data must:

Clearly identify its test purpose.
Avoid real customer secrets.
Avoid live payment information.
Avoid real bank references.
Avoid real contractor tax information.
Be resettable where required.
Support repeatable test execution.
Include known expected totals for reconciliation tests.
4.3 Production-Like Data Validation
Before migration, sample production-like records must be tested for:

Required fields
Duplicate records
Invalid project codes
Invalid statuses
Missing relationships
Invalid dates
Invalid amounts
Unsupported payment methods
Missing vendor references
Duplicate serial numbers
5. End-to-End Test Scenarios
5.1 Standard Project Lifecycle
Test the complete path:

Create customer.
Create project.
Generate project identification code.
Create Master Sales Record.
Approve Master Sales Record.
Generate invoice, contract, and certificate.
Complete contract signatures.
Complete invoice signatures.
Schedule installation.
Enforce the three-day waiting period.
Perform scope of work.
Complete Certificate signatures.
Create payables.
Record customer payment.
Verify payment.
Mark project complete.
Calculate commissions.
Include the project in the correct commission report.
Approve and distribute the report.
Include transactions in the appropriate financial period.
5.2 Scope-Change Lifecycle
Test:

Proposed scope change
Scope-change pending state
Authorized approval or rejection
New Master Sales Record version
Revised document generation
New signature sequence
Preservation of original records
Revised cost and profitability calculations
Commission impact
5.3 Payment Exception Lifecycle
Test:

Cashier’s check entered without confirmation
Bank wire entered without reference or proof
Payment rejection
Payment verification
Partial payment
Overpayment
Refund
Payment reversal
Project-completion impact
Commission-adjustment impact
5.4 Financial-Period Lifecycle
Test:

Open period
Report preparation
Review
Sales Manager approval
Comptroller approval
Signature
Publication
Period lock
Formal adjustment
Preservation of original report
6. Permission and Security Testing
6.1 Access Dimensions
Testing must cover permissions at:

Screen level
Module level
Record level
Field level
Action level
Document level
Export level
Report level
API or service level, if applicable
6.2 Required Security Tests
The test suite must verify that:

Sales Associates see only authorized projects.
Sales Associates see only their own commission data.
Technicians see only assigned projects.
Contractors see only assigned records and fields.
Accounts Payable Associates cannot modify project costs.
Installation Managers cannot sign financial statements.
Database Administrators cannot approve financial records without assigned authority.
Unauthorized users cannot view payment amounts.
Unauthorized users cannot view profitability.
Unauthorized users cannot view payroll or commission reports.
Unauthorized users cannot access restricted files.
Unauthorized users cannot export restricted search results.
Permission failures are logged.
Search does not reveal restricted record existence.
Direct URL or object-reference manipulation cannot bypass authorization.
Archived and superseded files retain access controls.
6.3 Authentication Testing
Test:

Valid login
Invalid login
Failed-login logging
Logout
Session expiration
Concurrent sessions, if restricted
Password reset, if implemented
Disabled user access
Role changes
Access after role removal
Access after contractor status becomes inactive
7. Workflow and State-Machine Testing
Each state transition must be tested for:

Valid transition
Invalid transition
Required fields
Required approval
Required signature
Required payment verification
Required role
Required exception handling
Audit entry
Notification creation
Dashboard update
Specific tests must confirm that users cannot:

Skip Master Sales Record approval.
Generate documents from incomplete records.
Send an invoice before the contract is signed.
Schedule installation before required signatures.
Install before the three-day waiting period.
Complete work without required scope controls.
Complete a project before verified payment.
Earn commissions before project completion.
Publish financial reports without required approvals.
Edit locked periods without a formal adjustment.
Approve their own restricted action when separation of duties prohibits it.
8. Document-Consistency Testing
Generated documents must be compared against the approved source records.

Test consistency for:

Customer name
Customer contact details
Project location
Project identification code
Sales Associate
Scope of work
Equipment
Serial numbers
Retail price
Payment option
State-specific license artwork
Document version
Master Sales Record version
Signature fields
Revision references
The test suite must verify that:

Invoice, contract, and certificate data agree.
Revised documents use the revised Master Sales Record.
Original documents remain unchanged.
Superseded documents are clearly identified.
Signed PDFs remain linked to the correct signature audit certificate.
State-specific artwork is correct.
Unauthorized artwork overrides are blocked.
Generated documents cannot alter underlying project data.
9. Financial Reconciliation Testing
9.1 Ledger Tests
Verify:

text


Total Debits = Total Credits
Test:

Cash receipts
Customer refunds
Vendor payments
Payable creation
Inventory receipts
Inventory issues
Fixed-asset acquisition
Depreciation
Loan origination
Loan payments
Formal adjustments
9.2 Receivable Reconciliation
Verify:

text


Remaining Balance =
Adjusted Amount Due
- Verified Amount Paid
Reconcile:

Invoice totals
Approved adjustments
Verified payments
Refunds
Reversals
Accounts-receivable balances
9.3 Payable Reconciliation
Verify:

text


Outstanding Balance =
Amount Owed
- Amount Paid
Reconcile:

Certificate-triggered payables
Equipment payables
Dealer-fee payables
Materials payables
Vendor payments
Payable adjustments
9.4 Profitability Reconciliation
Reconcile:

Invoice amount
Equipment Cost
Material Cost
Dealer Fee
Lead Cost
Technician Cost
Damon Cost
Jerry Cost
Gross Profit
Ryan commission
Doug commission
Net Profit
Sales Associate share
GECC share
9.5 Financial-Report Reconciliation
Verify:

Balance sheet balances.
Ending cash reconciles to the ledger.
Accounts receivable agrees with source records.
Accounts payable agrees with source records.
Inventory agrees with FIFO layers.
Fixed assets agree with asset records.
Accumulated depreciation agrees with depreciation entries.
Loan balances agree with loan records.
Retained earnings reconcile.
Published report versions remain reproducible.
10. Audit-Log Testing
10.1 Audit Coverage
Verify audit logging for:

Login
Failed login
Logout
Views
Searches
Creates
Edits
Approvals
Exports
Prints
Downloads
Emails
Signatures
Permission failures
Restricted-access attempts
Administrative changes
Financial changes
File changes
Notification changes
Report publication
Period locking
Formal adjustments
10.2 Hash-Chain Testing
The test suite must verify that:

Each audit entry references the previous entry.
Each entry contains the expected integrity value.
Reordering an entry causes verification failure.
Deleting an entry causes verification failure.
Altering an entry causes verification failure.
Inserting an entry causes verification failure.
Replaying an entry is detected or rejected.
Verification results are recorded.
Only authorized users can run integrity verification.
Verification does not modify the audit chain.
10.3 Audit Immutability
Confirm that ordinary application users cannot:

Edit audit entries
Delete audit entries
Reorder audit entries
Disable audit logging
Bypass audit logging through exports or direct actions
11. Document, File, and Email Testing
Test:

File upload
File-type validation
File-size validation
Security scanning
File quarantine
File checksum
Document versioning
Superseded files
Signed-document preservation
File download authorization
File print authorization
Email attachment authorization
Email delivery
Email failure
Email retry
Duplicate-email prevention
Notification read status
Annual contractor-summary delivery
Financial-report distribution
File restoration
The exact attachment sent must remain associated with the email record.

12. Inventory, Asset, and Loan Testing
12.1 Inventory
Test:

Product identity
Serialized item
Nonserialized quantity
Receipt
Cost-layer creation
FIFO issuance
Multiple-layer issuance
Project allocation
Return
Damage
Scrap
Cost correction
Duplicate serial prevention
Negative inventory prevention
12.2 Fixed Assets
Test:

Vehicle
Computer
Office
Additional asset category
Acquisition
Placed-in-service date
Straight-line depreciation
Accumulated depreciation
Net book value
Disposal
Disposal adjustment
Historical depreciation preservation
12.3 Loans
Test:

Loan origination
Principal balance
Interest rate
Loan payment
Principal allocation
Interest allocation
Fee allocation
Loan correction
Loan payoff
Loan report reconciliation
13. Performance Testing
Performance testing must cover:

Login
Dashboard loading
Dashboard filtering
Project search
Customer search
Serial-number search
Financial report generation
Large document lists
Large audit-log searches
Commission-report generation
Annual-summary generation
File upload
File download
Email queue processing
Audit-chain verification
Database backup
Database restore
The project must define and record:

Expected response time
Maximum concurrent users
Maximum supported record volume
Maximum file size
Maximum report size
Maximum export size
Acceptable background-processing delay
Failure behavior when limits are exceeded
14. Backup and Restore Testing
14.1 Backup Requirements
Backups must include, as applicable:

Relational database
File metadata
Stored files
Configuration
Templates
Artwork versions
Audit records
Search-index rebuild information
Notification and email history
14.2 Restore Tests
Test restoration of:

Full database
Partial database
File metadata
Document files
Audit chain
Configuration
Template versions
Artwork versions
The restored system must verify:

Record counts
Foreign-key relationships
File references
File checksums
Audit-chain integrity
Ledger balances
Report reproducibility
Version-history preservation
14.3 Recovery Evidence
Each restore test must record:

Backup identifier
Backup date and time
Restore date and time
Restored environment
Duration
Records restored
Files restored
Integrity results
Exceptions
Tester
Approval
15. Migration Validation
Before migration, the system must validate:

Customer duplicates
Project duplicates
Invalid project codes
Missing customer references
Missing project locations
Unsupported statuses
Invalid dates
Invalid currency amounts
Duplicate serial numbers
Missing vendor references
Invalid contractor assignments
Missing historical document references
Inconsistent spreadsheet values
Migration validation must produce:

Accepted-record count
Rejected-record count
Corrected-record count
Duplicate count
Unresolved-exception count
Migration report
Approval record
No production migration may proceed with unresolved release-blocking data errors.

16. Defect Management
16.1 Defect Severity
Defects must be classified as:

`BLOCKER
CRITICAL
HIGH
MEDIUM
LOW
16.2 Release-Blocking Defects
A release is blocked by:

Unauthorized access to restricted information
Unauthorized financial modification
Incorrect payment verification
Incorrect project completion
Incorrect commission calculation
Incorrect financial report totals
Unbalanced journal entry
Audit-log integrity failure
Data loss
Document inconsistency affecting legal or financial terms
Failed backup restoration
Duplicate or missing critical records
Inability to preserve signed or published documents
16.3 Defect Lifecycle
Permitted defect statuses:

OPEN
TRIAGED
IN_PROGRESS
RESOLVED
READY_FOR_RETEST
VERIFIED
REOPENED
DEFERRED
CLOSED
Deferred defects require:

Business impact
Risk assessment
Owner
Target sprint or release
Approval
17. User Acceptance Testing
User acceptance testing must include representatives or approved test owners for:

Sales Associate
Sales Manager
Comptroller
Installation Manager
Technician
Accounts Payable Associate
Database Administrator
Other contractor access, where applicable
User acceptance testing must confirm:

Workflow usability
Dashboard usefulness
Search accuracy
Document consistency
Payment and payable workflows
Commission reports
Financial reports
Notification handling
File access
Permission behavior
Error-message clarity
Each user acceptance test must have an owner and approval result.

18. Sprint 15 Decisions
Decision 15-01 — Multi-level testing
The project will use unit, integration, end-to-end, security, performance, backup, restore, migration, and user acceptance testing.

Decision 15-02 — Separate environments
Development, test, user acceptance, and production environments remain separate.

Decision 15-03 — Protected test data
Test environments use synthetic, masked, or otherwise approved data. Live sensitive records are not copied into test environments without authorization.

Decision 15-04 — Release-blocking security defects
Any defect that permits unauthorized access to restricted records, fields, documents, reports, or exports blocks release.

Decision 15-05 — Release-blocking financial defects
Any defect that causes incorrect payment status, project completion, profitability, commission, ledger balance, or financial-report totals blocks release.

Decision 15-06 — Audit integrity gate
The release is blocked if audit-chain verification fails or if required events bypass the audit system.

Decision 15-07 — Immutable historical records
Testing must verify that approved, signed, published, paid, posted, and audited records cannot be silently overwritten.

Decision 15-08 — Reconciliation gate
Financial reports cannot be approved for release unless required reconciliation tests pass.

Decision 15-09 — Backup and restore gate
A tested backup that cannot be restored with verified integrity is not acceptable for production release.

Decision 15-10 — Repeatable tests
Critical tests must be automated or repeatable using controlled test data.

Decision 15-11 — Defect evidence
Every failed test must reference a defect or approved explanation.

Decision 15-12 — UAT approval
User acceptance testing requires documented approval from authorized business representatives.

Decision 15-13 — Migration gate
Production migration requires a completed migration-validation report and approval of unresolved exceptions.

Decision 15-14 — Security test coverage
Security testing includes direct user-interface actions, search, exports, document links, APIs or services where applicable, and manipulated object references.

Decision 15-15 — Regression testing
Every release candidate must pass regression tests for all previously approved sprints.

19. Open Questions
Open Question 15-01 — External penetration testing
Is an independent penetration test required before production deployment?

Recommended default: Obtain an independent security review before production if budget and schedule permit. At minimum, complete documented internal authorization and access testing.

Open Question 15-02 — Performance targets
What exact response-time and concurrency targets define acceptable performance?

Recommended default: Establish targets by function before final release testing, including dashboard, search, report generation, file operations, and login.

Open Question 15-03 — Recovery objectives
What are the required:

Recovery Point Objective
Recovery Time Objective
Recommended default: Define these before Sprint 16 deployment planning and test restoration against them.

Open Question 15-04 — Backup frequency
How frequently must backups run?

Recommended default: Use scheduled daily full or equivalent backups with more frequent transaction or incremental protection where supported.

Open Question 15-05 — Backup retention
How many backup versions must be retained?

Recommended default: Define a rotation that includes recent daily backups, weekly backups, and monthly archival backups.

Open Question 15-06 — Test-data reset
Should test environments reset automatically after each test cycle?

Recommended default: Maintain repeatable seed data and provide an authorized reset process rather than automatically resetting shared environments.

Open Question 15-07 — Audit-chain verification frequency
How frequently should production audit-chain verification run?

Recommended default: Run scheduled verification at least daily and after any restoration or migration.

Open Question 15-08 — Migration reconciliation tolerance
What differences are acceptable between source and migrated data?

Recommended default: No unexplained differences in financial records, project codes, signed documents, payment records, or audit history. Any other tolerance requires explicit approval.

Open Question 15-09 — Data masking standard
Which fields require masking in lower environments?

Recommended default: Mask customer contact details, payment references, contractor contact information, financial values, signature information, and uploaded documents unless synthetic replacements are used.

Open Question 15-10 — UAT signers
Which individuals have authority to approve user acceptance testing?

Recommended default: Require approval from the project owner or Comptroller plus role representatives for operational areas affected by the release.

Open Question 15-11 — Accessibility testing
Which accessibility standard should be used?

Recommended default: Target WCAG 2.1 AA or the applicable current organizational standard.

Open Question 15-12 — Browser and device support
Which browsers and devices must be supported?

Recommended default: Define a supported browser matrix before production deployment and test the primary desktop workflows.

Open Question 15-13 — Defect acceptance
Who may approve deferral of a High-severity defect?

Recommended default: Require approval from the project owner and Comptroller, with documented risk and mitigation.

Open Question 15-14 — Disaster recovery environment
Will restoration occur in a separate recovery environment or the primary environment?

Recommended default: Use a separate recovery environment for testing to avoid destructive restoration tests.

20. Acceptance Criteria
Sprint 15 is accepted when:

Unit tests cover critical calculations and validation rules.
Integration tests cover cross-module data flow.
End-to-end tests cover the complete project lifecycle.
Scope-change workflows are tested.
Payment, verification, refund, and adjustment workflows are tested.
Payable creation and payment workflows are tested.
Inventory FIFO and serial-number rules are tested.
Fixed-asset depreciation and disposal are tested.
Loan principal and interest tracking are tested.
Cost, profitability, and commission calculations reconcile.
Financial reports reconcile to the ledger and source records.
Balance-sheet and cash-flow integrity tests pass.
Workflow-state bypass attempts are blocked.
Role, record, field, action, document, and export permissions are tested.
Restricted search and direct-record access attempts are blocked.
Unauthorized access attempts are audited.
Generated documents match approved Master Sales Record data.
Signed and published documents remain immutable.
Email attachments match the intended document version.
Notification delivery failures are handled and audited.
File upload, quarantine, versioning, archival, and restoration are tested.
Audit-chain tampering tests detect alteration, deletion, insertion, and reordering.
Posted journal entries cannot be edited or deleted.
Locked financial periods cannot be ordinarily edited.
Formal adjustments preserve original reports.
Backup restoration succeeds with verified database and file integrity.
Migration-validation results are documented.
No unresolved Blocker or Critical defects remain.
All deferred High-severity defects have documented approval.
Required user acceptance tests are approved.
Regression tests for Sprints 00–14 pass.
Performance results meet approved targets.
Release evidence is complete and reproducible.
21. Automated Test Scenarios
Core Regression Tests
Run the approved baseline acceptance tests.
Run Sprint 08 electronic-signature tests.
Run Sprint 09 payment and payable tests.
Run Sprint 10 cost and commission tests.
Run Sprint 11 inventory, asset, and loan tests.
Run Sprint 12 financial-reporting tests.
Run Sprint 13 dashboard and search tests.
Run Sprint 14 notification and file tests.
Integrity Tests
Alter a posted journal entry.
Delete an audit entry.
Reorder audit entries.
Insert an audit entry.
Alter a signed document.
Alter a published financial report.
Change an approved Master Sales Record without revision.
Change a historical commission calculation.
Change a closed-period transaction.
Confirm every attempt is blocked and audited.
Recovery Tests
Restore a full database backup.
Restore file metadata and stored documents.
Verify document checksums.
Verify foreign keys.
Verify ledger balances.
Verify audit-chain integrity.
Rebuild or restore the search index.
Reproduce a published financial report.
Confirm restored permissions and configurations.
22. Sprint 15 Completion Definition
Sprint 15 is complete when:

All critical workflows have automated or repeatable tests.
Security and permission controls are verified.
Documents, files, reports, and financial records reconcile.
Audit-chain integrity is verified.
Backup and restore testing succeeds.
Migration validation is documented.
Performance testing meets approved targets.
User acceptance testing is approved.
No unresolved release-blocking defects remain.
Deferred defects have documented owners and approvals.
Regression testing for Sprints 00–14 passes.
Release evidence is complete.
Open questions are resolved or formally carried forward.
The Sprint 15 bundle is approved and preserved unchanged.
23. Next Markdown Bundle
The next bundle is:

text


GECC-16-deployment-and-operations.md
Its objective is to implement:

Deployment instructions
Environment configuration
Production setup
Initial-user setup
Role assignment
Permission configuration
Data-migration plan
Migration execution controls
Backup procedures
Recovery procedures
Monitoring
Alerting
Audit-chain verification operations
Administrator guide
User guides
Operational runbooks
Release checklist
Rollback procedures
Incident response
Support procedures
Maintenance procedures
Configuration management
Version management
Production acceptance
Go-live readiness
Post-deployment validation
Handover and operational ownership


