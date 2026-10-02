# GECC-02-auth-and-permissions.md

# GECC Sales Back Office System — Sprint 02 Authentication, Roles, and Permissions

## Bundle Metadata

- Bundle name: GECC-02-auth-and-permissions.md
- Project: GECC Sales Back Office System
- Company: Go Ecco Climate Control
- Company abbreviation: GECC
- Bundle type: Sprint 02 design and implementation bundle
- Status: Approved design; ready for implementation
- Created date: 2026-09-20
- Previous bundle: GECC-01-data-model.md
- Next planned bundle: GECC-03-audit-log.md
- Database: PostgreSQL
- Primary-key strategy: Numeric BIGINT identity keys
- Business timezone: America/New_York
- System of record: PostgreSQL relational database

---

## 1. Sprint Objective

Implement the authentication and authorization foundation required to ensure that:

- Only authenticated users can access the system.
- Each user receives only the permissions appropriate to their role.
- Access can be restricted at the screen, record, field, and action levels.
- Sales Associates see only commission information for their own sales.
- Technicians see only assigned or authorized project information.
- Financial, payroll, commission, and audit information is restricted.
- Permission failures are logged.
- Authorization decisions are enforceable by the application and database access layer.

---

## 2. Sprint 02 Deliverables

Sprint 02 must produce:

1. Authentication design
2. User-account model
3. Role model
4. Permission model
5. Role-permission assignments
6. Record-level access scopes
7. Field-level access rules
8. Action-level authorization rules
9. Session-management design
10. Login and logout behavior
11. Failed-login handling
12. Account activation, deactivation, and lockout behavior
13. Permission-denial handling
14. Approval-separation rules
15. Commission-visibility controls
16. Financial-data access controls
17. Authorization test cases
18. Integration hooks for Sprint 03 audit logging

---

## 3. Authorization Model

GECC uses a combined authorization model consisting of:

1. Role-based access control
2. Record-scope access control
3. Field-level access control
4. Action-level access control

### 3.1 Role-based access control

RBAC determines the general capabilities granted to a user's role.

Examples:

- Sales Associates may create and update unapproved sales records.
- Sales Managers and Comptrollers may approve sales and financial records.
- Technicians may update installation completion data.
- Accounts Payable Associates may update authorized payable payments.

### 3.2 Record-scope access control

Record scope restricts which records a role may access.

Examples:

- A Sales Associate may access assigned customers and projects.
- A Technician may access assigned projects.
- An Other Contractor may access only specifically assigned projects and fields.
- A Sales Manager and Comptroller may access all business data.
- A Database Administrator may access authorized database functions but does not automatically receive approval rights.

### 3.3 Field-level access control

Certain fields require additional restrictions even when the surrounding record is visible.

Examples:

- Retail price
- Approved costs
- Commission amounts
- Payroll information
- Financial statement data
- Bank information
- Payment verification details
- Audit-log integrity fields

### 3.4 Action-level access control

The system must separately authorize actions such as:

- Create
- View
- Edit
- Approve
- Sign
- Reopen
- Generate
- Send
- Export
- Print
- Download
- Verify payment
- Enter overhead expense
- Approve adjustment
- Publish financial statement
- Lock financial period
- View audit log
- Export audit log

---

## 4. Authentication Requirements

The system must require authenticated login before access to protected functionality.

### 4.1 Required authentication behavior

The system must provide:

- Unique login identity for every user
- Account activation and deactivation
- Password or approved external authentication method
- Failed-login tracking
- Account lockout or equivalent protective response
- Session creation after successful authentication
- Session termination on logout
- Session expiration after an approved inactivity period
- Re-authentication for sensitive actions where required
- No shared user accounts
- No anonymous access to business records
- No authorization based only on hidden user-interface controls

### 4.2 Sensitive actions

The following actions require explicit permission checks at the point of action:

- Approving a Master Sales Record
- Approving retail price or costs
- Reopening an approved record
- Approving credits, refunds, or adjustments
- Signing an invoice or contract
- Signing a certificate as substitute Comptroller
- Approving commission changes
- Approving commission reports
- Publishing financial statements
- Locking a financial period
- Accessing the tamper-evident audit log
- Exporting restricted financial or commission information

