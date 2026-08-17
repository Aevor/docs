# Aevor AI — Codebase Intelligence & Open-Source Contribution Assistant

## 1. Purpose

Aevor AI is a dedicated AI microservice within the Aevor ecosystem.

It is not intended to be a generic chatbot or a simple code-generation assistant.

The objective is to build a codebase-aware engineering intelligence system that can understand an entire software repository and help developers throughout the software-development lifecycle.

Aevor AI should understand:

- Repository structure
- Source code
- Functions
- Classes
- Packages
- Services
- APIs
- Database models
- Database schemas
- Configuration
- Dependencies
- Tests
- Documentation
- Git history
- Issues
- Pull requests
- Architecture
- Existing development conventions
- Relationships between different parts of the codebase

The long-term goal is to allow a developer to connect a repository and interact with it naturally.

Examples:

- "Explain this project."
- "Explain the architecture."
- "Where is authentication implemented?"
- "How does this API request flow through the system?"
- "Why is this endpoint returning 401?"
- "Find the code responsible for this behavior."
- "Which files should I modify?"
- "Find me a small issue I can contribute to."
- "Explain this GitHub issue."
- "Implement this issue."
- "What tests should I add?"
- "Review this PR."
- "Explain this review comment."
- "Fix the changes requested by the maintainer."
- "Prepare a PR description for this contribution."
- "Help me make my first open-source contribution."

The core principle is:

> Aevor AI should understand the repository before attempting to generate, modify, or review code.

---

# 2. Aevor Architecture Context

Aevor is being developed as a microservice-based platform.

The repositories are separated by responsibility.

Current conceptual structure:

    Aevor
    ├── platform
    │   └── Core Aevor platform / API / application services
    │
    ├── ai
    │   └── Aevor AI microservice
    │
    ├── infra
    │   └── Infrastructure / deployment / operational components
    │
    └── docs
        └── Product requirements / architecture / technical documentation

The AI repository is a separate microservice.

Do not turn the AI repository into the entire Aevor backend.

The AI service should expose well-defined APIs that can communicate with the main Aevor platform.

The AI implementation should remain modular so that model providers, embedding providers, vector stores, repository analysis components, and AI workflows can evolve independently.

---

# 3. Core Product Vision

Aevor AI should become an engineering intelligence layer over a software repository.

The long-term lifecycle is:

    Repository
        ↓
    Repository Ingestion
        ↓
    Codebase Understanding
        ↓
    Semantic / Structural Indexing
        ↓
    Context Retrieval
        ↓
    AI Reasoning
        ↓
    Developer Assistance
        ↓
    Implementation
        ↓
    Testing
        ↓
    Pull Request
        ↓
    PR Analysis
        ↓
    Human Review
        ↓
    Maintainer Feedback
        ↓
    Fixes
        ↓
    Re-analysis
        ↓
    Merge

---

# 4. Human Control

Aevor AI assists developers.

It does not replace repository maintainers.

The AI must NOT automatically:

- Merge pull requests
- Approve its own pull requests
- Bypass branch protection
- Modify repository permissions
- Delete repositories
- Force push
- Deploy production without authorization
- Silently modify important code
- Automatically submit arbitrary open-source contributions
- Claim an issue is definitely a bug without evidence
- Claim tests passed when they were not executed
- Invent repository files
- Invent APIs
- Invent dependencies
- Invent architectural relationships

The preferred workflow is:

    AI Analysis
        ↓
    AI Recommendation
        ↓
    Developer Review
        ↓
    Developer Decision
        ↓
    Implementation
        ↓
    Testing
        ↓
    Pull Request
        ↓
    Human Review
        ↓
    Maintainer Approval
        ↓
    Merge

Aevor should make developers more capable while keeping final control with humans.

---

# 5. Repository Ingestion

The first major capability is repository ingestion.

Aevor AI should be able to ingest and understand:

- Source code
- Directory structure
- README
- CONTRIBUTING.md
- CODE_OF_CONDUCT.md
- Documentation
- Configuration
- Dockerfiles
- Docker Compose files
- CI/CD configuration
- Dependency manifests
- Test files
- Infrastructure files
- API definitions
- Database schemas
- Migration files
- Relevant Git metadata
- GitHub issues
- GitHub pull requests

The ingestion system should not treat every file equally.

It should identify the purpose and role of each file.

For example:

    Repository
    ├── cmd/
    ├── internal/
    │   ├── auth/
    │   ├── users/
    │   ├── repository/
    │   └── database/
    ├── tests/
    ├── docs/
    ├── Dockerfile
    ├── docker-compose.yml
    ├── go.mod
    └── README.md

Aevor should understand the likely purpose of each component from the actual repository rather than blindly assuming a fixed architecture.

---

# 6. Codebase Indexing

Aevor should break the repository into meaningful units.

Potential units include:

- Repository
- Service
- Package
- Directory
- File
- Class
- Function
- Method
- API endpoint
- Database model
- Configuration
- Test
- Documentation section

Conceptually:

    Repository
        ↓
    Service
        ↓
    Package
        ↓
    File
        ↓
    Symbol
        ├── Function
        ├── Method
        └── Class

