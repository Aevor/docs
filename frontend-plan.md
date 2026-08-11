# Aevor Frontend Plan — Phase 1 (Approved)

Status: APPROVED final (2026-08-11).
Owner: Developer 2 ("Bro") — frontend only. No Go required.
Scope: Frontend architecture and phased plan ONLY. No implementation until further notice.

---

## 1. Product / User Flow

Aevor converts GitHub contribution history into evidence-backed skills and recommends open-source issues with explanations.

```
Landing Page
   ↓  [Continue with GitHub]
GET /auth/github/login
   ↓
GitHub OAuth (backend-owned redirect)
   ↓
Backend callback + session (contract owned by backend)
   ↓
Dashboard
   ↓
Developer Profile
   ↓
Skills / Evidence
   ↓
Repositories / Contributions
   ↓
Recommended Issues
   ↓
Issue Details (includes "Why this issue?" panel)
   ↓
View Issue on GitHub (external link)
```

Core principle: **LLM = perception/interpretation; deterministic backend = decision/ranking.**
The frontend visualizes evidence. It never computes authoritative scores.

## 2. Frontend Architecture

Stack (decided):

- React + JavaScript
- Vite
- React Router
- CSS (plain, with design tokens)
- fetch via a thin `client.js` wrapper

No state-management library, no CSS framework, no data-fetching library unless a real requirement emerges.

Hard boundaries (frontend NEVER):

- connects directly to PostgreSQL
- contains GitHub client secret / JWT signing secret / GitHub access token
- implements authoritative recommendation or scoring logic

Frontend communicates ONLY with the Go API.

Proposed structure (grows feature-by-feature, do NOT create all at once):

```
apps/web/
├── index.html
├── package.json
├── vite.config.js
└── src/
    ├── main.jsx
    ├── App.jsx
    ├── api/
    │   ├── client.js
    │   └── auth.js          # initiates GET /auth/github/login
    ├── context/
    │   └── AuthContext.jsx
    ├── routes/
    │   └── ProtectedRoute.jsx
    ├── components/
    │   ├── layout/
    │   ├── common/          # Spinner, Skeleton, EmptyState, ErrorState, Badge, Avatar, StatTile
    │   ├── profile/
    │   ├── skills/
    │   ├── repositories/
    │   └── recommendations/
    ├── pages/
    ├── hooks/
    ├── styles/
    └── __tests__/
```

Do NOT create `users.js`, `repositories.js`, `skills.js`, `recommendations.js` until their backend endpoints exist.

## 3. Routes (Seven, not eight)

| Path | Page | Auth |
|---|---|---|
| `/` | Login | public |
| `/dashboard` | Dashboard | protected |
| `/profile` | Profile | protected |
| `/skills` | Skills | protected |
| `/repositories` | Repositories | protected |
| `/recommendations` | Recommendations | protected |
| `/issues/:id` | IssueDetails | protected |

Correction applied: there is NO separate "Why This Issue?" route. The explanation is a panel inside Issue Details and a summary on each recommendation card.

Protected routes redirect unauthenticated users to `/`.

## 4. Wireframes (low-fidelity)

**1. Landing / Login**
```
┌──────────────────────────────────────────────┐
│ Aevor                                          │
│ Your GitHub history, turned into your next     │
│ open-source contribution.                      │
│                                                │
│   [  Continue with GitHub  ]                   │
│                                                │
│ We use your public contribution data to build  │
│ your skill evidence and match you to issues.   │
└──────────────────────────────────────────────┘
```

**2. Dashboard**
```
┌──────────────────────────────────────────────┐
│ ☰ Aevor          [avatar @sanjeev] [Logout]   │
├──────────┬───────────────────────────────────┤
│ Nav      │  Welcome, Sanjeev                  │
│ Dashboard│  GitHub: @sanjeev                  │
│ Profile  │  [ PRs: 24 ] [ Repos: 9 ] [ Issues: 12 ]
│ Skills   │                                    │
│ Repos    │  Top Skills                        │
│ Recommen-│  Go ████████   PostgreSQL ██████   │
│ dations  │  Docker █████  REST API ███████    │
│          │  Recommended Issues                │
│          │  ┌ Issue card ───────────────┐     │
│          │  │ "Add cursor pagination…"  │     │
│          │  │ example/project           │     │
│          │  │ [View]                   │     │
│          │  └──────────────────────────┘     │
└──────────┴───────────────────────────────────┘
```

