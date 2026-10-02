GECC-13-dashboard-and-search.md
GECC Sales Back Office System — Sprint 13
Bundle Metadata
Bundle name: GECC-13-dashboard-and-search.md
Project: GECC Sales Back Office System
Company: Go Ecco Climate Control
Sprint: 13
Sprint name: Activity Dashboard and Search
Baseline: GECC-00-project-baseline.md
Previous bundle: GECC-12-financial-reporting.md
Status: Implementation bundle
Date: 2026-09-20
Next planned bundle: GECC-14-notifications-and-files.md
Primary database: Relational database
System of record: GECC database
Google Sheets: Export or reporting destination only
1. Sprint Objective
Implement the activity dashboard, role-specific dashboard views, authorized global search, operational filters, exception indicators, and related audit logging.

Sprint 13 must ensure that authorized users can quickly identify:

Current project status
Current task
Assigned user or role
Next required action
Overdue work
Expiring documents
Payment and signature exceptions
Installation status
Negative-profit projects
Commission and financial exceptions permitted by their role
Search must provide useful access to authorized records without exposing restricted records, financial information, or the existence of unauthorized data.

2. Sprint Scope
2.1 Included
Activity dashboard
Role-specific dashboard views
Project status summaries
Current-task indicators
Next-action indicators
Assignment indicators
Due dates
Days in current status
Last-activity date and time
Warning indicators
Exception indicators
Customer search
Contact search
Project search
Location search
Project-code search
Equipment search
Serial-number search
Vendor search
Payable search
Document search
Date-range filters
Status filters
Signature filters
Payment filters
State and location filters
Sales Associate and Technician filters
Overdue-task filters
Expiring-document filters
Negative-profit filters
Role-based result filtering
Financial-information masking
Dashboard and search audit logging
Search and dashboard performance testing
2.2 Excluded
Email and notification delivery
Customer-file document storage implementation
Electronic-signature implementation
New workflow states
New accounting calculations
Technician scheduling beyond existing installation-task records
GPS tracking
Technician time tracking
Unauthorized cross-role data access
3. Dashboard Design
3.1 Dashboard Record
The dashboard must be generated from current authoritative records rather than manually maintained status fields.

Each project dashboard item must be capable of displaying:

Project identification code
Customer name
Project location
Assigned Sales Associate
Assigned Technician or Technicians
Current project status
Current task
Assigned role or user
Contract status
Invoice status
Certificate status
Installation status
Payment status
Commission status
Next required action
Due date
Days in current status
Last activity date and time
Warning indicator
Exception indicator
3.2 Dashboard Data Freshness
Dashboard values must reflect current approved system records.

The implementation must define whether values are:

Calculated on request
Updated by event-driven projection
Refreshed on a scheduled interval
Refreshed manually
The dashboard must display the last refresh timestamp where data is not calculated synchronously.

3.3 Current Task
The current task is the next incomplete task permitted by the project state machine.

Examples include:

Review Master Sales Record
Approve Master Sales Record
Review generated contract
Obtain customer contract signature
Obtain GECC contract signature
Obtain GECC invoice signature
Obtain customer invoice signature
Schedule installation
Wait for installation eligibility date
Complete scope of work
Obtain customer certificate signature
Obtain Installation Manager certificate signature
Verify payment
Approve adjustment
Review commission report
The system must not display a task as complete when the underlying required action remains incomplete.

3.4 Next Required Action
The next required action must be derived from:

Current project status
Incomplete lifecycle requirement
Assigned role or user
Due date
Exception state
Required approval
Required signature
Required payment verification
Where multiple actions are available, the state-machine priority order must determine the primary displayed action.

4. Role-Specific Dashboard Views
4.1 Sales Associate
May view:

Assigned projects
Customer and project information
Contract status
Invoice status
Certificate status
Installation status
Payment status as operationally necessary
Current task
Next required action
Due dates
Signature exceptions
Commission status for their own sales
May not view:

Another Sales Associate’s commission information
Unauthorized costs
Company-wide financial statements
Unauthorized payment amounts
Unauthorized payroll data
4.2 Sales Manager
May view:

