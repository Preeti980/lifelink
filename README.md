# ❤️ LifeLink

### Smart Blood Donation & Request Management Platform

LifeLink is a full-stack blood donation platform that connects
**patients, donors, and hospitals** through a role-based workflow for
creating blood requests, finding compatible donors, accepting donations,
completing donations, managing hospital inventory, and receiving
notifications.

> Built as a full-stack project with a focus on real-world blood-request
> and donation workflows.

------------------------------------------------------------------------

## 🚀 Project Overview

LifeLink supports three primary user roles:

-   🧑‍⚕️ **Patient** --- creates and tracks blood requests.
-   🩸 **Donor** --- discovers compatible nearby requests and accepts
    donations.
-   🏥 **Hospital** --- creates/manages blood requirements, manages
    inventory, and completes accepted donations.

The core workflow is:

``` text
Patient / Hospital creates blood request
                ↓
        Request becomes PENDING
                ↓
      Compatible donor finds request
                ↓
          Donor accepts
                ↓
        Request → ACCEPTED
                ↓
       Hospital completes donation
                ↓
       Donation → COMPLETED
                ↓
       Request → COMPLETED
                ↓
        Notifications + History
```

------------------------------------------------------------------------

## ✨ Key Features

### 🩸 Donor

-   Donor registration and authentication
-   Donor profile management
-   Blood-group information
-   Availability management
-   Nearby blood requests
-   Filter requests by:
    -   City
    -   Blood group
    -   Urgency
-   Blood-group compatibility validation
-   Accept blood request
-   Donation history
-   Donation status tracking
-   Notifications when a donation is completed

### 🧑‍⚕️ Patient

-   Patient registration and authentication
-   Patient profile
-   Create blood requests
-   Specify:
    -   Blood group
    -   Units required
    -   Hospital
    -   City/location
    -   Urgency
    -   Description
-   Track active requests
-   See accepted donations
-   Track request status
-   Request history
-   Notifications when a donor accepts

### 🏥 Hospital

-   Hospital authentication
-   Hospital profile
-   Create and manage blood requests
-   View request status
-   View accepted donations
-   Manage blood inventory
-   Update available blood units
-   Complete accepted donations
-   Donation/request history
-   Notifications

### 🔐 Platform

-   JWT authentication
-   Role-based authorization
-   Protected API routes
-   Prisma ORM
-   PostgreSQL database
-   REST API architecture
-   Transaction-based donation workflow
-   Blood-group validation
-   Request status management
-   Notification system
-   Responsive dashboard UI

------------------------------------------------------------------------

## 🔄 Blood Request Lifecycle

``` text
PENDING
   │
   │ Donor accepts
   ▼
ACCEPTED
   │
   │ Hospital completes donation
   ▼
COMPLETED
```

The backend validates the state transition so an already
accepted/completed request cannot be accepted again.

------------------------------------------------------------------------

## 🧠 Donation Workflow

### 1. Request Creation

A patient or hospital creates a blood request.

``` text
POST /api/v1/patient/requests
```

or the corresponding hospital request endpoint.

The request is created with:

``` text
status = PENDING
```

### 2. Donor Discovery

The donor opens **Nearby Requests** and can filter requests by location,
blood group, and urgency.

### 3. Donor Acceptance

``` text
POST /api/v1/donor/requests/:id/accept
```

The backend verifies:

-   Donor profile exists
-   Blood request exists
-   Request is still `PENDING`
-   Donor blood group matches the requested blood group

Then:

``` text
Donation → ACCEPTED
BloodRequest → ACCEPTED
```

A notification is created for the request owner.

### 4. Hospital Completion

``` text
PATCH /api/v1/donation/:donationId/complete
```

The backend verifies that the logged-in user is a hospital and that the
donation belongs to that hospital.

Then:

``` text
Donation → COMPLETED
BloodRequest → COMPLETED
```

The donor receives a completion notification.

------------------------------------------------------------------------

