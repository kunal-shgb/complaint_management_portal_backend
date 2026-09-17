# Complaint Management Portal

## 1. Overview

The **Complaint Management Portal** is an internal complaint management system designed to receive, track, assign, monitor, escalate, and resolve customer complaints submitted through the Bank's official website.

Customers will lodge complaints through the Bank's website. The complaint data will be captured centrally and made available to authorized Bank officials through the Complaint Management Portal for further processing and resolution.

The system will provide an end-to-end complaint lifecycle, starting from **complaint registration** and continuing through **assignment, investigation, resolution, customer communication, escalation, and closure**.

The Complaint Management Portal will **not maintain its own authentication or authorization mechanism**. Authentication and authorization of Bank employees/users will be handled through the Bank's existing **User Management / Identity and Access Management (IAM) system**, exposed through secure APIs.

---

# 2. Objectives

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
* Integrate with the Bank's existing User Management / IAM system for employee authentication and authorization.

---

# 3. Technology Stack

## Backend

```text
.NET 10 (LTS)
C#
ASP.NET Core Web API
Entity Framework Core
```

## Frontend

```text
Next.js
React
TypeScript
```

## Database

```text
PostgreSQL
```

## Cache / Background Processing

```text
Redis
.NET Background Services
```

## Containerization

```text
Docker
Docker Compose
```

## Reverse Proxy

```text
Nginx
```

## Authentication & Authorization

```text
Bank's Existing User Management / IAM System
```

> The Complaint Management Portal will not maintain its own employee authentication system or password database.

---

# 4. High-Level Architecture

```text
                         CUSTOMER
                            │
                            ▼
                   ┌─────────────────┐
                   │  Bank Website   │
                   │ Complaint Form  │
                   └────────┬────────┘
                            │
                         HTTPS
                            │
                            ▼
                   ┌─────────────────┐
                   │   ASP.NET Core  │
                   │    Web API      │
                   │    .NET 10      │
                   └────────┬────────┘
                            │
             ┌──────────────┼──────────────┐
             ▼              ▼              ▼
        PostgreSQL        Redis       File Storage
             │
             │
             ▼
    ┌────────────────────────┐
    │ Complaint Management   │
    │        Portal          │
    │       Next.js          │
    └────────────┬───────────┘
                 │
                 │ Authentication /
                 │ User Information
                 ▼
    ┌────────────────────────┐
    │ Bank User Management / │
    │          IAM           │
    └────────────────────────┘
```

---

# 5. Authentication and Authorization

## 5.1 External User Management

The Complaint Management Portal will **not create or maintain employee user accounts**.

The following functions will remain with the Bank's existing User Management / IAM system:

* User authentication
* Username/password validation
* MFA/OTP, if applicable
* User identity
* User status
* Enterprise roles
* Department information
* Region/office information
* Branch information
* Authorization-related attributes

The Complaint Portal will consume the required information through secure APIs/token-based integration.

---

# 6. Authentication Flow

A typical flow will be:

```text
Bank Official
      │
      ▼
Complaint Portal
      │
      ▼
Bank User Management / IAM
      │
      ▼
User Authentication
      │
      ▼
Authentication Token
      │
      ▼
ASP.NET Core API
      │
      ▼
Token Validation
      │
      ▼
Authorization
      │
      ▼
Complaint Data
```

The exact authentication protocol will depend upon the Bank's existing User Management API.

Possible mechanisms include:

* OAuth 2.0
* OpenID Connect
* SAML
* JWT
* Bank-specific authentication API

The implementation should follow the Bank's approved IAM integration specification.

---

# 7. ASP.NET Core Authorization

The backend will use **ASP.NET Core's built-in authentication and authorization middleware** to protect APIs.

Conceptually:

```text
Request
   │
   ▼
Authentication Middleware
   │
   ▼
Validate Bank IAM Token
   │
   ▼
User Claims
   │
   ▼
Authorization Middleware
   │
   ├── Role
   ├── Employee ID
   ├── Department
   ├── Region
   └── Branch
   │
   ▼
Controller / API Endpoint
```

