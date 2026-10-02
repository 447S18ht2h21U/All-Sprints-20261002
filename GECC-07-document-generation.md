# GECC-07-document-generation.md

# GECC Sales Back Office System — Sprint 07 Document Generation and Artwork

## Bundle Metadata

- Bundle name: GECC-07-document-generation.md
- Project: GECC Sales Back Office System
- Company: Go Ecco Climate Control
- Company abbreviation: GECC
- Bundle type: Sprint 07 design and implementation bundle
- Status: Approved design; ready for implementation
- Created date: 2026-09-20
- Previous bundle: GECC-06-workflow-state-machine.md
- Next planned bundle: GECC-08-electronic-signatures.md
- Database: PostgreSQL
- Primary-key strategy: Numeric BIGINT identity keys
- Business timezone: America/New_York
- System of record: PostgreSQL relational database
- Audit model: Append-only, hash-chained, permanently retained

---

## 1. Sprint Objective

Implement controlled generation of:

- Invoices
- Contracts
- Certificates

Documents must be generated from an approved Master Sales Record and stored as immutable document versions in the customer/project file.

The system must support:

- Existing GECC PDF templates
- Automatically generated invoices
- GECC logo placement
- State-specific license artwork
- Artwork versioning
- Document preview
- PDF generation
- Saving
- Printing
- Emailing
- Signature-system handoff
- Document-history preservation

Electronic signature execution is implemented in Sprint 08.

---

## 2. Sprint 07 Deliverables

Sprint 07 must produce:

1. Invoice-generation process
2. Contract-template population process
3. Certificate-template population process
4. PDF-template registration
5. Template versioning
6. Document-version model
7. GECC logo management
8. State-specific license-artwork management
9. Base64 artwork validation
10. Effective-dated artwork activation
11. Artwork preview
12. Automatic state-artwork selection
13. Authorized artwork override
14. Document-generation validation
15. PDF storage and file references
16. Document preview
17. Print support
18. Email handoff
19. Electronic-signature handoff interface
20. Document-generation audit events
21. Document consistency tests
22. Sprint 08 signature-system handoff

---

## 3. Requirements Carried Forward

The implementation must preserve these approved requirements:

- Documents are generated from the approved Master Sales Record.
- The invoice number equals the project identification code.
- Contract and certificate forms are existing GECC PDF templates.
- Invoices are generated automatically.
- Contracts and certificates are populated automatically.
- Documents can be saved.
- Documents can be printed.
- Documents can be emailed.
- Documents can be sent for electronic signature.
- Generated documents are saved to the customer/project file.
- Original documents remain permanently preserved.
- Revised documents receive new versions.
- Generated documents must not be independently edited in a way that changes underlying project data.
- State-specific license artwork is selected from the project location's state.
- Artwork overrides require alternate-artwork selection, approval reason, Sales Manager or Comptroller approval, and an audit event.
- Document generation must stop when required artwork is missing.
- Document generation must stop when required approvals or data are missing.
- Each document retains its template and artwork version references.
- Document versions remain linked to the associated Master Sales Record version.

---

## 4. Document Types

### 4.1 Invoice

The invoice must include or reference:

- Invoice number
- Project identification code
- Customer name
- Customer address where applicable
- Project location
- Scope of work
- Retail price
- Payment option
- Adjustment amount where applicable
- Adjusted amount due where applicable
- GECC signer fields
- Customer signer fields
- GECC logo
- State-specific license artwork where required
- Master Sales Record version
- Document version
- Template version
- Artwork version

The invoice number must equal the unique project identification code.

### 4.2 Contract

The contract uses an existing GECC PDF template.

The populated contract must include or reference:

- Customer information
- Project identification code
- Project location
- Scope of work
- Equipment
- Applicable pricing and contract terms
- Required signature fields
- GECC logo
- State-specific license artwork where required
- Master Sales Record version
- Template version
- Document version
- Artwork version

### 4.3 Certificate

The certificate uses an existing GECC PDF template.

The populated certificate must include or reference:

- Customer information
- Project identification code
- Project location
- Approved scope of work
- Equipment
- Serial numbers
- Installation completion information
- Customer signature fields
- Installation Manager signature fields
- Comptroller substitute-signature fields where applicable
- Substitute-signature reason where applicable
- GECC logo
- State-specific license artwork where required
- Master Sales Record version
- Template version
- Document version
- Artwork version

---

## 5. Document Data Model

### 5.1 `document`

| Field | Type | Requirement |
|---|---|---|
| `id` | BIGINT | Primary key |
| `project_id` | BIGINT | Required foreign key |
| `document_type` | VARCHAR(30) | Invoice, contract, certificate |
| `current_version_id` | BIGINT | Optional foreign key |
| `document_status` | VARCHAR(40) | Required |
| `required_for_workflow` | BOOLEAN | Required |
| `created_at` | TIMESTAMPTZ | Required |
| `created_by` | BIGINT | Required foreign key |
| `updated_at` | TIMESTAMPTZ | Required |
| `updated_by` | BIGINT | Required foreign key |
| `is_active` | BOOLEAN | Required |
| `retired_at` | TIMESTAMPTZ | Optional |
| `retired_by` | BIGINT | Optional foreign key |
| `retirement_reason` | TEXT | Optional |

Constraint:

```text
UNIQUE(project\_id, document\_type)
A project has one logical invoice, one logical contract, and one logical certificate, each of which may have multiple versions.

5.2 document_version
Field	Type	Requirement
id	BIGINT	Primary key
document_id	BIGINT	Required foreign key
project_id	BIGINT	Required foreign key
msr_version_id	BIGINT	Required foreign key
version_number	INTEGER	Required
document_status	VARCHAR(50)	Draft, generated, current, superseded, cancelled
template_version_id	BIGINT	Required foreign key
artwork_selection_id	BIGINT	Required foreign key where artwork is required
file_location	TEXT	Required
file_name	VARCHAR(255)	Required
content_type	VARCHAR(100)	Required
content_hash	VARCHAR(128)	Required
file_size_bytes	BIGINT	Required
generated_at	TIMESTAMPTZ	Required
generated_by	BIGINT	Required foreign key
is_current	BOOLEAN	Required
is_signed	BOOLEAN	Required
supersedes_document_version_id	BIGINT	Optional foreign key
signature_request_id	BIGINT	Optional foreign key
signature_audit_certificate_id	BIGINT	Optional foreign key
Standard audit columns	—	Required
Constraints:

text


UNIQUE(document\_id, version\_number)
Only one document version may be current for a logical document.

5.3 document_field_value
Field	Type	Requirement
id	BIGINT	Primary key
document_version_id	BIGINT	Required foreign key
field_name	VARCHAR(150)	Required
source_entity_type	VARCHAR(100)	Required
source_entity_id	BIGINT	Required
source_field_name	VARCHAR(150)	Required
rendered_value_hash	VARCHAR(128)	Required
rendered_value_masked	TEXT	Optional
created_at	TIMESTAMPTZ	Required
This table provides traceability from rendered document fields to source records without unnecessarily storing sensitive values.

6. Template Management
6.1 document_template
Field	Type	Requirement
id	BIGINT	Primary key
template_type	VARCHAR(30)	Invoice, contract, certificate
template_name	VARCHAR(200)	Required
description	TEXT	Optional
is_active	BOOLEAN	Required
Standard retirement columns	—	Required
6.2 document_template_version
Field	Type	Requirement
id	BIGINT	Primary key
document_template_id	BIGINT	Required foreign key
version_number	INTEGER	Required
file_location	TEXT	Required
content_type	VARCHAR(100)	Required
content_hash	VARCHAR(128)	Required
field_map_definition	JSONB	Required
signature_placement_definition	JSONB	Optional
effective_from	DATE	Required
effective_to	DATE	Optional
template_status	VARCHAR(30)	Draft, active, retired
uploaded_by	BIGINT	Required foreign key
uploaded_at	TIMESTAMPTZ	Required
retired_by	BIGINT	Optional foreign key
retirement_reason	TEXT	Optional
Constraints:

Template versions cannot overlap for the same template type and effective scope.
A template version used by a document remains preserved.
Retiring a template does not invalidate historical documents.
6.3 Template registration
The system must support:

Uploading an existing GECC contract PDF
Uploading an existing GECC certificate PDF
Registering an invoice template or invoice-rendering definition
Defining field placements
Defining artwork placements
Defining signature placements
Previewing a template
Activating a template version
Retiring a template version
Preserving the template version used by each document
The actual contract and certificate files remain an open implementation input until supplied by GECC.

7. Invoice Generation
7.1 Invoice creation
An invoice may be generated only when:

The Master Sales Record is approved.
The selected Master Sales Record version is current and approved.
Required customer data exists.
Required project data exists.
Project-code consistency passes.
Retail price is present.
Payment option is present.
Required artwork exists.
Required template exists.
No blocking workflow condition exists.
7.2 Invoice number
The invoice number must equal:

text


project.project\_code
The system must prevent:

Duplicate invoice numbers
Invoice-number changes after generation
Invoice generation from a mismatched project code
Invoice generation from a superseded Master Sales Record version
7.3 Invoice amount
The initial invoice amount is sourced from the approved retail price.

Approved adjustments may later produce:

Original invoice amount
Adjustment amount
Adjusted amount due
The original invoice amount remains preserved.

7.4 Invoice versioning
A new invoice version is required when:

The Master Sales Record is revised.
Retail price changes.
Payment terms change.
Required invoice data changes.
Approved adjustments require a revised invoice.
A document-generation correction is authorized.
The original invoice version remains preserved.

8. Contract Generation
8.1 Contract prerequisites
The contract may be generated only when:

The Master Sales Record is approved.
The required contract template is active.
Required customer fields exist.
Required project fields exist.
Scope data is complete.
Contract field mapping is valid.
State-specific artwork is available or an approved override exists.
Required signature fields are mapped.
8.2 Contract population
The system must populate the approved template from controlled data.

The system must not require the Sales Associate to re-enter data already stored in the Master Sales Record.

8.3 Contract review
After generation:

The Sales Associate may review the generated contract.
The Sales Associate may report an error.
The Sales Associate may not directly alter the PDF to change source data.
Corrections must be made to the source record or authorized template configuration.
Regeneration creates a new document version where appropriate.
9. Certificate Generation
9.1 Certificate prerequisites
The certificate may be generated when:

The approved Master Sales Record exists.
Required project and customer data exists.
Approved scope exists.
Equipment and serial-number fields required for the certificate are present.
The certificate template is active.
Required artwork exists or an approved override exists.
Required signature fields are mapped.
9.2 Certificate updates
The certificate may use completion information recorded later by the Technician, including:

Actual equipment serial numbers
Actual materials used
Installation completion date
Completion notes
Approved scope version
A certificate generated before completion may be regenerated after required completion data becomes available.

9.3 Certificate versioning
A certificate regenerated after a scope or source-data change receives a new version.

The original certificate remains preserved.

10. Artwork Management
10.1 artwork_asset
Field	Type	Requirement
id	BIGINT	Primary key
artwork_type	VARCHAR(40)	GECC logo, state license
name	VARCHAR(200)	Required
state_code	CHAR(2)	Required for state license artwork
file_location	TEXT	Required
base64_content_hash	VARCHAR(128)	Required
mime_type	VARCHAR(100)	Required
file_extension	VARCHAR(20)	Required
width_pixels	INTEGER	Required where applicable
height_pixels	INTEGER	Required where applicable
artwork_status	VARCHAR(30)	Draft, active, retired
uploaded_by	BIGINT	Required foreign key
uploaded_at	TIMESTAMPTZ	Required
Standard retirement columns	—	Required
10.2 artwork_version
Field	Type	Requirement
id	BIGINT	Primary key
artwork_asset_id	BIGINT	Required foreign key
version_number	INTEGER	Required
effective_from	DATE	Required
effective_to	DATE	Optional
is_active	BOOLEAN	Required
approval_status	VARCHAR(30)	Required
approved_by	BIGINT	Optional foreign key
approved_at	TIMESTAMPTZ	Optional
retired_by	BIGINT	Optional foreign key
retirement_reason	TEXT	Optional
Standard audit columns	—	Required
10.3 Artwork requirements
The system must support:

Upload of Base64 artwork
Approved image-format validation
GECC logo artwork
State-specific license artwork
State association
Effective date
Activation
Retirement
Preview before activation
Historical preservation
Document-level artwork references
10.4 Base64 validation
The system must validate:

Valid Base64 encoding
Permitted MIME type
Permitted image format
Decodable image content
Non-empty content
Maximum file size
Image dimensions
Content hash
Malware or unsafe-file screening where supported
The approved format list remains a configuration item.

10.5 Automatic state selection
For a project in state XX, the system selects the active artwork version where:

text


artwork\_type = state\_license
state\_code = XX
effective\_from <= document\_date
effective\_to is null or effective\_to >= document\_date
approval\_status = approved
is\_active = true
The selected artwork version is stored on the document version.

10.6 Artwork override
An authorized user may override automatic selection only when:

Alternate artwork is selected.
An approval reason is entered.
Sales Manager or Comptroller approval is recorded.
The override is linked to the document-generation request.
An audit event is created.
Document generation must stop if the override is incomplete.

10.7 Missing artwork
Document generation must stop if:

Required state artwork is missing.
No active artwork applies to the project state.
Artwork is not approved.
Artwork is expired.
Artwork content fails validation.
An override lacks approval or reason.
The artwork version cannot be preserved.
11. Artwork Selection
document_artwork_selection
Field	Type	Requirement
id	BIGINT	Primary key
document_version_id	BIGINT	Required foreign key
project_state_code	CHAR(2)	Required
automatic_artwork_version_id	BIGINT	Optional foreign key
selected_artwork_version_id	BIGINT	Required foreign key
selection_method	VARCHAR(30)	Automatic, approved override
override_reason	TEXT	Required for override
override_approved_by	BIGINT	Required for override
override_approved_at	TIMESTAMPTZ	Required for override
selected_at	TIMESTAMPTZ	Required
selected_by	BIGINT	Required foreign key
Each document retains:

Selected state
Artwork version
Artwork effective date
Uploading or changing user
Date and time used
Selection method
Override reason where applicable
12. Document-Generation Validation
Before generation, the system must validate:

Required fields
Customer name
Customer contact data where required
Project code
Project location
State
Scope
Equipment where required
Retail price where required
Payment option
Required signer fields
Required template
Required artwork
Consistency
Customer matches the project.
Project matches the Master Sales Record.
Project code matches the invoice number where applicable.
Current Master Sales Record version is used.
Scope matches the approved version.
Equipment and serial numbers match the approved or current authorized data.
Pricing matches the approved version.
Document references the correct version.
State artwork matches the project location unless an approved override exists.
Workflow
Required approval exists.
Required task or state permits generation.
The project is not cancelled.
The source version is not superseded.
A revised document is generated when a required source version changes.
Document generation must fail with actionable validation messages.

13. Document Rendering
The rendering process must:

Load the current approved source version.
Load the active template version.
Resolve document field mappings.
Resolve the state-specific artwork.
Validate all required fields.
Render the document.
Create a content hash.
Save the PDF to controlled file storage.
Create the document-version record.
Save source-field traceability references.
Mark the document version generated.
Create the audit event.
Return the preview or saved-file reference.
The source Master Sales Record must not be modified by rendering.

14. File Storage
Generated documents must be stored in the customer/project file structure.

The file system or object store must preserve:

Project identifier
Document type
Document version
Master Sales Record version
Generation timestamp
Content hash
Current or superseded status
Signature status
File location
Access restrictions
Recommended logical path:

text


/customer/{customer\_id}/project/{project\_id}/{document\_type}/
Recommended file naming pattern:

text


{project\_code}-{document\_type}-v{version\_number}.pdf
The exact physical storage technology remains an implementation question.

15. Save, Preview, Print, and Email
Authorized forms must support:

Save source data
Save draft
Generate document
Preview document
Save PDF
Print document
Email document
Send document for electronic signature
Clear form
Cancel form
15.1 Preview
Preview must:

Use the same data and template as final generation.
Display selected artwork.
Display document version information where configured.
Not create a final current document version unless explicitly saved.
Record preview activity in the audit log.
15.2 Print
Printing must:

Use the generated document version.
Respect user authorization.
Record the print event.
Preserve the printed document's content hash where available.
15.3 Email
Emailing must:

Use an authorized document version.
Record sender, recipient, timestamp, document version, and result.
Prevent unauthorized recipients where recipient rules apply.
Preserve the emailed document version.
Not silently replace a current document with an older version.
15.4 Electronic-signature handoff
Sprint 07 creates the document and signature-handoff request data.

Sprint 08 implements:

Signature delivery
Signer authentication
Signing order
Reminders
Expiration
Audit certificates
Signed PDF return
16. Document Version Rules
A document version is immutable after generation.

The system must preserve:

Source Master Sales Record version
Template version
Artwork version
Document version
File location
Content hash
Generated timestamp
Generating user
Field-source traceability
Signature references
Superseded-document relationship
A revised document:

Receives a new version number.
References the version it supersedes.
Is generated from the new approved source version.
Does not alter the original document.
Enters the required signature process where applicable.
17. Audit Events
Sprint 07 must create audit events for:

Template uploaded
Template previewed
Template activated
Template retired
Artwork uploaded
Artwork validated
Artwork previewed
Artwork activated
Artwork retired
Artwork override requested
Artwork override approved
Artwork override rejected
Document-generation request
Document-generation validation failure
Document generated
Document previewed
Document saved
Document printed
Document emailed
Document sent for signature
Document superseded
Document cancelled
Document downloaded
Unauthorized document-generation attempt
Missing-artwork failure
Template-mapping failure
Source-data mismatch
Each event must reference:

User
Role
Session
Project
Customer
Document
Document version
Master Sales Record version
Template version
Artwork version where applicable
Outcome
Reason
Content hash where applicable
Audit-chain references
18. Sprint 07 Decisions
D-07-001 — Source data
All controlled documents are generated from the approved Master Sales Record version.

D-07-002 — Document immutability
Generated document versions are immutable.

Corrections create new document versions.

D-07-003 — Document traceability
Every document version references:

Master Sales Record version
Template version
Artwork version
Generation user
Generation time
File content hash
D-07-004 — Invoice number
The invoice number equals the project identification code.

D-07-005 — Automatic state artwork
The project location's state determines default license artwork.

D-07-006 — Artwork override
Artwork overrides require alternate artwork, a reason, Sales Manager or Comptroller approval, and an audit event.

D-07-007 — Missing-artwork block
Document generation stops when required artwork is missing, inactive, expired, invalid, or improperly overridden.

D-07-008 — Template preservation
A document retains the exact template version used for generation, even after that template is retired.

D-07-009 — Signature separation
Sprint 07 generates documents and signature handoff data. Sprint 08 controls the electronic-signature process.

D-07-010 — Source-data correction
A PDF may not be manually edited to change underlying project data. Source records must be corrected through the approved workflow and the document regenerated.

19. Sprint 07 Acceptance Criteria
Sprint 07 is accepted when:

An approved Master Sales Record can generate an invoice.
An approved Master Sales Record can populate a contract template.
An approved Master Sales Record can populate a certificate template.
The invoice number equals the project identification code.
Documents use the correct customer and project.
Documents use the correct approved Master Sales Record version.
Documents use the correct scope.
Documents use the correct equipment and serial numbers where applicable.
Documents use the correct retail price.
Documents use the correct payment option.
Required fields are validated before generation.
Document generation fails when required data is missing.
Document generation fails when customer, project, or project-code consistency fails.
Document generation fails when required approvals are missing.
Document generation fails when the source version is superseded.
Contract and certificate templates are versioned.
Generated documents are stored in the customer/project file.
Generated documents have content hashes.
Generated document versions are immutable.
Original documents remain preserved after revision.
Revised documents reference their superseded versions.
GECC logo artwork can be applied.
State-specific license artwork is selected automatically.
Artwork is validated before activation.
Artwork versions are effective-dated.
Historical documents retain the artwork version used.
Missing artwork prevents generation.
Unauthorized artwork overrides are rejected.
Authorized artwork overrides require reason and approval.
Preview uses the same source data and template as generation.
Printing is permission-controlled and audited.
Emailing is permission-controlled and audited.
Signature handoff references the exact document version.
All generation and artwork actions are audited.
The document-generation process is ready for Sprint 08 electronic signatures.
20. Sprint 07 Test Cases
Invoice tests
Generate an invoice from an approved Master Sales Record.
Confirm invoice number equals project code.
Reject invoice generation from an unapproved record.
Reject invoice generation from a superseded version.
Reject invoice generation with missing retail price.
Confirm payment option appears correctly.
Confirm the generated PDF is stored and hashed.
Confirm the invoice version is linked to the source version.
Contract tests
Upload a contract template.
Register a template version.
Populate the contract from approved data.
Confirm customer data is consistent.
Confirm project-code data is consistent.
Confirm scope data is consistent.
Reject generation with missing template field mapping.
Retire a template and confirm historical documents remain available.
Certificate tests
Generate a certificate with approved scope.
Populate equipment and serial numbers.
Reject generation when required serial numbers are missing.
Regenerate after completion information is recorded.
Preserve the original certificate version.
Confirm the new certificate references the current source version.
Artwork tests
Upload valid Base64 artwork.
Reject invalid Base64 content.
Reject unsupported image type.
Reject empty artwork.
Associate artwork with a state.
Activate artwork after approval.
Automatically select artwork based on project state.
Reject generation when artwork is missing.
Request an artwork override.
Reject override without a reason.
Reject override without Sales Manager or Comptroller approval.
Confirm the selected artwork version is retained on the document.
Versioning tests
Generate Version 1 of a document.
Change the approved source through an authorized revision.
Generate Version 2.
Confirm Version 1 remains unchanged.
Confirm Version 2 references Version 1 as superseded.
Confirm each version has a different content hash.
Confirm each version retains its source and template references.
Access and audit tests
Unauthorized user cannot generate restricted documents.
Unauthorized user cannot download restricted documents.
Unauthorized user cannot print restricted documents.
Unauthorized user cannot approve artwork overrides.
Document generation is audited.
Preview is audited.
Print is audited.
Email is audited.
Artwork activation is audited.
Validation failures are audited.
21. Sprint 07 Open Questions
These implementation questions do not change approved requirements:

What exact contract PDF will GECC provide?
What exact certificate PDF will GECC provide?
What invoice layout and field placement will be approved?
Which PDF-rendering or overlay approach will be used?
Which image formats are approved for logo and license artwork?
What maximum artwork dimensions and file sizes are permitted?
What exact signature field placements will the templates use?
What physical file-storage technology will be used?
What file-retention and backup policy applies to generated documents?
What email service will deliver documents?
What document metadata must appear visibly on each PDF?
What exact recipients may receive each document type?
Which document changes require a new customer signature?
What signature-handoff payload will Sprint 08 require?
What legal or operational wording is required on generated documents?
22. Sprint 08 Handoff
Sprint 08 will implement electronic signatures.

Sprint 08 must use:

Document ID
Document version ID
Project ID
Customer ID
Master Sales Record version
Template version
Artwork version
File location
Content hash
Signer roles
Required signing order
Current workflow state
Sprint 08 must preserve:

Original unsigned document
Signature-request version
Delivery history
Reminder history
Expiration history
Completed signed PDF
Signature audit certificate
Relationship between the signed document and source Master Sales Record version
Sprint 08 must not alter the underlying Master Sales Record through signature processing.

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
Sprint 00 decisions
Sprint 01 decisions
Sprint 02 decisions
Sprint 03 decisions
Sprint 04 decisions
Sprint 05 decisions
Sprint 06 decisions
Approved document, artwork, versioning, workflow, and audit requirements
Changes introduced by this bundle
Defines document-generation architecture.
Defines invoice, contract, and certificate generation.
Defines template and document versioning.
Defines artwork storage, validation, activation, and retirement.
Defines automatic state-artwork selection.
Defines artwork override controls.
Defines document rendering and storage.
Defines print, email, and signature-handoff behavior.
Defines Sprint 07 audit events.
Defines Sprint 08 electronic-signature handoff.