## 🏗️ Architecture

``` text
┌─────────────────────────────────────────────┐
│                 Next.js Client              │
│                                             │
│  Dashboards • Requests • Profile • History  │
│  Notifications • Settings                   │
└──────────────────────┬──────────────────────┘
                       │ REST API
                       ▼
┌─────────────────────────────────────────────┐
│              Node.js + Express              │
│                                             │
│ Routes → Controllers → Services → Prisma    │
│ Auth Middleware • Validation • Errors       │
└──────────────────────┬──────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────┐
│                 PostgreSQL                  │
│                                             │
│ Users • Profiles • Blood Requests           │
│ Donations • Inventory • Notifications      │
└─────────────────────────────────────────────┘
```

------------------------------------------------------------------------

## 🛠️ Tech Stack

### Frontend

  Technology     Purpose
  -------------- -----------------------------
  Next.js        React application framework
  React          UI
  TypeScript     Type safety
  Tailwind CSS   Styling
  React Query    Server state/data fetching
  Axios          API communication
  Lucide React   Icons

### Backend

  Technology         Purpose
  ------------------ ---------------------
  Node.js            Runtime
  Express.js         REST API
  TypeScript         Type safety
  Prisma             ORM/database access
  PostgreSQL         Database
  JWT                Authentication
  Zod / Validators   Request validation

------------------------------------------------------------------------

## 👥 User Roles

  Role       Main Responsibilities
  ---------- -------------------------------------------
  Patient    Create and track blood requests
  Donor      Find compatible requests and donate blood
  Hospital   Manage requests, inventory and donations
  Admin      Platform-level management

------------------------------------------------------------------------

## 📊 Main Dashboards

### Donor Dashboard

Includes:

-   Dashboard overview
-   Nearby Requests
-   Request details
-   Donate Blood action
-   Donation history
-   Notifications
-   Donor profile
-   Availability/settings

### Patient Dashboard

Includes:

-   Current Blood Requests
-   New Blood Request
-   Pending/Accepted/Completed counters
-   Request status
-   Request History
-   Notifications
-   Patient profile
-   Settings

### Hospital Dashboard

Includes:

-   Blood requests
-   Hospital inventory
-   Accepted donations
-   Donation completion
-   Request management
-   Notifications
-   Hospital profile
-   Settings

------------------------------------------------------------------------

## 📸 Screenshots

Add your final project screenshots here:

``` text
docs/
└── screenshots/
    ├── donor-dashboard.png
    ├── nearby-requests.png
    ├── request-details.png
    ├── patient-dashboard.png
    ├── patient-history.png
    ├── notifications.png
    ├── hospital-dashboard.png
    └── hospital-inventory.png
```

Example Markdown:

``` md
![Donor Dashboard](docs/screenshots/donor-dashboard.png)
![Nearby Requests](docs/screenshots/nearby-requests.png)
![Patient Dashboard](docs/screenshots/patient-dashboard.png)
![Hospital Dashboard](docs/screenshots/hospital-dashboard.png)
```

------------------------------------------------------------------------

## 🧪 Tested End-to-End Flow

The core workflow has been tested through the frontend and API:

-   Patient request creation
-   Hospital request creation
-   Request retrieval
-   Donor profile verification
-   Blood-group matching
-   Donor acceptance
-   Donation creation
-   Request status update
-   Patient request status update
-   Hospital donation completion
-   Donation status update
-   Request completion
-   Notification creation
-   Patient history
-   Donor history
-   Hospital request/history views

Example final state:

``` text
Patient Request
B_NEGATIVE
2 units
        ↓
Donor accepts
        ↓
Donation ACCEPTED
        ↓
Hospital completes
        ↓
Donation COMPLETED
        ↓
Request COMPLETED
```

------------------------------------------------------------------------

## 🔐 Security & Authorization

LifeLink uses authenticated API requests with role-based access.

Examples:

``` text
PATIENT
  → patient profile
  → patient requests

DONOR
  → donor profile
  → nearby requests
  → accept donation
  → donation history

HOSPITAL
  → hospital profile
  → hospital requests
  → hospital inventory
  → complete donation
```

Sensitive configuration such as database URLs and JWT secrets should be
stored in environment variables and should **not** be committed to
GitHub.

------------------------------------------------------------------------

## 📁 Project Structure

A simplified structure:

``` text
LifeLink/
│
├── client/
│   ├── app/
│   ├── features/
│   │   ├── donor/
│   │   ├── patient/
│   │   ├── hospital/
│   │   └── admin/
│   ├── shared/
│   └── providers/
│
├── server/
│   └── src/
│       ├── controllers/
│       ├── routes/
│       ├── services/
│       ├── middleware/
│       ├── validators/
│       ├── utils/
│       └── prisma/
│
├── README.md
├── DOCUMENTATION.md
└── package.json
```

------------------------------------------------------------------------

## ⚙️ Environment Variables

Create environment files locally.

Typical backend configuration:

``` env
DATABASE_URL="your_postgresql_connection_string"
JWT_SECRET="your_jwt_secret"
PORT=5000
```

Typical frontend configuration:

``` env
NEXT_PUBLIC_API_URL="http://localhost:5000/api/v1"
```

Use your project's actual environment variable names when configuring
deployment.

> Never commit `.env` files or production secrets.

------------------------------------------------------------------------

## 🚀 Run Locally

### 1. Clone

``` bash
git clone <your-github-repository-url>
cd LifeLink
```

### 2. Install dependencies

Install dependencies for the frontend and backend according to the
project structure.

``` bash
npm install
```

### 3. Configure environment variables

Create the required `.env` files.

### 4. Configure PostgreSQL

Create a PostgreSQL database and set the connection string in
`DATABASE_URL`.

### 5. Run Prisma

``` bash
npx prisma generate
npx prisma migrate dev
```

### 6. Start backend

``` bash
npm run dev
```

### 7. Start frontend

``` bash
npm run dev
```

Then open:

``` text
http://localhost:3000
```

------------------------------------------------------------------------

## 🌐 Deployment

For production deployment:

### Frontend

The Next.js frontend can be deployed on platforms such as Vercel.

### Backend

The Express/Node.js API can be deployed on a Node-compatible hosting
platform.

### Database

Use a managed PostgreSQL provider.

Production configuration should include:

``` text
Frontend
   ↓
Production API URL
   ↓
Express Backend
   ↓
Production PostgreSQL
```

Before deployment, verify:

-   Production `DATABASE_URL`
-   Production `JWT_SECRET`
-   Frontend API URL
-   CORS configuration
-   Prisma migrations
-   Environment variables
-   Authentication flow
-   Role-based routes
-   API health
-   Database connectivity

------------------------------------------------------------------------

## 📚 Documentation

For complete technical documentation, architecture, API details,
database design, workflows, testing information, and implementation
details:

**[DOCUMENTATION.md](DOCUMENTATION.md)**

------------------------------------------------------------------------

## 🎯 Future Improvements

Potential improvements include:

-   Real-time notifications
-   Email/SMS alerts
-   Location-based distance calculation
-   Google Maps integration
-   Advanced donor matching
-   Blood inventory alerts
-   Donation certificates
-   Admin analytics
-   Hospital verification
-   Cloud image/document storage
-   Production monitoring
-   Automated testing
-   CI/CD pipeline

------------------------------------------------------------------------

## 👨‍💻 Developer

**Preeti Chauhan**

B.Tech --- Computer Science & Engineering

Full-Stack Developer

------------------------------------------------------------------------

## ⭐ Project Goal

LifeLink was built to demonstrate a complete real-world full-stack
workflow where multiple user roles interact with the same system through
secure APIs, database transactions, role-based authorization, and a
responsive dashboard experience.

If you find the project useful, consider giving the repository a ⭐.