The application should use claims received from the Bank IAM wherever possible.

Example claims:

```text
EmployeeId
Name
Email
Role
Department
Region
Branch
Designation
```

---

# 8. Authorization Model

Authorization will operate at two levels.

### Level 1 — Bank IAM

Responsible for:

* Employee authentication
* Employee identity
* Enterprise roles
* User activation/deactivation
* Authentication lifecycle

### Level 2 — Complaint Portal

Responsible for:

* Complaint-specific permissions
* Complaint visibility
* HO/RO/Branch access
* Assignment permissions
* Status change permissions
* Escalation permissions
* Report access

```text
                 Bank IAM
                    │
             Identity + Claims
                    │
                    ▼
            ASP.NET Core API
                    │
            Application Policy
                    │
       ┌────────────┼────────────┐
       ▼            ▼            ▼
     Branch         RO           HO
     Access       Access       Access
```

---

# 9. User Roles

Suggested logical application roles:

| Role                 | Access                        |
| -------------------- | ----------------------------- |
| Super Admin          | System configuration          |
| HO Admin             | Manage complaints at HO level |
| HO Department User   | Process department complaints |
| Regional Office User | Process RO complaints         |
| Branch User          | Process branch complaints     |
| Nodal Officer        | Monitoring and escalation     |
| Management           | Dashboard and MIS             |
| Auditor              | Read-only access              |

These roles may be mapped from the roles/claims supplied by the Bank IAM.

---

# 10. No Local Authentication Database

The Complaint Portal will **not maintain**:

```text
Username
Password
Password Hash
OTP
MFA Secret
Security Questions
Authentication Credentials
```

The portal may maintain application-specific mappings where required.

For example:

```text
IAM Role
    │
    ▼
Application Role
    │
    ▼
Complaint Permission
```

Example database table:

```text
application_role_mapping

id
iam_role
application_role
is_active
created_at
updated_at
```

---

# 11. Complaint Lifecycle

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
Complaint Received
   │
   ▼
Categorisation / Validation
   │
   ▼
Assignment
   │
   ▼
Investigation
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

If the complaint exceeds its prescribed TAT:

```text
Pending
   │
   ▼
SLA Warning
   │
   ▼
Overdue
   │
   ▼
Escalation
   │
   ▼
Higher Authority
```

---

# 12. Customer Complaint Module

The customer-facing complaint form will be integrated with the Bank's website.

Possible fields:

* Customer Name
* Mobile Number
* Email ID
* Customer ID
* Account Number, wherever applicable
* Branch
* Complaint Category
* Complaint Sub-category
* Transaction ID
* Transaction Date
* Transaction Amount
* Complaint Description
* Supporting Documents
* Preferred Communication Channel

On successful submission, the system will generate a unique complaint number.

Example:

```text
HGB-2026-00001245
```

---

# 13. Complaint Tracking

Customers should be able to track complaints using:

* Complaint Reference Number
* Registered Mobile Number
* OTP verification

The customer should be able to view:

* Complaint status
* Registration date
* Last action taken
* Response/resolution
* Closure date

Internal remarks, employee details and sensitive information must not be exposed.

---

# 14. Complaint Categories

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

Categories and sub-categories should be configurable.

---

# 15. Complaint Status

Suggested statuses:

```text
NEW
RECEIVED
ASSIGNED
UNDER_PROCESS
ESCALATED
RESOLVED
CUSTOMER_RESPONSE
CLOSED
REOPENED
REJECTED
DUPLICATE
WITHDRAWN
TRANSFERRED
```

The final workflow should be configurable according to Bank policy.

---

# 16. Complaint Assignment

Complaints can be assigned based on:

* Category
* Sub-category
* Branch
* Region
* Department
* Product
* Complaint type

Assignment can be:

### Manual

Authorized users assign complaints to another user/office.

### Automatic

The system assigns complaints using predefined routing rules.

Example:

```text
UPI Complaint
      │
      ▼
Digital Banking Division
      │
      ▼
Concerned RO
      │
      ▼
Concerned Branch
      │
      ▼
Assigned Officer
```

---

# 17. SLA / TAT Monitoring

The system should calculate:

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
       ├── Normal
       │
       ├── SLA Warning
       │
       └── Overdue
              │
              ▼
          Escalation
```

---

# 18. Escalation Management

Suggested hierarchy:

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

Escalation rules should be configurable.

---

# 19. Complaint Priority

Suggested priority levels:

```text
LOW
MEDIUM
HIGH
CRITICAL
```

Priority may be determined based on:

* Complaint type
* Financial impact
* Fraud/suspected fraud
* Customer impact
* Regulatory requirements
* Business rules

---

# 20. Complaint Timeline / Audit Trail

Every significant action must be recorded.

Example:

```text
15-09-2026 10:15 AM
Complaint Registered

15-09-2026 10:18 AM
Complaint Assigned

16-09-2026 02:30 PM
Complaint Viewed

16-09-2026 04:20 PM
Remark Added

17-09-2026 03:45 PM
Complaint Resolved

17-09-2026 04:00 PM
Response Sent
```

Audit information should include:

```text
Employee ID
Employee Name
Action
Module
Record ID
Timestamp
IP Address
Remarks
```

Employee identity should be obtained from the authenticated IAM context.

---

# 21. Attachments

Supporting documents may be uploaded by customers and authorized Bank officials.

Possible formats:

```text
PDF
JPG
JPEG
PNG
XLS
XLSX
```

Security controls:

* File size validation
* File extension validation
* MIME type validation
* Malware scanning
* Secure storage
* Access control
* Download authorization
* Audit logging

---

# 22. Communication

The system may integrate with:

* SMS Gateway
* Email Gateway

Notifications may be generated for:

1. Complaint registration
2. Assignment
3. Status change
4. Additional information required
5. Resolution
6. Closure
7. Escalation
8. Reopening

---

# 23. Search and Filters

Search options:

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
* Assigned Employee
* Date Range

---

# 24. MIS and Reports

Reports should include:

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
* Ageing analysis

Export formats:

```text
Excel
CSV
PDF
```

---

# 25. Management Dashboard

Dashboard may contain:

```text
Total Complaints
New Complaints
Pending Complaints
Overdue Complaints
Resolved Complaints
Closed Complaints
Escalated Complaints
Reopened Complaints
```

Charts:

* Daily complaint trend
* Category distribution
* Branch-wise complaints
* Region-wise complaints
* TAT performance
* Pending ageing
* Resolution trend

---

# 26. Security Requirements

The application should implement:

* Bank IAM integration
* ASP.NET Core authentication middleware
* ASP.NET Core authorization policies
* Role-based access control
* Organizational-level access control
* HTTPS/TLS
* Input validation
* SQL injection protection
* XSS protection
* CSRF protection where applicable
* Rate limiting
* Secure file upload
* Audit logging
* Access logging
* Data encryption where required
* Database backup
* Disaster recovery

The system must never store Bank employee passwords or authentication credentials.

---

# 27. ASP.NET Core API Architecture

The backend should follow a clean and modular architecture.

Recommended structure:

```text
ComplaintManagement/
│
├── src/
│   │
│   ├── ComplaintManagement.Api/
│   │   ├── Controllers/
│   │   ├── Middleware/
│   │   ├── Filters/
│   │   ├── Extensions/
│   │   ├── Program.cs
│   │   └── appsettings.json
│   │
│   ├── ComplaintManagement.Application/
│   │   ├── Complaints/
│   │   ├── Categories/
│   │   ├── Assignments/
│   │   ├── Escalations/
│   │   ├── Reports/
│   │   ├── Notifications/
│   │   └── Common/
│   │
│   ├── ComplaintManagement.Domain/
│   │   ├── Entities/
│   │   ├── Enums/
│   │   ├── ValueObjects/
│   │   └── Interfaces/
│   │
│   ├── ComplaintManagement.Infrastructure/
│   │   ├── Persistence/
│   │   ├── Repositories/
│   │   ├── IAM/
│   │   ├── Notifications/
│   │   ├── FileStorage/
│   │   └── Services/
│   │
│   └── ComplaintManagement.Contracts/
│       ├── Requests/
│       ├── Responses/
│       └── DTOs/
│
└── tests/
    ├── ComplaintManagement.UnitTests/
    └── ComplaintManagement.IntegrationTests/
