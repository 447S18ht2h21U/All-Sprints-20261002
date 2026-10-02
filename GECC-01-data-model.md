# GECC-01-data-model.md

# GECC Sales Back Office System — Sprint 01 Data Model and Accounting Foundation

## Bundle Metadata

- Bundle name: GECC-01-data-model.md
- Project: GECC Sales Back Office System
- Company: Go Ecco Climate Control
- Company abbreviation: GECC
- Bundle type: Sprint 01 design and implementation bundle
- Status: Draft for Sprint 01 execution
- Created date: 2026-09-20
- Previous bundle: GECC-00-project-baseline.md
- Next planned bundle: GECC-02-auth-and-permissions.md
- Primary database: Robust searchable relational database
- System of record: Relational database
- Google Sheets: Optional export or reporting destination only
- Status: Sprint 01 design in progress
- Database platform: PostgreSQL
- Primary-key strategy: Numeric BIGINT identity keys
- Business-entity identifier strategy: Numeric IDs
- Business timezone: America/New_York
- Effective dating: Non-overlapping effective-date ranges with version history
- Entity retirement policy: Controlled retirement/deactivation for all entities; append-only preservation for historical and audit records
- Audit retention: Permanent retention with partitioned searchable storage and immutable archival
- Accounting minimum-table decision: Open
- Financial calculated-value storage decision: Open
- Historical calculation-snapshot decision: Open
The approved baseline and Sprint 00 requirements remain unchanged.

---

## 1. Sprint Objective

Convert the approved GECC project baseline into a detailed relational database model and initial accounting foundation.

The Sprint 01 design must preserve all approved requirements from:

- GECC-00-project-baseline.md
- Sprint 00 decisions recorded in the preceding bundle

No Sprint 01 design may silently remove, weaken, or contradict an approved requirement.

---

## 2. Sprint 01 Deliverables

Sprint 01 must produce:

1. Entity relationship model
2. Table definitions
3. Field definitions
4. Primary keys and foreign keys
5. Unique constraints and indexes
6. Controlled enumerations
7. Required-field rules
8. Status-transition data structures
9. Master Sales Record versioning model
10. Document-version references
11. Approval and signature references
12. Payment and verification structures
13. Accounts receivable structures
14. Accounts payable structures
15. Cost and profitability data structures
16. Commission data structures
17. Financial-period structures
18. Initial accounting ledger model
19. Audit-reference fields
20. Database acceptance tests
21. Data-integrity and transaction rules
22. Migration and seed-data considerations

---

## 3. Authoritative Requirements Carried Forward

### 3.1 System of record

The relational database is the authoritative system of record.

PDFs, emails, exports, and Google Sheets are generated copies or outputs.

### 3.2 Master Sales Record

Each project must have an authoritative Master Sales Record.

The design must support:

- Master Sales Record ID
- Project association
- Customer association
- Version number
- Creation date and creator
- Assigned Sales Associate
- Retail price
- Payment option
- Scope
- Equipment
- Serial numbers
- Material costs
- Equipment costs
- Dealer fees
- Lead cost
- Technician cost
- Damon cost
- Jerry cost
- Contract data
- Invoice data
- Certificate data
- Approval status
- Document-version references
- Revision reason
- Revision timestamp
- Revising user

Approved versions must remain preserved.

### 3.3 Project identification code

The project identification code must:

- Use an automatically generated sequential number beginning at 101.
- Include Sales Associate initials.
- Include the Master Sales Record creation date.
- Use the format `Sequential Number-Sales Associate Initials-YYYY-MM-DD`.
- Prevent duplicates.
- Reserve the sequential number during creation.
- Prevent reuse of cancelled project codes.
- Preserve the original code through revisions.
- Warn about an active project at the same customer location.
- Require an explanation or correction before another active project at that location is allowed.

### 3.4 Versioning

The design must support:

- Immutable approved Master Sales Record versions.
- New versions for authorized reopening or scope changes.
- Immutable document versions.
- Revision user, date, time, and reason.
- Current and superseded version indicators.
- Preservation of original signatures and audit records.
- Links between revised records and their predecessors.

### 3.5 Audit references

Business records must be able to reference:

- Creation event
- Modification event
- Approval event
- Reopening event
- Revision event
- Related audit-log identifiers
- User responsible for the action
- Date and time of the action

The detailed tamper-evident audit implementation is planned for Sprint 03, but Sprint 01 must reserve the required relational references.

---

## 4. Core Entity Groups

Sprint 01 must model at least the following entity groups.

### 4.1 Identity and users

- User
- Role
- Permission
- User-role assignment
- User status
- Session reference placeholder

### 4.2 Customers and projects

- Customer
- Customer contact
- Project location
- Project
- Project assignment
- Project status history

### 4.3 Sales records

- Master Sales Record
- Master Sales Record version
- Scope selection
- Equipment
- Equipment serial number
- Cost component
- Pricing record
- Payment-option record

### 4.4 Documents

- Document
- Document version
- Document type
- Template version reference
- Artwork version reference
- Signature request reference
- Signature audit certificate reference

### 4.5 Workflow

- Workflow state
- State transition
- Task
- Task assignment
- Approval
- Exception
- Required action

### 4.6 Payments and receivables

- Payment
- Payment verification
- Payment proof
- Adjustment
- Credit
- Refund
- Accounts receivable balance or transaction structure

### 4.7 Payables and expenses

