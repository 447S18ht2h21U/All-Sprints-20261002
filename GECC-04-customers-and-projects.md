# GECC-04-customers-and-projects.md

# GECC Sales Back Office System — Sprint 04 Customers, Contacts, Locations, and Projects

## Bundle Metadata

- Bundle name: GECC-04-customers-and-projects.md
- Project: GECC Sales Back Office System
- Company: Go Ecco Climate Control
- Company abbreviation: GECC
- Bundle type: Sprint 04 design and implementation bundle
- Status: Approved design; ready for implementation
- Created date: 2026-09-20
- Previous bundle: GECC-03-audit-log.md
- Next planned bundle: GECC-05-master-sales-record.md
- Database: PostgreSQL
- Primary-key strategy: Numeric BIGINT identity keys
- Business timezone: America/New_York
- System of record: PostgreSQL relational database
- Audit model: Append-only, hash-chained, permanently retained

---

## 1. Sprint Objective

Implement customer, contact, project-location, project, assignment, duplicate-detection, project-code, and authorized-search functionality.

Sprint 04 must establish the foundational records used by all later project, document, payment, commission, and financial processes.

---

## 2. Sprint 04 Deliverables

Sprint 04 must produce:

1. Customer records
2. Multiple customer contacts
3. Customer contact preferences
4. Project locations
5. State-based location data
6. Customer and location duplicate detection
7. Project records
8. Project assignments
9. Project-status foundation
10. Project-code generation
11. Project-code reservation
12. Cancelled-code protection
13. Active-location duplicate warning and override
14. Authorized search
15. Search filters
16. Role-scoped search results
17. Customer and project access controls
18. Customer and project audit events
19. Database acceptance tests
20. Sprint 05 Master Sales Record handoff

---

## 3. Requirements Carried Forward

The implementation must preserve these approved requirements:

- A customer may have multiple projects.
- A project has one customer.
- A project has one project location.
- A project location has separate street, city, state, and ZIP fields.
- The project location's state determines default license artwork.
- Users enter Sales Associate initials.
- The system automatically generates the sequential project number.
- The sequence begins at 101.
- The project-code format is:

```text
Sequential Number-Sales Associate Initials-YYYY-MM-DD
The date is the Master Sales Record creation date.
Project codes must be unique.
Project-code numbers are reserved during creation.
Cancelled project codes cannot be reused.
Revisions preserve the original project code.
The system warns about an existing active project at the same customer location.
A second active project at the same location requires an explanation or correction.
Search results must respect role-based access restrictions.
All access and search activity must be audited.
The relational database is the system of record.
Customer and project records are never silently overwritten.
4. Customer Data Model
4.1 customer
Field	Type	Requirement
id	BIGINT	Primary key
customer_number	VARCHAR(50)	Required, unique
customer_name	VARCHAR(250)	Required
customer_status	VARCHAR(30)	Required
customer_folder_reference	TEXT	Optional
duplicate_check_key	VARCHAR(500)	Required, indexed
notes	TEXT	Optional
created_at	TIMESTAMPTZ	Required
created_by	BIGINT	Required foreign key
updated_at	TIMESTAMPTZ	Required
updated_by	BIGINT	Required foreign key
is_active	BOOLEAN	Required
retired_at	TIMESTAMPTZ	Optional
retired_by	BIGINT	Optional foreign key
retirement_reason	TEXT	Optional
The customer number is system-generated and is separate from the project identification code.

4.2 customer_phone
Field	Type	Requirement
id	BIGINT	Primary key
customer_id	BIGINT	Required foreign key
phone_number	VARCHAR(50)	Required
normalized_phone_number	VARCHAR(30)	Required, indexed
phone_type	VARCHAR(30)	Mobile, home, work, other
is_primary	BOOLEAN	Required
is_active	BOOLEAN	Required
Standard audit columns	—	Required
4.3 customer_email
Field	Type	Requirement
id	BIGINT	Primary key
customer_id	BIGINT	Required foreign key
email_address	VARCHAR(320)	Required
normalized_email_address	VARCHAR(320)	Required, indexed
email_type	VARCHAR(30)	Personal, work, other
is_primary	BOOLEAN	Required
is_active	BOOLEAN	Required
Standard audit columns	—	Required
4.4 customer_contact
Field	Type	Requirement
id	BIGINT	Primary key
customer_id	BIGINT	Required foreign key
contact_name	VARCHAR(200)	Required
contact_type	VARCHAR(50)	Required
phone_number	VARCHAR(50)	Optional
email_address	VARCHAR(320)	Optional
is_primary	BOOLEAN	Required
preferred_contact_method	VARCHAR(30)	Optional
contact_status	VARCHAR(30)	Required
Standard audit columns	—	Required
A customer may have multiple contacts.

Only one active primary contact may exist for a given customer and contact type unless an approved exception is recorded.

5. Project-Location Data Model
5.1 project_location
Field	Type	Requirement
id	BIGINT	Primary key
street_address	VARCHAR(250)	Required
address_line_2	VARCHAR(250)	Optional
city	VARCHAR(100)	Required
state_code	CHAR(2)	Required
postal_code	VARCHAR(20)	Required
country_code	CHAR(2)	Required, default US
normalized_street_address	VARCHAR(250)	Required
normalized_city	VARCHAR(100)	Required
normalized_postal_code	VARCHAR(20)	Required
normalized_location_key	VARCHAR(500)	Required, indexed
Standard audit columns	—	Required
The state code must use a controlled list of approved state values.

5.2 Location normalization
The system must normalize locations for duplicate detection while preserving the original entered values.

Normalization may include:

Uppercase comparison values
Standardized street suffixes
Consistent whitespace
Consistent punctuation
Standardized ZIP-code representation
Separate handling of apartment, unit, lot, or suite information
The original address remains the display and historical value.

5.3 Location matching
The system must support:

Exact normalized-location matching
Same-customer active-project matching
Existing active-project matching across customers
Possible-match warnings for incomplete or similar addresses
Authorized override with an explanation
The precise fuzzy-matching algorithm remains an implementation decision.

6. Project Data Model
6.1 project
Field	Type	Requirement
id	BIGINT	Primary key
customer_id	BIGINT	Required foreign key
project_code	VARCHAR(80)	Required, unique
project_location_id	BIGINT	Required foreign key
assigned_sales_associate_id	BIGINT	Required foreign key
current_msr_version_id	BIGINT	Optional until Sprint 05
project_status	VARCHAR(50)	Required
cancellation_reason	TEXT	Required when cancelled
completion_date	DATE	Optional
last_activity_at	TIMESTAMPTZ	Required
Standard audit columns	—	Required
6.2 Initial project statuses
Sprint 04 must support at least:

text


draft
active
duplicate\_review
cancelled
superseded
completed
Later sprints will add controlled lifecycle states for:

Approval
Contract signing
Invoice signing
Installation
Certificate signing
Payment
Commission eligibility
Final completion
Sprint 04 must not bypass the future controlled lifecycle.

6.3 project_assignment
Field	Type	Requirement
id	BIGINT	Primary key
project_id	BIGINT	Required foreign key
user_id	BIGINT	Required foreign key
assignment_type	VARCHAR(40)	Sales Associate, Technician, other
effective_from	DATE	Required
effective_to	DATE	Optional
assignment_status	VARCHAR(30)	Required
assigned_by	BIGINT	Required foreign key
assignment_reason	TEXT	Optional
Standard audit columns	—	Required
A project may have multiple technicians.

A project must have one active assigned Sales Associate unless the project is in a draft or exception state.

6.4 project_status_history
Field	Type	Requirement
id	BIGINT	Primary key
project_id	BIGINT	Required foreign key
previous_status	VARCHAR(50)	Optional
new_status	VARCHAR(50)	Required
changed_at	TIMESTAMPTZ	Required
changed_by	BIGINT	Required foreign key
change_reason	TEXT	Required when applicable
audit_event_id	BIGINT	Optional foreign key
Status history is append-only.

7. Project-Code Generation
7.1 Format
The project identification code is:

text


Sequential Number-Sales Associate Initials-YYYY-MM-DD
Example:

text


101-JD-2026-09-19
The date is the Master Sales Record creation date.

7.2 Number sequence
The sequence:

Begins at 101.
Is generated by the database or a transaction-safe application service.
Is reserved during project or initial Master Sales Record creation.
Cannot be reused after cancellation.
Cannot be released back to the available pool after a failed or cancelled project creation.
Must remain unique permanently.
7.3 Sales Associate initials
The system must:

Require Sales Associate initials before code generation.
Validate the initials format.
Preserve the initials used in the original code.
Prevent revisions from changing the original project code.
Record the user who entered or confirmed the initials.
The exact initials format remains configurable but must be consistently enforced.

7.4 Project-code generation transaction
The following must occur transactionally:

Validate the Sales Associate.
Validate initials.
Reserve the next sequence number.
Determine the Master Sales Record creation date.
Construct the project code.
Check uniqueness.
Create the project.
Create the initial project audit event.
Commit the transaction.
If the transaction fails, the reserved sequence number must not be reused.

8. Duplicate Detection
8.1 Customer duplicate detection
The system should identify possible duplicate customers using combinations of:

Normalized customer name
Phone number
Email address
Mailing address
Existing customer number
Duplicate-check key
Possible matches must be shown before creation is completed.

8.2 Project-location duplicate detection
Before creating an active project, the system must check for:

Existing active project at the same normalized location
Existing active project for the same customer and location
Existing cancelled project at the location
Existing completed project at the location
Similar but not exact address matches
8.3 Duplicate warning behavior
If an active project exists at the same location:

Display the existing project code.
Display only fields authorized for the current user.
Require the user to review the possible duplicate.
Require an explanation or correction.
Prevent creation until the required explanation or correction is recorded.
Require an authorized override where applicable.
Log the warning, decision, and user.
8.4 Duplicate override
A duplicate-location override must include:

Existing project identifier
New project identifier
User requesting the override
Date and time
Explanation
Approval or authorization result
Approving user where required
Related audit event
A duplicate override must not delete, merge, or overwrite either project.

9. Customer and Project Search
9.1 Search fields
Authorized users may search by:

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
Project status
Date range
Customer number
Additional search fields will become available in later sprints:

Serial number
Brand
Model
Contract status
Invoice status
Certificate status
Payment status
Vendor
Payable
Document type
9.2 Search behavior
Search must:

Require authentication.
Apply record-level authorization.
Apply field-level authorization.
Return only records the user is permitted to view.
Prevent unauthorized data inference through counts or autocomplete.
Support exact and partial matching where appropriate.
Normalize phone, email, ZIP, and address search values.
Record the search as an audit event.
Record whether restricted results were requested.
Support pagination.
Display the user's permitted fields only.
9.3 Search result fields
The basic project search result should include:

Project identification code
Customer name
Project location
Assigned Sales Associate
Assigned Technician where authorized
Current project status
Creation date
Last activity date
Warning or exception indicator where authorized
Financial amounts, commissions, profitability, and payroll information must not appear unless separately authorized.

9.4 Search sorting
Default sorting:

Most recently active project
Most recently modified record
Project identification code
The system must provide authorized sorting by:

Customer name
Project code
Location
Status
Sales Associate
Creation date
Last activity date
10. Access Rules
Sales Associate
May:

Create customers
Create projects
View assigned customers
View assigned projects
Edit unapproved assigned project information
Search within authorized assigned records
May not:

View another Sales Associate's commission information
View unauthorized projects
View restricted financial amounts
Override duplicate-location restrictions without authorization
Edit approved project information
Sales Manager and Comptroller
May:

View all customers and projects
Create and edit authorized records
Resolve duplicate-location exceptions
Assign or reassign authorized users
Search all business records
View all project and customer data
Installation Manager
May:

View installation-related projects
View assigned or authorized project locations
Search authorized projects
Assign or review installation personnel where permitted
Technician
May:

View assigned projects
View assigned project locations
Search assigned projects
View approved project information needed for technical work
Accounts Payable Associate
May:

View customer and project fields required for payable duties
Search authorized project records
View project codes and related payable references
Database Administrator
May:

Search authorized records
Maintain approved database functions
Perform approved data-quality operations
The Database Administrator does not automatically receive unrestricted business-data access.

11. Customer and Project Forms
11.1 Customer form
The customer form must support:

Create new customer
Save as draft where applicable
Add multiple phone numbers
Add multiple email addresses
Add multiple contacts
Set preferred contact method
Identify possible duplicates
Save customer
Edit unapproved customer information
Retire customer through authorized action
Clear form
Cancel form
11.2 Project form
The project form must support:

Select or create customer
Enter project location
Select Sales Associate
Enter and validate Sales Associate initials
Identify possible duplicate location
Require duplicate explanation or correction
Create project code
Save project
Save draft where applicable
Assign technicians later through authorized workflow
View project history
Cancel form
11.3 Form validation
The system must validate:

Required customer fields
Required location fields
Valid state code
Valid ZIP-code format
Valid phone format
Valid email format
Valid Sales Associate
Valid initials
Duplicate customer warning
Duplicate active project warning
Project-code uniqueness
Authorized user scope
Record status
Required explanation for duplicate override
Required cancellation reason
12. Audit Events
Sprint 04 must create audit events for:

Customer events
Customer created
Customer viewed
Customer edited
Customer retired
Possible duplicate identified
Duplicate accepted
Duplicate rejected
Contact created
Contact edited
Contact retired
Location events
Location created
Location viewed
Location edited
Location duplicate warning
Location duplicate override
Location retired
Project events
Project creation started
Project-code number reserved
Project created
Project viewed
Project edited
Project assigned
Project reassigned
Project status changed
Project cancelled
Project duplicate warning
Project duplicate override
Project search
Restricted project-access attempt
Audit events must include:

User
Role
Session
Resource
Resource identifier
Project or customer association
Action
Outcome
Reason where applicable
Previous and new status where applicable
Audit-chain references
13. Sprint 04 Decisions
D-04-001 — Customer and project separation
Customers and projects are separate entities.

A customer may have multiple projects.

D-04-002 — Project location separation
Project location is a separate entity from the customer mailing address.

A project location includes distinct street, city, state, and ZIP fields.

D-04-003 — Project-code permanence
A project code is permanent once assigned and cannot be reused, including after cancellation.

D-04-004 — Project-code sequence
The project-code sequence begins at 101 and is reserved transactionally.

D-04-005 — Location duplicate behavior
An active project at the same customer location triggers a warning and requires an explanation or correction before another active project can be created.

D-04-006 — Duplicate preservation
Duplicate projects and customers are not silently merged or deleted.

Any later merge process must preserve both original records and create a separate audit trail.

D-04-007 — Search authorization
Search applies the same role, record, and field permissions as ordinary record viewing.

D-04-008 — Historical status preservation
Project-status history is append-only and is not replaced when the current status changes.

D-04-009 — State data
Project location state is stored as a controlled state code and is reserved for later automatic state-license-artwork selection.

D-04-010 — Sprint 05 dependency
The project may be created before the full Master Sales Record is implemented, but the project must not be treated as an approved sale until Sprint 05 and later lifecycle controls are complete.

14. Sprint 04 Acceptance Criteria
Sprint 04 is accepted when:

A customer can be created and saved.
A customer can have multiple phone numbers.
A customer can have multiple email addresses.
A customer can have multiple contacts.
A project can be linked to exactly one customer.
A customer can have multiple projects.
A project location stores separate street, city, state, and ZIP fields.
A project can be linked to exactly one project location.
The location state is stored as a controlled value.
Possible duplicate customers are identified before creation.
Possible duplicate project locations are identified before active-project creation.
An active project at the same location triggers a warning.
A duplicate-location warning cannot be bypassed without an explanation or correction.
Authorized duplicate overrides are recorded.
Duplicate records are not silently merged or deleted.
The project-code sequence begins at 101.
Project codes use the approved format.
Project codes are unique.
Sequence numbers are reserved transactionally.
Cancelled project codes cannot be reused.
Project revisions preserve the original project code.
A project has an assigned Sales Associate.
A project may have multiple assigned Technicians.
Project assignments are effective-dated.
Project-status history is append-only.
Authorized users can search customers and projects.
Unauthorized users cannot use search to discover restricted records.
Search results respect role and field permissions.
Search activity is logged.
Customer and project views are logged.
Project-code reservation is logged.
Duplicate warnings and overrides are logged.
Required fields are validated before save.
Invalid state, ZIP, email, phone, and project-code data are rejected.
Retired customer, location, and project records remain historically retrievable according to permissions.
No project is treated as approved solely because it exists in Sprint 04.
15. Sprint 04 Test Cases
Customer tests
Create a valid customer.
Reject a customer with no required name.
Add multiple phone numbers.
Add multiple email addresses.
Add multiple contacts.
Set a primary contact.
Prevent conflicting active primary contacts.
Identify a possible duplicate by normalized name and phone.
Retire a customer with a required reason.
Preserve the customer's history after retirement.
Location tests
Create a valid location.
Reject a location without street, city, state, or ZIP.
Reject an invalid state code.
Normalize equivalent address values.
Match equivalent normalized locations.
Preserve the original entered address.
Identify a possible active-project location duplicate.
Project-code tests
Create the first project with sequence number 101.
Create a second project with sequence number 102.
Validate the initials portion.
Validate the date portion.
Reject a duplicate project code.
Cancel a project and confirm its code cannot be reused.
Create a later revision and confirm the code remains unchanged.
Simulate concurrent project creation and confirm unique sequence allocation.
Simulate failed project creation and confirm the reserved number is not reused.
Access tests
Sales Associate sees assigned projects.
Sales Associate cannot see unassigned projects.
Technician sees assigned projects only.
Accounts Payable Associate sees only authorized project fields.
Installation Manager sees authorized installation-related projects.
Sales Manager sees all projects.
Comptroller sees all projects.
Database Administrator receives only authorized access.
Unauthorized search results are excluded.
Unauthorized fields are not returned through search, print, or export.
Audit tests
Customer creation creates an audit event.
Project creation creates an audit event.
Project-code reservation creates an audit event.
Project assignment creates an audit event.
Duplicate warning creates an audit event.
Duplicate override creates an audit event.
Restricted-access attempt creates an audit event.
Search creates an audit event.
Project cancellation creates an audit event.
16. Sprint 04 Open Questions
These implementation questions do not change approved requirements:

What exact customer-number format will be used?
What address-normalization library or service will be used?
What level of fuzzy address matching is required?
Which duplicate matches are warnings versus hard blocks?
Who may approve a duplicate-location override?
What exact Sales Associate initials format will be enforced?
Should project creation reserve a number before or after customer confirmation?
What additional project statuses are needed before Sprint 06?
What customer-merge process, if any, will be implemented later?
What states and territories must be included in the initial state-code reference table?
What search result limits and pagination sizes are appropriate?
What fields may be printed by each role?
17. Sprint 05 Handoff
Sprint 05 will implement the Master Sales Record.

Sprint 05 must use the Sprint 04 structures for:

Customer
Customer contacts
Project
Project location
Assigned Sales Associate
Project code
Project status
Project audit history
Sprint 05 must preserve:

The original project identification code
The project-to-customer relationship
The project location
The assigned Sales Associate
Existing project assignments
Existing duplicate warnings and overrides
All customer and project audit events
Sprint 05 must not create a second project identifier for the same project.

18. Historical Record
Supersedes
None.

Preserves
GECC-00-project-baseline.md
GECC-01-data-model.md
GECC-02-auth-and-permissions.md
GECC-03-audit-log.md
Sprint 00 decisions
Sprint 01 decisions
Sprint 02 decisions
Sprint 03 decisions
Approved access restrictions
Approved project-code requirements
Approved duplicate-location requirements
Changes introduced by this bundle
Defines the customer and contact data structures.
Defines project-location storage and normalization.
Defines project and assignment structures.
Defines permanent project-code generation behavior.
Defines customer and project duplicate detection.
Defines authorized customer and project search.
Defines Sprint 04 audit events.
Defines Sprint 05 Master Sales Record handoff.