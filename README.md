# Bank Complaint Management Portal

## 1. Overview

The **Bank Complaint Management Portal** is an internal complaint management system designed to receive, track, assign, monitor, and resolve customer complaints submitted through the Bank's official website.

Customers will lodge complaints through the Bank's website. The complaint data will be captured centrally and made available to authorized Bank officials through the Complaint Management Portal for further processing and resolution.

The system will provide an end-to-end complaint lifecycle, starting from **complaint registration** and continuing through **assignment, investigation, resolution, customer communication, escalation, and closure**.

---

## 2. Objectives

The primary objectives of the portal are:

* Provide customers with a convenient channel to lodge complaints.
* Generate a unique complaint/reference number for every complaint.
* Centralize all customer complaints at one location.
* Automatically route complaints to the concerned department/office/branch.
* Enable authorized officials to view and process complaints.
* Track the status and movement of each complaint.
* Monitor pending and overdue complaints.
* Maintain complete complaint history and audit trails.
* Facilitate escalation of unresolved complaints.
* Provide management with MIS and complaint-monitoring dashboards.
* Improve transparency and accountability in complaint resolution.
* Maintain records required for compliance, audit, and regulatory purposes.

---

# 3. Complaint Lifecycle

The proposed complaint lifecycle is:

```text
Customer
   │
   ▼
Complaint Lodged
   │
   ▼
Unique Complaint Number Generated
   │
   ▼
Complaint Received in Portal
   │
   ▼
Categorisation / Validation
   │
   ▼
Assignment to Concerned Office / Department
   │
   ▼
Complaint Investigation
   │
   ▼
Action / Resolution
   │
   ▼
Response to Customer
   │
   ▼
Customer Feedback
   │
   ▼
Closure
```

If the complaint is not resolved within the prescribed timeline:

```text
Pending Complaint
       │
       ▼
Reminder / Escalation
       │
       ▼
Higher Authority / Nodal Officer
       │
       ▼
Further Action
```

---

# 4. Major Components

The system will consist of the following major components:

### 4.1 Customer Complaint Module

This module will be integrated with the Bank's website and will allow customers to lodge complaints.

Customers may be required to provide:

* Customer Name
* Mobile Number
* Email ID
* Customer ID / Account Number, wherever applicable
* Branch
* Complaint Category
* Complaint Sub-category
* Transaction Details, wherever applicable
* Transaction Date
* Transaction Amount
* Complaint Description
* Supporting Documents
* Preferred Communication Channel

After successful submission, the system will generate a **unique Complaint Reference Number**.

---

### 4.2 Complaint Tracking

Customers should be able to track their complaint using:

* Complaint Reference Number
* Registered Mobile Number / Email
* OTP-based verification

The customer should be able to view:

* Complaint Status
* Complaint Registration Date
* Assigned Office / Department, where appropriate
* Last Action Taken
* Response / Resolution
* Closure Date

Sensitive internal information should not be exposed to customers.

---

### 4.3 Complaint Management Dashboard

Authorized Bank officials will have access to a dashboard showing complaints based on their role and access level.

Dashboard indicators may include:

* Total Complaints
* New Complaints
* Assigned Complaints
* Pending Complaints
* Complaints Under Process
* Resolved Complaints
* Closed Complaints
* Reopened Complaints
* Escalated Complaints
* Overdue Complaints
* Complaints Due Today
* Complaints Due in Next Few Days

Example:

```text
┌────────────────────────────────────────────────────┐
│              COMPLAINT MANAGEMENT                  │
├────────────┬────────────┬────────────┬──────────────┤
│   TOTAL    │    NEW     │  PENDING   │   OVERDUE    │
│    1,245   │     86     │    312     │      27      │
├────────────┼────────────┼────────────┼──────────────┤
│  RESOLVED  │   CLOSED   │ ESCALATED  │   REOPENED   │
│     720    │     650    │     41     │       9      │
└────────────┴────────────┴────────────┴──────────────┘
```

---

# 5. User Roles

The portal should implement **Role-Based Access Control (RBAC)**.

### Suggested Roles

| Role                 | Access                                             |
| -------------------- | -------------------------------------------------- |
| Super Admin          | Complete system administration                     |
| HO Admin             | Manage complaints at Head Office level             |
| HO Department User   | View and process complaints assigned to department |
| Regional Office User | Manage complaints assigned to RO                   |
| Branch User          | Manage complaints assigned to branch               |
| Nodal Officer        | Monitor/escalate complaints                        |
| Management           | Dashboard, MIS and monitoring                      |
| Auditor              | Read-only access and audit records                 |

