# ❤️ LifeLink

### Smart Blood Donation & Request Management Platform

LifeLink is a full-stack blood donation platform that connects **patients, donors, hospitals, and administrators** through a role-based workflow for creating blood requests, finding compatible donors, accepting donations, completing donations, managing hospital inventory, tracking history, and receiving notifications.

> Built as a real-world full-stack project with complete request-to-donation workflows and separate production frontend and backend deployments.

---

## 🌐 Live Demo

### Frontend
**https://life-link-frontend-steel.vercel.app/**

### Backend API
**https://lifelink-backend-6yo2.onrender.com/**

> Open the backend root URL to verify that the LifeLink API is running.

---

## 💻 Source Code

The project is maintained in two separate GitHub repositories:

- **Frontend:** https://github.com/Preeti980/LifeLink-Frontend
- **Backend:** https://github.com/Preeti980/LifeLink-Backend

---

## 🚀 Project Overview

LifeLink provides dedicated workflows for four user roles:

- 🧑‍⚕️ **Patient** — creates and tracks blood requests.
- 🩸 **Donor** — discovers compatible requests and accepts donations.
- 🏥 **Hospital** — creates and manages blood requirements, manages inventory, and completes accepted donations.
- 🔐 **Admin** — manages users and monitors overall platform activity.

### Core Workflow

```text
Patient / Hospital creates blood request
                ↓
          Request → PENDING
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

---

# ✨ Key Features

## 🩸 Donor

- Donor registration and authentication
- Donor profile management
- Blood-group information
- Availability management
- Nearby blood requests
- Filtering by city, blood group, and urgency
- Blood-group compatibility validation
- Accept blood requests
- Donation status tracking
- Donation history
- Completion notifications

## 🧑‍⚕️ Patient

- Patient registration and authentication
- Patient profile management
- Create blood requests
- Specify blood group, units, hospital, city/location, urgency, and description
- Track active requests
- See accepted donations
- Track request status
- Request history
- Notifications when a donor accepts a request

## 🏥 Hospital

- Hospital authentication and profile
- Create and manage blood requests
- View request status
- View accepted donations
- Manage blood inventory
- Update available blood units
- Complete accepted donations
- Donation/request history
- Notifications

## 🔐 Admin

- Role-based admin access
- User management
- Platform activity monitoring
- Request and donation monitoring
- Dashboard analytics
- System settings

## ⚙️ Platform

- JWT authentication
- Role-based authorization
- Protected API routes
- REST API architecture
- Prisma ORM
- PostgreSQL database
- Transaction-based donation workflow
- Blood-group validation
- Request status management
- Notification system
- Responsive dashboards
- Production CORS configuration

---

# 🔄 Blood Request Lifecycle

```text
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

The backend validates state transitions so an already accepted or completed request cannot be accepted again.

---

# 🧠 End-to-End Donation Workflow

### 1. Request Creation

A patient or hospital creates a blood request.

```text
POST /api/v1/patient/requests
```

or the corresponding hospital request endpoint.

The request starts with:

```text
status = PENDING
```

### 2. Donor Discovery

The donor opens **Nearby Requests** and can filter requests by location, blood group, and urgency.

### 3. Donor Acceptance

```text
POST /api/v1/donor/requests/:id/accept
```

The backend verifies:

- Donor profile exists
- Blood request exists
- Request is still `PENDING`
- Donor blood group matches the requested blood group

Then:

```text
Donation → ACCEPTED
BloodRequest → ACCEPTED
```

A notification is created for the request owner.

### 4. Hospital Completion

```text
PATCH /api/v1/donation/:donationId/complete
```

The backend verifies that the logged-in user is a hospital and that the donation belongs to that hospital.

Then:

```text
Donation → COMPLETED
BloodRequest → COMPLETED
```

The donor receives a completion notification.

---

# 🏗️ Architecture

```text
┌─────────────────────────────────────────────┐
│              Next.js Frontend               │
│                                             │
│ Dashboards • Requests • Profile • History   │
│ Notifications • Settings • Analytics       │
└──────────────────────┬──────────────────────┘
                       │ REST API
                       ▼
┌─────────────────────────────────────────────┐
│            Node.js + Express Backend        │
│                                             │
│ Routes → Controllers → Services → Prisma   │
│ Auth • Validation • Errors • Transactions  │
└──────────────────────┬──────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────┐
│                PostgreSQL                   │
│                                             │
│ Users • Profiles • Blood Requests           │
│ Donations • Inventory • Notifications      │
└─────────────────────────────────────────────┘
```