This allows the AI to retrieve relevant context rather than sending the entire repository to the model for every request.

---

# 7. Semantic Repository Search

Aevor should support semantic repository search.

A developer should be able to ask:

> "Where is authentication handled?"

even if the exact word "authentication" does not appear in every relevant file.

Search should eventually combine:

- Keyword search
- Semantic search
- Symbol search
- File paths
- Repository structure
- Code relationships
- Metadata
- Git information

Search results should identify:

- Relevant files
- Relevant functions/classes
- Why they are relevant
- Relationships between them

---

# 8. Repository Question Answering

Aevor should support repository-specific Q&A.

Examples:

    "Explain how authentication works."

    "Where is MongoDB initialized?"

    "What happens when a user creates a repository?"

    "Where are permissions checked?"

    "How does this API request flow through the system?"

    "Which service handles this request?"

    "Which files would I modify to add email verification?"

    "Explain the architecture of this repository."

    "Where is this error generated?"

Answers should be grounded in repository evidence.

Whenever possible, answers should reference:

- File paths
- Functions
- Classes
- Services
- Relevant code relationships
- Tests

The AI should distinguish repository facts from assumptions.

---

# 9. Codebase Architecture Understanding

Aevor should eventually understand the architecture of the repository.

For example:

    HTTP Request
        ↓
    API Handler
        ↓
    Service Layer
        ↓
    Repository Layer
        ↓
    Database

For a microservice system:

    Frontend
        ↓
    API Gateway
        ↓
    Auth Service
        ↓
    Repository Service
        ↓
    Database

However, these are examples only.

Aevor must derive architecture from the actual repository.

It should not assume that every project follows the same architecture.

The AI should be able to explain:

- Service boundaries
- Request flow
- Data flow
- Dependencies
- API relationships
- Database relationships
- Important modules
- Critical paths

---

# 10. Codebase-Aware Debugging

Aevor should eventually support repository-wide debugging.

Example:

> "Login is returning 401 after the OAuth callback. Find the problem."

Aevor should investigate:

    Problem
        ↓
    Authentication Flow
        ↓
    OAuth Callback
        ↓
    Session / Token Creation
        ↓
    Middleware
        ↓
    Database Interaction
        ↓
    Relevant Tests
        ↓
    Relevant Git History
        ↓
    Potential Root Cause

The result should explain:

- What was inspected
- What was found
- Relevant files
- Likely cause
- Evidence
- Recommended fix
- Tests to add

The system must distinguish:

    Confirmed
    Likely
    Possible
    Unknown

It must not present speculation as fact.

---

# 11. Implementation Planning

Before generating code, Aevor should be able to produce a repository-specific implementation plan.

Example:

    Feature:
    Password Reset

    Plan:

    1. Inspect existing authentication architecture.
    2. Identify user/account model.
    3. Identify existing email infrastructure.
    4. Add secure reset-token handling.
    5. Add reset-request endpoint.
    6. Add reset-password endpoint.
    7. Add token expiration.
    8. Add validation.
    9. Add tests.
    10. Update documentation if required.

The plan must reference actual repository components when possible.

---

# 12. Codebase-Aware Code Generation

Aevor should generate code according to existing project conventions.

Before generating code it should inspect:

- Naming conventions
- Directory structure
- Existing abstractions
- Error handling
- Dependency usage
- API conventions
- Database patterns
- Testing conventions
- Formatting
- Existing architecture

For example, if a repository follows:

    Handler
        ↓
    Service
        ↓
    Repository

Aevor should extend that architecture instead of introducing a completely unrelated pattern.

The target is:

    Existing Architecture
            +
    Existing Conventions
            +
    Existing Dependencies
            +
    New Requirement
            ↓
    Repository-Compatible Implementation

---

# 13. Patch and Diff Generation

Aevor should eventually be able to propose changes instead of immediately modifying the repository.

Example:

    Files to modify:

    internal/auth/service.go
    internal/auth/middleware.go
    internal/auth/service_test.go

The developer should be able to inspect the proposed diff.

Possible actions:

- Accept
- Reject
- Modify
- Regenerate

The AI should not silently make important changes.

---

# 14. Issue-to-Implementation Workflow

Aevor should support:

    GitHub Issue
        ↓
    Understand Issue
        ↓
    Understand Codebase
        ↓
    Map Issue to Code
        ↓
    Implementation Plan
        ↓
    Proposed Changes
        ↓
    Tests
        ↓
    Diff
        ↓
    Developer Review
        ↓
    Pull Request

Example:

    Issue:
    Add repository stars.

    Potential affected components:

    Backend:
    - Repository model
    - Star relationship
    - Repository service
    - API endpoint

    Frontend:
    - Repository page
    - Star button
    - Star count

    Tests:
    - Star repository
    - Unstar repository
    - Duplicate star prevention

Aevor must verify these components from the actual repository.

---

# 15. Test Generation

Aevor should analyze existing behavior and identify missing test cases.