---

## 5. Permission Data Model

### 5.1 `permission`

| Field | Type | Requirement |
|---|---|---|
| id | BIGINT | Primary key |
| permission_code | VARCHAR(100) | Required, unique |
| resource_type | VARCHAR(80) | Required |
| action_code | VARCHAR(50) | Required |
| description | TEXT | Required |
| is_sensitive | BOOLEAN | Required |
| record_status | VARCHAR(30) | Required |
| is_active | BOOLEAN | Required |
| retired_at | TIMESTAMPTZ | Optional |
| retired_by | BIGINT | Optional foreign key |
| retirement_reason | TEXT | Optional |

Example permission codes:

```text
customer.view
customer.create
customer.edit
project.view
project.create
project.edit
master\_sales\_record.approve
master\_sales\_record.reopen
invoice.sign
contract.sign
certificate.sign
payment.verify
adjustment.approve
commission.view\_own
commission.view\_all
commission\_report.approve
financial\_report.publish
financial\_period.lock
audit\_log.view
audit\_log.export
5.2 role_permission
Field	Type	Requirement
id	BIGINT	Primary key
role_id	BIGINT	Required foreign key
permission_id	BIGINT	Required foreign key
effective_from	DATE	Required
effective_to	DATE	Optional
granted_by	BIGINT	Required foreign key
grant_reason	TEXT	Required
record_status	VARCHAR(30)	Required
is_active	BOOLEAN	Required
retired_at	TIMESTAMPTZ	Optional
retired_by	BIGINT	Optional
retirement_reason	TEXT	Optional
Constraint:

text


UNIQUE(role\_id, permission\_id, effective\_from)
5.3 record_access_scope
Field	Type	Requirement
id	BIGINT	Primary key
user_id	BIGINT	Required foreign key
resource_type	VARCHAR(80)	Required
resource_id	BIGINT	Required
scope_type	VARCHAR(40)	Assignment, ownership, authorization
effective_from	DATE	Required
effective_to	DATE	Optional
granted_by	BIGINT	Required foreign key
grant_reason	TEXT	Required
record_status	VARCHAR(30)	Required
is_active	BOOLEAN	Required
retired_at	TIMESTAMPTZ	Optional
retired_by	BIGINT	Optional
retirement_reason	TEXT	Optional
This table supports explicit access for assigned projects and contractors.

5.4 field_permission
Field	Type	Requirement
id	BIGINT	Primary key
role_id	BIGINT	Required foreign key
resource_type	VARCHAR(80)	Required
field_name	VARCHAR(120)	Required
can_view	BOOLEAN	Required
can_edit	BOOLEAN	Required
can_export	BOOLEAN	Required
effective_from	DATE	Required
effective_to	DATE	Optional
record_status	VARCHAR(30)	Required
is_active	BOOLEAN	Required
5.5 user_session
Field	Type	Requirement
id	BIGINT	Primary key
user_id	BIGINT	Required foreign key
session_identifier	VARCHAR(200)	Required, unique
created_at	TIMESTAMPTZ	Required
last_activity_at	TIMESTAMPTZ	Required
expires_at	TIMESTAMPTZ	Required
logout_at	TIMESTAMPTZ	Optional
termination_reason	VARCHAR(100)	Optional
session_status	VARCHAR(30)	Active, expired, logged_out, revoked
client_reference	TEXT	Optional
Session records are retained for audit integration.

5.6 authentication_event
Field	Type	Requirement
id	BIGINT	Primary key
user_id	BIGINT	Optional foreign key
login_identifier	VARCHAR(320)	Required
event_type	VARCHAR(40)	Login success, login failure, logout, lockout
occurred_at	TIMESTAMPTZ	Required
session_id	BIGINT	Optional foreign key
success_status	BOOLEAN	Required
failure_reason	VARCHAR(200)	Optional
source_reference	TEXT	Optional
6. Initial Roles
The system must support these roles:

Sales Associate
Sales Manager
Comptroller
Installation Manager
Technician
Accounts Payable Associate
Database Administrator
Other Contractor
A user may have one or more roles, subject to effective dates and administrative approval.

One role may be designated the user's primary role.

7. Role Permission Matrix
Capability	Sales Associate	Sales Manager	Comptroller	Installation Manager	Technician	AP Associate	DBA
Create customer	Yes	Yes	Yes	Authorized scope	No	Authorized scope	Authorized
Create Master Sales Record	Yes	Yes	Yes	No	No	No	Authorized
Edit unapproved sales record	Assigned records	Yes	Yes	Authorized fields	No	No	Authorized
Approve Master Sales Record	No	Yes	Yes	Per applicable workflow	No	No	No
View all business data	No	Yes	Yes	Authorized scope	No	Authorized scope	Authorized scope
View own commission data	Yes	Yes	Yes	No	No	No	No
View all commission data	No	Yes	Yes	No	No	No	No
Edit approved retail price	No	Yes	Yes	No	No	No	No
Edit approved costs	No	Yes	Yes	Authorized cost fields	No	No	No
Enter overhead expense	No	Yes	Yes	No	No	No	No
Approve adjustment	No	Yes	Yes	No	No	No	No
Sign contract	No	Yes	Yes	No	No	No	No
Sign invoice	No	Yes	Yes	No	No	No	No
Sign certificate	No	Substitute only	Substitute only	Yes	No	No	No
Schedule installation	No	Authorized exception	Authorized exception	Yes	No	No	Authorized
Update installation completion	No	Authorized	Authorized	Authorized	Assigned projects	No	Authorized
Enter payment	No	Authorized	Authorized	No	No	Yes	Authorized
Verify payment	No	Yes	Yes	No	No	No	Authorized only if assigned
Approve commission report	No	Yes	Yes	No	No	No	No
Publish financial statement	No	Yes	Yes	No	No	No	No
View audit log	No	Yes	Yes	Yes	No	No	No
Administer database functions	No	No	No	No	No	No	Yes
“Authorized scope” means access is limited by record assignment, field permissions, and applicable workflow rules.

8. Role-Specific Record Access
8.1 Sales Associate
May access:

Customers they created or are assigned
Projects assigned to them
Their own Master Sales Records
Documents for their assigned projects
Their own commission information
Tasks assigned to them
May not access:

Another Sales Associate's commission information
Unassigned projects
Restricted financial reports
Audit-log contents
Approved financial fields for editing
8.2 Sales Manager and Comptroller
May access:

All business records
All projects and customers
All commission and financial data
All workflow exceptions
Audit-log search and verification functions
Approval authority remains distinct from ordinary view access.

8.3 Installation Manager
May access:

Installation-related projects
Assigned technicians and installation tasks
Authorized installation cost fields
Project change history
Audit-log access
May not:

Sign financial statements
Approve financial statements
Access unauthorized commission or payroll data
8.4 Technician
May access:

Assigned projects
Approved scope of work
Assigned installation tasks
Required equipment and serial-number fields
Completion notes
Proposed scope-change fields
Certificate review and sending functions
May not access or modify:

Retail price
Approved costs
Contract terms
Commission data
Payroll data
Financial reports
8.5 Accounts Payable Associate
May access:

Authorized vendors
Authorized payable records
Payment-status fields
Payment dates and methods
Related project information necessary for payable duties
8.6 Database Administrator
May access:

Authorized database administration functions
Authorized search and maintenance functions
Data-quality tools
The Database Administrator does not receive approval authority by default.

9. Field-Level Restrictions
9.1 Financial fields
The following are restricted to Sales Manager, Comptroller, and authorized accounting users:

Retail price after approval
Equipment cost
Material cost
Dealer fee
Lead cost
Damon cost
Jerry cost
Technician cost
Gross profit
Net profit
Profitability reports
Balance-sheet data
Cash-flow data
Statement-of-operations data
Accounts payable balances
Accounts receivable balances
9.2 Commission and payroll fields
Sales Associates may view only commission information related to their own sales.

Restricted fields include:

Commission rates
Contractor commission amounts
Payroll-report totals
Other Sales Associates' commission data
Commission adjustments
Annual commission summaries for other contractors
9.3 Payment fields
Access to payment information must be limited according to role and duty.

Restricted fields include:

Bank name
Cashier's-check number
Wire reference number
Uploaded proof of receipt
Verification details
Adjustment approval details
9.4 Audit fields
Ordinary users must not edit or delete:

Hash values
Previous-event references
Event timestamps
Original actor identity
Integrity-verification results
Audit-event content
10. Authorization Enforcement
Authorization must be enforced at all applicable layers:

Application route or screen
Service or business-operation layer
Record-scope query
Field read/write filtering
Database transaction
Export and download operation
A hidden or disabled user-interface control is not sufficient protection.

Every permission failure must:

Deny the action.
Avoid disclosing restricted data.
Record the failed attempt.
Include the user, role, resource, action, timestamp, session, and reason.
Be available to authorized audit-log users.
11. Approval Separation Rules
The authorization design must preserve these rules:

Sales Associates cannot approve their own sales.
A user cannot approve an action they are not authorized to approve.
Commission changes require Sales Manager approval.
Early commission payment requires Sales Manager approval.
Published financial statements require Sales Manager and Comptroller approval.
The Installation Manager does not sign financial statements.
The Comptroller signs a certificate only when the Installation Manager is unavailable.
A Comptroller substitute certificate signature requires a recorded reason.
Approval actions reference the exact record version being approved.
A user may not approve an action that has been assigned to an incompatible role.
Approval must be rejected if the record has been superseded or already approved by another valid version.
12. Session and Account Rules
The implementation must support:

Active, inactive, locked, pending, and revoked account states
Session creation on successful login
Session expiration
Manual session revocation
Logout
Failed-login counting
Lockout or equivalent protective handling
Administrative account deactivation
Prevention of access using expired or revoked sessions
Recording of session identifiers with access events
Exact timeout and lockout values remain configuration items pending approval.

13. Sprint 02 Decisions
D-02-001 — Authorization model
Use combined:

Role-based access control
Record-scope access control
Field-level access control
Action-level access control
D-02-002 — Internal identifiers
Use numeric BIGINT identity IDs for users, roles, permissions, assignments, and access-scope records.

D-02-003 — Effective-dated permissions
Role assignments, role permissions, field permissions, and record scopes use effective dates and historical preservation.

D-02-004 — Default deny
If a permission is not explicitly granted or inherited from an active approved role assignment, access is denied.

D-02-005 — Database Administrator boundary
Database administration access does not automatically include approval, signing, commission, payroll, or financial-report authorization.

D-02-006 — Audit integration
Sprint 02 creates authentication and authorization event records and integration hooks.

Sprint 03 will implement the complete append-only tamper-evident audit log.

D-02-007 — Commission visibility
Sales Associates may view only commission information related to their own sales.

No role may bypass this restriction through search, export, report, or download functions unless separately authorized.

D-02-008 — Approval version binding
Approvals must reference the exact Master Sales Record, document, report, or financial-period version being approved.

14. Sprint 02 Acceptance Criteria
Sprint 02 is accepted when:

Unauthenticated users cannot access protected records or screens.
Each user has a unique identity.
Inactive or locked users cannot log in.
Failed logins are recorded.
Logout terminates the active session.
Session expiration is enforced.
Every protected operation performs an authorization check.
Sales Associates can see only their own commission data.
Technicians can see only assigned or authorized projects.
Other Contractors can see only explicitly assigned projects and fields.
Database Administrators do not automatically receive approval permissions.
Financial and payroll fields are hidden from unauthorized users.
Unauthorized edits to approved records are rejected.
Unauthorized exports and downloads are rejected.
Permission failures are recorded for future audit-log implementation.
Role assignments are effective-dated.
Access scopes are effective-dated.
Removing a role or assignment removes access after the effective time.
Approval actions identify the user and exact record version.
Certificate substitute signing requires a recorded reason.
The system does not rely solely on front-end controls for security.
A user cannot access a record outside the user's permitted scope.
A user cannot access restricted values through alternate search or report paths.
A revoked session cannot perform additional protected actions.
A superseded version cannot receive a new approval.
A user cannot approve an action that the user's role does not authorize.
15. Authorization Test Cases
Authentication
Valid active user can log in.
Invalid credentials are rejected.
Inactive user cannot log in.
Locked user cannot log in.
Successful login creates a session.
Logout terminates the session.
Expired session cannot access protected records.
Revoked session cannot execute protected actions.
Sales Associate access
Sales Associate can view an assigned project.
Sales Associate cannot view an unassigned project.
Sales Associate can view their own commission.
Sales Associate cannot view another Sales Associate's commission.
Sales Associate cannot approve their own Master Sales Record.
Sales Associate cannot edit an approved Master Sales Record.
Sales Associate cannot export restricted financial data.
Technician access
Technician can view an assigned project.
Technician cannot view an unassigned project.
Technician can update completion notes.
Technician can update serial numbers where authorized.
Technician cannot change retail price.
Technician cannot change approved costs.
Technician cannot view commission reports.
Technician cannot approve financial reports.
Manager and Comptroller access
Sales Manager can approve a Master Sales Record.
Comptroller can approve a Master Sales Record.
Sales Manager can approve an adjustment.
Comptroller can approve an adjustment.
Sales Manager can sign an invoice.
Comptroller can sign an invoice.
Financial statement publication requires both required approvals.
Comptroller substitute certificate signing requires a reason.
Database Administrator access
Database Administrator can use authorized administrative functions.
Database Administrator cannot approve a sale by default.
Database Administrator cannot approve commissions by default.
Database Administrator cannot publish financial statements by default.
Scope and field security
Restricted fields are omitted or masked for unauthorized users.
Restricted fields cannot be retrieved through alternate endpoints.
Search results exclude unauthorized rows.
Exports exclude unauthorized fields and records.
Print and download functions enforce the same permissions as screen views.
16. Sprint 02 Open Questions
These are implementation and configuration questions. They do not reopen the approved requirements baseline.

Will authentication use locally managed credentials, an external identity provider, or both?
What password and multi-factor authentication policy will be adopted?
What inactivity timeout is required?
What maximum failed-login threshold should cause account lockout?
Which users may grant or revoke role assignments?
What approval is required for emergency access?
What exact database row-level-security implementation will be used?
Which fields require re-authentication before viewing or changing?
What is the initial user list and role-assignment file?
What notification should be sent after repeated permission failures?
What session duration is appropriate for administrative users?
Will access be granted through individual assignments, role inheritance, or both?
What is the required process for temporary access?
What is the required process for emergency access revocation?
17. Sprint 03 Handoff
Sprint 03 must use the authentication and authorization events created by Sprint 02 to implement:

Append-only audit records
Hash-chain references
Access logging
Restricted-access logging
Permission-failure logging
Integrity verification
Audit-log search
Audit-log export controls
Audit-log retention and archival
Sprint 03 must preserve the following event attributes:

User identity
User role
Date and time
Record or screen accessed
Activity type
Related record identifier
Success or failure
Session identifier
Reason when required
Previous audit-log entry reference
Integrity hash or equivalent value
18. Historical Record
Supersedes
None.

Preserves
GECC-00-project-baseline.md
GECC-01-data-model.md
Sprint 00 decisions
Sprint 01 decisions
Approved role definitions
Approved access restrictions
Approved commission-visibility rules
Approved audit requirements
Changes introduced by this bundle
Defines the Sprint 02 authentication and authorization model.
Defines RBAC, record scope, field-level, and action-level authorization.
Defines authentication and session data structures.
Defines permission and access-scope data structures.
Defines role-access behavior.
Defines authorization acceptance tests.
Establishes Sprint 03 audit-log integration requirements.
