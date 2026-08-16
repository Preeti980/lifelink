# ❤️ LifeLink

> Smart Blood Donation Platform

![Next.js](https://img.shields.io/badge/Next.js-16-black)
![TypeScript](https://img.shields.io/badge/TypeScript-5-blue)
![Express](https://img.shields.io/badge/Express.js-Backend-green)
![Prisma](https://img.shields.io/badge/Prisma-ORM-2D3748)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Database-blue)
![JWT](https://img.shields.io/badge/JWT-Authentication-orange)

---
# ❤️ LifeLink --- Complete Project Documentation

> **Smart Blood Donation & Blood Request Management Platform**

LifeLink is a full-stack blood donation platform designed to connect
**Donors, Patients, and Hospitals** through a centralized system for
blood requests, donor acceptance, donation completion, inventory
management, notifications, and history tracking.

This document describes the project as it currently exists after
implementation and frontend integration/testing. It replaces the earlier
development-progress documentation with the **completed/current
implementation state**.

------------------------------------------------------------------------

# 📌 1. Project Information

  Property               Details
  ---------------------- ----------------------------------------------
  Project Name           LifeLink
  Type                   Blood Donation Management Platform
  Frontend               Next.js + React + TypeScript
  Backend                Node.js + Express.js + TypeScript
  Database               PostgreSQL
  ORM                    Prisma
  API                    REST API
  Authentication         JWT + Refresh Token
  Server State           TanStack React Query
  API Client             Axios
  Forms                  React Hook Form
  Validation             Zod
  Styling                Tailwind CSS
  Icons                  Lucide React
  Notifications UI       Sonner / dashboard notifications
  Architecture           Feature-First + Layered Backend Architecture
  Frontend               Next.js App Router
  Development Frontend   `http://localhost:3000`
  Development Backend    `http://localhost:5000`
  Repository             `https://github.com/Preeti980/lifelink`

------------------------------------------------------------------------

# 📖 2. Project Overview

LifeLink solves a common problem in blood donation workflows: patients
and hospitals need blood quickly, while donors need a simple way to
discover and respond to active requests.

The platform provides a centralized workflow:

``` text
Patient / Hospital
        |
        | Create Blood Request
        v
     PENDING
        |
        | Donor accepts
        v
     ACCEPTED
        |
        | Hospital completes donation
        v
    COMPLETED
```

The important business rule implemented in the current system is:

> **A blood request may originate from either a Patient or a Hospital,
> and a Donor can accept requests from both.**

After a donor accepts a request:

-   A `Donation` record is created.
-   The blood request becomes `ACCEPTED`.
-   The request owner receives a notification.

After the hospital completes the donation:

-   The donation becomes `COMPLETED`.
-   The blood request becomes `COMPLETED`.
-   The donor receives a completion notification.
-   History reflects the completed donation.

------------------------------------------------------------------------

# ❗ 3. Problem Statement

Traditional blood-request workflows often depend on:

-   Phone calls
-   Manual hospital coordination
-   Separate blood-bank records
-   Informal donor communication
-   Manual inventory tracking

This creates problems such as:

-   Delayed blood availability
-   Difficulty finding compatible donors
-   Poor visibility of active requests
-   No centralized request lifecycle
-   Limited request history
-   Limited communication between stakeholders

LifeLink provides a centralized digital workflow for these activities.

------------------------------------------------------------------------

# 💡 4. Solution

LifeLink provides:

-   Secure authentication
-   Role-based dashboards
-   Patient blood requests
-   Hospital blood requests
-   Donor request discovery
-   Blood-group matching
-   Donor acceptance
-   Hospital donation completion
-   Hospital inventory
-   Notifications
-   Request history
-   Donation history
-   Availability management
-   Location-based request filtering
-   Protected API endpoints
-   PostgreSQL persistence through Prisma

------------------------------------------------------------------------

# 👥 5. User Roles

LifeLink supports four roles at the architecture level:

``` text
DONOR
PATIENT
HOSPITAL
ADMIN
```

The currently implemented and tested application flow focuses on:

``` text
PATIENT ↔ DONOR ↔ HOSPITAL
```

The Admin architecture exists, while a full Admin dashboard is not part
of the currently completed user flow.

------------------------------------------------------------------------

# ❤️ 6. Donor Dashboard

The Donor dashboard is designed around one main purpose:

> **Find a compatible blood request and respond to it.**

## Donor navigation

The donor dashboard contains the main navigation areas:

``` text
Dashboard
Nearby Requests
History
Notifications
Profile
Settings
Logout
```

------------------------------------------------------------------------

## 6.1 Donor Dashboard

The donor dashboard provides the donor with:

-   Overview
-   Active blood-request information
-   Quick navigation
-   Notification access
-   Profile access
-   Request discovery

The dashboard uses the shared LifeLink layout with:

-   Sidebar
-   Header
-   User information
-   Notification icon
-   Role-specific navigation

------------------------------------------------------------------------

# 📍 7. Donor --- Nearby Requests

The Nearby Requests page is one of the main donor features.

Purpose:

> Allow donors to discover active blood requests in their area.

The UI contains:

``` text
Nearby Requests
Find active blood requests in your area.
```

## Filters

The page supports filtering by:

-   City
-   Blood group
-   Urgency

Example UI:

``` text
City
[e.g. Lucknow]

Blood group
[All blood groups]

Urgency
[All urgency levels]
```

The page also displays the number of active/pending requests found.

------------------------------------------------------------------------

## 7.1 Request Card

Each request card displays:

-   Blood group
-   Units required
-   Urgency
-   Request status
-   Hospital name
-   City
-   Request date
-   Description
-   View Request button

Example:

``` text
B-
2 units
MEDIUM
PENDING

AK Singh Hospital Lucknow
Lucknow

Requested 15 Aug 2026

"please save life"

[View request]
```

------------------------------------------------------------------------

# 🔎 8. Donor --- Request Details

When a donor opens a request, the application displays detailed
information.

The request detail page includes:

-   Back to requests
-   Blood group
-   Required units
-   Urgency
-   Current status
-   Hospital
-   Location
-   Request date
-   Required units
-   Additional information

Example:

``` text
Blood requirement
A+

2 units required

HIGH
PENDING

Hospital
Apollo Hospital Lucknow

Location
Lucknow

Requested
14 Aug 2026

Required
2 units
```

The donor can then choose:

``` text
Donate blood
```

------------------------------------------------------------------------

# 🩸 9. Donor --- Accept Blood Request

The donor acceptance flow is implemented through:

``` http
POST /api/v1/donor/requests/:id/accept
```

The backend verifies:

1.  The authenticated user has a donor profile.
2.  The requested blood request exists.
3.  The request is still `PENDING`.
4.  The donor's blood group matches the request.

If all checks pass:

``` text
Donation created
        ↓
Donation = ACCEPTED

Blood Request
        ↓
Status = ACCEPTED
```

The request owner is notified.

------------------------------------------------------------------------

# 🧬 10. Blood Group Matching

The backend contains explicit blood-group validation.

The donor cannot accept an incompatible request.

The current service logic checks:

``` text
Donor bloodGroup
        ==
BloodRequest bloodGroup
```

If they do not match:

``` text
Your blood group does not match this request
```

This prevents an incompatible donor from accepting the request through
the API.

------------------------------------------------------------------------

# 📜 11. Donor --- Donation History

The donor history page records the donor's previous donation activity.

History is based on the `Donation` entity and includes:

-   Blood request
-   Donation status
-   Donation date
-   Request details
-   Hospital/request information

The donor service uses:

``` http
GET /api/v1/donor/history
```

------------------------------------------------------------------------

# 👤 12. Donor --- Profile

The donor profile stores information required for donor matching and
availability.

Typical profile information:

``` text
Blood Group
Gender
Date of Birth
Weight
Address
City
State
Latitude
Longitude
Last Donation
Availability
```

The implemented profile APIs are:

``` http
GET /api/v1/donor/profile
PUT /api/v1/donor/profile
```

The donor profile is stored separately from the main `User` record.

------------------------------------------------------------------------

# 🟢 13. Donor Availability

The donor profile contains an availability flag:

``` text
available: true / false
```

Nearby-donor queries only return donors who are currently available.

The donor service applies:

``` text
available = true
```

when finding nearby donors.

------------------------------------------------------------------------

# 🏥 14. Hospital Dashboard

The Hospital dashboard provides hospital-side blood-management
functionality.

The hospital workflow includes:

``` text
Dashboard
Profile
Requests
Inventory
History
Notifications
Settings
Logout
```

The hospital is responsible for:

-   Creating hospital blood requests
-   Viewing hospital requests
-   Managing blood inventory
-   Tracking donor acceptance
-   Completing accepted donations
-   Reviewing history
-   Receiving notifications

------------------------------------------------------------------------

# 🩸 15. Hospital --- Blood Inventory

The Hospital Inventory module manages available blood stock.

The backend supports:

``` http
GET /api/v1/hospital/inventory
PUT /api/v1/hospital/inventory
```

Example update:

``` json
{
  "bloodGroup": "O_POSITIVE",
  "unitsAvailable": 25
}
```

The inventory response contains:

``` text
id
hospitalId
bloodGroup
unitsAvailable
createdAt
updatedAt
```

The inventory model uses the hospital and blood group relationship to
avoid duplicate blood-group records for the same hospital.

------------------------------------------------------------------------

# 📋 16. Hospital --- Requests

Hospitals can view requests associated with their account.

Endpoint:

``` http
GET /api/v1/hospital/requests
```

Hospital request records contain:

``` text
Blood group
Units
Hospital
City
Latitude
Longitude
Description
Created date
Urgency
Status
Request owner
Donations
```

The current implementation has been tested with request states
including:

``` text
PENDING
ACCEPTED
COMPLETED
```

------------------------------------------------------------------------

# 🤝 17. Hospital --- Donor Acceptance Flow

When a donor accepts a hospital-created request:

``` text
Hospital creates request
        ↓
PENDING
        ↓
Donor sees request
        ↓
Donor accepts
        ↓
Donation created
        ↓
Request becomes ACCEPTED
        ↓
Hospital sees accepted donation
```

The donor does not need to know whether the request originated from a
patient or hospital.

The donor simply sees an active blood request and can accept it when the
blood group matches.

------------------------------------------------------------------------

# ✅ 18. Hospital --- Complete Donation

After a donor accepts a request, the hospital completes the donation.

Endpoint:

``` http
PATCH /api/v1/donation/:donationId/complete
```

Example:

``` text
PATCH /api/v1/donation/cmsueveii0003tpn4n6lhb6c4/complete
```

Only a hospital can perform this operation.

The backend verifies:

1.  The authenticated user has a hospital profile.
2.  The donation exists.
3.  The donation is currently `ACCEPTED`.
4.  The donation belongs to the logged-in hospital.
5.  The donation is then changed to `COMPLETED`.

The request is also updated:

``` text
BloodRequest = COMPLETED
```

A notification is sent to the donor.

------------------------------------------------------------------------

# 🔔 19. Notification System

LifeLink includes a notification system for important events.

Notification records contain:

``` text
id
userId
title
message
isRead
createdAt
```

Notification APIs include:

``` http
GET /api/v1/notifications
PATCH /api/v1/notifications/:id/read
PATCH /api/v1/notifications/read-all
```

------------------------------------------------------------------------

## 19.1 Donor Acceptance Notification

When a donor accepts a request, the request owner receives:

``` text
Blood request accepted

A donor has accepted your blood request for B_NEGATIVE.
```

This flow has been tested in the Patient UI.

------------------------------------------------------------------------

## 19.2 Donation Completion Notification

When the hospital completes an accepted donation, the donor receives:

``` text
Donation completed
```

with information about the hospital and blood donation.

------------------------------------------------------------------------

# 👤 20. Patient Dashboard

The Patient dashboard is now implemented and integrated with the
backend.

The patient navigation contains:

``` text
Dashboard
Blood Requests
History
Notifications
Profile
Settings
Logout
```

The patient flow is:

``` text
Patient
  |
  | Create blood request
  v
PENDING
  |
  | Donor accepts
  v
ACCEPTED
  |
  | Hospital completes donation
  v
COMPLETED
```

------------------------------------------------------------------------

# 🩸 21. Patient --- Blood Requests

The Patient Blood Requests page displays current requests.

The page contains:

``` text
Current Blood Requests

Track your active blood requests and create
a new request when needed.

[+ New Blood Request]
```

------------------------------------------------------------------------

## 21.1 Patient Request Summary

The page displays counts for:

``` text
Pending
Accepted
Completed
```

Example:

``` text
Pending      0
Accepted     1
Completed    1
```

The counts are calculated from the current request data returned by the
API.

------------------------------------------------------------------------

## 21.2 Patient Request Card

Each request displays:

-   Blood group
-   Required units
-   Urgency
-   Hospital
-   City
-   Description
-   Requested date
-   Status

Example:

``` text
B NEGATIVE
2 units
MEDIUM

AK Singh Hospital Lucknow
Lucknow

please save life

Requested 8/15/2026

ACCEPTED
```

------------------------------------------------------------------------

# 📝 22. Patient --- Create Blood Request

Patients can create blood requests from the frontend.

The backend endpoint is:

``` http
POST /api/v1/patient/requests
```

The request requires a patient profile.

Example:

``` json
{
  "bloodGroup": "B_NEGATIVE",
  "units": 2,
  "hospitalName": "AK Singh Hospital Lucknow",
  "city": "Lucknow",
  "latitude": 25.25,
  "longitude": 75.897,
  "urgency": "MEDIUM",
  "description": "Please save life"
}
```

The request is initially created as:

``` text
PENDING
```

------------------------------------------------------------------------

# 📊 23. Patient Request Status Logic

The Patient Blood Requests page currently supports:

``` text
PENDING
ACCEPTED
COMPLETED
CANCELLED
```

Visual styling is different for each status.

Examples:

``` text
PENDING
Amber

ACCEPTED
Blue

COMPLETED
Green

CANCELLED
Red
```

Icons are also used for relevant states:

``` text
PENDING    → Clock
COMPLETED  → CheckCircle
CANCELLED  → Alert
```

------------------------------------------------------------------------

# 📜 24. Patient --- Request History

The Patient History page provides a table of previous blood requests.

Columns:

``` text
Blood Group
Hospital
Units
Urgency
Status
Date
```

Example:

``` text
B NEGATIVE | AK Singh Hospital Lucknow | 2 | MEDIUM | ACCEPTED | 8/15/2026

A POSITIVE | Apollo Hospital Lucknow    | 2 | HIGH   | COMPLETED | 8/14/2026
```

The history API is:

``` http
GET /api/v1/patient/history
```

The backend history endpoint returns the patient's requests regardless
of status.

------------------------------------------------------------------------

# 🔔 25. Patient --- Notifications

The Patient Notifications page displays donor activity.

The tested UI shows:

``` text
Your Notifications

2 unread notifications
```

Example:

``` text
Blood request accepted

A donor has accepted your blood request for B_NEGATIVE.

16 Aug 2026
```

The user can:

``` text
Mark read
Mark all as read
```

------------------------------------------------------------------------

# 🧪 26. Verified Patient End-to-End Test

The following flow has been successfully tested through the frontend.

### Step 1 --- Patient creates request

Example:

``` text
B_NEGATIVE
2 units
AK Singh Hospital Lucknow
Lucknow
MEDIUM
```

Initial state:

``` text
PENDING
```

### Step 2 --- Donor opens Nearby Requests

The request appears to the donor.

### Step 3 --- Donor opens request

The donor sees:

``` text
B-
2 units
MEDIUM
PENDING
```

### Step 4 --- Donor clicks Donate Blood

The donation is successfully submitted.

### Step 5 --- Patient receives notification

The Patient Notifications page displays:

``` text
Blood request accepted
```

### Step 6 --- Patient request changes

``` text
PENDING
   ↓
ACCEPTED
```

### Step 7 --- Hospital completes donation

The hospital calls:

``` http
PATCH /api/v1/donation/:donationId/complete
```

### Step 8 --- Patient request changes

``` text
ACCEPTED
   ↓
COMPLETED
```

### Step 9 --- Patient History updates

The completed request appears in history.

------------------------------------------------------------------------

# 🔄 27. Complete Business Workflow

## Patient-Originated Request

``` text
Patient Login
     ↓
Patient Dashboard
     ↓
Create Blood Request
     ↓
PENDING
     ↓
Donor Nearby Requests
     ↓
Donor opens request
     ↓
Blood group validation
     ↓
Donor clicks Donate Blood
     ↓
Donation ACCEPTED
     ↓
Blood Request ACCEPTED
     ↓
Patient Notification
     ↓
Hospital completes donation
     ↓
Donation COMPLETED
     ↓
Blood Request COMPLETED
     ↓
Donor Notification
     ↓
History Updated
```

------------------------------------------------------------------------

## Hospital-Originated Request

``` text
Hospital Login
     ↓
Hospital Dashboard
     ↓
Create Blood Request
     ↓
PENDING
     ↓
Donor Nearby Requests
     ↓
Donor accepts
     ↓
Donation ACCEPTED
     ↓
Hospital sees accepted donation
     ↓
Hospital completes donation
     ↓
Donation COMPLETED
     ↓
Blood Request COMPLETED
     ↓
Donor Notification
```

------------------------------------------------------------------------

# 🧱 28. Database Architecture

LifeLink uses PostgreSQL with Prisma ORM.

Main models:

``` text
User
DonorProfile
PatientProfile
HospitalProfile
BloodRequest
BloodInventory
Donation
Notification
RefreshToken
```

------------------------------------------------------------------------

# 👤 29. User Model

The `User` model is the central authentication entity.

Roles:

``` text
DONOR
PATIENT
HOSPITAL
ADMIN
```

Typical fields:

``` text
id
fullName
email
password
phone
role
isVerified
createdAt
updatedAt
```

Relationships:

``` text
User
 ├── DonorProfile
 ├── PatientProfile
 ├── HospitalProfile
 ├── Notification[]
 └── RefreshToken[]
```

------------------------------------------------------------------------

# ❤️ 30. DonorProfile

Stores donor-specific information:

``` text
userId
bloodGroup
gender
dateOfBirth
weight
address
city
state
latitude
longitude
lastDonation
available
createdAt
updatedAt
```

The donor profile is linked one-to-one with the user.

------------------------------------------------------------------------

# 👤 31. PatientProfile

Stores patient-specific information.

The patient request service requires a valid patient profile before
creating a request.

The profile architecture supports patient-specific data such as:

``` text
userId
personal information
location
contact information
```

The exact schema remains controlled by the Prisma schema and validators
in the project.

------------------------------------------------------------------------

# 🏥 32. HospitalProfile

Stores hospital information:

``` text
userId
hospitalName
licenseNumber
address
city
latitude
longitude
```

A hospital profile is associated with hospital inventory and
hospital-side request/donation operations.

------------------------------------------------------------------------

# 🩸 33. BloodRequest Model

A BloodRequest represents a request for blood.

Important fields:

``` text
id
bloodGroup
units
hospitalName
city
latitude
longitude
description
createdAt
urgency
status
userId
updatedAt
```

Status lifecycle:

``` text
PENDING
ACCEPTED
COMPLETED
CANCELLED
```

The `userId` identifies the user who created the request.

Therefore:

``` text
userId = Patient
```

or:

``` text
userId = Hospital
```

depending on the request source.

------------------------------------------------------------------------

# ❤️ 34. Donation Model

A Donation connects:

``` text
Donor
   |
Donation
   |
BloodRequest
```

Important fields:

``` text
id
donorId
bloodRequestId
donatedAt
certificateUrl
status
createdAt
updatedAt
```

Current donation status flow:

``` text
ACCEPTED
   ↓
COMPLETED
```

------------------------------------------------------------------------

# 🔔 35. Notification Model

Notifications are connected to a user.

Important fields:

``` text
id
userId
title
message
isRead
createdAt
```

Examples:

``` text
Blood request accepted
Donation completed
```

------------------------------------------------------------------------

# 🏥 36. BloodInventory Model

Hospital inventory records contain:

``` text
id
hospitalId
bloodGroup
unitsAvailable
createdAt
updatedAt
```

The hospital and blood group combination is used to prevent duplicate
inventory records.

------------------------------------------------------------------------

# 🔐 37. Authentication Architecture

LifeLink uses:

``` text
JWT Access Token
+
Refresh Token
```

Authentication flow:

``` text
Login
  ↓
Validate Credentials
  ↓
Generate Access Token
  ↓
Generate Refresh Token
  ↓
Authenticated User
  ↓
Protected APIs
```

Protected request:

``` http
Authorization: Bearer <ACCESS_TOKEN>
```

------------------------------------------------------------------------

# 🛡️ 38. Role-Based Authorization

The backend does not rely only on frontend navigation.

Role checks are performed on protected APIs.

Examples:

``` text
DONOR
→ Accept blood request

HOSPITAL
→ Complete donation

PATIENT
→ Create patient request

HOSPITAL
→ Manage hospital inventory
```

This prevents users from performing operations belonging to another
role.

------------------------------------------------------------------------

# 🏗️ 39. Backend Architecture

The backend follows a layered architecture:

``` text
Routes
   ↓
Controllers
   ↓
Services
   ↓
Prisma
   ↓
PostgreSQL
```

## Routes

Responsible for endpoint definitions.

## Controllers

Responsible for:

-   Reading request data
-   Calling services
-   Returning API responses
-   Passing errors to middleware

## Services

Contain business logic.

Examples:

``` text
patientRequest.service.ts
donor.service.ts
donorRequest.service.ts
donation.service.ts
hospital.service.ts
notification.service.ts
```

## Prisma

Handles database operations and type-safe queries.

------------------------------------------------------------------------

# 🧩 40. Backend Service Logic

## Donor Profile Service

Handles:

``` text
Create/update donor profile
Get donor profile
Find nearby available donors
```

## Patient Request Service

Handles:

``` text
Create/update patient profile
Get patient profile
Create patient blood request
Get active patient requests
Get patient request history
```

## Donation Service

Handles:

``` text
Accept donation
Complete donation
Get donor donation history
```

------------------------------------------------------------------------

# 🩸 41. Donor Acceptance Service Logic

The donor acceptance operation runs inside a Prisma transaction.

Flow:

``` text
Find donor profile
       ↓
Find blood request
       ↓
Check request status
       ↓
Check blood group
       ↓
Create donation
       ↓
Update blood request
       ↓
Create notification
       ↓
Commit transaction
```

If any step fails, the transaction is rolled back.

------------------------------------------------------------------------

# ✅ 42. Donation Completion Service Logic

Completion also runs inside a Prisma transaction.

Flow:

``` text
Find hospital profile
       ↓
Find donation
       ↓
Check donation status
       ↓
Verify hospital ownership
       ↓
Update donation
       ↓
Update blood request
       ↓
Notify donor
       ↓
Commit transaction
```

The hospital ownership check compares the request hospital with the
authenticated hospital profile.

------------------------------------------------------------------------

# 🌐 43. API Base URL

Development:

``` text
http://localhost:5000/api/v1
```

Production:

``` text
https://<deployed-backend-domain>/api/v1
```

------------------------------------------------------------------------

# 📡 44. Authentication APIs

``` http
POST /api/v1/auth/register
POST /api/v1/auth/login
GET  /api/v1/auth/me
POST /api/v1/auth/logout
```

------------------------------------------------------------------------

# ❤️ 45. Donor APIs

``` http
GET  /api/v1/donor/profile
PUT  /api/v1/donor/profile
GET  /api/v1/donor/nearby
GET  /api/v1/donor/history
POST /api/v1/donor/requests/:id/accept
```

------------------------------------------------------------------------

# 👤 46. Patient APIs

``` http
POST /api/v1/patient/profile
GET  /api/v1/patient/profile
POST /api/v1/patient/requests
GET  /api/v1/patient/requests
GET  /api/v1/patient/history
```

------------------------------------------------------------------------

# 🏥 47. Hospital APIs

``` http
POST /api/v1/hospital/profile
GET  /api/v1/hospital/profile
PUT  /api/v1/hospital/profile

GET /api/v1/hospital/inventory
PUT /api/v1/hospital/inventory

GET /api/v1/hospital/requests
```

Additional hospital request/donation endpoints are available according
to the current backend routes.

------------------------------------------------------------------------

# 🩸 48. General Blood Request APIs

``` http
POST   /api/v1/request
GET    /api/v1/request
GET    /api/v1/request/:id
PUT    /api/v1/request/:id
DELETE /api/v1/request/:id
```

These general endpoints are used by the role-specific request workflow
where applicable.

------------------------------------------------------------------------

# ❤️ 49. Donation APIs

``` http
POST  /api/v1/donor/requests/:id/accept
PATCH /api/v1/donation/:id/complete
```

------------------------------------------------------------------------

# 🔔 50. Notification APIs

``` http
GET   /api/v1/notifications
PATCH /api/v1/notifications/:id/read
PATCH /api/v1/notifications/read-all
```

------------------------------------------------------------------------

# 📜 51. History APIs

Donor:

``` http
GET /api/v1/donor/history
```

Patient:

``` http
GET /api/v1/patient/history
```

Hospital history is available through the hospital request/donation
workflow.

------------------------------------------------------------------------

# 📦 52. API Response Format

Success:

``` json
{
  "success": true,
  "message": "Success message",
  "data": {}
}
```

Error:

``` json
{
  "success": false,
  "message": "Error message"
}
```

------------------------------------------------------------------------

# 🎨 53. Frontend Architecture

The frontend uses:

``` text
Next.js App Router
+
Feature-First Architecture
```

High-level structure:

``` text
client/
├── src/
│   ├── app/
│   ├── features/
│   ├── shared/
│   └── providers/
└── middleware.ts
```

------------------------------------------------------------------------

# 📁 54. App Router Structure

The application contains route groups for:

``` text
(auth)
(dashboard)
```

Dashboard routes include areas such as:

``` text
dashboard/
├── page
├── profile
├── requests
├── history
├── nearby
├── notifications
├── inventory
└── settings
```

The exact visible navigation changes according to the authenticated
user's role.

------------------------------------------------------------------------

# 🧩 55. Feature Structure

Main feature modules:

``` text
features/
├── auth
├── dashboard
├── donor
├── patient
├── hospital
├── notification
├── blood-request
└── admin
```

Each feature can contain:

``` text
api
components
hooks
services
types
schemas
```

This keeps feature-specific logic isolated.

------------------------------------------------------------------------

# 🔗 56. API / Hook Architecture

The UI does not directly implement API business logic.

The general flow is:

``` text
UI Component
     ↓
React Query Hook
     ↓
Feature API
     ↓
Axios
     ↓
Express API
     ↓
Service
     ↓
Prisma
     ↓
PostgreSQL
```

This separation keeps UI and backend communication maintainable.

------------------------------------------------------------------------

# ⚛️ 57. React Query

TanStack React Query is used for server state.

Examples of feature hooks include:

``` text
usePatientRequests()
usePatientHistory()
usePatientProfile()
useDonorProfile()
useDonorRequests()
useDonationHistory()
useNotifications()
```

React Query provides:

-   Fetching
-   Caching
-   Loading state
-   Error state
-   Refetching
-   Mutation handling

------------------------------------------------------------------------

# 📡 58. Axios Architecture

The shared Axios layer handles:

-   Base API URL
-   Authentication token
-   Request interceptor
-   Response interceptor
-   Unauthorized responses

Conceptually:

``` text
Axios Client
   |
   ├── Base URL
   ├── Authorization
   ├── Error Handling
   └── API Requests
```

------------------------------------------------------------------------

# 🎨 59. Shared UI

Shared UI components are designed to avoid duplication.

Typical shared categories:

``` text
ui/
layout/
cards/
forms/
tables/
modals/
loaders/
```

Examples:

``` text
Button
Input
Select
Textarea
Badge
Card
Loader
DashboardShell
Navbar
Sidebar
```

------------------------------------------------------------------------

# 🧭 60. Shared Dashboard Layout

The dashboard uses a consistent shell containing:

``` text
Sidebar
Header
Main Content
User Information
Notification Access
Settings
Logout
```

Role-specific menu items are displayed according to the authenticated
role.

------------------------------------------------------------------------

# 🖥️ 61. Current Dashboard UI

The implemented UI uses a consistent LifeLink visual language:

``` text
White cards
Soft borders
Rounded corners
Light background
Red primary actions
Blue accepted states
Green completed states
Amber pending states
Lucide icons
Responsive layouts
```

The design is focused on:

-   Clean spacing
-   Readability
-   Clear status hierarchy
-   Simple request actions
-   Role-specific navigation

------------------------------------------------------------------------

# 📊 62. Dashboard Status Cards

The Patient Blood Requests dashboard currently displays:

``` text
Pending
Accepted
Completed
```

The counts are generated dynamically from the fetched request data.

This prevents hardcoded status counts.

------------------------------------------------------------------------

# 🧪 63. Testing Performed

The project has been tested using both:

``` text
Frontend UI
Postman / API testing
```

The backend endpoints were verified independently and then integrated
with the frontend.

------------------------------------------------------------------------

# ✅ 64. Authentication Testing

Tested:

``` text
Registration
Login
JWT authentication
Protected endpoints
Role-based access
Logout
```

------------------------------------------------------------------------

# ✅ 65. Donor Testing

Tested:

``` text
Donor profile
Donor blood group
Donor availability
Nearby requests
Request details
Donate Blood action
Donation creation
Donation history
Notifications
```

------------------------------------------------------------------------

# ✅ 66. Patient Testing

Tested through the frontend:

``` text
Patient profile
Create blood request
Current Blood Requests
Pending status
Accepted status
Completed status
Request History
Notifications
Donor acceptance notification
```

The B-negative patient request was successfully tested through the UI.

------------------------------------------------------------------------

# ✅ 67. Hospital Testing

Tested:

``` text
Hospital profile
Hospital inventory
Inventory update
Inventory retrieval
Hospital requests
Accepted donations
Donation completion
Request status update
Donation completion notification
```

------------------------------------------------------------------------

# 🔄 68. Verified Request Example

A tested patient request:

``` json
{
  "id": "cmsueopym0001tpn44f2egh8b",
  "bloodGroup": "B_NEGATIVE",
  "units": 2,
  "hospitalName": "AK Singh Hospital Lucknow",
  "city": "Lucknow",
  "urgency": "MEDIUM",
  "status": "ACCEPTED"
}
```

The corresponding donation became:

``` text
status = ACCEPTED
```

After the hospital completed the donation, the request lifecycle was
verified through the frontend and history.

------------------------------------------------------------------------

# 🔄 69. Verified Completed Request

A tested request followed:

``` text
A_POSITIVE
2 units
Apollo Hospital Lucknow
HIGH
```

The request changed:

``` text
PENDING
   ↓
ACCEPTED
   ↓
COMPLETED
```

The corresponding donation changed:

``` text
ACCEPTED
   ↓
COMPLETED
```

The Patient History UI then displayed the request as:

``` text
COMPLETED
```

------------------------------------------------------------------------

# 🔔 70. Verified Notification Flow

The Patient Notifications UI successfully displayed:

``` text
Blood request accepted

A donor has accepted your blood request for B_NEGATIVE.
```

The notification page also supports:

``` text
Mark read
Mark all as read
```

------------------------------------------------------------------------

# 🧪 71. Important API Test

Donation completion was successfully tested with:

``` http
PATCH /api/v1/donation/cmsueveii0003tpn4n6lhb6c4/complete
```

The response returned:

``` json
{
  "success": true,
  "message": "Donation completed successfully"
}
```

The returned donation had:

``` text
status: COMPLETED
```

and the related blood request was updated to:

``` text
status: COMPLETED
```

------------------------------------------------------------------------

# 🚦 72. HTTP Status Codes

The backend uses standard HTTP status codes:

``` text
200 OK
201 Created
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
500 Internal Server Error
```

------------------------------------------------------------------------

# 🛡️ 73. Error Handling

The backend uses application-level errors such as:

``` text
User not found
Donor profile not found
Patient profile not found
Blood request not found
Donation not found
Only a donor can accept a blood request
Only a hospital can complete a donation
Blood group mismatch
Request already accepted/completed
```

The frontend displays API errors through its existing error-handling/UI
mechanisms.

------------------------------------------------------------------------

# 🧠 74. Important Business Rules

## Rule 1 --- Request ownership

A request belongs to the user who created it:

``` text
BloodRequest.userId
```

The user can be:

``` text
PATIENT
```

or:

``` text
HOSPITAL
```

------------------------------------------------------------------------

## Rule 2 --- Donor acceptance

Only a donor can accept a blood request.

------------------------------------------------------------------------

## Rule 3 --- Blood group

The donor blood group must match the requested blood group.

------------------------------------------------------------------------

## Rule 4 --- Request status

A donor can only accept:

``` text
PENDING
```

requests.

------------------------------------------------------------------------

## Rule 5 --- Donation completion

Only a hospital can complete an accepted donation.

------------------------------------------------------------------------

## Rule 6 --- Hospital ownership

The hospital completing a donation must correspond to the hospital
associated with the request.

------------------------------------------------------------------------

## Rule 7 --- Notifications

Important state transitions generate notifications.

------------------------------------------------------------------------

# 🗂️ 75. Project Structure

The project contains two applications:

``` text
LifeLink/
├── client/
└── server/
```

Backend:

``` text
server/
├── prisma/
├── src/
│   ├── controllers/
│   ├── middleware/
│   ├── routes/
│   ├── services/
│   ├── validators/
│   ├── utils/
│   ├── prisma/
│   ├── app.ts
│   └── server.ts
└── ...
```

Frontend:

``` text
client/
├── public/
├── src/
│   ├── app/
│   ├── features/
│   ├── shared/
│   └── providers/
├── middleware.ts
└── ...
```

------------------------------------------------------------------------

# 🔐 76. Environment Variables

## Backend

Example:

``` env
PORT=5000
DATABASE_URL=
JWT_SECRET=
JWT_REFRESH_SECRET=
JWT_EXPIRES_IN=
JWT_REFRESH_EXPIRES_IN=
CORS_ORIGIN=
NODE_ENV=
```

## Frontend

Development:

``` env
NEXT_PUBLIC_API_URL=http://localhost:5000/api/v1
```

Production:

``` env
NEXT_PUBLIC_API_URL=https://your-backend-domain.com/api/v1
```

Never commit real secrets to GitHub.

------------------------------------------------------------------------

# 💻 77. Local Development

## Clone

``` bash
git clone https://github.com/Preeti980/lifelink.git
```

## Backend

``` bash
cd server
npm install
npx prisma generate
npx prisma migrate dev
npm run dev
```

Backend:

``` text
http://localhost:5000
```

## Frontend

``` bash
cd client
npm install
npm run dev
```

Frontend:

``` text
http://localhost:3000
```

------------------------------------------------------------------------

# 🗄️ 78. Production Database

For production:

``` bash
npx prisma generate
npx prisma migrate deploy
```

Do not use development migration commands against a production database
unless the deployment workflow explicitly requires them.

------------------------------------------------------------------------

# 🚀 79. Deployment Architecture

Recommended production architecture:

``` text
                 Internet
                    |
                    v
          +-------------------+
          | Next.js Frontend  |
          |     Vercel        |
          +---------+---------+
                    |
                 HTTPS
                    |
                    v
          +-------------------+
          | Express Backend   |
          | Render / Railway  |
          +---------+---------+
                    |
                 Prisma
                    |
                    v
          +-------------------+
          | PostgreSQL        |
          | Managed Database  |
          +-------------------+
```

------------------------------------------------------------------------

# ▲ 80. Frontend Deployment

Recommended platform:

``` text
Vercel
```

Build:

``` bash
npm run build
```

Start locally for production verification:

``` bash
npm run start
```

Configure:

``` env
NEXT_PUBLIC_API_URL=https://your-backend-domain.com/api/v1
```

------------------------------------------------------------------------

# 🟢 81. Backend Deployment

Possible platforms:

``` text
Render
Railway
DigitalOcean
VPS
```

Production setup:

``` bash
npm install
npx prisma generate
npx prisma migrate deploy
npm run build
```

Then start the backend using the project's production start command.

------------------------------------------------------------------------

# 🌐 82. CORS Configuration

Development frontend:

``` text
http://localhost:3000
```

Production frontend:

``` text
https://your-frontend-domain.com
```

The backend should allow the production frontend origin.

Do not use unrestricted CORS in production.

------------------------------------------------------------------------

# 🔒 83. Production Security Checklist

Before deployment:

-   [ ] Remove real test passwords from documentation
-   [ ] Do not commit `.env`
-   [ ] Do not commit database passwords
-   [ ] Do not commit JWT secrets
-   [ ] Use HTTPS
-   [ ] Configure restricted CORS
-   [ ] Verify JWT expiration
-   [ ] Verify role authorization
-   [ ] Validate all request payloads
-   [ ] Review production error responses
-   [ ] Remove unnecessary test data
-   [ ] Use production PostgreSQL credentials
-   [ ] Verify database backups
-   [ ] Verify frontend API URL

------------------------------------------------------------------------

# 🧪 84. Final Deployment Testing Checklist

## Authentication

-   [ ] Register donor
-   [ ] Register patient
-   [ ] Register hospital
-   [ ] Login
-   [ ] Logout
-   [ ] Protected routes
-   [ ] Role-based access

## Donor

-   [ ] Profile
-   [ ] Blood group
-   [ ] Availability
-   [ ] Nearby Requests
-   [ ] Search/filter
-   [ ] Request details
-   [ ] Donate Blood
-   [ ] Donation history
-   [ ] Notifications

## Patient

-   [ ] Profile
-   [ ] Create request
-   [ ] View current requests
-   [ ] Pending count
-   [ ] Accepted count
-   [ ] Completed count
-   [ ] Request history
-   [ ] Notifications
-   [ ] Mark notification read

## Hospital

-   [ ] Profile
-   [ ] Inventory
-   [ ] Update inventory
-   [ ] View requests
-   [ ] View accepted donation
-   [ ] Complete donation
-   [ ] Request status updates
-   [ ] Donation status updates
-   [ ] Notifications
-   [ ] History

## End-to-End

-   [ ] Patient creates request
-   [ ] Donor sees patient request
-   [ ] Donor accepts request
-   [ ] Patient sees `ACCEPTED`
-   [ ] Patient receives notification
-   [ ] Hospital sees accepted donation
-   [ ] Hospital completes donation
-   [ ] Patient sees `COMPLETED`
-   [ ] Donor sees completed donation
-   [ ] History reflects completion

Repeat the same flow for:

``` text
Hospital → Donor → Hospital
```

------------------------------------------------------------------------

# 📈 85. Current Implementation Status

The previous version of this document described Patient/Admin/frontend
areas as being under development. The current implementation has
progressed beyond that earlier state.

## Backend

``` text
Authentication       ✅
JWT                  ✅
Refresh Token        ✅
Role Middleware      ✅
Donor Profile        ✅
Patient Profile      ✅
Hospital Profile     ✅
Blood Inventory      ✅
Blood Requests       ✅
Donor Acceptance     ✅
Donation Completion  ✅
Notifications        ✅
Donor History        ✅
Patient History      ✅
Hospital Requests    ✅
```

## Frontend

``` text
Authentication       ✅
Shared Dashboard     ✅
Donor Dashboard      ✅
Donor Profile        ✅
Nearby Requests      ✅
Request Details      ✅
Donate Blood         ✅
Patient Dashboard    ✅
Patient Requests     ✅
Create Request       ✅
Patient History      ✅
Notifications        ✅
Hospital Dashboard   ✅
Hospital Inventory   ✅
Hospital Requests    ✅
Donation Completion  ✅
```

Admin dashboard/analytics remain future expansion areas rather than part
of the currently verified core workflow.

------------------------------------------------------------------------

# 📊 86. Current Core Product State

The core LifeLink product flow is functional:

``` text
PATIENT
   |
   | Creates request
   v
PENDING
   |
   | DONOR
   | Accepts
   v
ACCEPTED
   |
   | HOSPITAL
   | Completes donation
   v
COMPLETED
```

This is the primary production workflow of the current application.

------------------------------------------------------------------------

# 🎯 87. Future Improvements

Future versions can add:

## Admin

``` text
Admin Dashboard
User Management
Hospital Management
Donor Management
Patient Management
Reports
Analytics
System Settings
```

## Analytics

``` text
Monthly Donations
Blood Group Statistics
Hospital Performance
Active Requests
Daily Requests
Donor Growth
Inventory Reports
```

## Communication

``` text
Real-Time Notifications
Chat
Email Notifications
SMS Notifications
```

## Location

``` text
Google Maps
Live Tracking
Advanced Nearby Search
```

## Advanced Features

``` text
Appointment Scheduling
Emergency Broadcast
AI Blood Recommendation
Blood Demand Prediction
Inventory Prediction
```

## External Integration

``` text
Government Portal
National Blood Bank
Hospital Systems
```

------------------------------------------------------------------------

# 📋 88. Coding Guidelines

## Backend

-   TypeScript
-   Layered architecture
-   Service layer
-   Prisma ORM
-   PostgreSQL
-   JWT authentication
-   REST APIs
-   Zod validation
-   Transactional business operations

## Frontend

-   Next.js App Router
-   Feature-first architecture
-   Strict TypeScript
-   Reusable components
-   React Query
-   React Hook Form
-   Zod validation
-   Centralized Axios API layer
-   Separation of concerns

Avoid:

``` text
any
duplicated components
business logic inside UI components
direct Axios calls from presentation components
hardcoded API URLs
hardcoded user-specific status values
```

------------------------------------------------------------------------

# 🤝 89. Contribution Guidelines

When extending LifeLink:

1.  Keep features inside their feature module.
2.  Keep API calls inside API/service layers.
3.  Use React Query hooks for server state.
4.  Reuse shared components.
5.  Keep backend business logic inside services.
6.  Validate inputs.
7.  Preserve role-based authorization.
8.  Use transactions for multi-step critical database operations.
9.  Avoid duplicating logic.
10. Do not expose secrets.
11. Maintain the existing request lifecycle.
12. Test both the API and frontend flow after changes.

------------------------------------------------------------------------

# 🧭 90. Important Development Principle

The most important business flow should remain:

``` text
Request Creation
      ↓
Request Discovery
      ↓
Donor Acceptance
      ↓
Hospital Completion
      ↓
Notification
      ↓
History
```

Any future feature should integrate with this lifecycle rather than
bypassing it.

------------------------------------------------------------------------

# 👨‍💻 91. Developer

**Preeti Chauhan**

B.Tech --- Computer Science & Engineering

Full Stack Developer

GitHub:

``` text
https://github.com/Preeti980
```

Project:

``` text
https://github.com/Preeti980/lifelink
```

------------------------------------------------------------------------

# 📄 92. License / Usage

LifeLink is intended as a portfolio, educational, and
software-development project.

Before using the platform for real healthcare operations, additional
requirements should be addressed, including:

-   Security auditing
-   Privacy requirements
-   Data protection
-   Healthcare compliance
-   Production monitoring
-   Backup and recovery
-   Identity verification
-   Blood-bank operational policies
-   Appropriate medical/legal review

------------------------------------------------------------------------

# ❤️ 93. Final Summary

LifeLink is a full-stack blood donation platform built with:

``` text
Next.js
React
TypeScript
Tailwind CSS
TanStack React Query
Axios
Express.js
Prisma
PostgreSQL
JWT
Zod
```

The application now provides a complete core workflow for:

``` text
Patient
   ↓
Blood Request
   ↓
Donor
   ↓
Donation Acceptance
   ↓
Hospital
   ↓
Donation Completion
   ↓
Notifications
   ↓
History
```

The current frontend includes role-based dashboards and working pages
for the core Donor, Patient, and Hospital workflows.

The most important verified scenario is:

``` text
Patient creates B- request
        ↓
Donor sees request
        ↓
Donor clicks Donate Blood
        ↓
Patient receives notification
        ↓
Request becomes ACCEPTED
        ↓
Hospital completes donation
        ↓
Donation becomes COMPLETED
        ↓
Request becomes COMPLETED
        ↓
Patient History updates
        ↓
Donor receives completion notification
```

This document represents the current project state and should be
maintained as the primary technical documentation for LifeLink.

------------------------------------------------------------------------

# End of Documentation

**LifeLink --- Complete Project Documentation**
