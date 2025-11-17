# LearnR Solution Architecture

## Executive Summary

LearnR is an AI-powered adaptive learning platform built as a web application with React frontend and Python FastAPI backend. The system uses PostgreSQL for relational data, Qdrant for vector embeddings, and integrates OpenAI GPT-4 and Llama 3.1 for intelligent content generation and semantic search. The architecture supports dual user experiences (learner adaptive quizzing and admin course management) with scalability to 10,000+ users and multiple certifications.

## Project Initialization

This project is initialized from scratch with modern web development tools:

### Frontend Initialization

```bash
# Create React app with TypeScript and Vite
npm create vite@latest learnr-frontend -- --template react-ts
cd learnr-frontend
npm install

# Install core dependencies
npm install @mui/material @emotion/react @emotion/styled
npm install react-router-dom @tanstack/react-query axios
npm install recharts date-fns
npm install @mui/icons-material

# Install dev dependencies
npm install -D @types/node vitest @testing-library/react @testing-library/jest-dom
```

### Backend Initialization

```bash
# Create backend directory and virtual environment
mkdir learnr-backend
cd learnr-backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install core dependencies
pip install fastapi[all] uvicorn[standard]
pip install sqlalchemy psycopg2-binary alembic
pip install qdrant-client openai pydantic-settings
pip install python-jose[cryptography] passlib[bcrypt] python-multipart
pip install pytest pytest-asyncio httpx

# Create requirements.txt
pip freeze > requirements.txt
```

## Decision Summary

| Category | Decision | Version | Affects Epics | Rationale |
| -------- | -------- | ------- | ------------- | --------- |
| Frontend Framework | React with TypeScript | React 18.3+, TypeScript 5.3+ | All learner & admin UI | Industry standard, excellent ecosystem, TypeScript ensures type safety |
| Build Tool | Vite | 5.0+ | Frontend development | Faster dev experience than CRA, optimized builds, modern ESM support |
| UI Component Library | Material UI (MUI) | 5.15+ | All UI components | Framer-inspired customization, comprehensive components, accessibility built-in |
| Frontend State | TanStack Query + Context | Query 5.0+, React 18+ | Quiz sessions, progress tracking | Server state management with Query, local UI state with Context |
| Routing | React Router | 6.20+ | Navigation, protected routes | Standard routing solution, supports nested routes and lazy loading |
| Charts | Recharts | 2.10+ | Progress dashboard, admin analytics | React-native, responsive, simple API for data visualization |
| Backend Framework | FastAPI | 0.109+ | All API endpoints | Modern async Python, automatic OpenAPI docs, high performance |
| ORM | SQLAlchemy | 2.0+ | Database operations | Mature ORM, async support, type-safe with Python type hints |
| Database | PostgreSQL | 15+ | User data, responses, courses | ACID compliance, JSON support, excellent for relational data |
| Vector Database | Qdrant | 1.7+ | Question/content embeddings | Open source, high performance, local Docker for MVP, cloud for scale |
| AI/LLM Provider | OpenAI + Local Llama | GPT-4, text-embedding-3-large, Llama 3.1 | Content generation, embeddings, explanations | OpenAI for quality, Llama for cost-efficient volume generation |
| Authentication | JWT + FastAPI Security | python-jose 3.3+ | User auth, admin RBAC | Stateless auth, scalable, integrates with FastAPI security utilities |
| Password Hashing | Passlib with bcrypt | passlib 1.7+ | User authentication | Industry standard, strong security, slow hashing prevents brute force |
| Migrations | Alembic | 1.13+ | Database schema evolution | SQLAlchemy-native, version-controlled schema changes |
| API Client | Axios | 1.6+ | Frontend API calls | Interceptors for auth, request/response transformation, error handling |
| Testing (Frontend) | Vitest + Testing Library | Vitest 1.0+, Testing Library 14+ | Component tests, integration tests | Fast, Vite-native, React Testing Library for user-centric tests |
| Testing (Backend) | Pytest | 7.4+ | API tests, unit tests | Standard Python testing, excellent FastAPI integration |
| Deployment (MVP) | Railway or Render | N/A | Initial deployment | Simple deployment, PostgreSQL included, affordable for MVP validation |
| Deployment (GA) | AWS or GCP | N/A | Production scaling | Full infrastructure control, managed services, auto-scaling |
| Code Style (Frontend) | ESLint + Prettier | ESLint 8+, Prettier 3+ | Code quality | Consistent formatting, catch errors early |
| Code Style (Backend) | Black + Ruff | Black 23+, Ruff 0.1+ | Code quality | Opinionated formatting (Black), fast linting (Ruff) |

## Project Structure