---

# 🛠️ Tech Stack

### Frontend

| Technology | Purpose |
|---|---|
| Next.js | React application framework |
| React | UI |
| TypeScript | Type safety |
| Tailwind CSS | Styling |
| React Query | Server state/data fetching |
| Axios | API communication |
| Lucide React | Icons |

### Backend

| Technology | Purpose |
|---|---|
| Node.js | Runtime |
| Express.js | REST API |
| TypeScript | Type safety |
| Prisma | ORM/database access |
| PostgreSQL | Database |
| JWT | Authentication |
| Zod / Validators | Request validation |

---

# 👥 User Roles

| Role | Main Responsibilities |
|---|---|
| Patient | Create and track blood requests |
| Donor | Find compatible requests and donate blood |
| Hospital | Manage requests, inventory, and donations |
| Admin | Platform-level management and monitoring |

---

# 📊 Main Dashboards

### Donor Dashboard

- Dashboard overview
- Nearby Requests
- Request details
- Donate Blood action
- Donation history
- Notifications
- Donor profile
- Availability/settings

### Patient Dashboard

- Current Blood Requests
- New Blood Request
- Pending/Accepted/Completed counters
- Request status
- Request History
- Notifications
- Patient profile
- Settings

### Hospital Dashboard

- Blood requests
- Hospital inventory
- Accepted donations
- Donation completion
- Request management
- Notifications
- Hospital profile
- Settings

### Admin Dashboard

- User management
- Platform activity
- Request monitoring
- Donation monitoring
- Analytics
- Settings

---

# 🔐 Recruiter Demo Accounts

These accounts are provided only for **recruiter/demo testing**.

### Admin

```text
Email: admin@lifelink.com
Password: Admin@12345
```

### Donor

```text
Email: preeti@test.com
Password: Preeti@12345
```

### Patient

```text
Email: rahul.patient@test.com
Password: Rahul@12345
```

### Hospital

```text
Email: apollo@test.com
Password: Apollo@123
```

> **Security:** These must be dedicated demo accounts only. Never publish production credentials, database passwords, JWT secrets, API keys, or other private secrets in a public repository.

---

# 🧭 Recruiter Testing Flow

### Patient

```text
Login
  ↓
Patient Dashboard
  ↓
Create Blood Request
  ↓
View Active Request
  ↓
Track Request Status
  ↓
View Request History
```

### Donor

```text
Login
  ↓
Donor Dashboard
  ↓
View Nearby/Available Requests
  ↓
Open Request Details
  ↓
Accept Donation
  ↓
View Donation History
```

### Hospital

```text
Login
  ↓
Hospital Dashboard
  ↓
View Blood Requests
  ↓
View Accepted Donation
  ↓
Complete Donation
  ↓
Manage Inventory
```

### Admin

```text
Login
  ↓
Admin Dashboard
  ↓
View Users
  ↓
Monitor Requests
  ↓
View Analytics
  ↓
Manage Settings
```

---

# 🧪 Tested End-to-End Workflow

The core workflow has been tested through both the frontend and API:

- Patient request creation
- Hospital request creation
- Request retrieval
- Donor profile verification
- Blood-group matching
- Donor acceptance
- Donation creation
- Request status update
- Patient request status update
- Hospital donation completion
- Donation status update
- Request completion
- Notification creation
- Patient history
- Donor history
- Hospital request/history views
- Production frontend/backend communication
- Local and production CORS configuration

### Example Tested Flow

```text
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

---

# 📸 Screenshots

Add final screenshots under:

```text
docs/
└── screenshots/
    ├── donor-dashboard.png
    ├── nearby-requests.png
    ├── request-details.png
    ├── patient-dashboard.png
    ├── patient-history.png
    ├── notifications.png
    ├── hospital-dashboard.png
    ├── hospital-inventory.png
    └── admin-dashboard.png
```

Example:

```md
![Donor Dashboard](docs/screenshots/donor-dashboard.png)
![Patient Dashboard](docs/screenshots/patient-dashboard.png)
![Hospital Dashboard](docs/screenshots/hospital-dashboard.png)
![Admin Dashboard](docs/screenshots/admin-dashboard.png)
```

---

# 🔒 Security & Authorization

LifeLink uses authenticated API requests with role-based access.

```text
PATIENT
  → Patient profile
  → Patient requests
  → Request history

DONOR
  → Donor profile
  → Nearby requests
  → Accept donation
  → Donation history

