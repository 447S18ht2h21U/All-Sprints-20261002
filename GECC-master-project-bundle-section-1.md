**cat \\**

&#x20; **GECC-00-project-baseline.md \\**

&#x20; **GECC-01-data-model.md \\**

&#x20; **GECC-02-auth-and-permissions.md \\**

&#x20; **GECC-03-audit-log.md \\**

&#x20; **GECC-04-customers-and-projects.md \\**

&#x20; **GECC-05-master-sales-record.md \\**

&#x20; **GECC-06-workflow-state-machine.md \\**

&#x20; **> GECC-master-project-bundle-section-1.md**



**GECC 00 Project Baseline**

\# GECC-00-project-baseline.md



\# GECC Sales Back Office System — Project Baseline



\## Bundle Metadata



\- Bundle name: GECC-00-project-baseline.md

\- Project: GECC Sales Back Office System

\- Company: Go Ecco Climate Control

\- Company abbreviation: GECC

\- Bundle type: Authoritative project baseline

\- Status: Approved baseline for development planning

\- Created date: 2026-09-19

\- Previous bundle: None

\- Next planned bundle: GECC-01-data-model.md

\- Primary database: Robust searchable relational database

\- Google Sheets: Optional export or reporting destination; not the system of record



\---



\## 1. Project Purpose



GECC needs a unified sales back-office system that eliminates repetitive data entry and manual document preparation.



The system must allow a Sales Associate to enter project information once into a Master Sales Record. That information must then be reused consistently to generate:



\- Invoice

\- Contract

\- Certificate

\- Contractor commission reports

\- Payroll or contractor-payment summaries

\- Accounts receivable records

\- Accounts payable records

\- Project profitability reports

\- Customer profitability reports

\- Balance sheet

\- Cash-flow statement

\- Statement of operations

\- Inventory records

\- Fixed-asset records

\- Other approved system outputs



The system must prevent inconsistent data and preserve a complete audit trail of all records, documents, approvals, signatures, payments, revisions, and access activity.



\---



\## 2. Business Description



GECC is an HVAC sales and HVAC service company.



GECC canvasses neighborhoods to find residential customers who need to:



\- Replace HVAC equipment

\- Repair HVAC equipment

\- Service HVAC equipment

\- Remove old equipment

\- Install new equipment



Different people perform different functions, including:



\- Sales

\- Installation

\- Technical work

\- Database administration

\- Accounts payable

\- Payroll or contractor-payment administration

\- Financial management



GECC has approximately twelve employees or contractors:



\- 4 Sales Associates

\- 3 Technicians

\- 1 Sales Manager

\- 1 Installation Manager

\- 1 Database Administrator

\- 1 Accounts Payable Associate

\- 1 Comptroller



All workers are classified as independent contractors and are responsible for their own taxes and benefits.



\---



\## 3. Core Business Problem



The current process requires the same information to be entered repeatedly into:



1\. A Google spreadsheet serving as an informal database

2\. An invoice

3\. A contract

4\. A certificate

5\. Contractor payroll or commission records

6\. Financial reporting records



Documents are manually downloaded as PDFs and manually transferred for signature.



The new system must replace repetitive entry, manual document preparation, manual document transfer, inconsistent records, and fragmented reporting.



\---



\## 4. Project Success Criteria



The system is successful when:



\- A Sales Associate enters sale information once.

\- A Master Sales Record is created for every project.

\- The Master Sales Record is the single source of truth for that project.

\- Generated documents use the approved Master Sales Record.

\- No repetitive document data entry is required.

\- Contracts and certificates are populated automatically from existing PDF templates.

\- Invoices are generated automatically.

\- Documents can be saved, printed, emailed, and sent for electronic signature.

\- Contractor commission reports are generated automatically.

\- Financial reports are generated automatically.

\- Records are searchable.

\- Access is authorization-based.

\- Original records and all revisions are retained permanently.

\- Financial amounts and documents remain consistent.

\- Every significant action is auditable.

\- Approved records cannot be changed by unauthorized users.

\- Project status and outstanding tasks are visible through an activity dashboard.



\---



\## 5. Explicitly Out of Scope



The system must not:



\- Schedule neighborhood canvassing

\- Schedule technicians beyond project installation tasks

\- Track technician time

\- Track technician GPS locations

\- Manage employee benefits

\- Manage hiring

\- Manage general HR records

\- Replace general HR software

\- Automatically import bank statements during the initial implementation

\- Treat Google Sheets as the primary database



Google Sheets may be used as an export or reporting destination.



\---



\## 6. Primary System Principles



\### 6.1 Single source of truth



Each project has one Master Sales Record. Customer, project, pricing, scope, equipment, cost, payment, document, commission, and profitability information must be linked to that record.



\### 6.2 Controlled state machine



Users must not be able to skip required steps or perform actions out of order.



\### 6.3 Immutable approved records



Approved records are locked. Authorized reopening creates a new version and preserves the original.



\### 6.4 Consistent generated output



Invoices, contracts, certificates, commission reports, and financial reports must be generated from controlled database records.



\### 6.5 Auditability



Original data, revisions, approvals, signatures, documents, access attempts, payments, costs, and reports must remain auditable.



\### 6.6 Least-privilege access



Each user receives only the access needed for their role. Access applies at the screen, record, field, and action levels.



\---



\## 7. User Roles



\### 7.1 Sales Associate



May:



\- Create customers

\- Create Master Sales Records

\- Create project records

\- Enter and update unapproved project information

\- Review generated contracts

\- Send contracts to customers

\- View assigned customer and project information

\- View commission information related only to their own sales



May not:



\- Approve sales or contracts

\- Edit approved Master Sales Records

\- Change approved retail price

\- Change approved costs

\- View another Sales Associate’s commissions

\- Approve payroll or commission reports

\- Approve financial reports



\### 7.2 Sales Manager



May:



\- Review and approve Master Sales Records

\- Enter or confirm retail prices

\- Sign contracts and invoices

\- Approve payment credits, refunds, and adjustments

\- Enter overhead expenses

\- Enter or revise permitted costs

\- Reopen approved records

\- Approve commission changes

\- Approve payroll or commission reports

\- Approve and sign published financial statements

\- View all business data

\- View the tamper-evident audit log

\- Manage selected operational exceptions



\### 7.3 Comptroller



May:



\- Review and approve Master Sales Records

\- Enter or confirm retail prices

\- Sign contracts and invoices

\- Sign certificates only when the Installation Manager is unavailable

\- Enter overhead expenses

\- Enter or revise permitted costs

\- Reopen approved records

\- Approve commission changes

\- Approve payroll or commission reports

\- Approve and sign published financial statements

\- View all business data

\- View the tamper-evident audit log

\- Manage financial and accounting functions



\### 7.4 Installation Manager



May:



\- Schedule installation tasks

\- View installation-related projects

\- Review installation status

\- Enter or approve installation-related cost information

\- Sign certificates

\- View change history

\- Access the tamper-evident audit log

\- View assigned or authorized project information



The Installation Manager does not sign financial statements.



\### 7.5 Technician



May:



\- View assigned projects

\- View approved scope of work

\- Update equipment serial numbers

\- Record installation completion

\- Record actual materials used

\- Enter completion notes

\- Record proposed scope changes

\- Review and send certificates to customers



May not:



\- Change retail price

\- Change approved costs

\- Change contract terms

\- Approve sales

\- Approve contracts

\- Approve financial reports

\- View unauthorized commissions, payroll, or financial information



\### 7.6 Accounts Payable Associate



May:



\- View authorized vendor and payable records

\- Enter payable payments

\- Update payable status

\- Record payment dates and methods

\- View related project information necessary for accounts-payable duties



\### 7.7 Database Administrator



May:



\- Search authorized records

\- Maintain database records according to assigned permissions

\- Maintain data quality

\- Administer approved database functions



May not automatically approve sales, commissions, payroll, or financial reports unless separately assigned that authorization.



\### 7.8 Other Contractors



May:



\- View only projects and tasks specifically assigned to them

\- View only the fields required to complete their assigned work



\---



\## 8. Core Data Entities



The system should use a relational database with linked entities.



\### 8.1 Customer



Fields include:



\- Customer ID

\- Customer name

\- Multiple phone numbers

\- Multiple email addresses

\- Mailing address, if required

\- Customer status

\- Customer folder reference

\- Creation date

\- Last modified date

\- Duplicate-check information



\### 8.2 Customer Contact



Fields include:



\- Contact ID

\- Customer ID

\- Contact name

\- Contact type

\- Phone number

\- Email address

\- Primary contact indicator

\- Preferred contact method

\- Active or inactive status



\### 8.3 Project



Fields include:



\- Project ID

\- Customer ID

\- Project identification code

\- Project location

\- Assigned Sales Associate

\- Assigned Technician or Technicians

\- Scope of work

\- Project status

\- Creation date

\- Completion date

\- Cancellation status

\- Current Master Sales Record version



A customer may have multiple projects.



A single Master Sales Record exists for each project location and project scope.



\### 8.4 Project Location



Separate fields are required for:



\- Street address

\- City

\- State

\- ZIP code



The project location’s state determines the default license artwork.



\### 8.5 Master Sales Record



Fields include:



\- Master Sales Record ID

\- Project ID

\- Customer ID

\- Version number

\- Creation date

\- Created by

\- Assigned Sales Associate

\- Retail price

\- Payment option

\- Scope of work

\- Equipment

\- Serial numbers

\- Material costs

\- Equipment costs

\- Dealer fees

\- Lead cost

\- Technician Cost

\- Damon cost

\- Jerry cost

\- Contract data

\- Invoice data

\- Certificate data

\- Approval status

\- Document version references

\- Revision reason

\- Revision date and time

\- User making revision



The Master Sales Record is the authoritative source for project information.



\### 8.6 Equipment



Multiple equipment records may be associated with one project.



Fields include:



\- Equipment ID

\- Project ID

\- Brand

\- Model

\- Function

\- Serial number

\- Equipment category

\- New, existing, removed, or installed status

\- Inventory reference

\- Notes



\### 8.7 Invoice



Fields include:



\- Invoice ID

\- Invoice number

\- Project identification code

\- Master Sales Record version

\- Invoice amount

\- Payment option

\- Adjustment amount

\- Adjusted amount due

\- GECC signer

\- GECC signature date

\- Customer signer

\- Customer signature date

\- Invoice status

\- PDF version

\- Signature audit certificate

\- File location



The invoice number equals the unique project identification code.



\### 8.8 Contract



The contract is an existing PDF template provided by GECC.



Fields include:



\- Contract ID

\- Project ID

\- Master Sales Record version

\- Template version

\- Customer signer

\- Customer signature date

\- GECC signer

\- GECC signature date

\- Contract status

\- PDF version

\- Signature audit certificate

\- File location



\### 8.9 Certificate



The certificate is an existing PDF template provided by GECC.



Fields include:



\- Certificate ID

\- Project ID

\- Master Sales Record version

\- Template version

\- Customer signer

\- Customer signature date

\- Installation Manager signer

\- Installation Manager signature date

\- Comptroller substitute signer, if applicable

\- Substitute-signature reason

\- Certificate status

\- PDF version

\- Signature audit certificate

\- File location



\### 8.10 Payment



Fields include:



\- Payment ID

\- Project ID

\- Invoice ID

\- Project identification code

\- Payment method

\- Amount

\- Payment date

\- Received date

\- Verification date

\- Verification status

\- Cashier’s check number

\- Bank name

\- Bank-wire reference number

\- Uploaded proof of receipt

\- User entering payment

\- Notes

\- Adjustment reference



Supported initial payment methods:



\- Bank cashier’s check

\- Bank wire transfer



\### 8.11 Accounts Receivable



Accounts receivable must track:



\- Invoice amount

\- Amount paid

\- Approved credits

\- Approved refunds

\- Approved adjustments

\- Remaining balance

\- Payment status

\- Verification status

\- Project identification code

\- Customer

\- Invoice



Unpaid invoices remain visible as accounts receivable.



Revenue and profit are recognized only when cash is received, subject to the final accounting implementation and review.



\### 8.12 Contractor or Employee



Fields include:



\- Contractor or employee ID

\- Name

\- Address

\- Email

\- Phone

\- Role

\- Independent-contractor status

\- Active or inactive status

\- Commission configuration

\- Assigned projects

\- Payment history



\### 8.13 Overhead Expense



Fields include:



\- Expense ID

\- Date

\- Amount

\- Expense category

\- Required description

\- Vendor

\- Payment status

\- User entering expense

\- Project association, if applicable

\- Approval status

\- Notes



Only the Sales Manager and Comptroller may enter overhead expenses.



\### 8.14 Accounts Payable



Separate payables must be created for:



\- Equipment

\- Dealer fees

\- Materials



Fields include:



\- Payable ID

\- Project ID

\- Vendor

\- Payable category

\- Description

\- Amount owed

\- Certificate signing date

\- Payable creation date

\- Due date

\- Payment date

\- Payment method

\- Amount paid

\- Outstanding balance

\- Payment status

\- Notes

\- Approval history



Accounts payable is created on the Certificate signing date even if GECC has not paid the vendor.



\### 8.15 Inventory



Inventory is tracked on a First In, First Out basis.



Fields include:



\- Inventory ID

\- Serial number

\- Brand

\- Model

\- Function

\- Product type

\- Acquisition date

\- Acquisition cost

\- Vendor

\- Project allocation

\- Inventory status

\- Date issued

\- Cost layer



FIFO should be tracked by product identity and cost layer. Serial-number records must remain individually identifiable.



\### 8.16 Fixed Asset



Initial fixed-asset categories:



\- Vehicles

\- Computers

\- Offices



Vehicle fields include:



\- VIN

\- Description

\- Acquisition date

\- Cost

\- Placed-in-service date

\- Useful life

\- Accumulated depreciation

\- Net book value

\- Disposal information



Computer fields include:



\- Serial number

\- Description

\- Acquisition date

\- Cost

\- Placed-in-service date

\- Useful life

\- Accumulated depreciation

\- Net book value

\- Disposal information



Office fields include:



\- Address

\- Description

\- Acquisition date

\- Cost

\- Placed-in-service date

\- Useful life

\- Accumulated depreciation

\- Net book value

\- Disposal information



Additional asset categories may be created with:



\- Category name

\- Identifying code

\- Description

\- Acquisition date

\- Cost

\- Placed-in-service date

\- Useful life

\- Depreciation data



Depreciation method is always straight-line.



Depreciation begins on the placed-in-service date.



\### 8.17 Loan



Fields include:



\- Loan ID

\- Lender

\- Loan amount

\- Loan date

\- Term

\- Interest rate

\- Payment schedule

\- Principal balance

\- Interest paid

\- Loan status

\- Notes



\### 8.18 Financial-Report Period



Fields include:



\- Period ID

\- Month

\- Year

\- Period start date

\- Period end date

\- Report preparation date

\- Report publication date

\- Approval status

\- Signature status

\- Locked status

\- Adjustment references



Monthly financial reports are prepared on the fifth calendar day of the following month.



Example:



\- May reports are prepared on June 5.



Once published, the period is locked. Corrections require formal adjustment entries.



\---



\## 9. Project Identification Code



Users enter the Sales Associate initials, while the system automatically generates the sequential number.



The code format is:



```text

Sequential Number-Sales Associate Initials-Master Sales Record Creation Date

The sequence begins at 101.



The date format is:



text





YYYY-MM-DD

Example:



text





101-JD-2026-09-19

The system must:



Generate the sequential number automatically

Prevent duplicate project codes

Reserve the number during record creation

Prevent reuse of cancelled project codes

Use the Master Sales Record creation date

Preserve the original code through revisions

Warn about an existing active project at the same customer location

Require an explanation or correction before allowing another active project at that location

10\\. Scope of Work

The system must allow multiple scope selections:



Service

Repair

Install new

Remove old

Under Install New and Remove Old, users may select multiple items:



Condenser coil

Evaporator coil

Refrigerant

Air handler

Furnace

Thermostat

Air ducts

Vents

Service, repair, removal, and installation use the same general workflow.



A project may include multiple pieces of equipment and multiple serial numbers.



11\\. Approved Controlled Lifecycle

Step 1 — Create Master Sales Record

The Sales Associate interviews the customer and creates the Master Sales Record for the customer’s project.



Step 2 — Review and approve

The Sales Manager or Comptroller:



Reviews the record

Enters or confirms the retail price

Approves the Master Sales Record

Both do not need to approve. One authorized approval is sufficient.



Step 3 — Generate documents

After Master Sales Record approval, the system generates:



Invoice

Contract

Certificate

All documents must use the approved Master Sales Record.



Step 4 — Review contract

The Sales Associate reviews the generated contract to ensure that the inserted information is correct.



Step 5 — Customer signs contract

The customer signs and dates the contract first.



Step 6 — GECC signs contract

Either the Sales Manager or Comptroller signs and dates the contract for GECC.



Both do not need to sign.



Step 7 — Send invoice after contract signing

The invoice is generated after Master Sales Record approval, but it is sent for signature only after the contract has been signed.



Step 8 — GECC signs invoice

Either the Sales Manager or Comptroller signs and dates the invoice first.



Step 9 — Customer signs invoice

The customer signs and dates the invoice after the GECC signature.



Step 10 — Create installation task

After both the contract and invoice have all required signatures, the system creates an Installation Manager scheduling task.



Step 11 — Enforce waiting period

Installation may not occur earlier than three days after the customer signs both the contract and invoice.



The system must prevent an earlier installation date unless an authorized exception process is later approved.



Step 12 — Perform scope of work

The Technician performs the approved scope of work and records completion data.



The Technician may update:



Equipment serial numbers

Installation completion

Actual materials used

Completion notes

Proposed scope changes

The Technician may not change:



Retail price

Approved costs

Contract terms

Step 13 — Send certificate

The Technician reviews the populated certificate and sends it to the customer.



Step 14 — Customer signs certificate

The customer signs and dates the certificate.



Step 15 — GECC signs certificate

The Installation Manager signs the certificate.



The Comptroller may sign only when the Installation Manager is unavailable. The Comptroller must record the reason for serving as substitute signer.



Step 16 — Receive payment

The customer pays by:



Bank cashier’s check

Bank wire transfer

Step 17 — Verify cashier’s check

A cashier’s check is recognized as received only after the bank confirms the funds.



Step 18 — Record bank wire

Bank wires are manually recorded during the initial implementation.



Before a bank wire is marked verified, the system must require either:



A manually entered wire reference number, or

Uploaded proof of receipt

Step 19 — Apply approved adjustments

Approved credits, refunds, and adjustments may reduce the amount required for completion.



The Sales Manager must approve any adjustment.



The system must preserve:



Original invoice amount

Adjustment amount

Adjusted amount due

Reason

Approver

Date and time

Step 20 — Mark project complete

The project becomes complete only when:



Scope of work is complete

Contract has all required signatures

Certificate has all required signatures

Invoice has all required signatures

Invoice is fully paid after approved adjustments

Payment verification is complete

Step 21 — Earn commissions

Fixed and variable commissions become earned only after project completion.



Step 22 — Create commission reports

The system includes completed projects in the applicable biweekly commission reports.



Step 23 — Approve commission reports

The Sales Manager and Comptroller review and approve commission reports according to the approved commission-report workflow.



Step 24 — Distribute and retain commission reports

Approved commission reports are:



Sent by email to contractors

Retained permanently

Linked to the relevant projects and commission records

12\\. Scope-Change Workflow

If a Technician identifies a scope change:



Technician records the proposed change and notes.

Project enters a scope-change-pending state.

Sales Manager or Comptroller reviews the change.

Authorized user approves or rejects the change.

Authorized user enters or approves revised price and costs.

System creates a new Master Sales Record version.

System generates revised documents as required.

Revised contract is sent through a new signature sequence.

Original documents remain permanently preserved.

Work on the changed scope cannot continue until required approvals and signatures are complete.

13\\. Document Versioning

When an approved or signed project is revised:



Original Master Sales Record remains permanently preserved.

Original invoice, contract, and certificate remain permanently preserved.

Revised Master Sales Record receives a new version number.

Revised documents receive new version numbers.

Revision includes user, date, time, and reason.

Revised documents are sent through the required new signature sequence.

Original signature audit records remain attached to original documents.

Revised documents are clearly marked as current or superseding versions.

All document versions remain in the customer/project file.

Generated documents must not be independently edited in a way that changes the underlying project data.



14\\. Document Generation

The system must generate documents from the approved Master Sales Record.



Each form should allow authorized users to:



Save data to the database

Generate a document

Save the document to the customer/project file

Print the document

Email the document

Send the document for electronic signature

Clear the form for a new entry

Before generation, the system must validate:



Required fields

Customer consistency

Project consistency

Project-code consistency

Pricing consistency

Scope consistency

Equipment and serial-number requirements

Required signature fields

State-specific license artwork

Document version references

15\\. Company Logo and License Artwork

Invoices, contracts, and certificates must support:



GECC company logo

State-specific license artwork

Artwork requirements:



Upload Base64 artwork

Validate approved image format

Associate artwork with a state

Set effective date

Activate or retire artwork versions

Preview before activation

Preserve artwork version used in each document

License artwork is selected automatically based on the project location’s state.



An authorized user may override automatic state selection only by:



Selecting alternate artwork

Entering an approval reason

Receiving approval from the Sales Manager or Comptroller

Creating an audit-log entry

The system must stop document generation if required artwork is missing or an override lacks required approval.



Each generated document must retain:



Selected state

Artwork version

Artwork effective date

User who uploaded or changed the artwork

Date and time artwork was used

16\\. Electronic Signature System

The built-in signature system must support:



Email delivery

Signature links

Signing order

Customer authentication

Signer authentication

Automatic reminders

Seven-day reminder interval

Seventy-five-day expiration

Signature audit trail

Completed-document certificates

Downloadable signed PDFs

Signature status tracking

Delivery status tracking

Expired-request handling

Unsigned invoices, contracts, and certificates expire after 75 days.



Upon expiration:



Signature request is marked expired.

Sales Manager and Comptroller receive an action task.

Original request remains preserved.

Authorized user may resend, revise, cancel, or extend the request.

Reminder and expiration events remain in the audit trail.

17\\. Cost and Profitability Rules

17.1 Gross Profit

text





Gross Profit =

Invoice Amount

\\- Equipment Cost

\\- Material Cost

\\- Dealer Fee

\\- Lead Cost

\\- Technician Cost

\\- Damon Cost

\\- Jerry Cost

17.2 Cost permissions

Equipment Cost, Material Cost, and Dealer Fee may be entered by:



Sales Manager

Installation Manager

Comptroller

Technician Cost is a variable actual project cost and may be entered by:



Sales Manager

Installation Manager

Lead Cost, Damon Cost, and Jerry Cost may be edited for an individual project by authorized users.



17.3 Default fixed deductions

Initial default values:



Lead: $3,000

Damon: $500

Jerry: $250

Damon and Jerry are contractors. Their fixed costs are deducted from every project whether or not they were involved.



If a fixed deduction changes, the change applies only to the indicated project. It does not automatically change past, future, or other projects.



17.4 Variable commissions

Ryan:



text





Ryan Commission = 15% × Gross Profit

Doug:



text





Doug Commission = 5% × Gross Profit

If Gross Profit is zero or negative:



No variable commission is paid.

Negative profitability is preserved in reporting.

If Net Profit is zero or negative:



No variable commission is paid.

Negative profitability is preserved in reporting.

17.5 Net Profit

text





Net Profit =

Gross Profit

\\- Ryan Commission

\\- Doug Commission

Net Profit is split:



50% to the Sales Associate who created and owns the Master Sales Record

50% to GECC

17.6 Fixed compensation

Fixed compensation is paid once per completed project to:



Lead

Damon

Jerry

17.7 Commission eligibility

Commissions are earned only after:



Scope is complete

Certificate is fully signed

Invoice is fully paid after approved adjustments

Project is complete

Only the Sales Manager may approve:



Early commission payment

Commission changes

Commission changes caused by corrected project costs

Commission adjustments after payroll approval

If project cost is corrected after payroll approval:



Create an adjustment in the next payroll period.

Include an adjustment note.

Preserve the original commission report.

Link the adjustment to the original project and payroll period.

18\\. Contractor Commission Reports

All workers are independent contractors.



Contractor reports are created every two weeks.



Example:



Pay period: May 1 through May 14

Report date: May 15

Report reviewed by Sales Manager and Comptroller

Report approved

Report emailed to applicable contractors

Approved copy permanently retained

Each contractor’s report includes only their authorized commission information.



Sales Associates may see only commission information related to their own sales.



Every January 5, the system sends each contractor employed during the prior year:



Annual commission summary

Commission payment dates

Commission amounts

Copy of each individual biweekly commission report

19\\. Payment and Accounting Rules

Payment methods:



Bank cashier’s check

Bank wire transfer

Payment records must be allocated to the specific customer project using the project identification code.



Unpaid invoices remain visible as accounts receivable.



Revenue and profit are recognized only when cash is received.



Future implementation may add:



Bank-statement import

Bank reconciliation

Automatic transaction matching

Accounts payable is created when the Certificate is signed, even if the payable has not been paid.



Separate payables are required for:



Equipment

Dealer fees

Materials

20\\. Financial Reporting

The system must generate:



Balance sheet

Cash-flow statement

Statement of operations

Accounts receivable report

Accounts payable report

Inventory report

Depreciation report

Loan report

Fixed-asset report

Retained earnings report

Working Capital Requirement report

Customer profitability report

Project profitability report

Contractor commission reports

Financial reporting is cash-basis.



Financial reports are prepared on the fifth calendar day of the following month.



Example:



May report date: June 5

Published financial statements must be approved and signed by:



Sales Manager

Comptroller

The Installation Manager does not sign financial statements.



After publication:



The financial period is locked.

Ordinary edits are prevented.

Corrections require formal adjustment entries.

Original published statements remain preserved.

21\\. Financial Signatures

Master Sales Record

The Sales Manager, Comptroller, or Installation Manager may approve the Master Sales Record according to the applicable workflow.



Contract

Either the Sales Manager or Comptroller may sign for GECC.



Invoice

Either the Sales Manager or Comptroller may sign for GECC.



The customer signs after GECC signs.



Certificate

The Installation Manager signs.



The Comptroller may sign only when the Installation Manager is unavailable and must record the reason.



The customer signs before the authorized GECC signer.



Commission reports

Sales Manager and Comptroller review and approve commission reports.



Published financial statements

Sales Manager and Comptroller approve and sign published financial statements.



22\\. Activity Dashboard

Create an activity dashboard viewable by:



Sales Manager

Comptroller

Installation Manager

Technicians

Sales Associates

Other authorized Contractors

Dashboard access remains permission-based.



Recommended dashboard fields:



Project identification code

Customer name

Project location

Assigned Sales Associate

Assigned Technician

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

Warning or exception indicator

Dashboard filters:



Status

Customer

Project code

Location

State

Sales Associate

Technician

Date range

Payment status

Signature status

Overdue tasks

Expiring documents

Negative-profit projects

Projects requiring action

Financial amounts, commissions, profitability, and payroll information must not appear to users without authorization.



23\\. Search Requirements

Authorized users should be able to search by:



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

Search results must respect role-based access restrictions.



24\\. Authentication and Access Logging

The system must require authenticated login.



The system must log access to:



Dashboard

Database

Master Sales Record

Customer records

Project records

Documents

Financial reports

Commission reports

Administrative configuration

Each access event must record:



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

The system must log:



Successful logins

Failed login attempts

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

Attempts to access restricted information

Administrative changes

Users must not be able to edit or delete audit records through the application.



25\\. Tamper-Evident Audit Log

The audit log must be separate from ordinary business records.



It must be:



Append-only

Read-only to ordinary users

Tamper-evident

Searchable

Exportable

Protected from normal application edits and deletion

Each log entry references the preceding entry to create a chain.



An alteration, deletion, insertion, or reordering of audit entries should cause an integrity verification failure.



Audit-log access is limited to:



Sales Manager

Comptroller

Installation Manager

26\\. Form Functions

Authorized forms should support:



Save to database

Save as draft

Generate document

Save document to customer/project file

Print

Email

Send for signature

Clear form

Cancel form

Reopen authorized record

Create new record

Forms must validate data before saving or generating documents.



27\\. Validation and Error Prevention

The system must prevent:



Duplicate project codes

Invalid project-code format

Duplicate active project locations without explanation

Missing required fields

Unauthorized changes

Installation before the waiting period

Document generation from incomplete data

Contract, invoice, and certificate data mismatches

Unverified payment being treated as received

Commission before project completion

Variable commission on zero or negative profitability

Period edits after financial statements are published

State-artwork omission

Unauthorized artwork override

Document generation without required approvals

28\\. Exception Dashboard

The system should flag:



Contracts awaiting customer signature

Contracts awaiting GECC signature

Invoices awaiting GECC signature

Invoices awaiting customer signature

Certificates awaiting customer signature

Certificates awaiting Installation Manager signature

Signature requests nearing expiration

Expired signature requests

Installations awaiting scheduling

Installation dates violating the three-day rule

Projects with completed work but no payment

Unpaid invoices

Unverified cashier’s checks

Unverified bank wires

Scope changes awaiting approval

Negative-profit projects

Missing equipment serial numbers

Commission adjustments awaiting approval

Payables past due

Financial reports awaiting approval

Closed-period adjustment requests

29\\. Data Integrity and Versioning

The system should use:



Relational database constraints

Foreign keys

Required-field validation

Unique indexes

Controlled enumerated values

Immutable document versions

Versioned Master Sales Records

Append-only audit entries

Transactional saves

Duplicate detection

Status-transition validation

Consistency checks before document generation

The database is the authoritative system of record.



PDFs, emails, exports, and Google Sheets are generated copies or outputs.



30\\. Recommended Accounting Architecture

The system should use a double-entry accounting ledger internally, even though reporting is cash-basis.



The ledger should support:



Accounts

Debits

Credits

Journal entries

Cash receipts

Cash disbursements

Accounts receivable tracking

Accounts payable tracking

Inventory

Fixed assets

Depreciation

Loans

Retained earnings

Period close

Adjusting entries

Final accounting treatment should be validated during the financial-reporting sprint.



31\\. Recommended Sprint Plan

Sprint 00 — Project Baseline and Architecture

Deliver:



Approved requirements

Roles and permissions

Lifecycle

Business rules

Architecture assumptions

Acceptance criteria

Initial backlog

Sprint 01 — Database Model and Accounting Foundation

Deliver:



Relational schema

Core entities

Relationships

Constraints

Versioning model

Accounting foundation

Sprint 02 — Authentication, Roles, and Permissions

Deliver:



Login

Role model

Record permissions

Field permissions

Commission visibility controls

Session rules

Sprint 03 — Tamper-Evident Audit Logging

Deliver:



Append-only audit model

Hash-chain design

Access logging

Restricted-access logging

Verification process

Sprint 04 — Customers, Contacts, Locations, and Projects

Deliver:



Customer records

Multiple contacts

Locations

Duplicate detection

Project code generation

Search

Sprint 05 — Master Sales Record

Deliver:



Master Sales Record form

Scope selections

Equipment

Serial numbers

Pricing

Costs

Payment options

Draft and approved versions

Sprint 06 — Controlled Lifecycle and Task Workflow

Deliver:



State machine

Approval tasks

Installation tasks

Three-day restriction

Scope-change process

Reopen and revision process

Sprint 07 — Document Generation and Artwork

Deliver:



Invoice generation

Contract population

Certificate population

Logo

State license artwork

Base64 validation

Artwork override process

Sprint 08 — Electronic Signatures and Reminders

Deliver:



Signature requests

Signing order

Authentication

Reminders

Expiration

Audit certificates

Signed PDFs

Sprint 09 — Payments, Receivables, Payables, and Adjustments

Deliver:



Cashier’s check

Bank wire

Verification

Payment allocation

Credits

Refunds

Separate payables

Sprint 10 — Costs, Profitability, Commissions, and Contractor Reports

Deliver:



Cost rules

Gross Profit

Net Profit

Fixed compensation

Variable commissions

Commission adjustments

Biweekly reports

Annual summaries

Sprint 11 — Inventory, Assets, Loans, and Depreciation

Deliver:



FIFO inventory

Serial-number tracking

Fixed assets

Straight-line depreciation

Loans

Asset reports

Sprint 12 — Financial Ledger and Reporting

Deliver:



Double-entry ledger

Cash-basis reports

Balance sheet

Cash-flow statement

Statement of operations

Working Capital Requirement

Period close

Sprint 13 — Activity Dashboard and Search

Deliver:



Activity dashboard

Role-specific views

Filters

Exceptions

Project search

Commission visibility

Sprint 14 — Notifications, Email, and File Management

Deliver:



Notifications

Email delivery

Customer/project folders

Delivery audit records

Annual report email process

Sprint 15 — Testing, Security, and Data Integrity

Deliver:



End-to-end testing

Permission testing

Audit testing

Document consistency testing

Financial testing

Backup and restore testing

Sprint 16 — Deployment, Migration, and Operations

Deliver:



Deployment instructions

Initial-user setup

Data migration plan

Backup procedures

Recovery procedures

Administrator guide

User guides

Release checklist

32\\. Acceptance Criteria

The baseline is accepted when:



The Master Sales Record is the single source of truth.

Users cannot bypass required lifecycle steps.

Unauthorized users cannot view or edit restricted information.

Sales Associates see only their own commission data.

Approved records cannot be silently overwritten.

Revisions preserve previous versions.

Documents use consistent approved data.

State-specific artwork is automatically selected.

Artwork overrides require approval and a reason.

The three-day installation rule is enforced.

Projects cannot be complete before all required signatures and payment conditions are met.

Payments are linked to the correct project code.

Cashier’s checks require bank confirmation.

Bank wires require a reference number or proof of receipt.

Variable commissions are withheld when profitability is zero or negative.

Commission adjustments are preserved and approved.

Financial periods can be closed and corrected through adjustments.

Audit entries are append-only and tamper-evident.

Restricted-access attempts are logged.

The activity dashboard shows current project status and required actions.

Reports are searchable and reproducible.

Customer/project files preserve all required documents and versions.

33\\. Open Implementation Items

The following items require inputs or configuration during later sprints:



Existing contract PDF

Existing certificate PDF

Invoice layout and field placement

GECC logo artwork

State-specific license artwork

Exact document signature placement

Electronic-signature authentication method

Accounting chart of accounts

Asset useful lives by category

Vendor master list

Existing customer and project data

Email delivery configuration

User list and role assignments

Contractor payment-report formatting

Financial-report distribution list

Final accounting treatment of accounts payable and cash-basis reporting

34\\. Known Risks

Existing PDFs may not be fillable or may require specialized PDF-overlay processing.

Electronic-signature requirements may require additional legal and technical validation.

Incorrect cost configuration could produce incorrect profitability and commission results.

Cashier’s-check verification may remain manual until bank integration is implemented.

Financial reporting requires a carefully configured chart of accounts.

Existing spreadsheet data may contain duplicates or inconsistent values.

State license artwork may change and require version management.

Large audit logs may require archival and integrity-verification procedures.

35\\. Merge Instructions

This bundle is the baseline project context.



When a later bundle is created:



Preserve this file unchanged.

Create the next bundle using the next sprint number.

Record all new decisions and changes.

If a later decision conflicts with this baseline, the later approved decision supersedes the earlier one.

Preserve the superseded requirement in the historical record.

Do not silently delete prior requirements.

Append bundles in sprint order to create the master file.

Master file name:



text





GECC-master-project-bundle.md

Recommended bundle order:



text





GECC-00-project-baseline.md

GECC-01-data-model.md

GECC-02-auth-and-permissions.md

GECC-03-audit-log.md

GECC-04-customers-and-projects.md

GECC-05-master-sales-record.md

GECC-06-workflow-state-machine.md

GECC-07-document-generation.md

GECC-08-electronic-signatures.md

GECC-09-payments-and-payables.md

GECC-10-costs-and-commissions.md

GECC-11-inventory-assets-and-loans.md

GECC-12-financial-reporting.md

GECC-13-dashboard-and-search.md

GECC-14-notifications-and-files.md

GECC-15-testing-and-integrity.md

GECC-16-deployment-and-operations.md

36\\. Next Sprint

Sprint number: 01

Sprint name: Database Model and Accounting Foundation

Objective: Convert this approved baseline into a detailed relational database schema and accounting foundation.

Required inputs:

No additional business requirements required to begin

Existing documents may be deferred until Sprint 07

Expected deliverables:

Entity relationship model

Table definitions

Field definitions

Keys and relationships

Constraints

Versioning structure

Audit references

Initial accounting data model

Database acceptance tests

---

**# GECC 01 Data Model**

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
- Business timezone: America/New\_York
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

---

**# GECC 02 Auth And Permissions**

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
- Business timezone: America/New\_York
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
| permission\_code | VARCHAR(100) | Required, unique |
| resource\_type | VARCHAR(80) | Required |
| action\_code | VARCHAR(50) | Required |
| description | TEXT | Required |
| is\_sensitive | BOOLEAN | Required |
| record\_status | VARCHAR(30) | Required |
| is\_active | BOOLEAN | Required |
| retired\_at | TIMESTAMPTZ | Optional |
| retired\_by | BIGINT | Optional foreign key |
| retirement\_reason | TEXT | Optional |

Example permission codes:

```text
customer.view
customer.create
customer.edit
project.view
project.create
project.edit
master\\\_sales\\\_record.approve
master\\\_sales\\\_record.reopen
invoice.sign
contract.sign
certificate.sign
payment.verify
adjustment.approve
commission.view\\\_own
commission.view\\\_all
commission\\\_report.approve
financial\\\_report.publish
financial\\\_period.lock
audit\\\_log.view
audit\\\_log.export
5.2 role\_permission
Field	Type	Requirement
id	BIGINT	Primary key
role\_id	BIGINT	Required foreign key
permission\_id	BIGINT	Required foreign key
effective\_from	DATE	Required
effective\_to	DATE	Optional
granted\_by	BIGINT	Required foreign key
grant\_reason	TEXT	Required
record\_status	VARCHAR(30)	Required
is\_active	BOOLEAN	Required
retired\_at	TIMESTAMPTZ	Optional
retired\_by	BIGINT	Optional
retirement\_reason	TEXT	Optional
Constraint:

text


UNIQUE(role\\\_id, permission\\\_id, effective\\\_from)
5.3 record\_access\_scope
Field	Type	Requirement
id	BIGINT	Primary key
user\_id	BIGINT	Required foreign key
resource\_type	VARCHAR(80)	Required
resource\_id	BIGINT	Required
scope\_type	VARCHAR(40)	Assignment, ownership, authorization
effective\_from	DATE	Required
effective\_to	DATE	Optional
granted\_by	BIGINT	Required foreign key
grant\_reason	TEXT	Required
record\_status	VARCHAR(30)	Required
is\_active	BOOLEAN	Required
retired\_at	TIMESTAMPTZ	Optional
retired\_by	BIGINT	Optional
retirement\_reason	TEXT	Optional
This table supports explicit access for assigned projects and contractors.

5.4 field\_permission
Field	Type	Requirement
id	BIGINT	Primary key
role\_id	BIGINT	Required foreign key
resource\_type	VARCHAR(80)	Required
field\_name	VARCHAR(120)	Required
can\_view	BOOLEAN	Required
can\_edit	BOOLEAN	Required
can\_export	BOOLEAN	Required
effective\_from	DATE	Required
effective\_to	DATE	Optional
record\_status	VARCHAR(30)	Required
is\_active	BOOLEAN	Required
5.5 user\_session
Field	Type	Requirement
id	BIGINT	Primary key
user\_id	BIGINT	Required foreign key
session\_identifier	VARCHAR(200)	Required, unique
created\_at	TIMESTAMPTZ	Required
last\_activity\_at	TIMESTAMPTZ	Required
expires\_at	TIMESTAMPTZ	Required
logout\_at	TIMESTAMPTZ	Optional
termination\_reason	VARCHAR(100)	Optional
session\_status	VARCHAR(30)	Active, expired, logged\_out, revoked
client\_reference	TEXT	Optional
Session records are retained for audit integration.