Example:

    Function:
    AuthenticateUser()

    Potential test cases:

    ✓ Valid credentials
    ✓ Invalid credentials
    ✓ Missing credentials
    ✓ Disabled user
    ✓ Expired token
    ✓ Malformed token
    ✓ Database failure

Aevor can generate proposed tests.

However, it must clearly distinguish:

    Generated Test

from:

    Test Actually Executed

The AI must never claim that tests passed unless actual execution results confirm it.

---

# 16. Pull Request Intelligence

Pull request intelligence is one of the major Aevor AI capabilities.

When a PR is created, Aevor should analyze:

1. PR description
2. PR diff
3. Original issue
4. Relevant repository code
5. Architecture
6. Related tests
7. Git history
8. Dependencies
9. Repository conventions

Aevor should not review only the changed lines.

It should understand how those changes interact with the broader system.

---

# 17. PR Summary

Aevor should generate concise PR summaries.

Example:

    PR Summary

    Purpose:
    Adds repository starring functionality.

    Backend:
    - Added star relationship.
    - Added star/unstar service methods.
    - Added API endpoints.

    Frontend:
    - Added star button.
    - Added star count.

    Tests:
    - Added repository star tests.

The summary should be based on the actual PR.

---

# 18. PR Architectural Impact

Aevor should explain architectural impact.

Example:

    Repository Service
        ↓
    Repository Database Model
        ↓
    Repository API
        ↓
    Frontend Repository Page

The AI should identify:

- Affected services
- Affected modules
- API changes
- Database changes
- Dependency changes
- Potential downstream effects

---

# 19. PR Correctness Analysis

Aevor should analyze whether the implementation appears correct.

Potential findings:

- Incorrect conditions
- Missing error handling
- Incorrect state transitions
- Race conditions
- Null/empty handling
- Incorrect API behavior
- Database consistency problems
- Edge cases
- Broken assumptions
- Regression risks

Every finding should provide evidence.

---

# 20. PR Security Analysis

Aevor should inspect security-sensitive changes.

Important areas include:

- Authentication
- Authorization
- Input validation
- SQL injection
- Command injection
- Secrets
- Tokens
- Permissions
- Session handling
- Sensitive data exposure
- Unsafe file operations
- Dependency vulnerabilities where supported

Example:

    Security Finding

    Severity:
    High

    Confidence:
    High

    Location:
    auth/token.go

    Issue:
    Token expiration is not validated.

    Impact:
    Expired credentials may remain valid.

    Recommendation:
    Validate token expiration before accepting authentication.

The AI should provide evidence and confidence.

---

# 21. PR Test Analysis

Aevor should compare:

    Changed Behavior
        +
    Existing Tests
        +
    New Tests

Example:

    New behavior:
    Refresh token validation

    Existing tests:
    ✓ Valid token
    ✓ Invalid token

    Potentially missing:
    ⚠ Expired token
    ⚠ Revoked token

Aevor should recommend missing coverage rather than automatically assuming the code is broken.

---

# 22. PR Code Quality Analysis

Aevor may identify meaningful issues such as:

- Duplicated logic
- Unnecessary complexity
- Poor error handling
- Inconsistent patterns
- Dead code
- Maintainability problems
- Naming inconsistencies
- Architectural violations

The system should avoid generating noisy review comments.

Aevor should prioritize issues that have meaningful engineering impact.

---

# 23. PR Finding Format

A review finding should ideally contain:

    Finding
    Severity
    Confidence
    File
    Line / Symbol
    Problem
    Why it matters
    Evidence
    Suggested solution
    Suggested test

Example:

    Finding:
    Missing disabled-user validation

    Severity:
    Medium

    Confidence:
    High

    Location:
    auth/service.go

    Problem:
    The new authentication path does not perform the existing
    disabled-user validation.

    Why it matters:
    A disabled user may be able to authenticate through this path.

    Suggested solution:
    Reuse the existing user-status validation.

    Suggested test:
    Add authentication test for disabled users.

---

# 24. Finding-to-Fix Workflow

Aevor should allow developers to continue directly from a finding.

Workflow:

    PR Finding
        ↓
    Explain Finding
        ↓
    Locate Code
        ↓
    Understand Context
        ↓
    Propose Fix
        ↓
    Generate Patch
        ↓
    Generate / Update Tests
        ↓
    Developer Reviews
        ↓
    Apply Change
        ↓
    Re-analyze PR

---

# 25. New Contributor Mode

New contributors are a major target for Aevor AI.

A developer should be able to say:

> "I'm new to this project. Help me contribute."

Aevor should inspect:

- README
- CONTRIBUTING.md
- Code of Conduct
- Documentation
- Architecture
- Open issues
- Pull requests
- Labels
- Issue discussions
- Development setup
- Languages
- Frameworks
- Tests
- CI/CD

Then provide a beginner-friendly explanation.

Example:

    Project:
    Example Repository

    Technology:
    Go
    PostgreSQL
    Docker

    Architecture:
    - API layer
    - Authentication
    - Repository service
    - Database layer

    Recommended first contribution:
    Issue #123

    Difficulty:
    Beginner

    Affected components:
    - internal/repository
    - API handler
    - tests

    Why recommended:
    - Small isolated change
    - Existing implementation
    - Existing tests
    - Low architectural risk

