<div align="center">

# LabEasy

[![React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://reactjs.org)
[![Node.js](https://img.shields.io/badge/Node.js-Express-339933?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Prisma%20ORM-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://postgresql.org)
[![Solana](https://img.shields.io/badge/Solana-Blockchain-9945FF?style=for-the-badge&logo=solana&logoColor=white)](https://solana.com)
[![IPFS](https://img.shields.io/badge/IPFS-Decentralized%20Storage-65C2CB?style=for-the-badge&logo=ipfs&logoColor=white)](https://ipfs.tech)
[![Live](https://img.shields.io/badge/Live-labeasy.aadish.tech-22c55e?style=for-the-badge&logo=vercel&logoColor=white)](https://labeasy.aadish.tech)

**A full-stack medical lab aggregator platform connecting diagnostic labs with patients — featuring JWT authentication, a real-time sales dashboard, H-Index health scoring, IPFS-stored medical reports, and Solana blockchain integration.**

[Live Demo](https://labeasy.aadish.tech) · Built at HackCBS Hackathon · Top 15 at HackHaven 2.0

</div>

---

## Overview

Diagnostic labs in tier-2 and tier-3 cities lack online presence, making it hard for patients to compare prices and book tests. **LabEasy** solves this by acting as an aggregator — giving labs a digital storefront and giving patients a single place to discover, compare, and book medical tests.

Beyond basic listings, LabEasy introduces a **Health Index (H-Index)** — a computed health score derived from a patient's past test history — useful for both patients tracking their health trends and insurance companies calculating premiums.

---

## Features

**For Patients**
- Browse and compare 100+ diagnostic tests across labs
- Add tests to cart and book with lab confirmation
- View test results uploaded directly by the lab
- Health Dashboard with computed **H-Index** from past reports
- Secure registration and JWT-authenticated sessions

**For Labs**
- Register lab with license and GST verification
- List tests with custom pricing
- Real-time sales dashboard with revenue analytics (Recharts)
- Upload patient test results via secure drive link

**Platform**
- Medical reports stored on **IPFS** for decentralized, tamper-proof storage
- **Solana blockchain** integration for on-chain patient data records
- Role-based access: separate flows for patients and labs
- Input validation with **Zod** on all API endpoints

---

## Architecture

```
┌─────────────────────────────────────────────────────┐
│                    CLIENT (React)                   │
│                                                     │
│  Pages: Home · Tests · Cart · Results · Dashboard   │
│  State: Recoil atoms/selectors                      │
│  Animations: Framer Motion · GSAP · Three.js (3D)   │
│  Charts: Recharts (sales) · Chart.js (H-Index)      │
└────────────────────┬────────────────────────────────┘
                     │  REST API (Axios)
                     ▼
┌─────────────────────────────────────────────────────┐
│               BACKEND (Express.js)                  │
│                                                     │
│  /api/v1/auth   → User & Lab auth (JWT + bcrypt)    │
│  /api/v1/tests  → Test CRUD, lab test management    │
│  Middleware     → JWT verification, Zod validation  │
└────────────────────┬────────────────────────────────┘
                     │  Prisma ORM
                     ▼
┌─────────────────────────────────────────────────────┐
│              DATABASE (PostgreSQL)                  │
│                                                     │
│  User · Lab · Tests · LabTest · UserTest            │
│  Relational: users ↔ labs ↔ tests (many-to-many)   │
└─────────────────────────────────────────────────────┘
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
   ┌────────────┐       ┌────────────────┐
   │    IPFS    │       │    Solana      │
   │  Medical   │       │  Patient Data  │
   │  Reports   │       │  On-Chain      │
   └────────────┘       └────────────────┘
```

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend framework | React 18 + Vite |
| Styling | Tailwind CSS + Radix UI |
| State management | Recoil (atoms + selectors) |
| Animations | Framer Motion + GSAP |
| 3D graphics | React Three Fiber + Drei |
| Charts | Recharts + Chart.js |
| Backend | Node.js + Express.js |
| Database | PostgreSQL + Prisma ORM |
| Authentication | JWT + bcrypt.js |
| Validation | Zod |
| Decentralized storage | IPFS (ipfs-http-client) |
| Blockchain | Solana Web3.js |

---

## Database Schema

```
User            Lab             Tests
────────        ────────        ────────
id (PK)         id (PK)         id (PK)
name            lab_name        test_name
email (unique)  owner_name      test_description
phone (unique)  phone (unique)
password        email (unique)
                license_no       LabTest (junction)
                gst_no          ────────────────
                address         lab_id + test_id
                                test_price

UserTest (order record)
────────────────────────
user_id · lab_id · test_id
purchase_date · test_price
drive_link (result upload)
```

---

## Getting Started

### Prerequisites

```
Node.js v18+
PostgreSQL 14+
```

### Installation

```bash
# Clone the repo
git clone https://github.com/Madhav082003/LabEasy.git
cd LabEasy
```

**Backend setup:**
```bash
cd backend
npm install

# Configure environment
cp .env.example .env
# Edit .env:
#   DATABASE_URL = "postgresql://user:password@localhost:5432/labeasy"
#   JWT_SECRET = "your-secret-key"
#   PORT = 3000

# Run database migrations
npx prisma migrate dev

# Start server
npm start
# Backend runs on http://localhost:3000
```

**Frontend setup:**
```bash
cd ../frontend
npm install

# Configure environment
cp .env.example .env
# Edit .env:
#   VITE_BACKEND_URL = "http://localhost:3000"

# Start dev server
npm run dev
# Frontend runs on http://localhost:5173
```

---

## API Endpoints

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| POST | `/api/v1/auth/signup` | Patient registration | — |
| POST | `/api/v1/auth/signin` | Patient login → JWT | — |
| POST | `/api/v1/auth/lab/signup` | Lab registration | — |
| POST | `/api/v1/auth/lab/signin` | Lab login → JWT | — |
| GET | `/api/v1/tests` | List all tests | JWT |
| GET | `/api/v1/tests/lab` | Tests by lab | JWT |
| POST | `/api/v1/tests/book` | Book a test | JWT |
| GET | `/api/v1/tests/results` | User's test results | JWT |

---

## Project Structure

```
LabEasy/
├── backend/
│   ├── prisma/
│   │   ├── schema.prisma        # DB models: User, Lab, Tests, UserTest
│   │   └── migrations/
│   ├── src/
│   │   ├── index.js             # Express app entry point
│   │   ├── userauth.js          # Patient signup/signin routes
│   │   ├── labauth.js           # Lab signup/signin routes
│   │   ├── tests.js             # Test browsing & booking
│   │   ├── labtests.js          # Lab test management
│   │   ├── testData.js          # Test data seeding
│   │   ├── validation.js        # Zod schemas
│   │   └── middleware/
│   │       └── auth.js          # JWT verification middleware
│   ├── .env.example
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── pages/
│   │   │   ├── Home.jsx         # Landing page (Three.js 3D)
│   │   │   ├── Tests.jsx        # Test browser with cart
│   │   │   ├── Results.jsx      # Patient test results
│   │   │   ├── labsdashboard.jsx # Lab sales dashboard
│   │   │   ├── SigninPatients.jsx / SignUpPatients.jsx
│   │   │   └── SigninLab.jsx / SignUpLab.jsx
│   │   ├── components/
│   │   │   ├── Cart.jsx         # Cart with test summary
│   │   │   ├── TestCard.jsx     # Individual test listing card
│   │   │   ├── LabDetailsPopup.jsx
│   │   │   ├── navbar.jsx
│   │   │   └── footer.jsx
│   │   ├── store/atoms/         # Recoil state atoms
│   │   ├── services/
│   │   │   └── ipfsUpload.js    # IPFS report storage
│   │   └── solana/
│   │       └── patientDataStore.js  # Solana on-chain records
│   ├── .env.example
│   └── package.json
└── README.md
```

---

## Screenshots

| Home | Tests Browser |
|------|--------------|
| ![Home](https://drive.google.com/uc?id=1WDKnWV0erGIwXTbJdiwRmVUHHtfTnb6_) | ![Tests](https://drive.google.com/uc?id=18LcJXQcHWqEfigVrj7_KZjHcc2FlWPaW) |

| Lab Dashboard | Results |
|---------------|---------|
| ![Dashboard](https://drive.google.com/uc?id=1xcnURi8frBq85BjJvKGk5FE59aP7gqji) | ![Results](https://drive.google.com/uc?id=1LZ-Rwc2zBHDAdHtkGjou2vZ-LtOKltl1) |

---

## Key Design Decisions

**Why Recoil over Redux?** Recoil's atom/selector model maps naturally to the app's shape — cart state, auth state, and lab data are independent atoms, composed by selectors. No boilerplate reducers.

**Why IPFS for reports?** Medical reports are sensitive and permanent. IPFS content-addressing ensures a report URL always points to the same file — no server can alter or delete it after upload.

**Why Prisma?** Type-safe database queries with auto-generated client from the schema. The relational model (User ↔ UserTest ↔ Lab ↔ Tests) is expressed cleanly as Prisma relations.

---

## Future Work

- [ ] Payment gateway integration (Razorpay)
- [ ] SMS/email notifications on test booking
- [ ] AI-powered H-Index scoring using ML model
- [ ] Mobile app (React Native)
- [ ] Lab verification workflow (license validation)

---

## Author

**Yash Sharma**  
B.Tech CSE, VIPS-TC

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/yash-sharma-8a77581b5/)
[![GitHub](https://img.shields.io/badge/GitHub-yash00003-181717?style=flat-square&logo=github)](https://github.com/yash00003)
[![Email](https://img.shields.io/badge/Email-yashsharma3.1.2005%40gmail.com-EA4335?style=flat-square&logo=gmail)](mailto:yashsharma3.1.2005@gmail.com)
---

<div align="center">
  <sub>If you find this useful, please give it a ⭐</sub>
</div>
