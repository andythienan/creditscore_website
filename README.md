# Credit Scoring System

<div align="center">

![Version](https://img.shields.io/badge/version-0.1.0-blue.svg?cacheSeconds=2592000)
![Next.js](https://img.shields.io/badge/Next.js-15-black?logo=next.js)
![React](https://img.shields.io/badge/React-19-149eca?logo=react)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178c6?logo=typescript)
![TailwindCSS](https://img.shields.io/badge/TailwindCSS-4-38b2ac?logo=tailwindcss)

**Intelligent Credit Scoring System** - Modern web application with microservices architecture

[📖 Overview](#-overview) • [🏗️ Architecture](#️-architecture) • [🚀 Installation](#-installation) • [📚 Documentation](#-documentation)

</div>

---

## 📖 Overview

### Introduction

**Credit Scoring System** is a comprehensive credit assessment platform designed to help users understand and improve their credit scores. The system utilizes machine learning algorithms and data analysis to evaluate creditworthiness based on multiple factors.

### Objectives

- **Accurate Credit Assessment**: Use ML models to calculate credit scores based on user information
- **Detailed Analysis**: Provide insights into factors affecting credit scores
- **Future Prediction**: Simulate "What-If" scenarios to predict future credit scores
- **Excellent User Experience**: Modern, responsive, and user-friendly interface

### Target Users

- **Individual Users**: Want to track and improve their credit scores
- **Financial Institutions**: Need credit risk assessment tools
- **Analysts**: Research and analyze credit trends

---

## 🏗️ Architecture

### System Architecture Overview

The system is built using a **microservices architecture**, with frontend and backend separated, communicating via RESTful API.

```
┌─────────────────────────────────────────────────────────────┐
│                        Frontend Layer                         │
│  ┌──────────────────────────────────────────────────────┐   │
│  │         Next.js 15 Application (This Repo)           │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌───────────┐ │   │
│  │  │   Landing    │  │   Dashboard  │  │  Survey   │ │   │
│  │  │    Page      │  │   Pages      │  │   Wizard  │ │   │
│  │  └──────────────┘  └──────────────┘  └───────────┘ │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                            │
                            │ HTTP/REST API
                            │ (Axios Client)
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                      Backend Services Layer                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │    Auth      │  │    Score     │  │   Survey     │     │
│  │   Service    │  │   Service    │  │   Service    │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
│  ┌──────────────┐  ┌──────────────┐                         │
│  │   Profile    │  │    Alerts    │                         │
│  │   Service    │  │   Service    │                         │
│  └──────────────┘  └──────────────┘                         │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
                    ┌──────────────┐
                    │   Database   │
                    │   (External) │
                    └──────────────┘
```

### Core Components

#### 1. Frontend Application (This Repository)

**Technology**: Next.js 15, React 19, TypeScript

**Features**:
- **Landing Page**: Homepage with introduction, login/signup
- **Survey System**: Collect user information via multi-step wizard
- **Dashboard Pages**: 
  - Credit Score Overview
  - Credit Factor Analysis
  - What-If Scenario Simulator
  - Profile Management
  - Settings
  - Alert Management
  - Help & Support

**Characteristics**:
- Server-Side Rendering (SSR) and Client-Side Rendering (CSR) hybrid
- Responsive design with Tailwind CSS
- Real-time data visualization with Recharts
- Form handling with React Hook Form
- State management with React Context and Redux Toolkit

#### 2. Backend Services

##### Auth Service
- **Function**: User authentication and authorization
- **Endpoints**:
  - `POST /api/v1/auth/login` - User login
  - `POST /api/v1/auth/signup` - User registration
  - `POST /api/v1/auth/verify-token` - Token verification
  - `POST /api/v1/auth/forgot-password` - Password recovery
  - `POST /api/v1/auth/reset-password` - Password reset
- **Authentication**: JWT (JSON Web Token)

##### Score Service
- **Function**: Calculate and manage credit scores
- **Endpoints**:
  - `POST /api/v1/scores/{userId}` - Calculate score from survey answers
  - `GET /api/v1/scores/{userId}` - Get current score
  - `GET /api/v1/scores/{userId}/history` - Score history
  - `GET /api/v1/scores/{userId}/factors` - Factor analysis
  - `POST /api/v1/scores/{userId}/simulate` - Simulate score
  - `POST /api/v1/scores/{userId}/simulate/projection` - Time-based projection
- **Input**: Survey answers (age, income, credit usage, payment history, etc.)
- **Output**: Credit score (300-850), category, factors breakdown, recommendations

##### Survey Service
- **Function**: Manage survey questions and answers
- **Endpoints**:
  - `GET /api/v1/survey/questions` - Get question list
  - `POST /api/v1/survey/submit` - Submit answers
- **Schema**: Survey questions with options, user answers mapping

##### Profile Service
- **Function**: Manage user profile information
- **Endpoints**:
  - `GET /api/v1/profile/me` - Get profile
  - `POST /api/v1/profile/me` - Create profile
  - `PUT /api/v1/profile/me` - Update profile
  - `GET /api/v1/preferences/me` - Get preferences
  - `PUT /api/v1/preferences/me` - Update preferences
- **Data**: Full name, email, phone, avatar, date of birth, address

##### Alerts Service
- **Function**: Manage alerts and notifications
- **Endpoints**:
  - `GET /api/v1/alerts/{userId}` - Get alert list
  - `POST /api/v1/alerts/{alertId}/read` - Mark as read
  - `POST /api/v1/alerts/on-score-updated` - Create alert on score change
- **Types**: Payment reminders, utilization warnings, score changes

### Data Flow

#### 1. Registration and Survey Flow

```
User Registration
    │
    ▼
[Auth Service] → JWT Token
    │
    ▼
[Survey Wizard] → Collect Answers
    │
    ├─→ [Survey Service] → Save Answers
    │
    └─→ [Score Service] → Calculate Score
            │
            ├─→ Store Score in Database
            ├─→ Generate Factors Analysis
            └─→ Create Alerts (if needed)
                │
                ▼
        [Dashboard] → Display Results
```

#### 2. What-If Simulation Flow

```
User Inputs Scenario Parameters
    │
    ▼
[Frontend] → Validate & Format
    │
    ▼
[Score Service] → Simulate Score
    │
    ├─→ Calculate Projected Score
    ├─→ Generate Confidence Interval
    ├─→ Analyze Factor Impacts
    └─→ Create Monthly Projection
        │
        ▼
[Frontend] → Visualize Results
    │
    ├─→ Line/Area Chart
    ├─→ Results Panel
    └─→ Factor Impact Cards
```

#### 3. Dashboard Update Flow

```
User Opens Dashboard
    │
    ▼
[Frontend] → Fetch Data (Parallel)
    │
    ├─→ [Score Service] → Current Score
    ├─→ [Score Service] → Score History
    ├─→ [Score Service] → Factors Analysis
    ├─→ [Profile Service] → User Profile
    └─→ [Alerts Service] → Recent Alerts
        │
        ▼
[Frontend] → Aggregate & Display
    │
    ├─→ Score Gauge
    ├─→ Trend Chart
    ├─→ Factor Breakdown
    └─→ Alert Feed
```

### Frontend Architecture

#### Component Architecture

```
src/
├── components/           # Reusable UI components
│   ├── ui/              # Primitive components (Button, Input, etc.)
│   ├── common/          # Shared components
│   ├── layouts/         # Layout wrappers
│   └── survey/          # Survey-specific components
│
├── pages/               # Next.js pages (routes)
│   ├── dashboard/       # Dashboard pages
│   │   └── [dashboard-name]/
│   │       ├── components/  # Page-specific components
│   │       └── index.page.tsx
│   ├── api/            # API routes (mock & real)
│   └── survey.page.tsx
│
├── services/            # API service layer
│   ├── auth.service.ts
│   ├── survey.service.ts
│   ├── profile.service.ts
│   └── alerts.service.ts
│
├── lib/                 # Utilities & helpers
│   ├── apiClient.ts    # Axios instance with interceptors
│   └── mockApi.ts      # Mock API wrapper
│
├── contexts/           # React Context providers
│   ├── SurveyContext.tsx
│   └── ThemeLanguageProvider.tsx
│
└── configs/            # Configuration files
    ├── sectionsConfig.ts
    └── surveyConfig.ts
```

#### State Management

- **Local State**: React `useState`, `useReducer` for component-level state
- **Context API**: `SurveyContext` for survey state, `ThemeLanguageProvider` for theme
- **Redux Toolkit**: Ready for global state when needed (currently unused)
- **Server State**: Axios with interceptors, can integrate React Query later

#### API Communication

```typescript
// src/lib/apiClient.ts
- Base URL from environment variable
- Request interceptor: Automatically attach JWT token
- Response interceptor: Global error handling
- Timeout: 15 seconds
- CORS: withCredentials: true
```

#### Mock API System

During development, the system uses mock API:
- **Location**: `src/pages/api/mock/*`
- **Wrapper**: `src/lib/mockApi.ts`
- **Storage**: localStorage for scenarios
- **Purpose**: Develop frontend independently, without backend dependency

---

## 🛠 Technology Stack

### Frontend Stack

| Technology | Version | Reason |
|-----------|---------|--------|
| **Next.js** | 15 | SSR/CSR hybrid, automatic routing, API routes, built-in optimization |
| **React** | 19 | Powerful UI library, large ecosystem, component-based |
| **TypeScript** | 5 | Type safety, better DX, catch errors early |
| **Tailwind CSS** | 4 | Utility-first, rapid development, consistent design |
| **Recharts** | 2.15 | Flexible charting, React-friendly, good performance |
| **React Hook Form** | 7.62 | Performance, validation, minimal re-renders |
| **Axios** | 1.8 | HTTP client with interceptors, request/response handling |
| **Framer Motion** | 12.23 | Smooth animations, declarative API |

### Design System

- **Primary Color**: Neon Green `#00FF88` - Creates emphasis, easily recognizable
- **Typography**: Inter (sans-serif) - Readable, modern
- **Components**: Consistent, reusable, accessible
- **Responsive**: Mobile-first approach

---

## 🚀 Installation

### Requirements

- **Node.js**: v20.x or higher
- **npm**: v9.x or higher
- **OS**: Windows, macOS, or Linux

### Step 1: Clone repository

```bash
git clone <repository-url>
cd creditscore_website
```

### Step 2: Install dependencies

```bash
npm install
```

### Step 3: Configure environment

Create `.env.local` file:

```env
# Backend API Base URL
NEXT_PUBLIC_API_BASE_URL=https://score-service.onrender.com

# Profile Service Base URL (optional)
NEXT_PUBLIC_PROFILE_API_BASE_URL=https://profile-service-l6e7.onrender.com

# Enable/Disable Mock API (1 = enable, 0 = disable)
NEXT_PUBLIC_USE_MOCK=1
```

### Step 4: Run development server

```bash
npm run dev
```

Access: **http://localhost:3000**

### Step 5: Build for production

```bash
npm run build
npm run start
```

---

## 📚 Documentation

### Scripts

```bash
npm run dev      # Development server
npm run build    # Production build
npm run start    # Production server
npm run lint     # ESLint check
```

### Project Structure

See [Frontend Architecture](#frontend-architecture) above.

### API Integration

#### Connect to Real Backend

1. **Update `.env.local`**:
```env
NEXT_PUBLIC_API_BASE_URL=https://your-api-domain.com
NEXT_PUBLIC_USE_MOCK=0
```

2. **API Client automatically**:
   - Automatically attach JWT token from localStorage
   - Handle CORS
   - Timeout handling

3. **Service Layer**:
   - `auth.service.ts` - Authentication
   - `survey.service.ts` - Survey & Score calculation
   - `profile.service.ts` - User profile
   - `alerts.service.ts` - Alerts management

### Adding New Components

```typescript
// src/components/MyComponent.tsx
import React from 'react';

export default function MyComponent() {
  return <div>My Component</div>;
}

// Usage
import MyComponent from '@/components/MyComponent';
```

### Adding New Dashboard

1. Create folder: `src/pages/dashboard/my-dashboard/`
2. Create `index.page.tsx`:
```typescript
import DashboardShell from '@/components/layouts/DashboardShell';

export default function MyDashboard() {
  return (
    <DashboardShell>
      {/* Your content */}
    </DashboardShell>
  );
}
```

---

## 🔄 Workflow

### 1. User Journey

```
Landing Page
    ↓
[Sign Up / Login]
    ↓
[Survey Wizard] (4 steps)
    ├─ Step 1: Basic Information
    ├─ Step 2: Credit Usage
    ├─ Step 3: Payment History
    └─ Step 4: Financial Psychology
    ↓
[Calculate Score]
    ↓
[Dashboard Overview]
    ├─ View Score
    ├─ Analyze Factors
    ├─ Simulate Scenarios
    └─ Manage Profile
```

### 2. Score Calculation Process

```
Survey Answers
    ↓
[Map to Backend Format]
    ├─ camelCase → snake_case
    ├─ Add defaults for missing fields
    └─ Validate required fields
    ↓
[Score Service]
    ├─ Input: SurveyAnswersIn
    ├─ Process: ML Model
    └─ Output: Score, Category, Factors
    ↓
[Store Results]
    ├─ Database
    ├─ Session Storage (temporary)
    └─ Generate Alerts
```

### 3. What-If Simulation

```
User Inputs Scenario
    ├─ Payment Amount
    ├─ Utilization Change
    ├─ New Accounts
    ├─ Payoff Timeline
    ├─ Credit Limit
    └─ Account Age
    ↓
[Simulate Score]
    ├─ Calculate Impact per Factor
    ├─ Project Monthly Progress
    ├─ Generate Confidence Interval
    └─ Estimate Time to Target
    ↓
[Visualize Results]
    ├─ Line/Area Chart
    ├─ Results Panel
    └─ Factor Impact Cards
```

---

## 🧪 Development

### Mock API

The system includes mock API for independent development:

- **Endpoints**: `src/pages/api/mock/*`
- **Functions**: `src/lib/mockApi.ts`
- **Storage**: localStorage for scenarios

### Testing

Currently no test suite. Can add:
- **Unit Tests**: Jest + React Testing Library
- **E2E Tests**: Playwright or Cypress
- **Integration Tests**: API testing

### Code Style

- **TypeScript**: Strict typing
- **ESLint**: Next.js config
- **Components**: PascalCase
- **Files**: PascalCase for components, camelCase for utilities

---

## 🐛 Troubleshooting

### CORS Errors

**Issue**: Cannot connect to backend

**Solution**:
- Check CORS settings on backend
- Or use proxy in `next.config.ts`

### JWT Token Issues

**Issue**: Token not attached to request

**Solution**:
- Check localStorage for `auth_token`
- Check interceptor in `apiClient.ts`

### Environment Variables

**Issue**: Environment variables not loading

**Solution**:
- Ensure `NEXT_PUBLIC_` prefix
- Restart dev server after editing `.env.local`

---

## 📈 Roadmap

- [ ] Authentication & Role-based access control
- [ ] Server-side PDF/CSV export
- [ ] Unit tests & E2E tests
- [ ] Dark mode support
- [ ] Internationalization (i18n)
- [ ] Real-time notifications (WebSocket)
- [ ] Advanced analytics dashboard
- [ ] Mobile app (React Native)

---

## 🤝 Contributing

We welcome contributions! Please:

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

---

## 📄 License

This project is licensed under the MIT License.

---

## 👤 Author

**Truong Hoang Ngoc Nhi** - [@Lyfee-synr](https://github.com/Lyfee-synr)

---

<div align="center">

**If this project helps you, please give it a ⭐ on GitHub!**

Made with ❤️ using Next.js, React, and TypeScript

</div>