---

# 26. Contribution Difficulty

Aevor should classify potential contributions.

## Beginner

Examples:

- Documentation improvements
- Typo fixes
- Small validation improvements
- Missing tests
- Small error-handling improvements
- Small UI fixes
- Small bug fixes
- Configuration improvements

## Intermediate

Examples:

- API changes
- Database changes
- Service changes
- Non-trivial bug fixes
- Feature extensions
- Integration changes

## Advanced

Examples:

- Architecture changes
- Security-sensitive changes
- Database migrations
- Distributed-system changes
- Major refactoring
- Infrastructure changes
- Performance optimization

Aevor should explain why a contribution received a specific difficulty classification.

It should not rely only on GitHub labels.

---

# 27. Find a Good First Contribution

A developer should be able to ask:

> "Find me a small problem I can contribute to."

Aevor should analyze:

- Open issues
- Labels
- Issue descriptions
- Discussions
- Existing PRs
- Repository architecture
- Affected files
- Code complexity
- Test requirements
- Dependencies
- Contribution guidelines

Then rank opportunities.

Example:

    Recommended Contribution

    Issue:
    #184 — Improve validation error handling

    Difficulty:
    Beginner

    Affected files:
    - internal/user/service.go
    - internal/user/service_test.go

    Why recommended:
    - Isolated change
    - Existing implementation
    - Small number of affected files
    - Existing tests
    - No database migration
    - Low architectural risk

    Prerequisites:
    Basic Go knowledge

    Complexity:
    Low

---

# 28. AI-Discovered Contribution Opportunities

Aevor should eventually identify potential small improvements even when no GitHub issue exists.

Potential sources:

- TODOs
- FIXMEs
- Missing tests
- Documentation gaps
- Inconsistent error handling
- Missing edge-case handling
- Dead code
- Duplicated logic
- Developer experience improvements
- Obvious maintainability improvements
- Potential small performance improvements

Example:

    Potential Contribution

    Location:
    internal/auth/token.go

    Observation:
    The refresh-token path appears to lack explicit expiration
    test coverage.

    Suggested contribution:
    Add tests for expired refresh tokens.

    Difficulty:
    Beginner

    Affected files:
    - internal/auth/token_test.go

    Risk:
    Low

Important:

AI-discovered observations must be presented as:

    Potential issue
    Observation
    Potential improvement

and NOT automatically as:

    Confirmed bug

The contributor must verify the finding.

---

# 29. Issue Understanding

When a contributor selects an issue, Aevor should explain:

## What is the problem?

Explain the issue in simple language.

## Why does it matter?

Explain the user or developer impact.

## Where is the problem?

Identify relevant files, functions, services, and modules.

## How does the current implementation work?

Explain the current code path.

## What needs to change?

Explain expected behavior.

## What should NOT change?

Identify unrelated components.

## What tests are relevant?

Identify existing and missing tests.

---

# 30. Issue-to-Code Mapping

Aevor should connect an issue to actual repository components.

Example:

    Issue:
    Authentication problem
            ↓
    Authentication Service
            ↓
    Login Handler
            ↓
    Token Service
            ↓
    User Repository
            ↓
    Authentication Middleware
            ↓
    Tests

This prevents new contributors from randomly searching through the repository.

---

# 31. Explain Existing Code Before Writing Code

For new contributors, Aevor should explain the existing implementation before suggesting modifications.

Example:

    POST /login
          ↓
    Login Handler
          ↓
    AuthService.Login()
          ↓
    UserRepository.FindByEmail()
          ↓
    PasswordVerifier.Verify()
          ↓
    TokenService.Generate()
          ↓
    HTTP Response

The objective is to make the contributor understand the system before making changes.

---

# 32. Open-Source Contribution Assistant

Aevor should eventually support this complete journey:

    "I want to contribute"
            ↓
    Repository Understanding
            ↓
    Contribution Matching
            ↓
    Issue Selection
            ↓
    Issue Understanding
            ↓
    Codebase Understanding
            ↓
    Implementation
            ↓
    Testing
            ↓
    PR Preparation
            ↓
    PR Creation
            ↓
    AI PR Review
            ↓
    Maintainer Feedback
            ↓
    Feedback Explanation
            ↓
    Fixes
            ↓
    Re-analysis
            ↓
    Final Contribution

This should become one of the flagship capabilities of Aevor AI.

---

# 33. Contribution Matching

Aevor may eventually match developers with suitable contributions.

Possible factors:

- Programming languages
- Frameworks
- Developer skill level
- Issue complexity
- Repository complexity
- Required prerequisites
- Issue activity
- Repository health
- Contribution guidelines
- Test requirements

Conceptually:

    Developer Skills
            +
    Repository Requirements
            +
    Issue Difficulty
            +
    Codebase Complexity
            ↓
    Aevor Matching Engine
            ↓
    Recommended Contributions

Aevor should not encourage meaningless contributions simply to increase GitHub activity.

The goal is meaningful engineering work.

---

# 34. PR Writing Assistant

After implementation, the developer should be able to say:

> "Help me prepare my PR."