Users should only be able to access complaints permitted by their role and organizational hierarchy.

Example:

```text
Super Admin
     │
     └── Head Office
           │
           ├── Department
           │
           └── Regional Office
                  │
                  └── Branch
```

---

# 6. Complaint Categories

Complaint categories should be configurable from the Admin Panel.

Suggested categories:

### Digital Banking

* UPI
* IMPS
* AePS
* NFS / ATM
* Debit Card
* POS
* E-Commerce
* Internet Banking
* Mobile Banking
* QR / Merchant
* BBPS

### Banking Services

* Deposit Accounts
* Loans / Advances
* Cheque / CTS
* NEFT
* RTGS
* NACH
* Cash Transactions
* Passbook
* Account Services
* Customer Service

### Other

* Pension
* Government Schemes
* Staff Behaviour
* Branch Services
* Charges / Fees
* Fraud / Suspected Fraud
* Other

Categories and sub-categories should be configurable without requiring changes to the application code.

---

# 7. Complaint Status

The following statuses may be maintained:

```text
NEW
 │
 ▼
RECEIVED
 │
 ▼
ASSIGNED
 │
 ▼
UNDER_PROCESS
 │
 ├──────────────► ESCALATED
 │
 ▼
RESOLVED
 │
 ▼
CUSTOMER_RESPONSE
 │
 ▼
CLOSED
```

Additional statuses:

* Reopened
* Rejected
* Duplicate
* Withdrawn
* Transferred

The final list should be configurable according to the Bank's complaint-handling policy.

---

# 8. Complaint Details

Each complaint should have a detailed complaint screen.

Example:

```text
------------------------------------------------------
Complaint No.: HGB-2026-00001245
------------------------------------------------------

Customer Details
Name              : XXXXX XXXXX
Mobile            : XXXXXXXX90
Email             : customer@example.com
Customer ID       : XXXXXXXX

Complaint Details
Category          : UPI
Sub Category      : Failed Transaction
Transaction Date  : 15-09-2026
Transaction ID    : XXXXXXXXXXXXX
Amount            : ₹5,000
Description       : Payment deducted but beneficiary
                    has not received the amount.

Assignment
Region            : RO Rohtak
Branch            : ABC Branch
Department        : Digital Banking Division
Assigned To       : User Name

Status            : UNDER PROCESS
Priority          : HIGH
Due Date          : 18-09-2026

------------------------------------------------------
Timeline
------------------------------------------------------
15-09-2026  Complaint Registered
15-09-2026  Assigned to RO
16-09-2026  Taken up by Branch
16-09-2026  Response Awaited
------------------------------------------------------
```

---

# 9. Complaint Assignment

Complaints should be assignable based on:

* Complaint category
* Sub-category
* Branch
* Region
* Department
* Product
* Complaint type

Assignment may be:

### Manual

An authorized user assigns the complaint to a particular office/user.

### Automatic

The system automatically assigns the complaint based on predefined rules.

Example:

```text
UPI Complaint
      │
      ▼
Digital Banking Division
      │
      ▼
Concerned RO / Branch
      │
      ▼
Assigned Officer
```

---

# 10. SLA and TAT Monitoring

Each complaint should have a defined **Turnaround Time (TAT)**.

The system should automatically calculate:

* Complaint Age
* Due Date
* Remaining Time
* SLA Status
* Overdue Days

Example:

```text
Complaint Received
       │
       ▼
SLA Clock Starts
       │
       ├── Within TAT ──► Normal
       │
       ├── Near Due Date ► Warning
       │
       └── TAT Exceeded ► Overdue / Escalation
```

The system should generate alerts for:

* New complaints
* Pending complaints
* Complaints approaching SLA
* Overdue complaints
* Escalated complaints
* Complaints awaiting response

---

# 11. Escalation Management

An escalation mechanism should be incorporated into the portal.

Example:

```text
Level 1
Branch / Concerned Office
       │
       ▼
Level 2
Regional Office
       │
       ▼
Level 3
Head Office Department
       │
       ▼
Level 4
Nodal Officer / Higher Authority
```

Escalation rules should be configurable based on:

* Complaint category
* Severity
* TAT
* Amount
* Customer type
* Regulatory requirement

---

# 12. Priority

Complaints may be assigned priority levels:

* Low
* Medium
* High
* Critical

Priority may be determined manually or automatically.

For example:

