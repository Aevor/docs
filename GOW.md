# Aevor — Project Brief for Frontend

Bro, here is the complete context of what we are building so you can start working on your part properly.

## Project: Aevor

Aevor is an open-source contributor intelligence platform.

The main idea is:

```
GitHub developer
      ↓
GitHub contribution data
      ↓
Contribution evidence
      ↓
Evidence-backed skill profile
      ↓
Open-source issue understanding
      ↓
Developer ↔ Issue matching
      ↓
Recommended issue
      ↓
Explanation of WHY it was recommended
```

The goal is NOT to make another GitHub clone, CRUD app, chatbot, resume builder, or generic AI project.

We want a serious engineering project that we can defend in interviews.

---

## 1. What the User Does

A developer comes to Aevor.

1. Logs in using GitHub.
2. Aevor identifies the GitHub account.
3. Aevor retrieves the developer's GitHub contribution data.
4. Aevor builds a profile based on actual contribution evidence.
5. Aevor derives skills from that evidence.
6. Aevor looks at suitable open-source issues.
7. Aevor determines what skills/issues are relevant.
8. Aevor ranks issues for the developer.
9. The developer sees recommended issues.
10. The developer can see WHY an issue was recommended.

### Example

Developer has:

- Go PRs
- PostgreSQL work
- REST APIs
- Docker
- authentication work

Aevor should eventually be able to say something like:

> **Recommended issue:**
> "Implement pagination for repository API"
>
> **Why:**
> - Go experience detected
> - REST API experience detected
> - database-backed backend experience detected
> - difficulty is appropriate
> - repository technology overlaps with demonstrated experience

The recommendation should be evidence-backed and explainable.

---

## 2. Important Product Principle

The most important architectural principle:

> **LLM = perception / interpretation**
>
> **Deterministic system = decision / ranking**

Meaning:

AI can help understand messy GitHub content.

For example:

```
GitHub issue:
"Add support for cursor based pagination..."
```

AI may extract:

```json
{
  "skills": ["Go", "REST API", "PostgreSQL"],
  "difficulty": "medium",
  "topics": ["pagination", "API"]
}
```

But the final recommendation score should NOT simply be:

> "LLM says this developer is suitable."

Instead:

```
Developer evidence
+
Skill evidence
+
Repository familiarity
+
Issue requirements
+
Difficulty
=
deterministic recommendation score
```

This makes the system explainable and much easier to defend in an interview.

---

## 3. Overall Architecture

Current V1 architecture:

```
React frontend
       ↓
HTTP API
       ↓
Go backend
       ↓
PostgreSQL
```

Go backend also communicates with:

```
GitHub API
```

Later:

```
Go backend
       ↓
AI adapter
       ↓
Ollama/local model
```

We are intentionally NOT building microservices.

We are using a **modular monolith** for V1.

Do NOT introduce:

- Kubernetes
- Kafka
- Redis
- service mesh
- microservices
- vector database
- complicated distributed infrastructure

...unless there is an actual requirement later.

---

## 4. Your Role

You are primarily responsible for the **FRONTEND**.

You don't need to learn Go for your contribution.

Your main technologies:

- React
- JavaScript
- Vite
- React Router
- CSS
- fetch or Axios
- appropriate React state management only if actually needed

Do NOT introduce unnecessary frontend libraries just because they are popular.

The frontend should communicate ONLY with the Go API.

**Frontend should NEVER:**

- connect directly to PostgreSQL
- contain GitHub client secret
- contain JWT signing secret
- contain GitHub access token
- implement authoritative recommendation logic
- calculate the backend's final recommendation score

---

## 5. Frontend User Flow

The main user flow should eventually be:

```
Landing Page
     ↓
Login with GitHub
     ↓
GitHub OAuth
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
Issue Details
     ↓
Why This Issue?
```

The UI should make this flow obvious.

---

## 6. Screens We Need

Build the frontend around these screens.

### A. Landing / Login

**Purpose:**

Explain what Aevor does and allow GitHub login.

**Content:**

- Aevor logo/name
- short product explanation
- "Continue with GitHub"
- small explanation of what data is used
- clean professional layout

Do NOT make it look like a student dashboard.

---

### B. Dashboard

After login.

**Purpose:**

Give the developer a quick overview.

**Possible sections:**

1. Welcome / developer identity
2. GitHub username/avatar
3. contribution summary
4. top detected skills
5. repositories worked on
6. recommended issues
7. profile completeness / data sync state

**Example layout:**