Aevor should inspect:

- Original issue
- Changed files
- Diff
- Tests
- Contribution guidelines
- Existing PR conventions
- Documentation

Then generate a professional PR draft.

Example:

    # Fix refresh token expiration validation

    ## Summary

    Adds expiration validation to the refresh-token authentication path.

    ## Problem

    Expired refresh tokens were not consistently rejected.

    ## Changes

    - Added refresh-token expiration validation
    - Added expired-token test coverage
    - Preserved existing authentication behavior

    ## Testing

    - Added unit test for expired refresh tokens
    - Existing authentication tests pass

    ## Related Issue

    Closes #184

The generated PR should follow the repository's actual conventions when available.

---

# 35. PR Readiness Check

Before submitting a PR, Aevor should provide a readiness analysis.

Example:

    Contribution Readiness

    ✓ Repository guidelines reviewed
    ✓ Issue understood
    ✓ Relevant code understood
    ✓ Implementation completed
    ✓ Tests added
    ✓ Tests executed
    ✓ No unrelated changes
    ✓ Documentation checked
    ✓ PR description prepared
    ✓ Issue linked
    ✓ Diff reviewed

    Status:
    READY

If something is missing:

    Contribution Readiness

    Status:
    NOT READY

    Missing:
    - Test for error case
    - Documentation update
    - Unrelated file changes

---

# 36. Maintainer Review Assistance

Aevor should help contributors understand maintainer feedback.

Example maintainer comment:

> "Can you avoid introducing another helper here? We already have one in the utils package."

Aevor should explain:

    The maintainer is asking you to reuse an existing helper
    instead of creating duplicate functionality.

    Existing helper:
    utils/validation.go

    Your implementation:
    auth/validation.go

    Recommended change:
    Reuse utils.ValidateEmail().

The developer can then ask:

> "Fix it."

Aevor can propose the required changes.

---

# 37. Review Response Assistant

Aevor should help contributors prepare professional responses to maintainers.

Example:

    Thanks for pointing this out. You're right that the existing
    validation helper already covers this case. I've updated the
    implementation to reuse it and added the corresponding test.

The developer must review the response before posting it.

Aevor should not automatically post responses without explicit authorization.

---

# 38. Maintainer Feedback → Code Changes

The complete workflow should be:

    Original PR
        ↓
    Maintainer Review
        ↓
    Requested Changes
        ↓
    Aevor Explains Feedback
        ↓
    Developer Requests Fix
        ↓
    Aevor Understands Code
        ↓
    Aevor Proposes Change
        ↓
    Developer Reviews
        ↓
    Code Updated
        ↓
    Tests Updated
        ↓
    Aevor Re-analyzes
        ↓
    Developer Responds

---

# 39. New Contributor Learning Mode

Aevor should not simply solve everything for beginners.

It should explain:

- Why a file matters
- Why a function is used
- Why a particular implementation is preferred
- How the code flows
- How tests work
- Why an issue exists
- How the PR affects architecture
- Why a maintainer requested a change

The intended learning cycle is:

    Contributor
        ↓
    Aevor Explains
        ↓
    Contributor Understands
        ↓
    Contributor Implements
        ↓
    Contributor Learns

The goal is to make developers better engineers rather than making them dependent on AI.

---

# 40. Git History Intelligence

Aevor should eventually understand not only:

> "What does the code look like?"

but also:

> "Why does the code look like this?"

Potential questions:

    "Why was this function changed?"

    "When was this authentication system introduced?"

    "Which PR introduced this database schema?"

    "Why was this dependency added?"

    "Who changed this behavior and why?"

Aevor can combine:

    Current Code
        +
    Commits
        +
    Pull Requests
        +
    Issues
        +
    Documentation

to provide historical context.

---

# 41. Issue → Code → Commit → PR Relationship

Aevor should eventually understand the software development lifecycle.

    Issue
      ↓
    Implementation
      ↓
    Commit
      ↓
    Pull Request
      ↓
    Review
      ↓
    Changes
      ↓
    Merge

This allows Aevor to reason about how software evolves rather than looking only at the current code.

---

# 42. Developer Workspace Assistant

Aevor should eventually support:

> "What should I work on next?"

Potential inputs:

- Open issues
- Existing PRs
- Current repository state
- TODOs
- Roadmap
- Dependencies
- Previous work
- Project priorities

Example:

    Recommended Next Task

    Issue #38 — Add repository permissions.

    Reason:
    - Authentication foundation is complete.
    - Required database model already exists.
    - No current dependency is blocking the work.

This should remain a recommendation.

The developer decides what to work on.

---

# 43. Repository Health Intelligence

A future Aevor capability can analyze repository health.

Potential areas:

- Test coverage
- Stale issues
- Stale PRs
- Dependency health
- Documentation quality
- CI reliability
- Architectural complexity
- Duplicate code
- TODO/FIXME accumulation
- Security concerns
- Contribution friendliness

Example:

    Repository Health

    Documentation:
    Good

    Testing:
    Moderate

    CI:
    Good

    Contribution onboarding:
    Needs improvement

    Potential beginner issues:
    4

    Potential missing tests:
    7