5.6 authentication\_event
Field	Type	Requirement
id	BIGINT	Primary key
user\_id	BIGINT	Optional foreign key
login\_identifier	VARCHAR(320)	Required
event\_type	VARCHAR(40)	Login success, login failure, logout, lockout
occurred\_at	TIMESTAMPTZ	Required
session\_id	BIGINT	Optional foreign key
success\_status	BOOLEAN	Required
failure\_reason	VARCHAR(200)	Optional
source\_reference	TEXT	Optional
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

---

**# GECC 03 Audit Log**

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
- Business timezone: America/New\_York
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
- Audit timestamps use `America/New\_York` for business interpretation while preserving the actual event instant.

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
audit\\\_log.view
audit\\\_log.search
audit\\\_log.export
audit\\\_log.verify
audit\\\_log.view\\\_integrity\\\_failure
audit\\\_log.view\\\_archived
Audit-log access must be separately authorized from general database access.

5. Audit Event Data Model
5.1 audit\_event
Field	Type	Requirement
id	BIGINT	Primary key
event\_sequence\_number	BIGINT	Required, unique
event\_uuid	UUID	Required, unique event reference
occurred\_at	TIMESTAMPTZ	Required
recorded\_at	TIMESTAMPTZ	Required
user\_id	BIGINT	Optional foreign key
user\_role\_snapshot	VARCHAR(100)	Required when user identified
session\_id	BIGINT	Optional foreign key
event\_type	VARCHAR(80)	Required
activity\_type	VARCHAR(80)	Required
resource\_type	VARCHAR(100)	Optional
resource\_id	BIGINT	Optional
resource\_business\_key	VARCHAR(250)	Optional
related\_project\_id	BIGINT	Optional foreign key
related\_customer\_id	BIGINT	Optional foreign key
outcome	VARCHAR(30)	Success, failure, denied, error
failure\_reason	TEXT	Required for failures where available
reason\_text	TEXT	Required when business rules require a reason
request\_reference	VARCHAR(200)	Optional
source\_screen	VARCHAR(150)	Optional
source\_operation	VARCHAR(150)	Optional
event\_payload	JSONB	Required, canonicalized
previous\_event\_hash	VARCHAR(128)	Required except genesis event
event\_hash	VARCHAR(128)	Required
hash\_algorithm	VARCHAR(30)	Required
integrity\_status	VARCHAR(30)	Valid, unverified, failed
archive\_status	VARCHAR(30)	Hot, archived, restored
archive\_segment\_id	BIGINT	Optional
created\_at	TIMESTAMPTZ	Required
5.2 Audit event sequence
event\_sequence\_number is assigned monotonically.

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
The hash\_algorithm field must be stored with each event to support future algorithm migration.

6.2 Canonical event representation
The event hash is calculated from a canonical representation containing:

text


event\\\_sequence\\\_number
event\\\_uuid
occurred\\\_at
recorded\\\_at
user\\\_id
user\\\_role\\\_snapshot
session\\\_id
event\\\_type
activity\\\_type
resource\\\_type
resource\\\_id
resource\\\_business\\\_key
related\\\_project\\\_id
related\\\_customer\\\_id
outcome
failure\\\_reason
reason\\\_text
request\\\_reference
source\\\_screen
source\\\_operation
canonical\\\_event\\\_payload
previous\\\_event\\\_hash
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