**3. Developer Profile**
```
┌──────────────────────────────────────────────┐
│ [avatar] Sanjeev Kumar   @sanjeev             │
│ Member since 2021 · github.com/sanjeev        │
│                                                │
│ Contribution summary                           │
│ [ 24 PRs ] [ 9 repositories ] [ 12 issues ]   │
│                                                │
│ Evidence:                                      │
│ Repository  Language  Contrib  PRs  Tech       │
│ ─────────────────────────────────────────────  │
│ aevor/api   Go        12       8    Gin, GORM │
└──────────────────────────────────────────────┘
```

**4. Skills / Evidence** (evidence is the primary signal)
```
┌──────────────────────────────────────────────┐
│ Skills & Evidence                             │
│                                                │
│ Go — 6 PRs · 3 repositories · backend API work │
│  Repos: aevor/api, prometheus/client_golang    │
│                                                │
│ PostgreSQL — database-related PRs · schema work│
└──────────────────────────────────────────────┘
```

**5. Repositories / Contributions**
```
┌──────────────────────────────────────────────┐
│ Repositories                                   │
│ ┌───────────────┬───────────┬──────┬─────────┐│
│ │ Name          │ Language  │ PRs  │ Tech    ││
│ │ aevor/api     │ Go        │ 8    │ Gin     ││
│ │ sample-ui     │ TypeScript│ 5    │ React   ││
│ └───────────────┴───────────┴──────┴─────────┘│
└──────────────────────────────────────────────┘
```

**6. Recommended Issues**
```
┌──────────────────────────────────────────────┐
│ Recommended for you                            │
│ ┌──────────────────────────────────────────┐ │
│ │ "Add cursor pagination to API"           │ │
│ │ example/project · Medium · Go/REST/Postgres│ │
│ │ Why: Go evidence · REST experience ·     │ │
│ │       difficulty match                    │ │
│ │ [View Issue]                             │ │
│ └──────────────────────────────────────────┘ │
└──────────────────────────────────────────────┘
```

**7. Issue Details**
```
┌──────────────────────────────────────────────┐
│ example/project · Issue #123                   │
│ "Add cursor pagination to API"                │
│ author · created 2026-07-01 · labels: api, help-wanted
│ ─────────────────────────────────────────────  │
│ Body: <issue body>                            │
│ ─────────────────────────────────────────────  │
│ Required: Go, REST API, PostgreSQL · Medium    │
│ [ View on GitHub ]                            │
│ ─────────────────────────────────────────────  │
│ Why Aevor recommended this issue:             │
│ ✓ Demonstrated Go experience (6 PRs)          │
│ ✓ REST API contributions                      │
│ ✓ PostgreSQL experience                       │
│ ✓ Difficulty matches your evidence            │
└──────────────────────────────────────────────┘
```

## 5. Component Hierarchy

```
<App>
  <AuthProvider>
    <BrowserRouter>
      <Routes>
        <Route Login />                     (public)
        <Route element={<ProtectedRoute/>}>
          <AppShell>   (Nav + Outlet)
            Dashboard | Profile | Skills | Repositories | Recommendations | IssueDetails
              └─ shared: Skeleton, EmptyState, ErrorState, Badge, Avatar,
                         StatTile, SkillEvidenceList, IssueCard
```

Reusable components (created as features need them, not upfront):
`IssueCard`, `SkillEvidenceList`, `RepoTable`, `StatTile`, `DataState` (loading/empty/error wrapper).

## 6. API Requirements

The frontend initially initiates ONLY `GET /auth/github/login`. The backend owns the GitHub OAuth redirect and callback (`GET /auth/github/callback`). The frontend does NOT invent any callback endpoint.