This should help maintainers improve projects.

---

# 44. Aevor AI Long-Term Product Flow

The final long-term experience should resemble:

                        AEVOR AI

                   UNDERSTAND THE CODE
                            ↓
                   UNDERSTAND THE PROJECT
                            ↓
                  UNDERSTAND THE PROBLEM
                            ↓
                     FIND THE CONTEXT
                            ↓
                         EXPLAIN
                            ↓
                          PLAN
                            ↓
                       IMPLEMENT
                            ↓
                         TEST
                            ↓
                        REVIEW
                            ↓
                      IMPROVE
                            ↓
                     PREPARE PR
                            ↓
                  HANDLE FEEDBACK
                            ↓
                      RE-ANALYZE
                            ↓
                  HUMAN APPROVAL
                            ↓
                         MERGE

---

# 45. Engineering Requirements

The AI service should be built as a proper production-oriented microservice.

It should have clear separation between:

    API Layer
        ↓
    Application / Use Cases
        ↓
    Repository Analysis
        ↓
    Retrieval
        ↓
    AI / Model Layer
        ↓
    External Integrations

Avoid putting all AI logic inside one huge file or endpoint.

The implementation should remain modular.

Potential future components:

    Repository Ingestion
    Code Parser
    Code Chunker
    Embedding Service
    Vector Store
    Semantic Search
    Symbol Index
    Git Analyzer
    Issue Analyzer
    PR Analyzer
    Context Builder
    LLM Provider
    Prompt / Agent Layer
    Test Analyzer
    Security Analyzer
    Contribution Engine

These should only become separate components when the implementation actually requires them.

Do not over-engineer the first version.

---

# 46. AI Provider Abstraction

The AI service should not be permanently tied to one model provider.

Design the model layer so that providers can eventually be replaced.

Conceptually:

    Aevor AI
        ↓
    Model Interface
        ↓
    ┌───────────────────┐
    │ Provider A        │
    │ Provider B        │
    │ Local Model       │
    │ Future Provider   │
    └───────────────────┘

The rest of the system should not need to be rewritten when the model provider changes.

---

# 47. Retrieval Architecture

Aevor should eventually use multiple forms of retrieval.

Possible retrieval signals:

- Keyword
- Semantic Similarity
- Symbol Matching
- File Path
- Code Structure
- Dependency Relationship
- Git History
- Issue Relationship
- PR Relationship

The final context builder should combine the most relevant evidence.

Conceptually:

    User Query
        ↓
    Retrieval
        ├── Semantic Search
        ├── Keyword Search
        ├── Symbol Search
        ├── Structural Search
        └── Git / Issue Search
                 ↓
            Context Ranking
                 ↓
            Context Builder
                 ↓
                  LLM

---

# 48. Grounded AI Responses

Aevor must prioritize grounded responses.

The AI should prefer:

    Repository Evidence
            ↓
    Retrieved Context
            ↓
    Reasoning
            ↓
    Answer

rather than:

    Question
        ↓
    Generic Model Knowledge
        ↓
    Guess

When repository evidence is insufficient, Aevor should explicitly say that.

Example:

    I couldn't confirm this from the indexed repository.
    The available code does not provide enough evidence to determine
    the exact cause.

This is preferable to hallucinating an answer.

---

# 49. Confidence

Important AI outputs should have confidence information when appropriate.

Possible levels:

- High Confidence
- Medium Confidence
- Low Confidence
- Insufficient Evidence

Example:

    Likely cause:
    Expired token validation is missing.

    Confidence:
    High

    Evidence:
    - auth/token.go
    - middleware/auth.go
    - existing authentication tests

---

# 50. Security and Privacy

Aevor AI will potentially process source code, repository information, issues, and development data.

Security must therefore be considered from the beginning.

Important requirements:

- Do not expose repository data between users.
- Enforce repository-level authorization.
- Validate access tokens.
- Secure GitHub integrations.
- Avoid logging secrets.
- Avoid storing unnecessary credentials.
- Encrypt sensitive information where required.
- Clearly separate repository contexts.
- Do not send repository content to an external model provider without the required authorization and policy.
- Sanitize logs.
- Protect AI endpoints from abuse.
- Rate-limit expensive operations.

A repository's private code must never become available to another user.

---

# 51. GitHub Integration

Aevor should eventually integrate with GitHub for:

- Repository access
- Issues
- Pull requests
- Commits
- Branches
- Reviews
- Review comments
- Repository metadata

The integration should use least-privilege permissions wherever possible.

The AI should only perform write operations when explicitly authorized.

---

# 52. Important Rule for Git Operations

Aevor AI should not assume it has authority to:

- Push code
- Create branches
- Create PRs
- Comment on issues
- Reply to reviews
- Merge PRs

These operations should require explicit authorization.

A good design is:

    AI Recommendation
           ↓
    User Confirmation
           ↓
    Authorized GitHub Action

For sensitive actions:

    AI proposes
           ↓
    Human confirms
           ↓
    System executes

---

# 53. PR Creation Workflow

Eventually the user should be able to ask:

> "Prepare this contribution as a PR."