```
--------------------------------
Aevor Dashboard

Welcome, Sanjeev

GitHub: @sanjeev

Contribution Overview
[ PRs ] [ Repositories ] [ Issues ]

Top Skills
Go       ████████
Postgres ██████
Docker   █████

Recommended Issues
-------------------------------
Issue 1
Why this issue?
[View]
-------------------------------

Recent Contributions
-------------------------------
...
--------------------------------
```

Don't hardcode these values. They will eventually come from API responses.

---

### C. Developer Profile

Show:

- avatar
- username
- display name
- GitHub profile
- contribution summary
- repositories
- pull requests
- other evidence

The profile should feel like an **evidence-based engineering profile**.

Not a social-media profile.

---

### D. Skills / Evidence Page

This is one of the most important pages.

Show:

- Skill
- Evidence
- Confidence / score
- Related repositories
- Related PRs

**Example:**

```
Go
Score: 82

Evidence:
- 6 pull requests
- 3 repositories
- backend API contributions

PostgreSQL
Score: 71

Evidence:
- database-related PRs
- repository technology
- schema/API work
```

**Important:**

Do NOT make this look like arbitrary skill percentages.

The backend will eventually provide evidence.

Frontend's job is to **VISUALIZE** the evidence.

---

### E. Repositories / Contributions

Show repositories the developer has contributed to.

For each repository:

- name
- description
- language
- stars/forks if available
- contribution count
- PR count
- technologies
- link to GitHub

Also show contribution details where appropriate.

Use cards/table depending on what makes the information easiest to understand.

Don't over-design it.

---

### F. Recommended Issues

This is the core product screen.

Show recommended open-source issues.

Each issue card should contain:

- repository
- issue title
- short description
- labels
- difficulty
- required skills
- recommendation score
- reason / match summary
- View Issue button

**Example:**

```
--------------------------------
Recommended for you

Issue:
"Add cursor pagination to API"

Repository:
example/project

Difficulty:
Medium

Required:
Go
REST API
PostgreSQL

Match:
87%

Why recommended:
- Strong Go contribution evidence
- REST API experience
- PostgreSQL experience
- Appropriate difficulty

[View Issue]
--------------------------------
```

The "87%" should eventually come from backend logic.

Don't calculate it in React.

---

### G. Issue Details

When user opens an issue:

Show:

- repository
- issue title
- issue body
- labels
- author
- created date
- GitHub link
- required skills
- difficulty
- recommendation score
- recommendation explanation

Have a clear CTA:

> "View on GitHub"

---

### H. Why This Issue?

This is important because explainability is part of the product.

**Example:**

```
Why Aevor recommended this issue:

✓ You have demonstrated Go experience
✓ You have contributed to REST APIs
✓ You have PostgreSQL experience
✓ You have previously contributed to similar repositories
✓ Issue difficulty matches your current evidence
```

This should not be generic AI-generated marketing text.

Eventually it should be based on structured backend evidence.

---

## 7. Common UI States

Every important screen needs:

**Loading state**

Example: Skeletons / loading indicators.

**Empty state**

Example: "No GitHub contributions have been synchronized yet."

**Error state**

Example: "Unable to load recommendations. Try again."

**Success state**

Normal content.

Do NOT only design the happy path.

This is important for professional frontend engineering.

---

## 8. Frontend Structure

Use a clean structure.

Something approximately like:

```
src/
├── api/
│   ├── client.js
│   ├── auth.js
│   ├── users.js
│   ├── repositories.js
│   ├── skills.js
│   └── recommendations.js
│
├── components/
│   ├── common/
│   ├── layout/
│   ├── dashboard/
│   ├── profile/
│   ├── skills/
│   ├── repositories/
│   └── recommendations/
│
├── pages/
│   ├── Login.jsx
│   ├── Dashboard.jsx
│   ├── Profile.jsx
│   ├── Skills.jsx
│   ├── Repositories.jsx
│   ├── Recommendations.jsx
│   └── IssueDetails.jsx
│
├── hooks/
│
├── context/
│
├── routes/
│
├── styles/
│
└── App.jsx
```

Don't create every file immediately.

Create them as features are implemented.

---

## 9. Routing

Eventually something similar to:

```
/                Landing/Login
/dashboard       Dashboard
/profile         Developer profile
/skills          Skills + evidence
/repositories    Contributions/repositories
/recommendations Recommended issues
/issues/:id      Issue details
```

Use protected routes for authenticated pages.

Unauthenticated users should not access:

- `/dashboard`
- `/profile`
- `/skills`
- `/repositories`
- `/recommendations`

---

## 10. Backend API Contract

The backend is being written separately in Go.

Your frontend should consume HTTP APIs.

**Current implemented APIs include:**