| Page | Endpoint | Status |
|---|---|---|
| Login | `GET /auth/github/login` | backend-owned redirect; frontend navigates |
| all authenticated pages | session/`GET /users/me`-style contract | Task 1 (blocked) — use mock until it exists |
| Profile / Skills / Repos | skills, repositories, contributions endpoints | future — mock |
| Recommendations | recommendations endpoint | future — mock |
| Issue Details | issue + explanation | future — mock |

Per-page contract (method, path, request, response, errors) must be defined before coding that page, following the API-contract process.

## 7. State Management

- `AuthContext` holds session state (`user`, session credential, `status`). Credential handling follows the backend/security contract — storage mechanism is NOT decided yet.
- All other state is local component state via a `useApi` hook (loading/error/data).
- No Redux, Zustand, React Query, or other libraries unless a real requirement emerges.

## 8. Loading / Empty / Error States

Every data screen wraps content in `DataState`:

- **Loading:** skeleton blocks for lists/cards; spinner for buttons. No blank flashes.
- **Empty:** "No GitHub contributions have been synchronized yet." with a sensible action (e.g., refresh/relogin).
- **Error:** "Unable to load recommendations. Try again." + retry; 401 → redirect to `/`.
- **Success:** normal content.

Designing only the happy path is unacceptable.

## 9. Testing Strategy

- Unit (Vitest): api client wrapper (mock fetch), AuthContext, ProtectedRoute, pure helpers, IssueCard.
- Component (Testing Library): each page's loading/empty/error/success states.
- Integration (happy path): mock fetch → login → dashboard renders user data.
- No e2e in Phase 1. Tests never require a real backend.

## 10. Frontend / Backend Responsibility Split

Frontend owns: UI, routing, forms, API calls, data display, local UI state, loading/error/empty states, responsive design, accessibility, frontend tests.

Backend owns: authentication, authorization, GitHub API, database, business logic, skill calculation, recommendation calculation, security, data validation, deterministic ranking.

The frontend visualizes backend-provided evidence. It does not calculate scores or decide recommendations.

## 11. Mock-Data Policy

- Mock data is allowed ONLY for temporary visual development.
- Every mock file/value must be clearly marked `MOCK`.
- Mocks are isolated (e.g., `src/api/__mocks__/`) so they can be removed wholesale when the real API exists.
- Never present mock data as real production data. Never silently invent backend endpoints.

## 12. Development Phases

Phase 1 — Foundation: scaffold Vite + React + React Router; `client.js`, `AuthContext`, `ProtectedRoute`; Login page; Dashboard with marked mocks.

Phase 2 — Identity: Dashboard + Profile with real `GET /users/me` once Task 1 lands; shared layout/nav; reusable common components.

Phase 3 — GitHub data presentation: Repositories/Contributions when the sync endpoint exists.

Phase 4 — Skills UI: Skills/Evidence page when the skills endpoint exists.

Phase 5 — Recommendations: Recommendations + Issue Details + "Why this?" panel when endpoints exist.

Phase 6 — Testing, polish, accessibility, integration hardening.

Frontend grows feature-by-feature. Directories/files are created when their feature is implemented, not all upfront.

## 13. Frontend Risks

1. No backend endpoints beyond `GET /auth/github/login` exist yet → long mock-data period. Mitigation: isolated, clearly-marked mocks.
2. Session/credential storage is undecided → frontend must not lock in a storage assumption (no localStorage/sessionStorage/cookie persistence until the backend/security contract decides).
3. Skills page could degrade into unexplained percentage charts → evidence must be the primary signal.
4. Over-scaffolding (all api modules, all components) before endpoints exist → create per feature.
5. Drift between mock shapes and eventual API shapes → define contracts before coding each page.

## 14. Design Principles

- Professional developer-tool/product feel (GitHub, Linear, Vercel).
- Information hierarchy, readability, spacing, consistency, responsive design, accessibility, useful interactions.
- No college-project dashboards, excessive gradients/animations/colors, meaningless charts, fake statistics, or excessive cards.
- Evidence-first presentation. Scores only as secondary, backend-provided signals.
- Correct architecture, clean code, strong contracts, explainability, security, maintainability.
- The product must look credible in a portfolio/resume demo.