```text
Fraud / Suspected Fraud        → Critical
Financial Loss                 → High
Digital Transaction Issue     → High
General Service Complaint     → Medium
Information Request           → Low
```

The exact classification should be configurable as per Bank policy.

---

# 13. Communication

The portal should support automated communication through:

* SMS
* Email

Notifications may be generated for:

1. Complaint registration
2. Complaint assignment
3. Status change
4. Additional information required
5. Resolution
6. Closure
7. Escalation
8. Reopening

Example registration message:

```text
Your complaint has been successfully registered.

Complaint Reference No.: HGB-2026-00001245

Please use the above reference number to track
the status of your complaint.
```

---

# 14. Attachments

Customers and Bank officials should be able to upload supporting documents.

Supported file types may include:

* PDF
* JPG / JPEG
* PNG
* XLS / XLSX

The system should implement:

* File size restrictions
* File type validation
* Malware/security scanning
* Secure storage
* Access control
* Download authorization
* Audit logging

Sensitive documents should not be publicly accessible.

---

# 15. Complaint Timeline / Audit Trail

Every action performed on a complaint should be recorded.

Example:

```text
15-09-2026 10:15 AM
Complaint Registered
Source: Bank Website

15-09-2026 10:18 AM
Complaint Assigned
To: RO Rohtak

15-09-2026 02:30 PM
Complaint Viewed
User: Officer123

16-09-2026 11:20 AM
Remark Added
"Transaction details verified."

17-09-2026 03:45 PM
Complaint Resolved

17-09-2026 04:00 PM
Response Sent to Customer
```

Audit records should preferably be immutable for normal users.

---

# 16. Search and Filters

The portal should provide comprehensive search functionality.

Search by:

* Complaint Number
* Customer Name
* Mobile Number
* Account Number
* Customer ID
* Transaction ID / RRN
* Branch
* Region
* Category
* Sub-category
* Status
* Priority
* Assigned User
* Date Range

Filters should include:

```text
Date From
Date To
Category
Status
Priority
Region
Branch
Department
Assigned User
SLA Status
```

---

# 17. MIS and Reports

The system should provide downloadable MIS reports.

### Daily MIS

* Complaints received
* Complaints resolved
* Complaints pending
* Overdue complaints
* Escalated complaints

### Monthly MIS

* Category-wise complaints
* Branch-wise complaints
* Region-wise complaints
* Department-wise complaints
* Product-wise complaints
* TAT analysis
* Resolution analysis
* Reopened complaints

Reports should be exportable in:

* Excel
* CSV
* PDF

---

# 18. Management Dashboard

Management should have a consolidated view of complaint performance.

Example:

```text
                    COMPLAINT MIS

Total Complaints             1,245
Resolved                     720
Pending                      312
Overdue                       27
Escalated                     41

------------------------------------------------

Category-wise

UPI                          245
ATM / NFS                    180
IMPS                         110
AePS                          85
BBPS                          62
Deposits                     150
Loans                        205
Other                        208

------------------------------------------------

Region-wise

RO Rohtak                    180
RO Gurugram                  215
RO Hisar                     165
RO Karnal                    190
...
```

Charts can be provided for:

* Daily complaint trend
* Category distribution
* Region-wise complaints
* TAT performance
* Pending ageing
* Resolution trend

---

# 19. Complaint Ageing

The system should provide ageing analysis.

Example:

| Age        | Complaints |
| ---------- | ---------: |
| 0–2 Days   |        125 |
| 3–5 Days   |         86 |
| 6–10 Days  |         54 |
| 11–20 Days |         29 |
| >20 Days   |         18 |

This will help management identify long-pending complaints.

---

# 20. Security Requirements

Since the application will contain sensitive customer and transaction information, security should be a major consideration.

The system should implement:

* Role-Based Access Control
* Strong password policy
* Multi-Factor Authentication for internal users
* Session timeout
* Secure password hashing
* HTTPS/TLS
* Input validation
* SQL injection protection
* XSS protection
* CSRF protection
* Rate limiting
* Secure file upload
* Audit logs
* Access logs
* Data encryption where required
* Database backup
* Disaster recovery mechanism

Customer-facing APIs should not expose internal database IDs or sensitive information.

---

# 21. Privacy and Data Protection

The system should follow the Bank's applicable security, privacy, record-retention and regulatory requirements.

Important considerations:

* Customer information should only be accessible to authorized users.
* Account numbers and other sensitive information should be masked wherever possible.
* Complaint attachments should not be publicly accessible.
* API responses should contain only required information.
* Sensitive information should not be written unnecessarily into application logs.
* User activity should be auditable.
* Data retention should follow the Bank's approved policy.

---

# 22. Suggested Technology Stack

For an internal Bank application, the following architecture can be considered:

### Frontend

```text
Next.js
React
TypeScript
```

### Backend

```text
NestJS
Node.js
TypeScript
```

### Database

```text
PostgreSQL
```

### Cache / Queue

```text
Redis
```

### Deployment

```text
Docker
Linux Server
Nginx
```

### Authentication

```text
JWT / Session-based Authentication
RBAC
MFA / OTP where required
```

---

# 23. High-Level Architecture

```text
                    CUSTOMER
                       │
                       ▼
              ┌─────────────────┐
              │  Bank Website   │
              │ Complaint Form  │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │   API Gateway   │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │ Complaint API   │
              │    NestJS       │
              └────────┬────────┘
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
     PostgreSQL      Redis       File Storage
          │
          ▼
 ┌───────────────────────┐
 │ Complaint Management  │
 │       Portal          │
 │      Next.js          │
 └───────────┬───────────┘
             │
             ▼
     Bank Officials
```

---

# 24. Suggested Database Design

### complaints

```text
id
complaint_number
customer_id
customer_name
mobile_number
email
account_number
branch_id
category_id
subcategory_id
description
transaction_id
transaction_date
transaction_amount
priority
status
assigned_to
assigned_department
assigned_branch
sla_due_date
resolved_at
closed_at
created_at
updated_at
```

### complaint_categories

```text
id
name
description
is_active
created_at
updated_at
```

### complaint_status_history

```text
id
complaint_id
old_status
new_status
remarks
changed_by
changed_at
```

### complaint_assignments

```text
id
complaint_id
assigned_from
assigned_to
assigned_department
remarks
assigned_at
```

### complaint_attachments

```text
id
complaint_id
file_name
file_path
file_type
file_size
uploaded_by
uploaded_at
```

### complaint_remarks

```text
id
complaint_id
remark
visibility
created_by
created_at
```

### users

```text
id
employee_id
name
email
mobile
password_hash
role_id
branch_id
region_id
department_id
is_active
created_at
updated_at
```

### audit_logs

```text
id
user_id
action
module
record_id
ip_address
user_agent
created_at
```

---

# 25. API Structure

Suggested API structure:

```text
/api/v1/auth
/api/v1/complaints
/api/v1/complaints/:id
/api/v1/complaints/:id/status
/api/v1/complaints/:id/assign
/api/v1/complaints/:id/remarks
/api/v1/complaints/:id/attachments
/api/v1/complaints/:id/history
/api/v1/categories
/api/v1/branches
/api/v1/regions
/api/v1/users
/api/v1/reports
/api/v1/dashboard
```

---

# 26. Customer Complaint API

Example:

```http
POST /api/v1/complaints
```

Request:

```json
{
  "customerName": "Customer Name",
  "mobile": "XXXXXXXXXX",
  "email": "customer@example.com",
  "branchId": "BR001",
  "categoryId": "UPI",
  "subcategoryId": "FAILED_TRANSACTION",
  "transactionId": "XXXXXXXXXXXX",
  "transactionDate": "2026-09-15",
  "amount": 5000,
  "description": "Amount debited but transaction failed."
}
```

Response:

```json
{
  "success": true,
  "complaintNumber": "HGB-2026-00001245",
  "message": "Complaint registered successfully."
}
```

---

# 27. Project Structure

Suggested backend structure:

```text
complaint-management-backend/
│
├── src/
│   ├── auth/
│   ├── users/
│   ├── roles/
│   ├── complaints/
│   ├── categories/
│   ├── assignments/
│   ├── attachments/
│   ├── notifications/
│   ├── escalation/
│   ├── reports/
│   ├── dashboard/
│   ├── audit/
│   ├── database/
│   ├── common/
│   └── main.ts
│
├── test/
├── docker/
├── .env.example
├── docker-compose.yml
├── package.json
└── README.md
```

Frontend:

```text
complaint-management-frontend/
│
├── app/
│   ├── login/
│   ├── dashboard/
│   ├── complaints/
│   ├── reports/
│   ├── users/
│   ├── categories/
│   └── settings/
│
├── components/
├── services/
├── hooks/
├── lib/
├── types/
├── public/
├── package.json
└── README.md
```

---

# 28. Environment Variables

Example `.env`:

```env
NODE_ENV=production

PORT=3000

DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_NAME=complaint_management
DATABASE_USER=complaint_user
DATABASE_PASSWORD=********

REDIS_HOST=localhost
REDIS_PORT=6379

JWT_SECRET=********

FILE_STORAGE_PATH=/data/complaints

SMS_API_URL=
SMS_API_KEY=

SMTP_HOST=
SMTP_PORT=
SMTP_USER=
SMTP_PASSWORD=
```

Actual credentials must never be committed to Git.

---

# 29. Docker

The application should preferably be containerized.

Example services:

```text
Frontend
Backend
PostgreSQL
Redis
Nginx
```

Example:

```text
docker-compose
│
├── frontend
├── backend
├── postgres
├── redis
└── nginx
```

---

# 30. Logging

The application should maintain structured application logs.

Logs should capture:

* Request ID
* User ID
* API endpoint
* HTTP method
* Response status
* Execution time
* Error details

However, sensitive customer information such as:

* Full account number
* OTP
* Password
* Card number
* CVV
* Authentication credentials

should not be logged.

---

# 31. Backup and Recovery

The system should have:

* Regular PostgreSQL database backups
* Backup verification
* Attachment backup
* Backup retention policy
* Disaster Recovery procedure
* Restore testing

The backup strategy should be aligned with the Bank's approved IT/DR policy.

---

# 32. Notifications

A notification engine should be designed as a separate module.

Example:

```text
Complaint Event
      │
      ▼
Notification Service
      │
      ├── SMS
      │
      ├── Email
      │
      └── Portal Notification
```

This will allow additional notification channels to be added later without modifying the complaint module.

---

# 33. Future Enhancements

Possible future enhancements include:

* AI-assisted complaint categorisation
* Automatic complaint routing
* Duplicate complaint detection
* OCR for uploaded documents
* Customer feedback/rating
* Advanced SLA analytics
* Regulatory complaint reporting
* Integration with CBS
* Integration with Digital Banking systems
* Integration with CRM
* Integration with SMS Gateway
* Integration with Email Gateway
* API-based integration with Bank website
* Mobile application
* Knowledge base / FAQ
* Automated response suggestions

---

# 34. Development Roadmap

### Phase 1 — Core Complaint Management

* User authentication
* RBAC
* Complaint registration
* Complaint number generation
* Complaint listing
* Complaint details
* Assignment
* Status management
* Remarks
* Basic dashboard

### Phase 2 — Monitoring

* SLA/TAT
* Escalation
* Notifications
* Complaint ageing
* Advanced search
* MIS reports
* Excel/CSV export

### Phase 3 — Integration

* Bank website integration
* SMS gateway
* Email gateway
* Transaction verification
* CBS / Digital Banking integrations

### Phase 4 — Advanced Features

* Automated routing
* AI categorisation
* Duplicate detection
* Advanced analytics
* Management dashboards

---

# 35. Key Design Principles

The application should follow these principles:

1. **Security First**
   Customer and transaction information must be protected.

2. **Role-Based Access**
   Users should only see complaints relevant to their responsibilities.

3. **Complete Auditability**
   Every important action should be recorded.

4. **Configurable Workflow**
   Categories, SLA, escalation and assignment rules should be configurable.

5. **Scalability**
   The architecture should support increasing complaint volumes and users.

6. **Maintainability**
   Backend modules should remain independent and loosely coupled.

7. **API-First Architecture**
   Customer website and internal portal should communicate through secure APIs.

8. **No Direct Database Access from Frontend**
   All application operations should pass through authorized backend APIs.

---

# 36. Success Criteria

The portal will be considered operationally successful when it provides:

* Centralized complaint repository
* Unique complaint identification
* Real-time complaint tracking
* Controlled assignment and reassignment
* SLA/TAT monitoring
* Automated escalation
* Complete complaint history
* Management MIS
* Audit trail
* Secure document management
* Customer status tracking
* Role-based access
* Reliable backup and recovery

---

# 37. Conclusion

The **Bank Complaint Management Portal** will provide a centralized and structured platform for managing customer complaints from registration to final resolution.

The proposed architecture separates the **customer-facing complaint submission system** from the **internal complaint management portal**, while using a common secure backend. This approach will allow the Bank to integrate the portal with its existing website and gradually introduce integrations with CBS, digital banking platforms, notification systems and other internal applications.

The system should be developed with particular emphasis on **security, auditability, SLA monitoring, role-based access, data confidentiality and management visibility**.