```

---

# 28. Layer Responsibilities

## API Layer

Responsible for:

* HTTP requests/responses
* Controllers
* Authentication middleware configuration
* Authorization policies
* API validation
* Exception handling

## Application Layer

Responsible for:

* Business logic
* Use cases
* Complaint workflows
* SLA calculations
* Assignment
* Escalation
* Notifications

## Domain Layer

Responsible for:

* Business entities
* Domain rules
* Enums
* Interfaces
* Domain-specific logic

## Infrastructure Layer

Responsible for:

* PostgreSQL
* Entity Framework Core
* Repository implementations
* Bank IAM integration
* SMS/Email integrations
* File storage
* Redis
* External APIs

## Contracts Layer

Responsible for:

* Request DTOs
* Response DTOs
* API contracts

---

# 29. Suggested API Structure

```text
/api/v1/complaints
/api/v1/complaints/{id}
/api/v1/complaints/{id}/status
/api/v1/complaints/{id}/assign
/api/v1/complaints/{id}/remarks
/api/v1/complaints/{id}/attachments
/api/v1/complaints/{id}/history

/api/v1/categories
/api/v1/subcategories

/api/v1/branches
/api/v1/regions
/api/v1/departments

/api/v1/reports
/api/v1/dashboard

/api/v1/notifications
```

There should be **no local `/auth/login` endpoint** for Bank employees if authentication is performed by the Bank's existing IAM.

---

# 30. Example Complaint API

### Create Complaint

```http
POST /api/v1/complaints
Content-Type: application/json
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

# 31. Example ASP.NET Core Controller

Conceptual example:

```csharp
[ApiController]
[Route("api/v1/complaints")]
[Authorize]
public class ComplaintsController : ControllerBase
{
    private readonly IComplaintService _complaintService;

    public ComplaintsController(IComplaintService complaintService)
    {
        _complaintService = complaintService;
    }

    [HttpGet]
    public async Task<IActionResult> GetComplaints(
        [FromQuery] ComplaintFilterRequest request)
    {
        var result = await _complaintService.GetComplaintsAsync(request);

        return Ok(result);
    }

    [HttpGet("{id:guid}")]
    public async Task<IActionResult> GetComplaint(Guid id)
    {
        var result = await _complaintService.GetComplaintAsync(id);

        return Ok(result);
    }
}
```

Authorization policies can then be applied to specific operations.

Example:

```csharp
[Authorize(Policy = "Complaint.Assign")]
[HttpPost("{id:guid}/assign")]
public async Task<IActionResult> AssignComplaint(
    Guid id,
    AssignComplaintRequest request)
{
    // Assignment logic
    return Ok();
}
```

---

# 32. IAM Integration

The Bank IAM integration should be isolated inside:

```text
ComplaintManagement.Infrastructure/
        │
        └── IAM/
```

Example:

```text
IAM/
├── IamClient.cs
├── IamUserService.cs
├── IamTokenValidator.cs
├── IamModels.cs
└── IamOptions.cs
```

The rest of the application should not directly depend on the external IAM API implementation.

Example abstraction:

```csharp
public interface IIamUserService
{
    Task<IamUser?> GetUserAsync(string employeeId);
}
```

This allows the Bank's IAM implementation to be changed without affecting the complaint business logic.

---

# 33. Database Design