All projects
All operational tasks
All financial exceptions
All payment and receivable exceptions
All commission exceptions
Negative-profit projects
Audit-related warnings
All authorized dashboard filters
4.3 Comptroller
May view:

All projects
All financial and accounting exceptions
Payment, receivable, payable, commission, and profitability information
Financial-report status
Period-lock and adjustment exceptions
All authorized dashboard filters
4.4 Installation Manager
May view:

Installation-related projects
Assigned technicians
Installation tasks
Scope status
Equipment and serial-number requirements
Installation-related costs when authorized
Certificate-signature tasks
Three-day waiting-period exceptions
May not view unauthorized commission, payroll, or financial-report information.

4.5 Technician
May view:

Assigned projects
Approved scope
Assigned tasks
Equipment information required for assigned work
Serial-number requirements
Installation status
Completion notes
Proposed scope-change tasks
May not view:

Unauthorized prices
Unauthorized costs
Commission information
Payroll information
Financial statements
Unassigned project details
4.6 Accounts Payable Associate
May view:

Authorized vendor records
Authorized payables
Payable statuses
Payment tasks
Project information required for payable duties
May not view unrelated project profitability, commissions, or financial statements.

4.7 Database Administrator
May view authorized operational and data-quality records.

The role may view data-quality exceptions but may not automatically receive financial approval authority.

4.8 Other Contractors
May view only assigned projects, assigned tasks, and required fields.

5. Dashboard Filters
The dashboard must support the following filters, subject to role authorization:

Project status
Customer
Project identification code
Street address
City
State
ZIP code
Sales Associate
Technician
Assigned role
Date range
Payment status
Contract status
Invoice status
Certificate status
Installation status
Signature status
Commission status
Overdue tasks
Expiring documents
Negative-profit projects
Projects requiring action
Current task
Exception type
Filters must be combinable.

For example:

Projects assigned to a Sales Associate with unsigned contracts
Projects in a particular state awaiting installation
Negative-profit completed projects
Unverified bank wires for a date range
Payables overdue for a selected vendor
Projects awaiting Certificate signature
A user must not be able to use a filter to reveal records that are otherwise unauthorized.

6. Exception Indicators
6.1 Signature Exceptions
The dashboard must identify:

Contracts awaiting customer signature
Contracts awaiting GECC signature
Invoices awaiting GECC signature
Invoices awaiting customer signature
Certificates awaiting customer signature
Certificates awaiting Installation Manager signature
Certificates awaiting Comptroller substitute signature
Signature requests nearing expiration
Expired signature requests
6.2 Installation Exceptions
The dashboard must identify:

Installations awaiting scheduling
Installation dates violating the three-day rule
Projects eligible for installation but not scheduled
Projects with incomplete installation records
Projects with completed work but missing certificate signatures
6.3 Payment Exceptions
The dashboard must identify:

Projects with completed work but no verified payment
Unpaid invoices
Partially paid invoices
Unverified cashier’s checks
Unverified bank wires
Bank wires missing a reference number or proof
Overpayments
Payment reversals affecting completed projects
6.4 Scope and Data Exceptions
The dashboard must identify:

Scope changes awaiting approval
Missing equipment serial numbers
Missing required costs
Duplicate project-location warnings
Inconsistent project or invoice data
Missing vendor information
Invalid or incomplete document data
6.5 Profitability and Commission Exceptions
Authorized users may see:

Negative-profit projects
Zero-profit projects
Projects awaiting profitability calculation
Commission adjustments awaiting approval
Commission reports awaiting review
Commission reports awaiting approval
Post-payroll corrections
Financial amounts must remain hidden from users without financial authorization.

6.6 Reporting Exceptions
Authorized users may see:

Financial reports awaiting approval
Financial reports awaiting signatures
Closed-period adjustment requests
Ledger-balance errors
Unreconciled accounts receivable
Unreconciled accounts payable
Inventory or asset reconciliation exceptions
7. Search Requirements
7.1 Searchable Fields
Authorized users must be able to search by:

Customer name
Phone number
Email address
Project identification code
Street address
City
State
ZIP code
Sales Associate
Technician
Serial number
Brand
Model
Project status
Contract status
Invoice status
Certificate status
Payment status
Vendor
Payable
Document type
Date range
7.2 Search Behavior
Search must support:

Exact matching
Partial text matching
Case-insensitive matching
Normalized phone-number matching
Normalized address matching
Project-code matching
Serial-number matching
Filter combinations
Sorting by relevance or date
Pagination
Empty-result handling
Clear filters
Saved searches, if authorized
The system must prevent unauthorized users from discovering restricted records through:

Search results
Result counts
Autocomplete
Error messages
Duplicate warnings
Export functions
Search suggestions
7.3 Search Result Fields
Search-result fields must be role-specific.

Operational users may see:

Project identification code
Customer name
Project location
Assigned personnel
Current project status
Current task
Document statuses
Installation status
Payment status when authorized
Financial users may additionally see:

Invoice amount
Paid amount
Remaining balance
Profitability
Costs
Commission status
Payable status
Unauthorized financial values must be masked or omitted, not displayed as blank values that imply existence.

7.4 Search Result Actions
Actions available from search results must be permission-controlled, including:

Open customer
Open project
Open Master Sales Record
Open document
Open payment
Open payable
Open commission record
Open financial report
Export results
Print results
Download document
8. Search and Dashboard Security
The system must apply authorization at:

Module level
Record level
Field level
Action level
Export level
Document level
A search query must execute against the user’s authorized data scope.

The system must not:

Return unauthorized records
Reveal restricted record counts
Reveal restricted customer existence
Reveal restricted financial totals
Permit unauthorized sorting by hidden financial fields
Permit unauthorized export of hidden fields
Include restricted data in autocomplete suggestions
Every permission failure must be logged.

9. Dashboard and Search Audit Requirements
The system must log:

Dashboard access
Dashboard filter use
Dashboard result viewing
Search execution
Search filters
Search result viewing
Search result export
Search result printing
Search result downloads
Opening a record from search
Opening a document from search
Permission failures
Attempts to search restricted fields
Attempts to access restricted records
Saved-search creation or modification
Dashboard configuration changes
Each event must include:

User identity
Role
Date and time
Activity type
Query or filter reference, where appropriate
Related record identifier, where applicable
Success or failure
Session identifier
Reason when required
Previous audit-log entry reference
Integrity hash or equivalent value
Sensitive search terms must be handled according to the system’s approved logging policy.

10. Performance and Usability Requirements
The system should:

Return ordinary dashboard views within the configured performance target.
Return ordinary searches within the configured performance target.
Use pagination for large result sets.
Avoid loading all records into the browser.
Provide a visible loading state.
Provide a clear no-results state.
Preserve filters when returning from a record.
Provide a clear way to reset filters.
Support keyboard-accessible controls.
Use consistent status and warning terminology.
Display dates and times in the configured company time zone.
Exact performance targets must be approved before production deployment.

11. Data and Search Indexing
The implementation may use relational indexes or a dedicated search index, provided that:

The database remains the authoritative source of truth.
Search indexes do not retain unauthorized data longer than permitted.
Index updates are auditable where required.
Deleted, cancelled, or superseded records are correctly represented.
Permissions are applied at query time.
Search results identify the current record version.
Historical document versions remain discoverable only to authorized users.
Search indexes must be refreshed when changes occur to:

Customer records
Project records
Assignments
Project status
Document status
Payment status
Equipment or serial numbers
Vendor and payable records
Commission status
Financial-report status
12. Sprint 13 Decisions
Decision 13-01 — Authoritative dashboard data
The dashboard is generated from current authoritative records and workflow state, not manually edited dashboard fields.

Decision 13-02 — Current task derivation
Current task is derived from the controlled lifecycle and the next incomplete required action.

Decision 13-03 — Role-specific views
Dashboard content, filters, search fields, result fields, and actions are role-specific.

Decision 13-04 — Search authorization
Search is constrained by the same record, field, and action permissions used elsewhere in the system.

Decision 13-05 — No unauthorized existence disclosure
The system must not reveal the existence, count, or identifying information of records outside the user’s authorization scope.

Decision 13-06 — Financial masking
Financial amounts, commissions, profitability, payroll information, and ledger data are hidden or masked unless authorized.

Decision 13-07 — Exception-first dashboard
The dashboard prioritizes projects requiring action, overdue tasks, expiring documents, payment exceptions, signature exceptions, and data-integrity exceptions.