```
learnr_build/
├── learnr-frontend/                 # React TypeScript frontend
│   ├── src/
│   │   ├── components/              # Reusable UI components
│   │   │   ├── common/              # Shared components (buttons, cards, icons)
│   │   │   ├── quiz/                # Quiz-specific components
│   │   │   │   ├── QuestionCard.tsx
│   │   │   │   ├── AnswerOptions.tsx
│   │   │   │   ├── ExplanationPanel.tsx
│   │   │   │   └── ReadingContentCard.tsx
│   │   │   ├── dashboard/           # Dashboard components
│   │   │   │   ├── CompetencyBars.tsx
│   │   │   │   ├── ProgressChart.tsx
│   │   │   │   ├── ReviewsDueCard.tsx
│   │   │   │   └── ExamReadinessCard.tsx
│   │   │   └── admin/               # Admin-specific components
│   │   │       ├── CourseWizard.tsx
│   │   │       ├── MetricKPICard.tsx
│   │   │       ├── AnalyticsDashboard.tsx
│   │   │       └── RevenueChart.tsx
│   │   ├── pages/                   # Page-level components
│   │   │   ├── auth/
│   │   │   │   ├── Login.tsx
│   │   │   │   ├── Signup.tsx
│   │   │   │   └── ResetPassword.tsx
│   │   │   ├── learner/
│   │   │   │   ├── Dashboard.tsx
│   │   │   │   ├── Onboarding.tsx
│   │   │   │   ├── DiagnosticQuiz.tsx
│   │   │   │   ├── QuizSession.tsx
│   │   │   │   ├── KnowledgeAreaDetail.tsx
│   │   │   │   └── Settings.tsx
│   │   │   └── admin/
│   │   │       ├── AdminDashboard.tsx
│   │   │       ├── CourseManagement.tsx
│   │   │       ├── Analytics.tsx
│   │   │       ├── RevenueTracking.tsx
│   │   │       └── ContentEvaluation.tsx
│   │   ├── hooks/                   # Custom React hooks
│   │   │   ├── useAuth.ts
│   │   │   ├── useQuiz.ts
│   │   │   ├── useCompetency.ts
│   │   │   └── useSpacedRepetition.ts
│   │   ├── services/                # API service layer
│   │   │   ├── api.ts               # Axios instance with interceptors
│   │   │   ├── authService.ts
│   │   │   ├── quizService.ts
│   │   │   ├── contentService.ts
│   │   │   └── adminService.ts
│   │   ├── contexts/                # React Context providers
│   │   │   ├── AuthContext.tsx
│   │   │   └── ThemeContext.tsx
│   │   ├── types/                   # TypeScript type definitions
│   │   │   ├── user.ts
│   │   │   ├── quiz.ts
│   │   │   ├── competency.ts
│   │   │   └── admin.ts
│   │   ├── utils/                   # Utility functions
│   │   │   ├── competencyCalculator.ts
│   │   │   ├── spacedRepetition.ts
│   │   │   ├── dateFormatter.ts
│   │   │   └── validators.ts
│   │   ├── theme/                   # MUI theme customization
│   │   │   └── theme.ts
│   │   ├── App.tsx                  # Root component with routing
│   │   ├── main.tsx                 # Entry point
│   │   └── vite-env.d.ts
│   ├── public/
│   ├── package.json
│   ├── tsconfig.json
│   ├── vite.config.ts
│   └── .env.example
│
├── learnr-backend/                  # Python FastAPI backend
│   ├── app/
│   │   ├── main.py                  # FastAPI application entry
│   │   ├── config.py                # Configuration management
│   │   ├── database.py              # Database connection setup
│   │   ├── dependencies.py          # Shared dependencies (auth, db session)
│   │   ├── models/                  # SQLAlchemy ORM models
│   │   │   ├── user.py
│   │   │   ├── course.py
│   │   │   ├── question.py
│   │   │   ├── response.py
│   │   │   ├── competency.py
│   │   │   ├── reading_content.py
│   │   │   └── spaced_repetition.py
│   │   ├── schemas/                 # Pydantic request/response schemas
│   │   │   ├── user.py
│   │   │   ├── auth.py
│   │   │   ├── quiz.py
│   │   │   ├── competency.py
│   │   │   └── admin.py
│   │   ├── routers/                 # API route handlers
│   │   │   ├── auth.py              # POST /auth/login, /auth/signup
│   │   │   ├── users.py             # GET/PUT /users/me, DELETE /users/me
│   │   │   ├── onboarding.py        # POST /onboarding, GET /onboarding
│   │   │   ├── diagnostic.py        # GET /diagnostic/questions, POST /diagnostic/submit
│   │   │   ├── quiz.py              # GET /quiz/next-question, POST /quiz/submit-answer
│   │   │   ├── competency.py        # GET /competency/dashboard
│   │   │   ├── reading.py           # GET /reading/recommendations
│   │   │   ├── spaced_repetition.py # GET /reviews/due, POST /reviews/submit
│   │   │   └── admin/               # Admin-only routes
│   │   │       ├── courses.py       # POST /admin/courses, PUT /admin/courses/{id}
│   │   │       ├── analytics.py     # GET /admin/analytics/platform
│   │   │       ├── revenue.py       # GET /admin/revenue/daily
│   │   │       └── evaluation.py    # GET /admin/evaluations/content
│   │   ├── services/                # Business logic layer
│   │   │   ├── auth_service.py      # Password hashing, JWT generation
│   │   │   ├── competency_service.py # IRT competency estimation
│   │   │   ├── adaptive_service.py  # Adaptive question selection
│   │   │   ├── spaced_repetition_service.py # SM-2 algorithm
│   │   │   ├── vector_service.py    # Qdrant semantic search
│   │   │   ├── llm_service.py       # OpenAI/Llama integration
│   │   │   └── analytics_service.py # Admin analytics calculations
│   │   ├── utils/                   # Utility functions
│   │   │   ├── irt_calculator.py    # Item Response Theory algorithms
│   │   │   ├── sm2_algorithm.py     # Spaced repetition scheduling
│   │   │   └── validators.py
│   │   └── middleware/              # Custom middleware
│   │       ├── cors.py
│   │       ├── rate_limit.py
│   │       └── error_handler.py
│   ├── alembic/                     # Database migrations
│   │   ├── versions/
│   │   └── env.py
│   ├── tests/
│   │   ├── test_auth.py
│   │   ├── test_quiz.py
│   │   ├── test_competency.py
│   │   └── test_admin.py
│   ├── scripts/                     # Data preparation scripts
│   │   ├── parse_babok.py           # Parse and chunk BABOK content
│   │   ├── generate_embeddings.py   # Create embeddings for Qdrant
│   │   ├── generate_questions.py    # LLM question generation
│   │   └── seed_database.py         # Initial data seeding
│   ├── requirements.txt
│   ├── alembic.ini
│   └── .env.example
│
├── docs/                            # Documentation (existing)
│   ├── prd.md
│   ├── ux-design-specification.md
│   ├── user-journey-flows.md
│   ├── admin-user-flows.md
│   ├── ux-patterns-components.md
│   └── architecture.md              # This file
│
└── .gitignore
```

## Epic to Architecture Mapping

| Epic | Primary Components | Key Technologies | Notes |
| ---- | ----------------- | ---------------- | ----- |
| User Authentication | `routers/auth.py`, `AuthContext.tsx`, `pages/auth/` | JWT, bcrypt, FastAPI Security | Stateless JWT auth with 7-day expiration |
| Onboarding Flow | `pages/learner/Onboarding.tsx`, `routers/onboarding.py` | React Router, PostgreSQL | 7-question progressive disclosure form |
| Initial Diagnostic | `pages/learner/DiagnosticQuiz.tsx`, `routers/diagnostic.py` | PostgreSQL | 24 questions (4 per KA), no immediate feedback |
| Adaptive Quiz Engine | `services/adaptive_service.py`, `utils/irt_calculator.py` | IRT algorithm, PostgreSQL | Real-time competency updates, adaptive selection |
| Question Delivery | `components/quiz/`, `routers/quiz.py` | React Query, FastAPI | Immediate feedback, explanation display |
| Reading Content | `components/quiz/ReadingContentCard.tsx`, `services/vector_service.py` | Qdrant, text-embedding-3-large | Semantic search for relevant BABOK chunks |
| Spaced Repetition | `services/spaced_repetition_service.py`, `utils/sm2_algorithm.py` | SM-2 algorithm, PostgreSQL | Review scheduling with 1/3/7/14 day intervals |
| Progress Dashboard | `pages/learner/Dashboard.tsx`, `components/dashboard/` | Recharts, React Query | 6 KA competency bars, weekly trends |
| Admin Course Creation | `pages/admin/CourseManagement.tsx`, `components/admin/CourseWizard.tsx` | React multi-step form, PostgreSQL | 5-step wizard with validation |
| Admin Analytics | `pages/admin/Analytics.tsx`, `routers/admin/analytics.py` | PostgreSQL aggregations, Recharts | DAU, MAU, churn, concurrent users |
| Admin Revenue Tracking | `pages/admin/RevenueTracking.tsx`, `routers/admin/revenue.py` | PostgreSQL, date aggregations | Daily revenue, MRR, ARPU calculations |
| Admin Content Evaluation | `pages/admin/ContentEvaluation.tsx`, `routers/admin/evaluation.py` | PostgreSQL joins, MUI DataGrid | Reading scores, user feedback stream |

## Technology Stack Details

### Core Technologies