## complaints

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
assigned_employee_id
assigned_department
assigned_branch
sla_due_date
resolved_at
closed_at
created_at
updated_at
```

## complaint_categories

```text
id
name
description
is_active
created_at
updated_at
```

## complaint_status_history

```text
id
complaint_id
old_status
new_status
remarks
changed_by_employee_id
changed_at
```

## complaint_assignments

```text
id
complaint_id
assigned_from_employee_id
assigned_to_employee_id
assigned_department
remarks
assigned_at
```

## complaint_attachments

```text
id
complaint_id
file_name
file_path
file_type
file_size
uploaded_by_employee_id
uploaded_at
```

## complaint_remarks

```text
id
complaint_id
remark
visibility
created_by_employee_id
created_at
```

## audit_logs

```text
id
employee_id
action
module
record_id
ip_address
user_agent
created_at
```

## application_role_mapping

```text
id
iam_role
application_role
is_active
created_at
updated_at
```

> There is intentionally **no local users table containing passwords or authentication credentials**.

---

# 34. Entity Framework Core

Entity Framework Core will be used for database access.

Recommended approach:

```text
ASP.NET Core
      │
      ▼
Application Service
      │
      ▼
Repository / DbContext
      │
      ▼
Entity Framework Core
      │
      ▼
PostgreSQL
```

Database migrations should be maintained as part of source control.

Example:

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

# 35. Configuration

Example `appsettings.json`:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": ""
  },

  "Redis": {
    "ConnectionString": ""
  },

  "IAM": {
    "BaseUrl": "",
    "Authority": "",
    "ClientId": "",
    "ClientSecret": ""
  },

  "FileStorage": {
    "BasePath": ""
  },

  "Notification": {
    "SmsApiUrl": "",
    "EmailApiUrl": ""
  }
}
```

Secrets must be provided through secure configuration mechanisms and must not be committed to source control.

---

# 36. Background Processing

Background processing may be implemented using .NET Background Services.

Possible background jobs:

```text
SLA Monitoring
Complaint Escalation
SMS Notifications
Email Notifications
Pending Complaint Reminders
Daily MIS Generation
Data Cleanup
```

Example:

```text
Background Service
       │
       ├── Check SLA
       │
       ├── Identify Overdue
       │
       ├── Escalate
       │
       └── Send Notification
```

Redis may be used for distributed caching or queue-based processing where required.

---

# 37. Logging

ASP.NET Core logging should be used with structured logging.

Logs may contain:

```text
Request ID
Employee ID
Endpoint
HTTP Method
Response Status
Execution Time
Error Details
```

The following must not be logged:

```text
Password
OTP
Access Token
Refresh Token
Client Secret
CVV
Complete Card Number
Unmasked Sensitive Customer Data
```

---

# 38. API Documentation

The API should provide OpenAPI/Swagger documentation for development and testing.

Example:

```text
Swagger / OpenAPI
        │
        ▼
ASP.NET Core Web API
        │
        ├── Complaints
        ├── Categories
        ├── Assignments
        ├── Reports
        └── Dashboard
```

Swagger should be appropriately restricted or disabled in production according to Bank security policy.

---

# 39. Testing

The project should include:

### Unit Tests

Test:

* Complaint business logic
* SLA calculation
* Assignment rules
* Escalation rules
* Validation
* Authorization policies

### Integration Tests

Test:

* PostgreSQL
* IAM integration
* API endpoints
* File storage
* Notification services

### Security Testing

The application should undergo applicable:

* Vulnerability Assessment
* VAPT
* Security review
* API security testing
* Dependency/security scanning

---

# 40. Deployment

Recommended deployment architecture:

```text
                    Internal Network
                          │
                          ▼
                       Nginx
                          │
                          ▼
                ASP.NET Core API
                     .NET 10
                          │
              ┌───────────┼───────────┐
              ▼           ▼           ▼
          PostgreSQL    Redis     File Storage
```

Frontend:

```text
Next.js
   │
   ▼
Internal Web Server / Application Server
```

The exact deployment architecture should follow the Bank's infrastructure, network and security requirements.

---

# 41. Docker

The application should be containerized where permitted.

Suggested services:

```text
frontend
backend
postgres
redis
nginx
```

Example:

```text
docker-compose
│
├── complaint-frontend
├── complaint-api
├── postgres
├── redis
└── nginx
```

---

# 42. Backup and Recovery

The system should have:

* Regular PostgreSQL backups
* Attachment backups
* Backup verification
* Backup retention
* Disaster Recovery procedure
* Restore testing

The backup and DR design should follow the Bank's approved IT/DR policy.

---

# 43. Development Roadmap

## Phase 1 — Core Complaint Management

* Bank IAM integration
* Customer complaint registration
* Complaint number generation
* Complaint listing
* Complaint details
* Assignment
* Status management
* Remarks
* Basic dashboard

## Phase 2 — Monitoring

* SLA/TAT
* Escalation
* Notifications
* Complaint ageing
* Advanced search
* MIS reports
* Excel/CSV export

## Phase 3 — Integration

* Bank website integration
* SMS gateway
* Email gateway
* Transaction verification
* CBS / Digital Banking integrations

## Phase 4 — Advanced Features

* Automated routing
* AI-assisted categorisation
* Duplicate complaint detection
* Advanced analytics
* Management dashboards

---

# 44. Key Design Principles

1. **Centralized Authentication**
   Employee authentication will be performed by the Bank's existing User Management / IAM system.

2. **No Local Password Management**
   The Complaint Portal will not maintain employee passwords or authentication credentials.

3. **ASP.NET Core Authorization**
   API authorization will use ASP.NET Core authentication schemes, claims and authorization policies.

4. **Role-Based Access**
   Access will be based on roles/claims supplied by the Bank IAM and application-level authorization rules.

5. **Organizational Access Control**
   Complaint visibility will respect the Bank's HO/RO/Branch hierarchy.

6. **Security First**
   Customer and transaction information must be protected.

7. **Complete Auditability**
   Important actions must be recorded against the authenticated employee identity.

8. **Configurable Workflow**
   Categories, SLA, escalation and assignment rules should be configurable.

9. **API-First Architecture**
   The Bank website, internal portal, IAM and future Bank systems should communicate through secure APIs.

10. **Separation of Concerns**
    IAM handles identity and authentication; the Complaint Portal handles complaint processing and application authorization.

11. **Scalability**
    The application should support increasing complaint volumes and users.

12. **Maintainability**
    The backend should follow a clean, modular architecture with clear separation between API, application, domain and infrastructure layers.

---

# 45. Success Criteria

The portal will provide:

* Centralized complaint repository
* Unique complaint identification
* Customer complaint tracking
* Controlled assignment and reassignment
* SLA/TAT monitoring
* Automated escalation
* Complete complaint history
* Management MIS
* Audit trail
* Secure document management
* Customer status tracking
* Role-based access
* Integration with Bank's existing User Management / IAM
* No local employee password/authentication database
* Secure ASP.NET Core Web API
* Reliable backup and recovery

---

# 46. Conclusion

The **Bank Complaint Management Portal** will provide a centralized platform for managing customer complaints from registration through final resolution.

The application will use:

```text
Frontend       → Next.js / React / TypeScript
Backend        → .NET 10 LTS / C# / ASP.NET Core
Database       → PostgreSQL
Cache/Queue    → Redis
Container      → Docker
Authentication → Bank User Management / IAM
```

The application will maintain a clear separation between **identity management** and **complaint management**.

The Bank's existing User Management / IAM system will remain the source of truth for employee identity and authentication. The ASP.NET Core backend will consume the authenticated identity and relevant claims and enforce complaint-specific authorization through ASP.NET Core authorization policies.

The Complaint Portal will therefore focus on:

* Complaint registration
* Complaint categorisation
* Assignment
* Investigation
* Resolution
* SLA/TAT monitoring
* Escalation
* Customer communication
* Audit trail
* MIS
* Management monitoring

This architecture avoids duplication of the Bank's existing authentication infrastructure while providing a secure, modular and maintainable platform for centralized complaint management.