Aevor should:

1. Understand the issue.
2. Inspect the repository.
3. Inspect current changes.
4. Run or recommend relevant tests.
5. Review the diff.
6. Generate a PR title.
7. Generate a PR description.
8. Link the issue.
9. Present the final PR content.
10. Ask for explicit authorization before creating the PR.

The user remains responsible for final submission.

---

# 54. PR Review Workflow

For every PR:

    PR Created
        ↓
    Aevor Retrieves:
        ├── Issue
        ├── Diff
        ├── Repository Context
        ├── Architecture
        ├── Tests
        └── History
        ↓
    Analysis
        ├── Correctness
        ├── Security
        ├── Tests
        ├── Architecture
        └── Maintainability
        ↓
    Review Findings
        ↓
    Developer / Maintainer

---

# 55. Review Quality Principle

Aevor should optimize for useful findings rather than maximum findings.

Bad:

    You could rename this variable.
    You could add another comment.
    This line could be formatted differently.

Good:

    This change bypasses the existing authorization check and
    allows a user to access repositories they do not own.

The AI should prioritize:

1. Correctness
2. Security
3. Data integrity
4. Reliability
5. Regression risk
6. Architecture
7. Meaningful maintainability

Style comments should be low priority unless the repository has a strict convention.

---

# 56. Complete Open-Source Contribution Experience

A new contributor should eventually be able to enter Aevor and say:

> "I want to contribute to this repository."

Aevor should then help them through:

    Repository Introduction
            ↓
    Technology Overview
            ↓
    Architecture Explanation
            ↓
    Contribution Guidelines
            ↓
    Available Issues
            ↓
    Recommended Issues
            ↓
    Issue Explanation
            ↓
    Codebase Mapping
            ↓
    Implementation Guidance
            ↓
    Testing Guidance
            ↓
    Diff Review
            ↓
    PR Draft
            ↓
    PR Review
            ↓
    Maintainer Feedback
            ↓
    Feedback Explanation
            ↓
    Fix Assistance
            ↓
    Re-review
            ↓
    Contribution Complete

This is one of the most important long-term product directions.

---

# 57. Definition of Success

Aevor AI should eventually make this possible:

    Enter unfamiliar repository
            ↓
    Understand architecture
            ↓
    Find an appropriate contribution
            ↓
    Understand the issue
            ↓
    Understand relevant code
            ↓
    Implement the solution
            ↓
    Write appropriate tests
            ↓
    Prepare a professional PR
            ↓
    Understand maintainer feedback
            ↓
    Make requested changes
            ↓
    Successfully contribute

The contributor should finish the process with greater understanding of the project and the engineering concepts involved.

---

# 58. Final Product Definition

Aevor AI should NOT be positioned simply as:

> "An AI coding assistant."

A stronger definition is:

> Aevor AI is a codebase-aware engineering intelligence microservice that understands a repository, its architecture, code, history, issues, and pull requests, and uses that context to help developers search, understand, debug, plan, implement, test, review, and contribute to software while keeping final development and merge decisions under human control.

For open-source development:

> Aevor AI helps developers go from being completely new to a repository to making a meaningful, well-understood, tested, and professionally prepared contribution.

---

# 59. Central Principle

Everything in Aevor AI should follow this principle:

    Understand before generating.
    Understand before modifying.
    Understand before reviewing.
    Understand before recommending.

The repository should be treated as the primary source of truth.

The developer remains responsible for decisions.

Maintainers remain responsible for project acceptance.

GitHub branch protection remains responsible for enforcing repository rules.

Aevor AI's responsibility is to provide deep, repository-specific engineering intelligence and help developers move from:

    Problem
        ↓
    Understanding
        ↓
    Planning
        ↓
    Implementation
        ↓
    Testing
        ↓
    Review
        ↓
    Contribution

---

# 60. Development Roadmap

Do not attempt to implement the entire vision at once.

Build the AI service incrementally.

## Phase 1 — AI Foundation

Implement:

- Repository ingestion
- File processing
- Code chunking
- Embeddings
- Semantic indexing
- Retrieval
- Basic repository Q&A

Target:

    Connect Repository
            ↓
    Ask Question
            ↓
    Retrieve Relevant Code
            ↓
    Generate Grounded Answer

---

## Phase 2 — Codebase Intelligence

Implement:

- Symbol extraction
- Code relationships
- Architecture understanding
- Dependency understanding
- Issue-to-code mapping
- Git history understanding

Target:

> Aevor understands how the repository works.

---

## Phase 3 — Engineering Assistant

Implement:

- Debugging
- Root-cause analysis
- Implementation planning
- Code generation
- Test generation
- Patch generation
- Diff analysis

Target:

> Aevor can help developers solve real engineering problems.

---

## Phase 4 — Pull Request Intelligence

Implement:

- PR ingestion
- Diff analysis
- Codebase-aware review
- Correctness analysis
- Security analysis
- Test analysis
- Architecture analysis
- Review findings
- Re-analysis after changes

Target:

> Aevor becomes a codebase-aware PR assistant.

---

## Phase 5 — Open-Source Contribution Assistant