**Frontend Stack:**
- **React 18.3+**: Component-based UI, hooks for state management, concurrent rendering
- **TypeScript 5.3+**: Type safety, better IDE support, catch errors at compile time
- **Vite 5.0+**: Fast HMR, optimized builds, modern ESM-first approach
- **Material UI 5.15+**:
  - Customized with Framer-inspired design (pill buttons, hierarchical border radii)
  - Inter font family for modern, professional appearance
  - Vector icons (Material Icons) instead of emojis
  - Custom theme: bright lime green (#CDF348) accent, minimal 3-color palette
- **React Router 6.20+**: Client-side routing, protected routes, lazy loading
- **TanStack Query 5.0+**: Server state caching, automatic refetching, optimistic updates
- **Recharts 2.10+**: Responsive charts for competency bars, progress trends, admin analytics
- **Axios 1.6+**: HTTP client with auth interceptors, request/response transformation

**Backend Stack:**
- **FastAPI 0.109+**: Async Python framework, automatic OpenAPI docs, data validation
- **SQLAlchemy 2.0+**: Modern async ORM, type-safe queries, relationship management
- **PostgreSQL 15+**: Primary database for users, courses, questions, responses, competency tracking
- **Alembic 1.13+**: Database migrations, version-controlled schema evolution
- **Pydantic 2.0+**: Data validation, settings management, automatic schema generation
- **python-jose 3.3+**: JWT token creation and validation
- **passlib 1.7+**: Password hashing with bcrypt, strong security defaults

**AI & Vector Stack:**
- **Qdrant 1.7+**:
  - Vector database for question/reading content embeddings
  - Semantic similarity search for adaptive content delivery
  - Local Docker deployment for MVP, Qdrant Cloud for production
- **OpenAI API**:
  - **GPT-4**: High-quality explanation generation, question variations
  - **text-embedding-3-large**: 3072-dimension embeddings for semantic search
- **Llama 3.1** (local deployment):
  - Cost-efficient question generation at scale
  - Bulk content processing without API rate limits

**Development & Testing:**
- **Frontend**: Vitest, Testing Library, ESLint, Prettier
- **Backend**: Pytest, Black, Ruff, httpx (async test client)
- **CI/CD**: GitHub Actions for automated testing and deployment

### Integration Points

**OpenAI Integration:**
- **Endpoint**: `https://api.openai.com/v1/`
- **Usage**:
  - Embeddings: `POST /embeddings` with `text-embedding-3-large` model
  - Chat completions: `POST /chat/completions` with `gpt-4` for explanations
- **Rate Limits**: 3,500 RPM (tier 1), implement exponential backoff
- **Cost Management**: Cache embeddings, use Llama for bulk generation

**Qdrant Integration:**
- **Local MVP**: Docker container `qdrant/qdrant:v1.7.4`
- **Production**: Qdrant Cloud managed instance
- **Collections**:
  - `questions`: Question embeddings with metadata (KA, difficulty, concept tags)
  - `reading_chunks`: BABOK content embeddings with metadata (KA, section, difficulty)
- **Search Strategy**: Cosine similarity with metadata filtering (KA match, difficulty range)

**Email Service (Password Reset):**
- **MVP**: SMTP with Gmail (for testing)
- **Production**: Resend or SendGrid
- **Templates**: Password reset, welcome email, exam reminder (future)

**Payment Processing (Future):**
- **Provider**: Stripe
- **Integration**: Stripe Checkout for subscriptions, webhooks for payment events
- **Tiers**: Free (limited questions), Pro (full access), Enterprise (team features)

## Novel Pattern Designs

### Pattern 1: Adaptive Learning Loop

**Purpose**: Continuously adapt question difficulty and content recommendations based on real-time competency estimation.

**Components**:
1. **IRT Competency Estimator** (`irt_calculator.py`)
2. **Adaptive Question Selector** (`adaptive_service.py`)
3. **Vector Content Recommender** (`vector_service.py`)
4. **Real-time Dashboard Sync** (React Query auto-refetch)

**Data Flow**:
```
User answers question
    ↓
IRT model updates competency scores (all 6 KAs)
    ↓
Adaptive selector ranks next questions:
  - Prioritize weakest KAs (gap-based)
  - Match difficulty to competency level (±1 level)
  - Exclude recently seen questions (7-day window)
    ↓
Vector search retrieves reading content:
  - Query: Question concept + user weak concepts
  - Filter: Match KA, difficulty <= user competency
  - Return: Top 2-3 chunks by cosine similarity
    ↓
Frontend displays next question + reading recommendations
    ↓
Dashboard auto-refreshes with new competency scores
```

**Implementation Guidance**:
- IRT calculation must complete within 100ms (use simplified 1PL model for MVP)
- Adaptive selection pre-computes candidate pool (top 50 questions) and selects from pool
- Vector search includes metadata filters before similarity calculation (reduce search space)
- React Query refetch on mutation success (answer submission triggers competency refetch)

**Affected Epics**: Adaptive Quiz Engine, Reading Content, Progress Dashboard

---

### Pattern 2: Spaced Repetition Scheduling

**Purpose**: Schedule concept reviews using SM-2 algorithm adapted for 60-day exam preparation timeline.

**Components**:
1. **SM-2 Scheduler** (`sm2_algorithm.py`)
2. **Concept Tracker** (PostgreSQL `concept_mastery` table)
3. **Review Session Builder** (`spaced_repetition_service.py`)
4. **Mixed Session Generator** (`adaptive_service.py`)

**Data Flow**:
```
User answers question correctly
    ↓
Identify associated concepts (from question metadata)
    ↓
SM-2 calculates next review date:
  - First review: +1 day
  - Second review: +3 days
  - Third review: +7 days
  - Fourth+ review: +14 days
  - If incorrect: reset to +1 day
    ↓
Store review schedule in concept_mastery table
    ↓
Daily cron checks for due reviews:
  - Count concepts past review_date
  - Update user's reviews_due count
    ↓
When user starts quiz:
  - If reviews_due > 0: Create mixed session (40% reviews, 60% new)
  - If reviews_due == 0: Create new content session (100% new)
    ↓
Review questions labeled with "Review" badge in UI
```

**Implementation Guidance**:
- Concept granularity: One concept per question (simpler than multi-concept tracking)
- Review priority: Overdue reviews (past due_date) selected first
- Mixed session logic: Round down (e.g., 10 questions = 4 reviews + 6 new)
- Review questions use same adaptive difficulty matching as new questions

**Affected Epics**: Spaced Repetition, Quiz Session Management, Progress Dashboard

---

### Pattern 3: Admin Course Creation Pipeline

**Purpose**: Multi-step wizard for creating courses with textbook upload, knowledge area definition, and calibrated question bank.

**Components**:
1. **5-Step Wizard UI** (`CourseWizard.tsx`)
2. **File Upload Handler** (backend file processing)
3. **Textbook Chunker** (`parse_babok.py` script adapted for generic content)
4. **Embedding Generator** (batch OpenAI API calls)
5. **Question Validator** (calibrated question import with validation)

**Data Flow**:
```
Admin starts course creation
    ↓
Step 1: Basic Info (name, code, description, exam type, audience)
    → Validates uniqueness of course code
    ↓
Step 2: Knowledge Areas (dynamic builder)
    → Validates weights sum to 100%
    → Auto-generates KA codes from names
    ↓
Step 3: Textbook Upload
    → Accepts PDF/DOCX/TXT/EPUB (max 50MB)
    → OR URL input for online content
    → Chunking config: semantic vs fixed (500-2000 tokens, default 1000)
    → Overlap config: 0-200 tokens (default 50)
    ↓
Step 4: Calibrated Questions
    → Bulk upload: CSV/JSON/XLSX with template
    → Manual entry: Modal form per question
    → Validation: Minimum 20 questions per KA
    ↓
Step 5: Review & Create
    → Expandable accordions show all configuration
    → "Create Course" triggers async processing:
        - Upload textbook to S3/storage
        - Parse and chunk content (background job)
        - Generate embeddings (batch API calls)
        - Store chunks in Qdrant
        - Store questions in PostgreSQL
        - Associate all with course_id
    → Processing estimate: "10-15 minutes, email when ready"
    → Admin redirected to course list with "Processing" status
```

**Implementation Guidance**:
- Use background job queue (Celery or FastAPI BackgroundTasks) for processing
- Store wizard state in browser localStorage (persist across page refresh)
- File upload uses pre-signed S3 URLs (secure, client-side upload)
- Chunking happens server-side with progress updates via WebSocket or polling
- Email notification when course processing completes

**Affected Epics**: Admin Course Creation, Reading Content Management

---

### Pattern 4: Real-Time Analytics Aggregation

**Purpose**: Calculate platform metrics (DAU, MAU, churn, concurrent users) efficiently for admin dashboard.

**Components**:
1. **Analytics Service** (`analytics_service.py`)
2. **Materialized Views** (PostgreSQL for pre-aggregated metrics)
3. **Concurrent User Tracker** (Redis with TTL)
4. **Metric Cache** (Redis with 10-minute TTL)

**Data Flow**:
```
Admin views analytics dashboard
    ↓
Frontend requests: GET /admin/analytics/platform
    ↓
Backend checks Redis cache (key: "platform_metrics", TTL: 10 min)
    ↓
If cache miss:
    → Query PostgreSQL materialized views:
        - mv_daily_active_users (refreshed hourly)
        - mv_monthly_active_users (refreshed daily)
        - mv_user_churn (refreshed daily)
    → Query Redis for concurrent_users:
        - ZCOUNT active_users {now-5min} {now}
        - (sorted set with user_id + timestamp)
    → Calculate trends (vs yesterday, vs last month)
    → Store in Redis cache
    ↓
Return metrics with sparkline data (last 7 days for DAU, last 6 months for MAU)
    ↓
Frontend displays KPI cards with trends and charts
```

**Implementation Guidance**:
- Materialized views refresh on schedule (hourly/daily) via cron or background job
- Concurrent users tracked via middleware: On each API request, ZADD user_id + timestamp
- Redis sorted set auto-expires old entries (ZREMRANGEBYSCORE)
- Sparkline data pre-aggregated in materialized views (avoid expensive window queries)
- Admin dashboard polls every 30 seconds for updated metrics

**Affected Epics**: Admin Analytics, Admin Revenue Tracking

## Implementation Patterns

These patterns ensure consistent implementation across all development:

### Naming Conventions

**Frontend (TypeScript/React):**
- **Components**: PascalCase (`QuestionCard.tsx`, `CompetencyBars.tsx`)
- **Hooks**: camelCase with `use` prefix (`useAuth.ts`, `useQuiz.ts`)
- **Services**: camelCase with `Service` suffix (`authService.ts`, `quizService.ts`)
- **Types/Interfaces**: PascalCase with `I` prefix for interfaces optional (`User`, `IQuizSession`)
- **Constants**: UPPER_SNAKE_CASE (`API_BASE_URL`, `MAX_QUESTIONS_PER_SESSION`)
- **CSS Classes**: kebab-case for custom classes, follow MUI conventions for overrides

**Backend (Python/FastAPI):**
- **Files**: snake_case (`auth_service.py`, `competency_service.py`)
- **Classes**: PascalCase (`User`, `CompetencyEstimator`)
- **Functions**: snake_case (`calculate_competency`, `get_next_question`)
- **Database Tables**: snake_case plural (`users`, `questions`, `knowledge_areas`)
- **Database Columns**: snake_case (`user_id`, `created_at`, `competency_score`)
- **API Routes**: kebab-case with REST conventions (`/quiz/next-question`, `/admin/analytics`)
- **Environment Variables**: UPPER_SNAKE_CASE (`DATABASE_URL`, `OPENAI_API_KEY`)

**Consistency Rules:**
- TypeScript types mirror backend Pydantic schemas (e.g., `User` interface matches `UserSchema`)
- API route naming: `/resource/action` or `/resource/{id}/action`
- Boolean fields prefixed with `is_`, `has_`, or `should_` (`is_admin`, `has_completed_diagnostic`)

### Code Organization

**Frontend Component Structure:**
```typescript
// components/quiz/QuestionCard.tsx
import { FC } from 'react';
import { Card, Typography, Box } from '@mui/material';
import { Question } from '../../types/quiz';

interface QuestionCardProps {
  question: Question;
  onAnswer: (answerId: string) => void;
  isSubmitted: boolean;
}

export const QuestionCard: FC<QuestionCardProps> = ({
  question,
  onAnswer,
  isSubmitted
}) => {
  // Component logic
  return (
    <Card sx={{ borderRadius: '22px' }}>
      {/* JSX */}
    </Card>
  );
};
```

**Backend Route Structure:**
```python
# routers/quiz.py
from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.ext.asyncio import AsyncSession
from ..dependencies import get_current_user, get_db
from ..schemas.quiz import NextQuestionResponse, SubmitAnswerRequest
from ..services.adaptive_service import AdaptiveQuestionService

router = APIRouter(prefix="/quiz", tags=["quiz"])

@router.get("/next-question", response_model=NextQuestionResponse)
async def get_next_question(
    session_type: str = "mixed",
    current_user: User = Depends(get_current_user),
    db: AsyncSession = Depends(get_db)
):
    """Get next adaptive question based on user competency."""
    service = AdaptiveQuestionService(db)
    question = await service.select_next_question(
        user_id=current_user.id,
        session_type=session_type
    )
    return question
```

**Service Layer Pattern:**
```python
# services/adaptive_service.py
from sqlalchemy.ext.asyncio import AsyncSession
from typing import Optional
from ..models.question import Question
from ..models.user import User
from .competency_service import CompetencyService

class AdaptiveQuestionService:
    def __init__(self, db: AsyncSession):
        self.db = db
        self.competency_service = CompetencyService(db)

    async def select_next_question(
        self,
        user_id: int,
        session_type: str = "mixed"
    ) -> Optional[Question]:
        # Business logic for adaptive selection
        competency = await self.competency_service.get_user_competency(user_id)
        # ... selection logic
        return selected_question
```

**Consistency Rules:**
- **Frontend**: Pages → Components → Hooks → Services → Utils (dependency direction)
- **Backend**: Routers → Services → Models (routers never directly query database)
- **Shared Logic**: Extract to services (not in routers or components)
- **Single Responsibility**: One component/service per file, clear purpose

### Error Handling

**Frontend Error Handling:**
```typescript
// services/quizService.ts
import axios from 'axios';
import { handleApiError } from '../utils/errorHandler';

export const submitAnswer = async (questionId: string, answerId: string) => {
  try {
    const response = await api.post('/quiz/submit-answer', {
      question_id: questionId,
      answer_id: answerId
    });
    return response.data;
  } catch (error) {
    throw handleApiError(error); // Centralized error transformation
  }
};

// In component
const { mutate, isError, error } = useMutation({
  mutationFn: submitAnswer,
  onError: (error) => {
    toast.error(error.message); // User-friendly error display
  }
});
```

**Backend Error Handling:**
```python
# routers/quiz.py
from fastapi import HTTPException, status

@router.post("/submit-answer")
async def submit_answer(
    request: SubmitAnswerRequest,
    current_user: User = Depends(get_current_user),
    db: AsyncSession = Depends(get_db)
):
    try:
        service = QuizService(db)
        result = await service.submit_answer(
            user_id=current_user.id,
            question_id=request.question_id,
            answer_id=request.answer_id
        )
        return result
    except ValueError as e:
        # Business logic errors
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail=str(e)
        )
    except Exception as e:
        # Unexpected errors - log and return generic message
        logger.error(f"Error submitting answer: {e}", exc_info=True)
        raise HTTPException(
            status_code=status.HTTP_500_INTERNAL_SERVER_ERROR,
            detail="An error occurred processing your answer"
        )
```

**Consistency Rules:**
- **User-Facing Errors**: Clear, actionable messages (never expose stack traces)
- **Error Logging**: Include context (user_id, request_id, timestamp)
- **HTTP Status Codes**:
  - 400: Client error (validation failure, business rule violation)
  - 401: Unauthenticated
  - 403: Unauthorized (authenticated but insufficient permissions)
  - 404: Resource not found
  - 500: Server error (unexpected exceptions)
- **Retry Logic**: Exponential backoff for transient failures (network, rate limits)
- **Validation**: Validate at API boundary (Pydantic schemas), service layer re-validates critical business rules

### Logging Strategy

**Frontend Logging:**
```typescript
// utils/logger.ts
export const logger = {
  info: (message: string, meta?: Record<string, any>) => {
    console.log(`[INFO] ${message}`, meta);
    // In production: Send to analytics service
  },
  error: (message: string, error: Error, meta?: Record<string, any>) => {
    console.error(`[ERROR] ${message}`, error, meta);
    // In production: Send to error tracking (Sentry)
  },
  performance: (metric: string, duration: number) => {
    console.log(`[PERF] ${metric}: ${duration}ms`);
    // In production: Send to performance monitoring
  }
};

// Usage in components
logger.info('Quiz session started', { userId: user.id, sessionType: 'mixed' });
logger.performance('Question load time', Date.now() - startTime);
```

**Backend Logging:**
```python
# utils/logger.py
import logging
import json
from datetime import datetime

def get_logger(name: str):
    logger = logging.getLogger(name)
    handler = logging.StreamHandler()

    # Structured JSON logging for production
    formatter = logging.Formatter(
        json.dumps({
            "timestamp": "%(asctime)s",
            "level": "%(levelname)s",
            "logger": "%(name)s",
            "message": "%(message)s"
        })
    )
    handler.setFormatter(formatter)
    logger.addHandler(handler)
    return logger

# Usage in services
logger = get_logger(__name__)
logger.info(
    "Adaptive question selected",
    extra={
        "user_id": user_id,
        "question_id": question.id,
        "difficulty": question.difficulty,
        "ka": question.knowledge_area
    }
)
```

**Consistency Rules:**
- **Log Levels**:
  - DEBUG: Detailed diagnostic info (competency calculations, vector search scores)
  - INFO: Normal operations (quiz session started, answer submitted)
  - WARNING: Recoverable issues (API rate limit hit, using fallback)
  - ERROR: Errors that need attention (database errors, external API failures)
  - CRITICAL: System-level failures (database unreachable, Qdrant down)
- **Structured Logging**: JSON format in production for easy parsing
- **PII Protection**: Never log passwords, tokens, or sensitive user data
- **Request IDs**: Include unique request_id in all logs for request tracing
- **Performance Logging**: Log slow queries (>1s), API calls (>2s)

## Data Architecture

### Core Database Schema (PostgreSQL)

**Users Table:**
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    hashed_password VARCHAR(255) NOT NULL,
    full_name VARCHAR(255),
    is_admin BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW(),
    last_login TIMESTAMP,
    is_active BOOLEAN DEFAULT TRUE
);