HOSPITAL
  → Hospital profile
  → Hospital requests
  → Hospital inventory
  → Complete donation

ADMIN
  → User management
  → Platform monitoring
  → Analytics
```

Sensitive configuration such as database URLs and JWT secrets must be stored in environment variables and must **not** be committed to GitHub.

---

# 📁 Repository Structure

The production project is maintained in two separate repositories:

```text
LifeLink
│
├── LifeLink-Frontend
│   ├── src/
│   │   ├── app/
│   │   ├── features/
│   │   │   ├── donor/
│   │   │   ├── patient/
│   │   │   ├── hospital/
│   │   │   └── admin/
│   │   ├── shared/
│   │   └── providers/
│   └── ...
│
└── LifeLink-Backend
    ├── src/
    │   ├── controllers/
    │   ├── routes/
    │   ├── services/
    │   ├── middleware/
    │   ├── validators/
    │   ├── utils/
    │   └── prisma/
    └── ...
```

### Repositories

**Frontend:**  
https://github.com/Preeti980/LifeLink-Frontend

**Backend:**  
https://github.com/Preeti980/LifeLink-Backend

---

# ⚙️ Environment Variables

### Frontend

Local:

```env
NEXT_PUBLIC_API_URL=http://localhost:5000/api/v1
```

Production:

```env
NEXT_PUBLIC_API_URL=https://lifelink-backend-6yo2.onrender.com/api/v1
```

### Backend

Local:

```env
PORT=5000
DATABASE_URL=your_postgresql_connection_string
JWT_SECRET=your_jwt_secret
CLIENT_URL=http://localhost:3000
```

Production:

```env
CLIENT_URL=https://life-link-frontend-steel.vercel.app
```

> Never commit `.env` or `.env.local` files containing real secrets.

---

# 🚀 Run Locally

### Frontend

```bash
git clone https://github.com/Preeti980/LifeLink-Frontend.git
cd LifeLink-Frontend
npm install
npm run dev
```

Open:

```text
http://localhost:3000
```

### Backend

```bash
git clone https://github.com/Preeti980/LifeLink-Backend.git
cd LifeLink-Backend
npm install
npm run dev
```

The local API runs on:

```text
http://localhost:5000
```

Configure the required environment variables before starting the backend.

---

# 🌐 Production Deployment

### Frontend

**Platform:** Vercel

https://life-link-frontend-steel.vercel.app/

### Backend

**Platform:** Render

https://lifelink-backend-6yo2.onrender.com/

### Production Architecture

```text
Vercel
  │
  │ HTTPS / REST API
  ▼
Render Backend
  │
  ▼
PostgreSQL
```

Production configuration includes:

- Production frontend API URL
- Production CORS origin
- Database environment variables
- JWT secret configuration
- Prisma database connectivity
- Authentication and role-based route protection

---

# 📚 Documentation

For complete technical documentation, architecture, API details, database design, workflows, testing information, and implementation details:

**[DOCUMENTATION.md](DOCUMENTATION.md)**

---

# 🎯 Project Goal

LifeLink was built to demonstrate a complete real-world full-stack workflow where multiple user roles interact with the same system through secure APIs, database transactions, role-based authorization, responsive dashboards, and a complete blood request-to-donation lifecycle.

The goal is to make blood support more transparent, trackable, and organized—from creating a blood request and finding an eligible donor to accepting the donation and completing the request.

---
## 🔮 Upcoming Development

LifeLink is an ongoing project. Planned improvements include:

- AI-powered donor matching
- AI-assisted blood-request prioritization
- Intelligent emergency request detection
- Automated donor notifications
- Smart location-based donor recommendations
- AI-powered hospital inventory insights
- Automated request follow-ups
- Real-time notifications
- Email/SMS alerts
- Advanced analytics
- Hospital verification
- Donation certificates
- AI-powered admin insights
- Automated workflows
- Production monitoring
- Automated testing
- CI/CD improvements
# 👩‍💻 Developer

**Priti Chauhan**

B.Tech — Computer Science & Engineering

Full-Stack Developer

- **GitHub:** https://github.com/Preeti980
- **LinkedIn:** https://www.linkedin.com/in/priti-chauhan-352101256/

---

## ⭐ Project Links

- **Live Application:** https://life-link-frontend-steel.vercel.app/
- **Frontend Source:** https://github.com/Preeti980/LifeLink-Frontend
- **Backend Source:** https://github.com/Preeti980/LifeLink-Backend
- **Backend API:** https://lifelink-backend-6yo2.onrender.com/