- Vendor
- Accounts payable
- Payable category
- Overhead expense
- Payable payment

### 4.8 Contractors and commissions

- Contractor or employee
- Commission configuration
- Commission calculation
- Commission earning event
- Commission report
- Commission report item
- Commission adjustment
- Annual commission summary

### 4.9 Inventory and assets

- Inventory item
- Inventory cost layer
- Inventory allocation
- Fixed asset
- Asset category
- Depreciation record
- Loan

### 4.10 Financial accounting

- Account
- Journal entry
- Journal entry line
- Cash receipt
- Cash disbursement
- Financial-report period
- Period lock
- Adjusting entry
- Retained earnings reference

### 4.11 Audit and configuration references

- Audit-event reference
- Configuration version
- State artwork reference
- Effective-date configuration

---

## 5. Required Relational Design Rules

The schema must use:

- Primary keys for every persistent entity.
- Foreign keys for all required relationships.
- Unique constraints for project codes and other required unique values.
- Check constraints or controlled lookup tables for enumerated statuses.
- Required-field constraints for mandatory data.
- Transactional saves for multi-table business operations.
- Indexes supporting approved search requirements.
- Explicit effective and retirement dates where configuration changes over time.
- Version identifiers for records requiring historical preservation.
- No hard deletion of approved, signed, paid, published, or audited records.
- Referential integrity for customer, project, document, payment, cost, and report relationships.

---

## 6. Required Database Acceptance Tests

Sprint 01 acceptance testing must demonstrate that:

- A project code cannot be duplicated.
- A project code sequence begins at 101.
- Cancelled project codes cannot be reused.
- A project retains its original code through Master Sales Record revisions.
- A Master Sales Record version can be preserved and superseded without being overwritten.
- A document version remains linked to the correct Master Sales Record version.
- A customer may have multiple contacts and projects.
- A project may have multiple equipment records and serial numbers.
- Accounts payable can be separated into equipment, dealer-fee, and materials categories.
- Payment records can be linked to the correct project and invoice.
- Payment verification status is distinct from payment entry status.
- Approved adjustments retain their original amount, adjusted amount, reason, approver, and timestamp.
- Commission records can be linked to projects, contractors, reports, and adjustments.
- Financial periods can be locked.
- Published-period corrections can be represented by adjustment records.
- Inventory serial numbers remain individually identifiable.
- FIFO inventory cost layers can be represented.
- Fixed assets can store straight-line depreciation inputs and outputs.
- Loans can store principal, interest, and payment information.
- Accounting journal entries can contain balanced debit and credit lines.
- Business records can reference future audit events.
- Required search fields can be indexed.
- Foreign-key and required-field violations are rejected.
- Multi-record saves either complete transactionally or roll back.

---

## 7. Sprint 01 Open Design Questions

These questions are for Sprint 01 architecture decisions and do not reopen the approved requirements interview:

1. Which relational database platform will be used?
2. What primary-key strategy will be used?
3. What timezone will govern timestamps and due dates?
4. Will audit references use a single event ID, multiple event IDs, or a related-event table?
5. Which statuses will use lookup tables versus database enumerations?
6. How will calculated financial amounts be stored for historical reproducibility?
7. What is the minimum chart of accounts required for the initial ledger?
8. How will accounting periods relate to calendar months and publication dates?
9. How will FIFO cost layers be allocated when inventory is partially issued?
10. How will record retirement be represented without deletion?
11. What indexing strategy will support dashboard filters and authorized search?
12. What migration staging tables are needed for existing spreadsheet data?

---

## 8. Sprint 01 Decision Log

No new Sprint 01 decisions have been approved yet.

Approved Sprint 00 decisions carried forward:

- Relational database is the system of record.
- Master Sales Record is the project source of truth.
- Approved records are immutable.
- Revisions create new versions.
- Workflow and status transitions must be enforced.
- Audit references must be reserved in the schema.
- Internal double-entry accounting is the approved architectural direction.
- Initial bank-payment verification is manual.
- Existing document templates may be deferred to Sprint 07.

---

## 9. Sprint 01 Completion Criteria

Sprint 01 is complete when:

- The entity relationship model is approved.
- All required baseline entities are represented.
- Required relationships and foreign keys are defined.
- Required fields and constraints are documented.
- Master Sales Record versioning is fully defined.
- Document and signature references are represented.
- Payment verification and adjustment structures are represented.
- Accounts receivable and accounts payable structures are represented.
- Inventory, fixed assets, loans, and depreciation structures are represented.
- Contractor commissions and reports are represented.
- Financial periods and locking are represented.
- The initial double-entry ledger structure is represented.
- Audit-reference fields are included.
- Required indexes are defined for approved searches and dashboard filters.
- Database acceptance tests are documented and executable.
- No approved baseline requirement is silently removed or contradicted.
- The model is ready for Sprint 02 authentication and permissions design.

---

## 10. Historical Record

### Supersedes

None.

### Preserves

- GECC-00-project-baseline.md
- Sprint 00 architecture deliverables
- Sprint 00 acceptance criteria
- Sprint 00 decisions
- Sprint 00 open questions and deferred implementation items

### Changes introduced by this bundle

- Begins Sprint 01 schema and accounting-foundation planning.
- Converts the baseline entity list into a required data-model work plan.
- Defines database acceptance-test categories.
- Establishes Sprint 01 design questions without changing approved business requirements.