CREATE INDEX idx_users_email ON users(email);
```

**Courses Table:**
```sql
CREATE TABLE courses (
    id SERIAL PRIMARY KEY,
    course_code VARCHAR(50) UNIQUE NOT NULL,
    course_name VARCHAR(255) NOT NULL,
    description TEXT,
    exam_type VARCHAR(100),
    target_audience TEXT[],
    is_active BOOLEAN DEFAULT TRUE,
    created_by INTEGER REFERENCES users(id),
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

**Knowledge Areas Table:**
```sql
CREATE TABLE knowledge_areas (
    id SERIAL PRIMARY KEY,
    course_id INTEGER REFERENCES courses(id) ON DELETE CASCADE,
    ka_code VARCHAR(50) NOT NULL,
    ka_name VARCHAR(255) NOT NULL,
    description TEXT,
    weight_percentage DECIMAL(5,2) NOT NULL, -- Sum must equal 100 per course
    display_order INTEGER,
    created_at TIMESTAMP DEFAULT NOW(),
    UNIQUE(course_id, ka_code)
);

CREATE INDEX idx_ka_course ON knowledge_areas(course_id);
```

**Questions Table:**
```sql
CREATE TABLE questions (
    id SERIAL PRIMARY KEY,
    course_id INTEGER REFERENCES courses(id),
    knowledge_area_id INTEGER REFERENCES knowledge_areas(id),
    question_text TEXT NOT NULL,
    option_a TEXT NOT NULL,
    option_b TEXT NOT NULL,
    option_c TEXT NOT NULL,
    option_d TEXT NOT NULL,
    correct_answer CHAR(1) NOT NULL CHECK (correct_answer IN ('A','B','C','D')),
    explanation TEXT NOT NULL,
    difficulty_level INTEGER CHECK (difficulty_level BETWEEN 1 AND 5),
    concept_tags TEXT[],
    source VARCHAR(50), -- 'vendor' or 'llm'
    embedding_id VARCHAR(255), -- Qdrant vector ID
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_questions_course ON questions(course_id);
CREATE INDEX idx_questions_ka ON questions(knowledge_area_id);
CREATE INDEX idx_questions_difficulty ON questions(difficulty_level);
```

**Reading Content Table:**
```sql
CREATE TABLE reading_content (
    id SERIAL PRIMARY KEY,
    course_id INTEGER REFERENCES courses(id),
    knowledge_area_id INTEGER REFERENCES knowledge_areas(id),
    section_reference VARCHAR(100), -- e.g., "BABOK v3 - 3.2.1"
    content_text TEXT NOT NULL,
    chunk_index INTEGER, -- Order within section
    difficulty_level INTEGER CHECK (difficulty_level BETWEEN 1 AND 5),
    concept_tags TEXT[],
    embedding_id VARCHAR(255), -- Qdrant vector ID
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_reading_course ON reading_content(course_id);
CREATE INDEX idx_reading_ka ON reading_content(knowledge_area_id);
```

**User Responses Table:**
```sql
CREATE TABLE user_responses (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    question_id INTEGER REFERENCES questions(id),
    selected_answer CHAR(1) NOT NULL CHECK (selected_answer IN ('A','B','C','D')),
    is_correct BOOLEAN NOT NULL,
    time_taken_seconds INTEGER,
    session_type VARCHAR(50), -- 'diagnostic', 'mixed', 'new', 'ka_focused'
    is_review BOOLEAN DEFAULT FALSE,
    answered_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_responses_user ON user_responses(user_id);
CREATE INDEX idx_responses_question ON user_responses(question_id);
CREATE INDEX idx_responses_date ON user_responses(answered_at);
```

**Competency Tracking Table:**
```sql
CREATE TABLE user_competency (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    knowledge_area_id INTEGER REFERENCES knowledge_areas(id),
    competency_score DECIMAL(5,2) NOT NULL, -- 0-100 scale
    questions_answered INTEGER DEFAULT 0,
    last_updated TIMESTAMP DEFAULT NOW(),
    UNIQUE(user_id, knowledge_area_id)
);

CREATE INDEX idx_competency_user ON user_competency(user_id);
```

**Spaced Repetition Table:**
```sql
CREATE TABLE concept_mastery (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    concept_tag VARCHAR(255) NOT NULL,
    knowledge_area_id INTEGER REFERENCES knowledge_areas(id),
    repetition_count INTEGER DEFAULT 0,
    easiness_factor DECIMAL(3,2) DEFAULT 2.5, -- SM-2 easiness
    interval_days INTEGER DEFAULT 1,
    next_review_date DATE NOT NULL,
    last_reviewed TIMESTAMP,
    created_at TIMESTAMP DEFAULT NOW(),
    UNIQUE(user_id, concept_tag)
);

CREATE INDEX idx_mastery_user ON concept_mastery(user_id);
CREATE INDEX idx_mastery_review_date ON concept_mastery(next_review_date);
```

**User Onboarding Table:**
```sql
CREATE TABLE user_onboarding (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id) UNIQUE,
    referral_source VARCHAR(100),
    exam_date DATE,
    current_knowledge_level VARCHAR(50),
    target_score INTEGER,
    daily_study_minutes INTEGER,
    has_completed_diagnostic BOOLEAN DEFAULT FALSE,
    completed_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT NOW()
);
```

**Subscriptions Table (Future):**
```sql
CREATE TABLE subscriptions (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    tier VARCHAR(50) NOT NULL, -- 'free', 'pro', 'enterprise'
    status VARCHAR(50) DEFAULT 'active', -- 'active', 'cancelled', 'expired'
    stripe_subscription_id VARCHAR(255),
    current_period_start DATE,
    current_period_end DATE,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

### Qdrant Collections

**Questions Collection:**
```python
collection_name = "questions"
vector_config = {
    "size": 3072,  # text-embedding-3-large dimension
    "distance": "Cosine"
}
payload_schema = {
    "question_id": int,
    "course_id": int,
    "ka_id": int,
    "difficulty": int,
    "concept_tags": [str],
    "source": str
}
```

**Reading Content Collection:**
```python
collection_name = "reading_chunks"
vector_config = {
    "size": 3072,
    "distance": "Cosine"
}
payload_schema = {
    "content_id": int,
    "course_id": int,
    "ka_id": int,
    "section_ref": str,
    "difficulty": int,
    "concept_tags": [str]
}
```

### Data Relationships

- **One-to-Many**: Course → Knowledge Areas → Questions/Reading Content
- **One-to-Many**: User → Responses, Competency, Concept Mastery
- **Many-to-One**: Questions → Knowledge Area → Course
- **Vector Linking**: PostgreSQL records store `embedding_id` that maps to Qdrant vector points

## API Contracts

### Authentication Endpoints

**POST /auth/signup**
```typescript
Request: {
  email: string;
  password: string;
  full_name: string;
}
Response: {
  access_token: string;
  token_type: "bearer";
  user: {
    id: number;
    email: string;
    full_name: string;
    is_admin: boolean;
  }
}
```

**POST /auth/login**
```typescript
Request: {
  email: string;
  password: string;
}
Response: {
  access_token: string;
  token_type: "bearer";
  user: { id, email, full_name, is_admin }
}
```

### Quiz Endpoints

**GET /quiz/next-question**
```typescript
Query Params: {
  session_type?: "mixed" | "new" | "ka_focused";
  knowledge_area_id?: number;
}
Headers: {
  Authorization: "Bearer {token}"
}
Response: {
  question: {
    id: number;
    question_text: string;
    options: {
      a: string;
      b: string;
      c: string;
      d: string;
    };
    knowledge_area: string;
    is_review: boolean;
  };
  session_metadata: {
    questions_in_session: number;
    current_index: number;
  }
}
```

**POST /quiz/submit-answer**
```typescript
Request: {
  question_id: number;
  selected_answer: "A" | "B" | "C" | "D";
  time_taken_seconds: number;
}
Response: {
  is_correct: boolean;
  correct_answer: "A" | "B" | "C" | "D";
  explanation: string;
  reading_recommendations: [
    {
      id: number;
      section_reference: string;
      content_text: string;
      difficulty: number;
    }
  ];
  updated_competency: {
    knowledge_area_id: number;
    new_score: number;
    delta: number;
  }
}
```

### Competency Endpoints

**GET /competency/dashboard**
```typescript
Response: {
  knowledge_areas: [
    {
      id: number;
      name: string;
      competency_score: number;
      target_score: number;
      gap: number;
      questions_answered: number;
      last_updated: string;
    }
  ];
  exam_readiness: number;
  reviews_due: number;
  total_questions_answered: number;
  reading_chunks_viewed: number;
  weekly_progress: [
    { week: string; avg_competency: number }
  ];
}
```

### Admin Endpoints

**POST /admin/courses**
```typescript
Request: {
  course_code: string;
  course_name: string;
  description: string;
  exam_type: string;
  target_audience: string[];
  knowledge_areas: [
    {
      ka_code: string;
      ka_name: string;
      weight_percentage: number;
      description?: string;
    }
  ];
  textbook_config: {
    source_type: "upload" | "url";
    file_url?: string; // S3 presigned URL after upload
    url?: string;
    chunking_strategy: "semantic" | "fixed";
    chunk_size?: number;
    overlap?: number;
  };
  questions: [
    {
      ka_code: string;
      question_text: string;
      options: { a, b, c, d };
      correct_answer: "A" | "B" | "C" | "D";
      explanation: string;
      difficulty: number;
      concept_tags: string[];
    }
  ];
}
Response: {
  course_id: number;
  status: "processing";
  estimated_completion: string;
}
```

**GET /admin/analytics/platform**
```typescript
Response: {
  metrics: {
    dau: { value: number; trend: number; sparkline: number[] };
    mau: { value: number; trend: number; sparkline: number[] };
    concurrent_users: { value: number };
    churn_rate: { value: number; trend: number };
  };
  user_segmentation: {
    paid_users: number;
    free_users: number;
    paid_percentage: number;
  };
}
```

**GET /admin/revenue/daily**
```typescript
Query Params: {
  start_date?: string; // ISO date
  end_date?: string;
}
Response: {
  revenue_today: number;
  revenue_month: number;
  mrr: number;
  arpu: number;
  transactions: [
    {
      date: string;
      user_id: number;
      amount: number;
      tier: string;
      status: string;
    }
  ];
}
```

## Security Architecture

### Authentication Flow

1. **User Registration**:
   - Password validated (min 8 chars, complexity requirements)
   - Hashed with bcrypt (cost factor 12)
   - Stored in `users.hashed_password`

2. **Login**:
   - Email lookup (case-insensitive)
   - Password verification with bcrypt
   - JWT token generated (7-day expiration)
   - Token includes: `user_id`, `email`, `is_admin`, `exp`

3. **Token Validation**:
   - Every protected endpoint extracts JWT from `Authorization: Bearer {token}` header
   - Verify signature with secret key
   - Check expiration
   - Load user from database (ensure still active)

### Authorization

**Role-Based Access Control (RBAC):**
- **Learner Role**: Default for all users (`is_admin = false`)
  - Access: Quiz endpoints, dashboard, reading content, profile
  - Restrictions: Cannot access `/admin/*` routes
- **Admin Role**: Elevated permissions (`is_admin = true`)
  - Access: All learner routes + admin routes
  - Admin-only routes: Course management, analytics, revenue, user management

**Dependency Implementation:**
```python
async def get_current_admin(
    current_user: User = Depends(get_current_user)
) -> User:
    if not current_user.is_admin:
        raise HTTPException(status_code=403, detail="Admin access required")
    return current_user
```

### Data Protection

**Encryption in Transit:**
- HTTPS/TLS 1.3 for all connections
- Enforce HTTPS redirect in production

**Encryption at Rest:**
- Passwords: bcrypt hashed (never stored plaintext)
- Database: Managed PostgreSQL with encryption enabled
- File uploads: S3 server-side encryption (SSE-S3)

**Input Validation:**
- Pydantic schemas validate all request bodies
- SQL injection prevention: SQLAlchemy ORM (parameterized queries)
- XSS prevention: React auto-escapes JSX, Content-Security-Policy header
- CSRF protection: SameSite cookies, CORS configuration

**API Security:**
- Rate limiting: 100 requests/minute per user (auth endpoints: 10/minute)
- Request size limits: 10MB max (file uploads: 50MB)
- CORS: Whitelist frontend origin only

**Secrets Management:**
- Environment variables for all secrets (never in code)
- `.env` files excluded from git (`.gitignore`)
- Production secrets in Railway/Render secret management

## Performance Considerations

### Frontend Optimizations

**Code Splitting:**
- Route-based splitting with React.lazy
- Admin routes loaded only for admin users
- Heavy components (charts, modals) lazy loaded

**Bundle Optimization:**
- Vite tree-shaking removes unused code
- Target bundle size: <500KB gzipped
- Critical CSS inlined, fonts preloaded

**Caching Strategy:**
- Service worker caches static assets (fonts, icons)
- TanStack Query caches API responses (stale-while-revalidate)
- Cache invalidation on mutation success

**Image Optimization:**
- SVG for icons (scalable, small size)
- Lazy load images below fold
- Responsive images with srcset

### Backend Optimizations

**Database Query Optimization:**
- Indexes on foreign keys and filter columns
- Eager loading for common joins (user → competency → KA)
- Pagination for list endpoints (limit 50 per page)
- Connection pooling (SQLAlchemy pool size: 20)

**API Response Optimization:**
- Compress responses with gzip
- Selective field loading (only return needed fields)
- Batch endpoints (e.g., fetch all KA competencies in one request)

**Caching Layer (Redis - Future):**
- Cache dashboard data (TTL: 5 minutes)
- Cache question candidates for adaptive selection (TTL: 1 minute)
- Cache user competency scores (invalidate on answer submission)

**Async Operations:**
- FastAPI async request handlers
- Async database queries with SQLAlchemy
- Background tasks for non-critical operations (analytics updates)

### Vector Search Optimization

**Qdrant Performance:**
- Use metadata filters before vector search (reduce search space)
- Pre-filter by course_id, ka_id, difficulty range
- Limit results (top 10 for questions, top 3 for reading)
- Use quantization for faster search (scalar quantization)

**Embedding Strategy:**
- Pre-compute all embeddings (no real-time generation)
- Batch embed new questions (100 per API call)
- Cache embeddings in PostgreSQL `embedding_id` reference

## Deployment Architecture

### MVP Deployment (Railway/Render)

**Infrastructure:**
- **Frontend**: Deployed to Vercel or Netlify
  - Auto-deploy from `main` branch
  - Edge CDN for global distribution
  - Environment variables for API URL
- **Backend**: Railway or Render web service
  - Python 3.11+ runtime
  - Auto-deploy from `main` branch
  - Health check endpoint: `/health`
- **PostgreSQL**: Railway/Render managed PostgreSQL
  - Daily automatic backups
  - Point-in-time recovery (7 days)
- **Qdrant**: Docker container on same server
  - Persistent volume for data
  - Manual backups before major changes

**Configuration:**
```bash
# Frontend .env
VITE_API_BASE_URL=https://api.learnr.com

# Backend .env
DATABASE_URL=postgresql+asyncpg://user:pass@host:5432/learnr
QDRANT_URL=http://localhost:6333
OPENAI_API_KEY=sk-...
JWT_SECRET_KEY=randomly-generated-secret
CORS_ORIGINS=https://learnr.com
```

### Production Deployment (AWS/GCP - Future)

**Infrastructure:**
- **Frontend**: AWS S3 + CloudFront or Vercel
- **Backend**: ECS Fargate or Cloud Run (containerized FastAPI)
- **PostgreSQL**: AWS RDS or Cloud SQL (Multi-AZ, read replicas)
- **Qdrant**: Qdrant Cloud (managed service)
- **Redis**: ElastiCache or Memorystore
- **Load Balancer**: ALB or Cloud Load Balancing

**Scaling Strategy:**
- Auto-scaling: 2-10 backend instances based on CPU
- Database read replicas for analytics queries
- CDN caching for static assets and public content

## Development Environment

### Prerequisites

**Required Software:**
- Node.js 18+ and npm 9+
- Python 3.11+
- PostgreSQL 15+ (or Docker)
- Docker and Docker Compose (for Qdrant)
- Git

**Recommended Tools:**
- VS Code with extensions:
  - ESLint, Prettier, TypeScript
  - Python, Pylance, Black Formatter
- Postman or Insomnia (API testing)
- pgAdmin or TablePlus (database GUI)

### Setup Commands

```bash
# Clone repository
git clone https://github.com/yourusername/learnr_build.git
cd learnr_build

# Frontend setup
cd learnr-frontend
npm install
cp .env.example .env
# Edit .env with local backend URL (http://localhost:8000)
npm run dev
# Frontend runs on http://localhost:5173

# Backend setup (in new terminal)
cd learnr-backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env
# Edit .env with local database and Qdrant URLs

# Start PostgreSQL (Docker)
docker run -d \
  --name learnr-postgres \
  -e POSTGRES_PASSWORD=postgres \
  -e POSTGRES_DB=learnr \
  -p 5432:5432 \
  postgres:15

# Start Qdrant (Docker)
docker run -d \
  --name learnr-qdrant \
  -p 6333:6333 \
  -v $(pwd)/qdrant_storage:/qdrant/storage \
  qdrant/qdrant:v1.7.4

# Run database migrations
alembic upgrade head

# Seed initial data (optional)
python scripts/seed_database.py

# Start backend
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
# Backend runs on http://localhost:8000
# API docs at http://localhost:8000/docs
```

**Content Preparation (One-time):**
```bash
# Parse BABOK content into chunks
python scripts/parse_babok.py \
  --input data/BABOK_v3.pdf \
  --output data/babok_chunks.json \
  --chunk-size 1000 \
  --overlap 50

# Generate embeddings for questions and chunks
python scripts/generate_embeddings.py \
  --questions data/questions.json \
  --chunks data/babok_chunks.json \
  --output data/embeddings.json

# Upload to Qdrant
python scripts/upload_to_qdrant.py \
  --embeddings data/embeddings.json \
  --url http://localhost:6333
```

## Architecture Decision Records (ADRs)

### ADR-001: Use Qdrant for Vector Database

**Context**: Need vector database for semantic search of questions and reading content.

**Decision**: Use Qdrant over alternatives (Pinecone, Weaviate, pgvector).

**Rationale**:
- Open source (no vendor lock-in, local development)
- High performance (Rust-based, optimized for semantic search)
- Docker deployment for MVP (simple setup)
- Qdrant Cloud for production (managed scaling)
- Excellent metadata filtering (filter before vector search)

**Consequences**:
- Positive: Cost-effective for MVP, easy local development
- Positive: Strong filtering capabilities reduce irrelevant results
- Negative: Smaller community than Pinecone
- Mitigation: Qdrant Cloud provides enterprise support if needed

---

### ADR-002: Simplified IRT for Competency Estimation

**Context**: Need competency estimation algorithm for adaptive question selection.

**Decision**: Use simplified 1-parameter logistic (1PL) IRT model instead of full 3PL.

**Rationale**:
- 1PL requires only difficulty calibration (easier to implement)
- 3PL requires discrimination and guessing parameters (needs extensive calibration data)
- MVP timeline constraints (30 days)
- 1PL accuracy sufficient for adaptive targeting (research supports this)

**Consequences**:
- Positive: Faster implementation, requires less calibration
- Positive: Real-time calculation feasible (<100ms)
- Negative: Slightly less precise than 3PL
- Mitigation: Monitor accuracy with user data, upgrade to 3PL post-MVP if needed

---

### ADR-003: SM-2 Algorithm for Spaced Repetition

**Context**: Need spaced repetition scheduling for concept reviews.

**Decision**: Use SM-2 algorithm with adapted intervals for 60-day timeline.

**Rationale**:
- SM-2 is proven effective (used by Anki, SuperMemo)
- Simple to implement (easiness factor, interval calculation)
- Adapts to user performance (correct = longer interval, incorrect = reset)
- Can customize intervals for exam deadline (1/3/7/14 days vs. default exponential)

**Consequences**:
- Positive: Battle-tested algorithm, scientific backing
- Positive: Balances retention and efficiency
- Negative: Intervals may need tuning per certification
- Mitigation: Make intervals configurable, A/B test if needed

---

### ADR-004: React with Material UI Instead of Custom Design System

**Context**: Need UI component library for frontend development.

**Decision**: Use Material UI with Framer-inspired customization instead of building custom design system.

**Rationale**:
- Time constraints (30-day MVP)
- MUI provides comprehensive components (buttons, cards, forms, charts)
- Built-in accessibility (WCAG 2.1 Level AA compatible)
- Customizable theming (can achieve Framer-inspired design)
- Strong TypeScript support

**Consequences**:
- Positive: Faster development, accessibility out-of-box
- Positive: Consistent component behavior and styling
- Negative: Larger bundle size than custom components
- Mitigation: Tree-shaking removes unused components, code splitting for large pages

---

### ADR-005: FastAPI Instead of Django for Backend

**Context**: Need Python backend framework for REST API.

**Decision**: Use FastAPI instead of Django or Flask.

**Rationale**:
- Modern async support (better concurrency for I/O-bound operations)
- Automatic OpenAPI documentation (interactive API docs at `/docs`)
- Pydantic integration (request/response validation, type safety)
- High performance (comparable to Node.js, faster than Flask/Django)
- Lightweight (no ORM/admin forced, use what we need)

**Consequences**:
- Positive: Better performance, modern Python features
- Positive: Auto-generated API docs reduce manual documentation
- Negative: Smaller ecosystem than Django
- Mitigation: Use SQLAlchemy (mature ORM), well-established libraries

---

### ADR-006: Dual LLM Strategy (OpenAI + Local Llama)

**Context**: Need LLMs for content generation and embeddings.

**Decision**: Use OpenAI GPT-4 for quality, local Llama 3.1 for volume.

**Rationale**:
- OpenAI provides highest quality (explanations, calibration)
- Llama 3.1 reduces cost for bulk generation (question variations)
- text-embedding-3-large for embeddings (best semantic search performance)
- Local Llama has no rate limits or per-token costs

**Consequences**:
- Positive: Cost optimization (use expensive API only when needed)
- Positive: Local Llama provides backup if OpenAI has outages
- Negative: Complexity of running local Llama
- Mitigation: Use Llama only for non-critical generation, validate quality

---

### ADR-007: TanStack Query Instead of Redux for State Management

**Context**: Need frontend state management solution.

**Decision**: Use TanStack Query for server state, React Context for UI state.

**Rationale**:
- Server state (API data) is majority of application state
- TanStack Query handles caching, refetching, optimistic updates automatically
- React Context sufficient for simple UI state (theme, sidebar open/closed)
- Avoids Redux boilerplate and complexity

**Consequences**:
- Positive: Less code, automatic cache management
- Positive: Better developer experience (declarative data fetching)
- Negative: Learning curve for developers used to Redux
- Mitigation: Comprehensive documentation, clear patterns in codebase

---

_Generated on 2025-11-17 for LearnR Platform_
_BMAD Method - Solution Architecture Workflow v1.3_