event\\\_hash =
SHA-256(
  canonical\\\_event\\\_representation
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
  "field": "project\\\_status",
  "before": "contract\\\_pending",
  "after": "installation\\\_ready"
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

Partitions should generally be organized by month using recorded\_at.

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

Mark the affected event or segment as integrity\_failure.
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

---

**# GECC 04 Customers And Projects**

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
- Business timezone: America/New\_York
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
customer\_number	VARCHAR(50)	Required, unique
customer\_name	VARCHAR(250)	Required
customer\_status	VARCHAR(30)	Required
customer\_folder\_reference	TEXT	Optional
duplicate\_check\_key	VARCHAR(500)	Required, indexed
notes	TEXT	Optional
created\_at	TIMESTAMPTZ	Required
created\_by	BIGINT	Required foreign key
updated\_at	TIMESTAMPTZ	Required
updated\_by	BIGINT	Required foreign key
is\_active	BOOLEAN	Required
retired\_at	TIMESTAMPTZ	Optional
retired\_by	BIGINT	Optional foreign key
retirement\_reason	TEXT	Optional
The customer number is system-generated and is separate from the project identification code.

4.2 customer\_phone
Field	Type	Requirement
id	BIGINT	Primary key
customer\_id	BIGINT	Required foreign key
phone\_number	VARCHAR(50)	Required
normalized\_phone\_number	VARCHAR(30)	Required, indexed
phone\_type	VARCHAR(30)	Mobile, home, work, other
is\_primary	BOOLEAN	Required
is\_active	BOOLEAN	Required
Standard audit columns	—	Required
4.3 customer\_email
Field	Type	Requirement
id	BIGINT	Primary key
customer\_id	BIGINT	Required foreign key
email\_address	VARCHAR(320)	Required
normalized\_email\_address	VARCHAR(320)	Required, indexed
email\_type	VARCHAR(30)	Personal, work, other
is\_primary	BOOLEAN	Required
is\_active	BOOLEAN	Required
Standard audit columns	—	Required
4.4 customer\_contact
Field	Type	Requirement
id	BIGINT	Primary key
customer\_id	BIGINT	Required foreign key
contact\_name	VARCHAR(200)	Required
contact\_type	VARCHAR(50)	Required
phone\_number	VARCHAR(50)	Optional
email\_address	VARCHAR(320)	Optional
is\_primary	BOOLEAN	Required
preferred\_contact\_method	VARCHAR(30)	Optional
contact\_status	VARCHAR(30)	Required
Standard audit columns	—	Required
A customer may have multiple contacts.

Only one active primary contact may exist for a given customer and contact type unless an approved exception is recorded.

5. Project-Location Data Model
5.1 project\_location
Field	Type	Requirement
id	BIGINT	Primary key
street\_address	VARCHAR(250)	Required
address\_line\_2	VARCHAR(250)	Optional
city	VARCHAR(100)	Required
state\_code	CHAR(2)	Required
postal\_code	VARCHAR(20)	Required
country\_code	CHAR(2)	Required, default US
normalized\_street\_address	VARCHAR(250)	Required
normalized\_city	VARCHAR(100)	Required
normalized\_postal\_code	VARCHAR(20)	Required
normalized\_location\_key	VARCHAR(500)	Required, indexed
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
customer\_id	BIGINT	Required foreign key
project\_code	VARCHAR(80)	Required, unique
project\_location\_id	BIGINT	Required foreign key
assigned\_sales\_associate\_id	BIGINT	Required foreign key
current\_msr\_version\_id	BIGINT	Optional until Sprint 05
project\_status	VARCHAR(50)	Required
cancellation\_reason	TEXT	Required when cancelled
completion\_date	DATE	Optional
last\_activity\_at	TIMESTAMPTZ	Required
Standard audit columns	—	Required
6.2 Initial project statuses
Sprint 04 must support at least:

text


draft
active
duplicate\\\_review
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

6.3 project\_assignment
Field	Type	Requirement
id	BIGINT	Primary key
project\_id	BIGINT	Required foreign key
user\_id	BIGINT	Required foreign key
assignment\_type	VARCHAR(40)	Sales Associate, Technician, other
effective\_from	DATE	Required
effective\_to	DATE	Optional
assignment\_status	VARCHAR(30)	Required
assigned\_by	BIGINT	Required foreign key
assignment\_reason	TEXT	Optional
Standard audit columns	—	Required
A project may have multiple technicians.

A project must have one active assigned Sales Associate unless the project is in a draft or exception state.

6.4 project\_status\_history
Field	Type	Requirement
id	BIGINT	Primary key
project\_id	BIGINT	Required foreign key
previous\_status	VARCHAR(50)	Optional
new\_status	VARCHAR(50)	Required
changed\_at	TIMESTAMPTZ	Required
changed\_by	BIGINT	Required foreign key
change\_reason	TEXT	Required when applicable
audit\_event\_id	BIGINT	Optional foreign key
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

---

**# GECC 05 Master Sales Record**

# GECC-05-master-sales-record.md

# GECC Sales Back Office System — Sprint 05 Master Sales Record

## Bundle Metadata

- Bundle name: GECC-05-master-sales-record.md
- Project: GECC Sales Back Office System
- Company: Go Ecco Climate Control
- Company abbreviation: GECC
- Bundle type: Sprint 05 design and implementation bundle
- Status: Approved design; ready for implementation
- Created date: 2026-09-20
- Previous bundle: GECC-04-customers-and-projects.md
- Next planned bundle: GECC-06-workflow-state-machine.md
- Database: PostgreSQL
- Primary-key strategy: Numeric BIGINT identity keys
- Business timezone: America/New\_York
- System of record: PostgreSQL relational database
- Audit model: Append-only, hash-chained, permanently retained

---

## 1. Sprint Objective

Implement the Master Sales Record as the authoritative project record for:

- Customer information
- Project information
- Project location
- Sales Associate assignment
- Scope of work
- Equipment
- Serial numbers
- Retail price
- Payment option
- Project costs
- Contract data
- Invoice data
- Certificate data
- Approval status
- Document-version references
- Revision reason
- Revision history

The Master Sales Record must allow Sales Associates to enter project information once and provide the controlled source data for future invoices, contracts, certificates, commissions, payments, and financial reports.

---

## 2. Sprint 05 Deliverables

Sprint 05 must produce:

1. Master Sales Record form
2. Draft Master Sales Record behavior
3. Required-field validation
4. Customer and project consistency validation
5. Scope-of-work selections
6. Equipment records
7. Serial-number records
8. Pricing fields
9. Cost fields
10. Payment-option fields
11. Versioned Master Sales Records
12. Approval-ready records
13. Approval history
14. Revision reasons
15. Current and superseded version handling
16. Controlled access to pricing and costs
17. Master Sales Record search and display
18. Audit integration
19. Database acceptance tests
20. Sprint 06 workflow handoff

---

## 3. Requirements Carried Forward

The implementation must preserve these approved requirements:

- Each project has one Master Sales Record.
- The Master Sales Record is the authoritative source for project information.
- The Master Sales Record is linked to the customer and project.
- A project may include multiple pieces of equipment.
- A project may include multiple serial numbers.
- The Sales Associate creates the initial record.
- Unapproved information may be updated by authorized users.
- A Sales Manager or Comptroller reviews and approves the record.
- The authorized approver enters or confirms the retail price.
- One authorized approval is sufficient.
- Approved records are locked.
- Authorized reopening creates a new version.
- Original versions remain permanently preserved.
- Scope changes create a new Master Sales Record version.
- Revised documents use the revised version.
- Original documents and signatures remain preserved.
- The original project code remains unchanged through revisions.
- Documents must use the approved Master Sales Record.
- Unauthorized users may not edit approved retail price or approved costs.
- All material changes are auditable.

---

## 4. Master Sales Record Structure

### 4.1 `master\_sales\_record`

This table identifies the logical Master Sales Record for a project.

| Field | Type | Requirement |
|---|---|---|
| `id` | BIGINT | Primary key |
| `project\_id` | BIGINT | Required foreign key, unique |
| `current\_version\_id` | BIGINT | Required after initial version creation |
| `record\_status` | VARCHAR(30) | Draft, active, approved, superseded, cancelled |
| `created\_at` | TIMESTAMPTZ | Required |
| `created\_by` | BIGINT | Required foreign key |
| `updated\_at` | TIMESTAMPTZ | Required |
| `updated\_by` | BIGINT | Required foreign key |
| `is\_active` | BOOLEAN | Required |
| `retired\_at` | TIMESTAMPTZ | Optional |
| `retired\_by` | BIGINT | Optional foreign key |
| `retirement\_reason` | TEXT | Optional |

Constraint:

```text
UNIQUE(project\\\_id)
A project may have only one logical Master Sales Record.

4.2 master\_sales\_record\_version
Field	Type	Requirement
id	BIGINT	Primary key
master\_sales\_record\_id	BIGINT	Required foreign key
project\_id	BIGINT	Required foreign key
customer\_id	BIGINT	Required foreign key
version\_number	INTEGER	Required
version\_status	VARCHAR(30)	Draft, submitted, approved, rejected, superseded, cancelled
created\_at	TIMESTAMPTZ	Required
created\_by	BIGINT	Required foreign key
revision\_reason	TEXT	Required for version greater than 1
supersedes\_version\_id	BIGINT	Optional foreign key
assigned\_sales\_associate\_id	BIGINT	Required foreign key
retail\_price	NUMERIC(19,4)	Required before approval
payment\_option	VARCHAR(40)	Required
scope\_summary	TEXT	Required
approval\_status	VARCHAR(30)	Required
approved\_by	BIGINT	Optional foreign key
approved\_at	TIMESTAMPTZ	Optional
rejected\_by	BIGINT	Optional foreign key
rejected\_at	TIMESTAMPTZ	Optional
rejection\_reason	TEXT	Optional
source\_version\_id	BIGINT	Optional foreign key
document\_data\_version	INTEGER	Required
record\_hash	VARCHAR(128)	Optional integrity reference
Constraint:

text


UNIQUE(master\\\_sales\\\_record\\\_id, version\\\_number)
Only one version may be current for a logical Master Sales Record.

5. Master Sales Record Sections
The form must be divided into controlled sections.

5.1 Customer section
The form displays linked customer information:

Customer ID
Customer number
Customer name
Primary phone
Primary email
Authorized contacts
Customer information is retrieved from the Customer record.

The Master Sales Record must not create an independent conflicting copy of customer identity data.

5.2 Project section
The form displays linked project information:

Project ID
Project identification code
Project location
Assigned Sales Associate
Assigned Technician or Technicians
Current project status
Project creation date
The project identification code is read-only after creation.

5.3 Scope section
The form supports multiple scope selections:

Service
Repair
Install new
Remove old
Under Install New and Remove Old, the form supports multiple items:

Condenser coil
Evaporator coil
Refrigerant
Air handler
Furnace
Thermostat
Air ducts
Vents
A project may include more than one scope selection.

5.4 Equipment section
The form supports multiple equipment records.

Required fields may vary according to scope and equipment status:

Brand
Model
Function
Serial number
Equipment category
New, existing, removed, or installed status
Inventory reference
Notes
The form must support:

Add equipment
Edit unapproved equipment
Remove an unapproved equipment row
Record existing equipment
Record equipment to be removed
Record equipment to be installed
Update serial numbers where authorized
5.5 Pricing section
The pricing section includes:

Retail price
Payment option
Adjustment placeholder
Pricing notes where authorized
Retail price must be entered or confirmed by:

Sales Manager, or
Comptroller
The Sales Associate may enter proposed pricing where permitted but may not approve the final retail price.

5.6 Cost section
The cost section includes:

Equipment cost
Material cost
Dealer fee
Lead cost
Technician cost
Damon cost
Jerry cost
Initial default values:

text


Lead:  \\$3,000
Damon: \\$500
Jerry: \\$250
The default values apply to the project unless an authorized user creates a project-specific change.

A change to one project must not silently alter:

Past projects
Future projects
Other projects
Global defaults
5.7 Payment-option section
Initial supported payment methods:

Bank cashier’s check
Bank wire transfer
The payment option identifies the planned method and does not constitute payment receipt or verification.

5.8 Document-data section
The Master Sales Record stores or references the controlled data needed for:

Invoice
Contract
Certificate
Document generation is implemented in Sprint 07. Sprint 05 must provide stable version references for later document generation.

6. Scope and Equipment Tables
6.1 msr\_scope\_selection
Field	Type	Requirement
id	BIGINT	Primary key
msr\_version\_id	BIGINT	Required foreign key
scope\_code	VARCHAR(40)	Required
scope\_group	VARCHAR(40)	Required
is\_selected	BOOLEAN	Required
notes	TEXT	Optional
Standard audit columns	—	Required
6.2 msr\_equipment
Field	Type	Requirement
id	BIGINT	Primary key
msr\_version\_id	BIGINT	Required foreign key
project\_id	BIGINT	Required foreign key
equipment\_sequence	INTEGER	Required
brand	VARCHAR(150)	Required where applicable
model	VARCHAR(150)	Required where applicable
function	VARCHAR(100)	Required
serial\_number	VARCHAR(150)	Optional until required by workflow
equipment\_category	VARCHAR(80)	Required
equipment\_status	VARCHAR(40)	New, existing, removed, installed
inventory\_id	BIGINT	Optional foreign key
notes	TEXT	Optional
Standard audit columns	—	Required
Constraint:

text


UNIQUE(msr\\\_version\\\_id, equipment\\\_sequence)
6.3 Serial-number rules
Serial numbers must remain individually identifiable.
Duplicate serial numbers must be flagged.
A serial number may not be silently reassigned to a different project.
Serial-number changes after approval require authorized revision.
The original serial-number value remains available in history.
Missing serial numbers may be permitted in draft status only where the applicable scope does not yet require them.
7. Cost Tables
7.1 msr\_cost\_component
Field	Type	Requirement
id	BIGINT	Primary key
msr\_version\_id	BIGINT	Required foreign key
cost\_type	VARCHAR(40)	Required
amount	NUMERIC(19,4)	Required
default\_amount	NUMERIC(19,4)	Optional
is\_default\_applied	BOOLEAN	Required
is\_project\_override	BOOLEAN	Required
entered\_by	BIGINT	Required foreign key
entered\_at	TIMESTAMPTZ	Required
approval\_status	VARCHAR(30)	Required
approved\_by	BIGINT	Optional foreign key
approved\_at	TIMESTAMPTZ	Optional
notes	TEXT	Optional
7.2 Cost permissions
Equipment Cost, Material Cost, and Dealer Fee may be entered by:

Sales Manager
Installation Manager
Comptroller
Technician Cost may be entered by:

Sales Manager
Installation Manager
Comptroller
Lead Cost, Damon Cost, and Jerry Cost may be edited for an individual project by authorized users.

Sprint 05 must enforce field permissions but Sprint 06 will enforce the complete lifecycle timing.

8. Draft Behavior
8.1 Draft creation
A Sales Associate may create a draft Master Sales Record for an assigned project.

Draft creation must require:

Customer
Project
Project location
Assigned Sales Associate
Project code
Initial scope or stated scope-pending reason
8.2 Draft editing
Before approval, authorized users may edit permitted fields.

Draft edits must:

Update the draft version.
Preserve changed-field history.
Update updated\_at and updated\_by.
Generate an audit event.
Revalidate the complete record before submission.
8.3 Save as draft
The form must support:

Save draft
Resume draft
Cancel entry
Clear unsaved form
View validation errors
Submit for review
A draft is not an approved sale and cannot generate final controlled documents.

8.4 Draft deletion
Draft records must not be hard-deleted without an authorized retirement or cancellation action.

The system must preserve:

Creator
Creation time
Cancellation or retirement reason
Audit history
9. Submission and Approval
9.1 Submission for review
A Sales Associate may submit a complete draft for review.

Submission requires validation of:

Required customer information
Project consistency
Location consistency
Project-code consistency
Assigned Sales Associate
Scope
Equipment requirements
Pricing
Costs
Payment option
Duplicate data
Required explanations
Record version
9.2 Approval authority
The approved core lifecycle requires approval by:

Sales Manager, or
Comptroller
One authorized approval is sufficient.

The implementation must retain a flexible approval structure so the later workflow sprint can represent any approved applicable workflow distinction without weakening the core requirement.

9.3 Approval result
An approver may:

Approve
Reject
Return for correction
Approval must record:

Exact Master Sales Record version
Approver
Approver role
Date and time
Approval result
Approval notes
Related audit event
9.4 Approved record behavior
After approval:

The approved version becomes immutable.
Ordinary edits are rejected.
The current version reference is updated.
Later changes require authorized reopening or a new revision.
Documents may be generated from the approved version in later sprints.
The record cannot be silently overwritten.
10. Versioning and Revision
10.1 New version creation
A new version is required when:

An approved record is reopened.
A signed or approved project changes scope.
Approved pricing changes.
Approved costs change.
Contract data changes.
Invoice data changes.
Certificate data changes.
An authorized correction is required.
10.2 Revision requirements
Every revision must include:

New version number
Prior version reference
Revision reason
Revising user
Revision date and time
Changed fields
New status
Approval state
Related audit event
10.3 Version relationships
The system must preserve:

text


Version 1
  └── Version 2
        └── Version 3
Each version must remain independently retrievable.

10.4 Current version
Only one version may be marked current.

The current version must be:

Linked from master\_sales\_record.current\_version\_id
Linked from project.current\_msr\_version\_id
Clearly marked in user interfaces
Used by later document generation only when its status permits generation
10.5 Superseded versions
A superseded version:

Remains read-only
Remains searchable according to permissions
Remains linked to its documents and approvals
Cannot be edited
Cannot be used as the current source for new documents
Remains available for audit and historical reporting
11. Validation Rules
Before saving a draft:

Required draft fields must be present.
Foreign-key references must be valid.
The user must have permission to create or edit the record.
The project must exist.
The customer must match the project.
The project location must match the project.
The assigned Sales Associate must be valid.
Scope values must come from controlled values.
Equipment values must be valid.
Monetary values must be non-negative unless an approved accounting rule permits otherwise.
Payment option must be supported.
Duplicate serial numbers must be flagged.
Before submission for approval:

Required fields must be complete.
Customer consistency must pass.
Project consistency must pass.
Project-code consistency must pass.
Scope consistency must pass.
Equipment and serial-number requirements must pass.
Retail price must be present.
Costs must be present or explicitly marked not applicable.
Payment option must be present.
Required duplicate explanations must be present.
No prohibited draft errors may remain.
Before approval:

The approver must be authorized.
The record must still be the current submitted version.
The record must not already be approved.
The record must not be superseded.
The record must not be cancelled.
The approval must reference the exact version being approved.
12. Access Rules
Sales Associate
May:

Create Master Sales Records for assigned projects.
Edit unapproved records within assigned projects.
View generated draft information.
Submit records for review.
Review the generated contract in later sprints.
View commission information related only to their own sales.
May not:

Approve a Master Sales Record.
Edit an approved Master Sales Record.
Change approved retail price.
Change approved costs.
Approve commission or payroll reports.
View another Sales Associate's commissions.
Sales Manager
May:

View all Master Sales Records.
Create or edit authorized records.
Enter or confirm retail price.
Enter or revise permitted costs.
Approve Master Sales Records.
Reopen approved records.
Approve applicable corrections.
Comptroller
May:

View all Master Sales Records.
Create or edit authorized records.
Enter or confirm retail price.
Enter or revise permitted costs.
Approve Master Sales Records.
Reopen approved records.
Approve applicable corrections.
Installation Manager
May:

View installation-related Master Sales Records.
Enter or approve installation-related cost information according to the applicable authorization.
View approved scope and equipment information.
The core sales-approval lifecycle remains Sales Manager or Comptroller approval unless a later approved decision explicitly changes it.

Technician
May:

View approved scope of work for assigned projects.
View required equipment information.
Update authorized serial-number and completion information in later workflow stages.
Propose scope changes.
May not:

Change retail price.
Change approved costs.
Change contract terms.
Approve the Master Sales Record.
Accounts Payable Associate
May:

View Master Sales Record fields necessary for payable duties.
View project, vendor, cost-category, and payable references where authorized.
Database Administrator
May:

Maintain approved database functions.
Search authorized records.
Perform approved data-quality maintenance.
The Database Administrator does not automatically receive Master Sales Record approval authority.

13. Audit Events
Sprint 05 must create audit events for:

Master Sales Record draft created
Draft viewed
Draft edited
Draft saved
Draft resumed
Draft cancelled
Draft submitted for review
Validation failure
Master Sales Record approved
Master Sales Record rejected
Master Sales Record returned for correction
Master Sales Record reopened
New version created
Version superseded
Revision reason entered
Scope changed
Equipment added
Equipment changed
Serial number added
Serial number changed
Retail price entered
Retail price changed
Cost entered
Cost changed
Payment option changed
Unauthorized edit attempt
Unauthorized approval attempt
Restricted Master Sales Record access attempt
Each event must include:

User
Role
Session
Project
Customer
Master Sales Record
Version
Action
Outcome
Reason where applicable
Previous and new status where applicable
Audit-chain references
14. Sprint 05 Decisions
D-05-001 — Master Sales Record authority
The Master Sales Record is the authoritative source of project information for downstream documents, payments, commissions, and reports.

D-05-002 — One logical record per project
Each project has one logical Master Sales Record with one or more preserved versions.

D-05-003 — Immutable approved versions
Approved Master Sales Record versions are immutable.

D-05-004 — Revision behavior
Changes to approved records create new versions and preserve previous versions.

D-05-005 — Core approval authority
The core approval lifecycle requires approval by either the Sales Manager or Comptroller.

One authorized approval is sufficient.

D-05-006 — Approval version binding
Approvals must reference the exact Master Sales Record version being approved.

D-05-007 — Project-code preservation
Master Sales Record revisions do not create a new project code.

D-05-008 — Default-cost isolation
Changes to Lead, Damon, Jerry, or other project costs apply only to the indicated project version unless a separately approved configuration change is made.

D-05-009 — Document-generation dependency
Sprint 05 stores stable version references for documents, but document generation itself is implemented in Sprint 07.

D-05-010 — Calculation separation
Sprint 05 stores source pricing and cost inputs. Final profitability snapshots and accounting calculations remain subject to later financial-reporting design.

15. Sprint 05 Acceptance Criteria
Sprint 05 is accepted when:

A Sales Associate can create a Master Sales Record for an assigned project.
A Master Sales Record is linked to exactly one project.
A project has only one logical Master Sales Record.
The customer and project relationships are validated.
The project code is inherited from the project and cannot be changed through the Master Sales Record.
Scope selections support service, repair, install new, and remove old.
Install-new and remove-old sub-items are supported.
Multiple equipment records can be added.
Multiple serial numbers can be stored.
Duplicate serial numbers are detected.
Retail price is stored as a fixed-precision amount.
Payment option supports cashier's check and bank wire.
Required cost categories are supported.
Initial fixed default costs are represented.
Project-specific cost overrides do not change other projects.
Draft records can be saved and resumed.
Draft records can be submitted for review.
Required fields are validated before submission.
Inconsistent customer, project, location, and project-code values are rejected.
An authorized Sales Manager or Comptroller can approve a submitted version.
One authorized approval is sufficient.
An unauthorized user cannot approve the record.
An approved version cannot be silently edited.
An approved version can be superseded only through an authorized revision process.
Revision reason, user, date, and time are preserved.
Previous versions remain retrievable.
Only one current version exists.
Superseded versions cannot be used as the current source for new documents.
Access to pricing and cost fields is role-controlled.
Sales Associates cannot view another Sales Associate's commission data.
All material Master Sales Record actions create audit events.
Failed and unauthorized actions create audit events.
The data model is ready for Sprint 06 lifecycle and state-machine enforcement.
16. Sprint 05 Test Cases
Creation and draft tests
Create a valid Master Sales Record.
Reject creation for an unauthorized project.
Save an incomplete draft where permitted.
Resume a saved draft.
Reject invalid customer-project combinations.
Reject invalid project-location combinations.
Reject unsupported payment options.
Reject invalid monetary values.
Clear a draft form without saving unsaved changes.
Scope and equipment tests
Select service.
Select repair.
Select install new.
Select remove old.
Select multiple install-new sub-items.
Select multiple remove-old sub-items.
Add multiple equipment records.
Add multiple serial numbers.
Reject duplicate serial numbers.
Preserve equipment history after revision.
Pricing and cost tests
Enter proposed retail price.
Confirm retail price as Sales Manager.
Confirm retail price as Comptroller.
Reject Sales Associate approval.
Enter permitted equipment cost.
Enter permitted material cost.
Enter permitted dealer fee.
Apply initial fixed defaults.
Override a fixed default for one project.
Confirm the override does not change another project.
Approval tests
Submit a valid record for review.
Reject submission with missing required data.
Approve as Sales Manager.
Approve as Comptroller.
Reject approval by Sales Associate.
Reject approval by Technician.
Reject approval of a superseded version.
Reject duplicate approval of the same version.
Preserve approver and timestamp.
Versioning tests
Create Version 1.
Approve Version 1.
Reopen through an authorized action.
Create Version 2.
Preserve Version 1 unchanged.
Record revision reason.
Mark Version 1 superseded.
Mark Version 2 current.
Confirm the project code remains unchanged.
Confirm Version 1 remains searchable according to permission.
Security and audit tests
Unauthorized user cannot view restricted cost fields.
Unauthorized user cannot edit approved fields.
Unauthorized user cannot approve a record.
Unauthorized user cannot reopen a record.
Draft creation is audited.
Approval is audited.
Revision is audited.
Serial-number changes are audited.
Unauthorized attempts are audited.
Failed audit creation prevents the required business action from succeeding.
17. Sprint 05 Open Questions
These implementation questions do not change approved requirements:

What exact fields are mandatory for each scope and equipment category?
Which equipment categories require serial numbers before approval?
What exact initials format will be used for Sales Associates?
What cost fields may the Installation Manager edit before installation?
What fields may be entered by the Comptroller when acting as a substitute signer later?
What exact return-for-correction workflow will Sprint 06 implement?
What approval reason values should be standardized?
What calculation-rule version should be stored with pricing and cost inputs?
What project-specific cost override process requires Sales Manager approval?
What Master Sales Record fields must be copied into document-generation staging tables?
18. Sprint 06 Handoff
Sprint 06 will implement the controlled lifecycle and task workflow.

Sprint 06 must use the following Sprint 05 structures:

Master Sales Record
Master Sales Record version
Scope selections
Equipment
Serial numbers
Pricing
Cost components
Payment option
Approval status
Revision reason
Current-version reference
Approval history
Sprint 06 must enforce:

Required state transitions
Approval sequencing
Installation task creation
Three-day installation restriction
Scope-change process
Authorized reopening
New-version creation
Revised approval and signature requirements
Sprint 06 must not overwrite Sprint 05 versions or bypass Master Sales Record immutability.

19. Historical Record
Supersedes
None.

Preserves
GECC-00-project-baseline.md
GECC-01-data-model.md
GECC-02-auth-and-permissions.md
GECC-03-audit-log.md
GECC-04-customers-and-projects.md
Sprint 00 decisions
Sprint 01 decisions
Sprint 02 decisions
Sprint 03 decisions
Sprint 04 decisions
Approved project-code requirements
Approved lifecycle requirements
Approved versioning and audit requirements
Changes introduced by this bundle
Defines the Master Sales Record structure.
Defines draft, submission, approval, and revision behavior.
Defines scope and equipment data.
Defines pricing, payment-option, and cost inputs.
Defines Master Sales Record versioning.
Defines access restrictions for pricing and cost data.
Defines Sprint 05 audit events.
Defines Sprint 06 workflow handoff.

---

**# GECC 06 Workflow State Machine**

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
- Business timezone: America/New\_York
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
sales\\\_record\\\_pending\\\_review
sales\\\_record\\\_rejected
sales\\\_record\\\_approved
contract\\\_pending\\\_customer\\\_signature
contract\\\_pending\\\_gecc\\\_signature
contract\\\_signed
invoice\\\_pending\\\_gecc\\\_signature
invoice\\\_pending\\\_customer\\\_signature
invoice\\\_signed
installation\\\_waiting\\\_period
installation\\\_scheduling\\\_required
installation\\\_scheduled
installation\\\_in\\\_progress
installation\\\_completed
certificate\\\_pending\\\_customer\\\_signature
certificate\\\_pending\\\_gecc\\\_signature
certificate\\\_signed
payment\\\_pending
payment\\\_verification\\\_pending
adjustment\\\_pending\\\_approval
project\\\_ready\\\_for\\\_completion
completed
scope\\\_change\\\_pending
scope\\\_change\\\_approved
scope\\\_change\\\_rejected
revision\\\_pending\\\_approval
cancelled
exception\\\_review
Some document-specific states may be stored on document records while the project maintains the overall business state.

4.2 State descriptions
State	Meaning
draft	Project or Master Sales Record is being prepared
sales\_record\_pending\_review	Record is submitted for authorized review
sales\_record\_rejected	Record was rejected or returned for correction
sales\_record\_approved	Master Sales Record is approved and locked
contract\_pending\_customer\_signature	Contract is awaiting customer signature
contract\_pending\_gecc\_signature	Contract is awaiting GECC signature
contract\_signed	Contract has all required signatures
invoice\_pending\_gecc\_signature	Invoice is awaiting GECC signature
invoice\_pending\_customer\_signature	Invoice is awaiting customer signature
invoice\_signed	Invoice has all required signatures
installation\_waiting\_period	Required three-day waiting period is active
installation\_scheduling\_required	Installation Manager scheduling task is open
installation\_scheduled	Installation date has been assigned
installation\_in\_progress	Work has started
installation\_completed	Technician has recorded completion
certificate\_pending\_customer\_signature	Certificate is awaiting customer signature
certificate\_pending\_gecc\_signature	Certificate is awaiting GECC signature
certificate\_signed	Certificate has all required signatures
payment\_pending	Payment has not yet been recorded
payment\_verification\_pending	Payment is recorded but not verified
adjustment\_pending\_approval	Credit, refund, or adjustment requires approval
project\_ready\_for\_completion	All completion prerequisites are satisfied
completed	Project is complete and eligible for commissions
scope\_change\_pending	Technician has proposed a scope change
scope\_change\_approved	Scope change has been approved and revision work may proceed
scope\_change\_rejected	Scope change was rejected
revision\_pending\_approval	Revised Master Sales Record awaits approval
cancelled	Project is cancelled and cannot reuse its project code
exception\_review	Authorized exception review is active
5. Allowed State Transitions
5.1 Initial sales process
text


draft
  → sales\\\_record\\\_pending\\\_review

sales\\\_record\\\_pending\\\_review
  → sales\\\_record\\\_approved
  → sales\\\_record\\\_rejected

sales\\\_record\\\_rejected
  → draft
  → cancelled

sales\\\_record\\\_approved
  → contract\\\_pending\\\_customer\\\_signature
5.2 Contract process
text


contract\\\_pending\\\_customer\\\_signature
  → contract\\\_pending\\\_gecc\\\_signature
  → contract\\\_pending\\\_customer\\\_signature
  → cancelled

contract\\\_pending\\\_gecc\\\_signature
  → contract\\\_signed
  → contract\\\_pending\\\_customer\\\_signature
  → cancelled
The customer must sign the contract before the GECC signer signs it.

5.3 Invoice process
text


contract\\\_signed
  → invoice\\\_pending\\\_gecc\\\_signature

invoice\\\_pending\\\_gecc\\\_signature
  → invoice\\\_pending\\\_customer\\\_signature
  → cancelled

invoice\\\_pending\\\_customer\\\_signature
  → invoice\\\_signed
  → invoice\\\_pending\\\_gecc\\\_signature
  → cancelled
The GECC signer must sign the invoice before the customer signs it.

5.4 Installation process
text


invoice\\\_signed
  → installation\\\_waiting\\\_period

installation\\\_waiting\\\_period
  → installation\\\_scheduling\\\_required
  → exception\\\_review

installation\\\_scheduling\\\_required
  → installation\\\_scheduled

installation\\\_scheduled
  → installation\\\_in\\\_progress
  → installation\\\_scheduling\\\_required

installation\\\_in\\\_progress
  → installation\\\_completed
  → scope\\\_change\\\_pending
5.5 Certificate process
text


installation\\\_completed
  → certificate\\\_pending\\\_customer\\\_signature

certificate\\\_pending\\\_customer\\\_signature
  → certificate\\\_pending\\\_gecc\\\_signature

certificate\\\_pending\\\_gecc\\\_signature
  → certificate\\\_signed
The customer must sign the certificate before the authorized GECC signer signs it.

5.6 Payment and completion process
text


certificate\\\_signed
  → payment\\\_pending
  → payment\\\_verification\\\_pending

payment\\\_pending
  → payment\\\_verification\\\_pending

payment\\\_verification\\\_pending
  → adjustment\\\_pending\\\_approval
  → project\\\_ready\\\_for\\\_completion

adjustment\\\_pending\\\_approval
  → payment\\\_verification\\\_pending
  → project\\\_ready\\\_for\\\_completion
A project may enter project\_ready\_for\_completion only when all completion conditions are satisfied.

text


project\\\_ready\\\_for\\\_completion
  → completed
5.7 Scope-change process
text


installation\\\_in\\\_progress
  → scope\\\_change\\\_pending

scope\\\_change\\\_pending
  → scope\\\_change\\\_approved
  → scope\\\_change\\\_rejected

scope\\\_change\\\_rejected
  → installation\\\_in\\\_progress

scope\\\_change\\\_approved
  → revision\\\_pending\\\_approval

revision\\\_pending\\\_approval
  → sales\\\_record\\\_approved
  → sales\\\_record\\\_rejected
After the revised Master Sales Record is approved, revised documents must enter a new required signature sequence.

5.8 Reopening process
An approved or signed project may be reopened only by an authorized user.

text


approved\\\_or\\\_signed\\\_state
  → exception\\\_review
  → revision\\\_pending\\\_approval
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


installation\\\_eligible\\\_at =
later(contract\\\_customer\\\_signed\\\_at, invoice\\\_customer\\\_signed\\\_at)
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


America/New\\\_York
The system must preserve the underlying timestamp and display the business-date interpretation in Eastern time.

9.4 Exception process
The initial workflow blocks early installation.

If an authorized exception process is implemented later:

The project enters exception\_review.
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
The project enters scope\_change\_pending.
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
workflow\_task
Field	Type	Requirement
id	BIGINT	Primary key
project\_id	BIGINT	Required foreign key
msr\_version\_id	BIGINT	Optional foreign key
task\_type	VARCHAR(80)	Required
task\_status	VARCHAR(30)	Open, assigned, completed, cancelled, blocked, expired
assigned\_user\_id	BIGINT	Optional foreign key
assigned\_role\_code	VARCHAR(60)	Optional
created\_at	TIMESTAMPTZ	Required
created\_by	BIGINT	Required foreign key
assigned\_at	TIMESTAMPTZ	Optional
due\_at	TIMESTAMPTZ	Optional
completed\_at	TIMESTAMPTZ	Optional
completed\_by	BIGINT	Optional foreign key
blocked\_reason	TEXT	Optional
completion\_notes	TEXT	Optional
priority	VARCHAR(20)	Normal, high, urgent
related\_document\_id	BIGINT	Optional foreign key
related\_version\_id	BIGINT	Optional foreign key
audit\_event\_id	BIGINT	Optional foreign key
Initial task types
text


sales\\\_record\\\_review
sales\\\_record\\\_correction
contract\\\_customer\\\_signature
contract\\\_gecc\\\_signature
invoice\\\_gecc\\\_signature
invoice\\\_customer\\\_signature
installation\\\_scheduling
installation\\\_assignment
installation\\\_completion
scope\\\_change\\\_review
scope\\\_change\\\_revision
certificate\\\_customer\\\_signature
certificate\\\_gecc\\\_signature
payment\\\_entry
payment\\\_verification
adjustment\\\_approval
project\\\_completion\\\_review
exception\\\_review
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
Waiting-period calculations use America/New\_York.

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