- `GET /health`
- `POST /users`
- `GET /users/:id`
- `GET /users/github/:id`

These are development/temporary APIs.

They are NOT the final authenticated product API.

The authentication work currently being implemented will move us toward:

- `GET /auth/github/login`
- `GET /auth/github/callback`
- `GET /users/me`

The temporary public user creation/query endpoints will eventually be removed.

**DO NOT build the frontend around `POST /users`.**

The real product authentication will be GitHub OAuth.

---

## 11. Authentication

The backend authentication flow is:

```
React
  ↓
GET /auth/github/login
  ↓
GitHub OAuth
  ↓
callback
  ↓
Go backend
  ↓
Aevor JWT
  ↓
authenticated API requests
```

**Important:**

The GitHub access token belongs to the backend.

Frontend must NEVER receive it.

Frontend only works with the Aevor authentication/session mechanism exposed by the backend.

Do not invent a GitHub token flow on the frontend.

---

## 12. Frontend/Backend Responsibility

**Frontend:**

- UI
- routing
- forms
- API calls
- displaying data
- local UI state
- loading states
- error states
- responsive design
- accessibility
- frontend tests

**Backend:**

- authentication
- authorization
- GitHub API
- database
- business logic
- skill calculation
- recommendation calculation
- security
- data validation
- deterministic ranking

**AI:**

- issue interpretation
- structured extraction
- semantic assistance

AI does NOT own the final recommendation decision.

---

## 13. Design Style

We want a professional developer-tool/product feel.

Think:

- GitHub
- Linear
- Vercel
- Modern developer dashboards

**NOT:**

- college project dashboard
- excessive gradients
- giant animations
- 20 different colors
- meaningless charts
- fake statistics
- excessive cards

**Prioritize:**

- information hierarchy
- readability
- spacing
- consistency
- responsive design
- accessibility
- useful interactions

The product should look credible in a portfolio/resume demo.

---

## 14. Wireframe First

Before implementing the complete UI, create low-fidelity wireframes for:

1. Login
2. Dashboard
3. Profile
4. Skills
5. Repositories
6. Recommendations
7. Issue Details
8. Why This Issue

The wireframe should show:

- page layout
- navigation
- major sections
- component hierarchy
- important actions

Do NOT spend days making the wireframe beautiful.

It is for validating information architecture.

---

## 15. API Contract Process

When a page needs backend data:

First define:

```
Endpoint: GET /example
Request:  ...
Response: ...
Errors:   ...
```

Then implement the frontend against that contract.

If an API doesn't exist yet, use mock data temporarily.

Clearly mark mock data.

Do NOT silently invent backend endpoints.

---

## 16. Your First Deliverable

Don't immediately build the whole application.

Start with:

**Phase 1:**

1. Analyze this entire project brief.
2. Inspect the existing frontend repository.
3. Inspect its current structure.
4. Do NOT destroy existing work.
5. Propose the final frontend architecture.
6. Create the wireframes.
7. Define the page/component hierarchy.
8. Define the API contract required by each page.
9. Define reusable components.
10. Define the routing structure.
11. Define the frontend development phases.

Then send me:

- **A.** Current frontend structure
- **B.** Proposed frontend structure
- **C.** Wireframe for every screen
- **D.** User flow
- **E.** Component hierarchy
- **F.** Routing plan
- **G.** API requirements
- **H.** State management plan
- **I.** Loading/error/empty states
- **J.** Testing plan
- **K.** What you think is wrong/unnecessary in our current idea

Do NOT start implementing the entire frontend until we review this.

---

## 17. Important Engineering Rule

Don't just agree with me.

If you think:

- a screen is unnecessary
- a feature is useless
- the UX is confusing
- the architecture is over-engineered
- an API is badly designed
- a component should be different
- something will create technical debt

...tell me.

I want you to contribute engineering decisions, not just write JSX.

---

## 18. What Success Looks Like

Eventually the frontend should allow a developer to go:

```
GitHub Login
    ↓
Dashboard
    ↓
"My skills are based on my actual GitHub evidence"
    ↓
"Here are issues I can realistically contribute to"
    ↓
"This issue is recommended because of these specific pieces
of evidence"
    ↓
"Open issue on GitHub"
```

That is the core Aevor experience.

---

## 19. Important

This is a serious placement project.

Don't optimize for number of files or number of features.

Optimize for:

- correct architecture
- clean code
- good UX
- strong API contracts
- testing
- explainability
- security
- maintainability

We will build it feature-by-feature and integrate continuously.

Now inspect the actual frontend repository and start with the Phase 1 deliverables above.
Do NOT implement the entire application yet.