Implement:

- New contributor onboarding
- Good-first-issue discovery
- Contribution difficulty classification
- Small-problem discovery
- Issue explanation
- Issue-to-code mapping
- PR writing
- Contribution readiness
- Maintainer-comment explanation
- Review response assistance

Target:

> Aevor helps a new developer make a meaningful open-source contribution.

---

## Phase 6 — Advanced Engineering Intelligence

Future capabilities:

- Contribution matching
- Repository health analysis
- Advanced Git history reasoning
- Developer workspace recommendations
- Cross-repository intelligence
- Personalized learning assistance
- Advanced architectural reasoning
- Deeper project lifecycle understanding

Target:

> Aevor becomes an engineering intelligence layer over the software-development lifecycle.

---

# 61. AI Team Working Rules

Every AI-service implementation task should maintain a clear TODO.

Each task should define:

    TODO

    Current objective:
    ...

    Files / components involved:
    ...

    Implementation:
    ...

    Tests:
    ...

    Validation:
    ...

    Remaining work:
    ...

The AI team must continuously update the relevant project documentation and `AGENTS.md` / agent instructions when architecture, conventions, workflows, or responsibilities change.

After completing a task:

1. Update the TODO.
2. Mark completed work.
3. Record remaining work.
4. Update relevant architecture documentation.
5. Update agent instructions if required.
6. Run appropriate tests.
7. Document any known limitations.
8. Do not claim completion without verification.

---

# 62. Development Philosophy

Build incrementally.

Prefer:

    Small working capability
            ↓
    Test
            ↓
    Validate
            ↓
    Document
            ↓
    Expand

over:

    Build huge architecture
            ↓
    Hope everything works

Do not over-engineer features before validating the core AI workflow.

Every component should have a clear reason to exist.

Every AI feature should solve an actual developer problem.

Every automated action should have appropriate authorization.

Every AI conclusion should be grounded in evidence whenever repository information is available.

---

# 63. Immediate Implementation Priority

Do not attempt the entire roadmap immediately.

The AI team should begin with the smallest useful end-to-end system:

    Repository
        ↓
    Ingestion
        ↓
    Code Processing
        ↓
    Indexing
        ↓
    Retrieval
        ↓
    Context Building
        ↓
    LLM
        ↓
    Repository Q&A

Once this works reliably, expand toward:

    Repository Q&A
            ↓
    Codebase Understanding
            ↓
    Issue Understanding
            ↓
    Implementation Planning
            ↓
    Code Assistance
            ↓
    Testing
            ↓
    PR Intelligence
            ↓
    Open-Source Contribution Assistant

Do not build advanced autonomous workflows before the repository-understanding foundation is reliable.

---

# 64. Ultimate Aevor AI Goal

The ultimate goal is not to create an AI that merely writes code.

The goal is to create an AI that understands software engineering context deeply enough to help a developer throughout the entire development lifecycle.

    AEVOR AI

    UNDERSTAND
        ↓
    EXPLAIN
        ↓
    SEARCH
        ↓
    DEBUG
        ↓
    PLAN
        ↓
    IMPLEMENT
        ↓
    TEST
        ↓
    REVIEW
        ↓
    PREPARE PR
        ↓
    UNDERSTAND FEEDBACK
        ↓
    FIX ISSUES
        ↓
    RE-ANALYZE
        ↓
    HUMAN APPROVAL
        ↓
    MERGE

Aevor AI should become the intelligence layer that connects:

    Developer
        ↕
    Codebase
        ↕
    Issues
        ↕
    Git History
        ↕
    Pull Requests
        ↕
    Tests
        ↕
    Architecture
        ↕
    Open-Source Contribution

The final product should make software development more understandable, more efficient, and more accessible without removing human ownership and engineering judgment.

---

# 65. Final Acceptance Criteria for the AI Team

The AI repository should not be considered complete merely because an LLM API responds successfully.

The team should progressively demonstrate:

- A repository can be ingested.
- Repository files can be indexed.
- Relevant code can be retrieved.
- Questions can be answered using repository context.
- The system can identify relevant files and symbols.
- The system can explain repository architecture.
- The system can understand GitHub issues.
- The system can map issues to code.
- The system can produce implementation plans.
- The system can generate repository-compatible code.
- The system can generate or recommend tests.
- The system can analyze diffs.
- The system can review PRs using repository context.
- The system can identify meaningful correctness/security risks.
- The system can explain findings.
- The system can re-analyze after changes.
- The system can help a new contributor find a suitable issue.
- The system can explain an issue to a beginner.
- The system can guide a contributor through implementation.
- The system can help prepare a PR.
- The system can explain maintainer feedback.
- The system can help prepare fixes.
- The system keeps humans in control of repository write operations.

---

# 66. Final Product Standard

Aevor AI should be judged by one question:

> Can a developer enter an unfamiliar repository, understand what matters, identify a meaningful problem, implement a correct solution, test it, prepare a high-quality PR, understand maintainer feedback, improve the implementation, and successfully contribute — with Aevor helping at every stage?

If the answer eventually becomes yes, Aevor AI has achieved its core mission.