Decision 13-08 — Combined filters
Dashboard and search filters may be combined, but every filter combination remains subject to authorization.

Decision 13-09 — Project version display
The dashboard and search results display the current approved project or Master Sales Record version, while authorized users may access historical versions through the record.

Decision 13-10 — Search result pagination
Search results use pagination or controlled incremental loading to prevent large-result performance and data-exposure problems.

Decision 13-11 — Audited access
Dashboard access, searches, filter use, record openings, exports, downloads, and permission failures are audit logged.

Decision 13-12 — Search index authority
Any search index is a derived copy. The relational database remains the system of record.

Decision 13-13 — Status terminology
Dashboard and search status labels must use controlled enumerated values from the workflow and financial systems.

Decision 13-14 — Date and time display
Dashboard due dates, activity dates, and search date filters use the company’s configured time zone.

13. Open Questions
Open Question 13-01 — Dashboard refresh model
Should dashboard data refresh:

On every page request
On a scheduled interval
Through event-driven projections
Through manual refresh
Recommended default: Use event-driven updates for important status changes with a manual refresh option and a visible last-refresh timestamp.

Open Question 13-02 — Performance targets
What response-time targets should apply to dashboards and searches?

Recommended default: Establish separate targets for ordinary searches, complex filtered searches, and large exports before production deployment.

Open Question 13-03 — Saved searches
Should users be able to save personal or shared searches?

Recommended default: Permit personal saved searches in Sprint 13 and defer shared organizational searches until Sprint 14 or later.

Open Question 13-04 — Search-term retention
How long should search terms and filter details remain in audit records?

Recommended default: Retain the audit event and a protected query reference while minimizing storage of sensitive free-text search terms.

Open Question 13-05 — Financial masking
Should unauthorized users see masked values such as •••• or no value at all?

Recommended default: Omit unauthorized financial fields rather than display masked values that may reveal the existence of restricted information.

Open Question 13-06 — Dashboard ownership
Should users be able to customize dashboard layouts?

Recommended default: Provide role-specific fixed layouts initially and defer personal widget customization.

Open Question 13-07 — Exception priority
How should multiple simultaneous exceptions be prioritized?

Recommended default: Prioritize safety or legal timing requirements, then overdue tasks, expiring signatures, payment blockers, approval blockers, data-quality issues, and informational warnings.

Open Question 13-08 — Expiring-document threshold
How many days before expiration should a document appear as nearing expiration?

Recommended default: Display warnings at 14 days, 7 days, and 1 day before expiration.

Open Question 13-09 — Search of historical versions
Should historical Master Sales Record and document versions appear in ordinary search results?

Recommended default: Show current versions in ordinary search and provide historical versions through an authorized version-history action.

Open Question 13-10 — Search of cancelled projects
Should cancelled projects appear in ordinary results?

Recommended default: Exclude cancelled projects by default but provide an authorized status filter to include them.

Open Question 13-11 — Search export limits
What limits should apply to search-result exports?

Recommended default: Require authorization, log every export, apply maximum row limits, and require an export reason for sensitive financial results.

Open Question 13-12 — Dashboard financial totals
Should authorized financial users see aggregate totals directly on the dashboard?

Recommended default: Provide aggregate totals only to the Sales Manager and Comptroller, with separate permission controls for each financial category.

Open Question 13-13 — Stale task handling
When a project is reopened or superseded, how should old tasks appear?

Recommended default: Mark them superseded, retain them in history, and exclude them from active-task counts unless an unresolved action remains.

14. Acceptance Criteria
Sprint 13 is accepted when:

Authorized users can access a role-appropriate activity dashboard.
Dashboard projects display the required project, assignment, status, task, due-date, and activity fields.
Current tasks are derived from the controlled lifecycle.
Next required actions are accurately identified.
Days in current status are calculated correctly.
Last activity date and time are displayed correctly.
Dashboard filters work individually and in combination.
Signature, installation, payment, scope, profitability, commission, payable, and reporting exceptions appear correctly.
Sales Associates see only authorized projects and their own commission information.
Technicians see only assigned or authorized projects.
Contractors see only assigned projects and required fields.
Sales Managers and Comptrollers can view authorized company-wide information.
Financial amounts are hidden from unauthorized users.
Unauthorized records do not appear in dashboard counts or results.
Users can search by all required customer, project, location, personnel, equipment, vendor, payable, document, status, and date fields.
Partial, exact, case-insensitive, normalized phone, address, project-code, and serial-number searches work as designed.
Search results are paginated or otherwise controlled for large result sets.
Search result fields vary according to the user’s permissions.
Unauthorized fields are not exposed through search, sorting, filters, autocomplete, or exports.
Opening a record from search applies normal permissions.
Historical versions are handled according to the approved search policy.
Cancelled and superseded records follow the configured search behavior.
Dashboard and search activity is recorded in the tamper-evident audit log.
Permission failures are logged.
Export, print, and download actions are authorized and audited.
Dashboard data refreshes according to the configured refresh model.
Search and dashboard performance meets the approved targets.
Search results remain consistent with authoritative database records.
Financial and operational reports linked from the dashboard open only for authorized users.
Automated Sprint 13 tests pass.
15. Automated Test Scenarios
Dashboard Tests
Display a project with all required dashboard fields.
Confirm the current task for a project awaiting customer contract signature.
Confirm the current task for a project awaiting payment verification.
Confirm the current task for a project awaiting Certificate signature.
Confirm days in current status.
Confirm last activity updates after a recorded action.
Filter by project status.
Filter by Sales Associate.
Filter by Technician.
Filter by state.
Filter by payment status.
Filter by overdue tasks.
Filter by expiring documents.
Filter by negative-profit projects.
Combine multiple filters.
Confirm that unauthorized projects do not affect counts.
Role Tests
Confirm a Sales Associate sees assigned projects only.
Confirm a Sales Associate sees only their own commission information.
Confirm a Technician sees assigned projects only.
Confirm an Installation Manager sees installation-related projects.
Confirm an Accounts Payable Associate sees authorized payable records.
Confirm the Sales Manager sees all authorized project records.
Confirm the Comptroller sees all authorized financial records.
Confirm contractors see only assigned records.
Confirm unauthorized financial amounts are omitted.
Search Tests
Search by customer name.
Search by phone number in multiple formatting styles.
Search by email address.
Search by project identification code.
Search by address.
Search by serial number.
Search by brand and model.
Search by vendor.
Search by payable.
Search by document type.
Filter search by date range.
Filter search by status.
Combine search terms and filters.
Confirm partial and case-insensitive search behavior.
Confirm pagination.
Confirm empty-result behavior.
Confirm cancelled-project behavior.
Confirm historical-version behavior.
Security Tests
Attempt to search for an unauthorized customer.
Confirm no restricted record count is returned.
Attempt to sort by an unauthorized financial field.
Attempt to export unauthorized results.
Attempt to access a restricted document from search.
Confirm every permission failure is audited.
Confirm autocomplete does not expose restricted records.
Audit Tests
Access the dashboard.
Run a search.
Apply filters.
Open a result.
Export results.
Print results.
Download a document.
Confirm all events appear in the tamper-evident audit chain.
16. Sprint 13 Completion Definition
Sprint 13 is complete when:

Role-specific dashboards are operational.
Current tasks and next actions are accurately derived from workflow state.
Required operational and exception indicators are displayed.
Search covers all approved search fields.
Search respects record, field, action, and export permissions.
Financial information is protected from unauthorized exposure.
Dashboard and search results remain consistent with authoritative records.
Access, searches, exports, and failures are audit logged.
Performance and usability requirements are met.
Acceptance tests pass.
Open questions are resolved or formally carried forward.
The Sprint 13 bundle is approved and preserved unchanged.
17. Next Markdown Bundle
The next bundle is:

text


GECC-14-notifications-and-files.md
Its objective is to implement:

In-app notifications
Email notifications
Customer and project folders
Document storage references
Document metadata
File-version management
Delivery tracking
Signature-related reminders
Expiration notifications
Approval notifications
Payment-verification notifications
Commission-report distribution
Annual-report email process
Notification preferences
Failed-delivery handling
File-access permissions
File-download audit records
Customer/project document search integration
Retention and archival rules
Notification and file-management testing

