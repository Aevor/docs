# Aevor AI — Feature & Product Roadmap

> Repository: `Aevor/ai`
>
> Parent Project: `Aevor`
>
> Service Type: AI / ML Microservice
>
> Primary Owner: Sneha
>
> Overall Project Owner: Sanjeev
>
> Status: Proposed / Under Development

---

# 1. Purpose

Aevor AI is the dedicated intelligence layer of the Aevor platform.

The goal is not to create a generic chatbot.

The goal is to build an AI system that understands software repositories and provides useful intelligence to developers working with those repositories.

Aevor AI should eventually be capable of understanding:

- source code
- repository structure
- files
- functions
- classes
- dependencies
- commits
- pull requests
- issues
- documentation
- project architecture

The AI service should expose these capabilities through APIs that can be consumed by the Aevor platform.

---

# 2. Core Vision

The long-term vision is:

    Developer
        |
        v
    Aevor Platform
        |
        v
    Aevor AI
        |
        +-------------------+
        |                   |
        v                   v
    Repository         Developer
    Intelligence      Intelligence
        |                   |
        +---------+---------+
                  |
                  v
             AI / ML Layer
                  |
                  v
              Models

Aevor AI should become the intelligence layer that understands the software development environment inside Aevor.

---

# 3. Important Principle

Aevor AI is NOT just:

    User Question
         |
         v
    Generic LLM
         |
         v
      Answer

Instead:

    Repository
         |
         v
    Repository Ingestion
         |
         v
    Code / Document Processing
         |
         v
    Semantic Representation
         |
         v
    Retrieval
         |
         v
    Relevant Context
         |
         v
    AI Model
         |
         v
    Useful Developer Result

The repository itself should provide the context for the AI.

---

# 4. Feature Roadmap

The features are divided into phases.

Do not attempt to implement everything simultaneously.

Recommended progression:

    Phase 1
        AI Service Foundation

        ↓

    Phase 2
        Repository Intelligence

        ↓

    Phase 3
        Semantic Search

        ↓

    Phase 4
        Repository AI Assistant

        ↓

    Phase 5
        Pull Request Intelligence

        ↓

    Phase 6
        Issue & Commit Intelligence

        ↓

    Phase 7
        Code Quality Intelligence

        ↓

    Phase 8
        Advanced AI Platform

---

# 5. Phase 1 — AI Service Foundation

## Objective

Create a reliable AI microservice that can be independently run and consumed by other Aevor services.

---

## Features

### 5.1 AI Service API

Create the basic API service.

Initial endpoints:

    GET /health

    GET /ready

Future endpoints will be versioned:

    /api/v1/...

---

### 5.2 Model Abstraction

Do not tightly couple the entire service to one model.

The architecture should allow the model layer to be replaced later.

Conceptually:

    API
      |
      v
    AI Service
      |
      v
    Model Interface
      |
      +---- Model A
      |
      +---- Model B
      |
      +---- Future Model

The exact implementation depends on the selected technology.

---

### 5.3 Basic Inference

Implement a minimal inference capability.

The purpose is to prove that:

    Request
       ↓
    AI Service
       ↓
    Model
       ↓
    Response

works correctly.

This should be small.

Do not immediately build RAG or repository intelligence.

---

### 5.4 Health and Readiness

Implement:

    /health

and:

    /ready

Health:

    Is the service alive?

Readiness:

    Can the service currently process AI requests?

---

### 5.5 Testing

Add tests for:

- API startup
- health endpoint
- readiness endpoint
- valid inference
- invalid inference
- model unavailable
- error handling

---

# 6. Phase 2 — Repository Intelligence

This is where Aevor AI starts becoming different from a generic AI API.

## Objective

Allow Aevor AI to understand the structure and contents of software repositories.

---

# 7. Repository Ingestion

Aevor AI should eventually be able to ingest a repository.

Input may include:

- repository metadata
- directory structure
- source files
- documentation
- configuration files
- dependency information

The ingestion system should identify what content is useful for AI processing.

---

## 7.1 Repository Scanner

The scanner should understand:

    Repository
       |
       +-- directories
       |
       +-- source files
       |
       +-- tests
       |
       +-- configuration
       |
       +-- documentation

It should avoid treating everything as ordinary source code.

---

## 7.2 File Classification

Files should be classified where useful.

Possible categories:

    source
    test
    documentation
    configuration
    build
    dependency
    generated
    binary
    ignored

Example:

    main.go       → source
    auth_test.go  → test
    README.md     → documentation
    Dockerfile    → infrastructure
    package.json  → dependency/configuration

---

## 7.3 Repository Metadata

Capture useful metadata such as:

- repository name
- language
- file count
- directory structure
- dependency information
- important configuration
- source files
- test files

This metadata can later be used by the AI.

---

# 8. Code Understanding

The system should progressively understand code at multiple levels.

Instead of treating a complete repository as one huge text document:

    Repository
        |
        +-- File
        |    |
        |    +-- Functions
        |    +-- Classes
        |    +-- Imports
        |
        +-- File
        |
        +-- File

The goal is to create meaningful units of information.

---

# 9. Code Chunking

Aevor AI should eventually support intelligent code chunking.

Do not blindly split source code every N characters.

Prefer logical boundaries where possible:

- function
- class
- method
- module
- interface
- configuration section
- documentation section

This improves retrieval quality.

---

# 10. Embeddings

Generate semantic embeddings for useful repository content.

Possible embedding targets:

- files
- functions
- classes
- documentation
- issues
- pull requests
- commits

Conceptually:

    Code / Document
          |
          v
       Embedding
          |
          v
     Vector Store

These embeddings enable semantic retrieval.

---

# 11. Vector Storage

A vector storage layer should eventually support:

- storing embeddings
- retrieving similar content
- filtering by repository
- filtering by file
- filtering by content type
- updating embeddings
- deleting stale embeddings

The implementation should be chosen based on Aevor infrastructure.

Do not introduce a vector database merely for the sake of having one.

---

# 12. Incremental Indexing

Aevor AI should not need to rebuild an entire repository every time something changes.

Preferred:

    Repository
        |
        v
    Detect Changes
        |
        +---- New file
        |
        +---- Modified file
        |
        +---- Deleted file
        |
        v
    Update affected embeddings

This becomes important as repositories become large.

---

# 13. Phase 3 — Semantic Code Search

## Objective

Allow developers to search repositories using meaning rather than exact keywords.

---

# 14. Natural Language Code Search

Example query:

    "Where is authentication handled?"

Instead of only searching for:

    authentication

Aevor AI should retrieve semantically relevant code.

Possible response:

    Relevant Files

    1. services/auth/service.go
       Authentication service

    2. middleware/auth.go
       JWT validation middleware

    3. handlers/login.go
       Login endpoint

The exact response format can evolve.

---

# 15. Search Features

Potential capabilities:

- natural language search
- semantic search
- file filtering
- language filtering
- repository filtering
- symbol search
- similarity search

---

# 16. Search Result Ranking

Search results should consider relevance.

Potential signals:

- semantic similarity
- file importance
- symbol relevance
- repository context
- exact matches
- dependency relationships

Do not rely exclusively on vector similarity if better ranking signals are available.

---

# 17. Search Explanation

Where practical, explain why a result was retrieved.

Example:

    services/auth/service.go

    Reason:
    Contains the authentication service responsible for
    validating user credentials and generating authentication
    tokens.

This makes the AI more useful and trustworthy.

---

# 18. Phase 4 — Aevor Repository Assistant

## Objective

Build an AI assistant that can answer questions about a repository using repository context.

---

# 19. Repository Question Answering

Examples:

    "How does authentication work?"

    "Where is the MongoDB connection created?"

    "What happens when a user creates a repository?"

    "Which service handles GitHub OAuth?"

    "How does the API communicate with the database?"

The system should retrieve relevant repository context before generating an answer.

---

# 20. RAG Architecture

The intended architecture is:

    User Question
          |
          v
    Query Processing
          |
          v
    Semantic Retrieval
          |
          v
    Relevant Repository Context
          |
          v
       AI Model
          |
          v
        Answer

The AI should not blindly receive the entire repository.

---

# 21. Context Management

The retrieval system should provide the model with only relevant context.

Possible context:

- relevant files
- functions
- classes
- documentation
- dependencies
- recent changes

This helps:

- reduce unnecessary token usage
- improve relevance
- improve response quality
- reduce hallucinations

---

# 22. Code Explanation

Allow users to request explanations of code.

Examples:

    Explain this function.

    Why does this middleware exist?

    What happens when this endpoint is called?

    Explain the relationship between these two services.

The AI should reference actual repository context.

---

# 23. Architecture Explanation

The AI should eventually be able to explain repository architecture.

Example:

    Frontend
       |
       v
    Platform API
       |
       +---- Authentication
       |
       +---- Repository Service
       |
       +---- AI Service
       |
       v
    Database

The explanation should be generated from actual repository information rather than assumptions.

---

# 24. Dependency Understanding

The AI should eventually understand relationships such as:

    Service A
       |
       v
    Service B
       |
       v
    Database

and:

    File A
       |
       v
    Function B
       |
       v
    Function C

This can improve repository Q&A and architecture explanations.

---

# 25. Phase 5 — Pull Request Intelligence

## Objective

Use AI to help developers understand and review pull requests.

---

# 26. PR Summary

Given a PR, generate:

- summary
- files changed
- major changes
- potential impact
- tests added
- tests missing
- possible risks

Example:

    PR Summary

    Added GitHub OAuth login flow.

    Major Changes:
    - Added OAuth callback handler
    - Added authentication service
    - Added user lookup logic

    Potential Risk:
    Authentication state handling should be tested
    for failed OAuth callbacks.

---

# 27. PR Change Explanation

The AI should answer:

    What exactly changed in this PR?

It should compare:

    Base Branch
          |
          v
    Pull Request
          |
          v
       Diff
          |
          v
    AI Analysis

---

# 28. PR Risk Detection

The AI can identify potentially risky changes.

Examples:

- authentication changes
- authorization changes
- database schema changes
- API changes
- dependency changes
- infrastructure changes
- security-sensitive changes

Important:

AI suggestions are advisory.

They do not replace human review.

---

# 29. AI-Assisted Code Review

Potential output:

    Finding:
    Authentication middleware may accept an unexpected
    token format.

    Severity:
    Medium

    Location:
    middleware/auth.go

    Reason:
    ...

This should be treated as an AI recommendation.

The actual GitHub/Aevor review workflow remains governed by repository rules.

---

# 30. Test Awareness

For a PR, Aevor AI should eventually understand:

- which tests changed
- which tests were not changed
- whether important areas appear untested
- what tests may be useful

Example:

    Authentication logic changed.

    Existing tests cover:
    - successful login

    Missing apparent coverage:
    - expired token
    - invalid token
    - OAuth callback failure

---

# 31. Phase 6 — Commit Intelligence

## Objective

Make repository history easier to understand.

---

# 32. Commit Summaries

Generate understandable summaries of commits.

Example:

    Commit:
    feat: implement github oauth login flow initialization

    Summary:
    Introduces the initial GitHub OAuth authentication flow,
    including the service/API foundation required for OAuth
    integration.

---

# 33. Historical Understanding

Aevor AI should eventually answer questions such as:

    "Why was this file changed?"

    "When was OAuth introduced?"

    "What changed between these versions?"

    "Which commit introduced this API?"

This requires combining:

- Git history
- code
- documentation
- PR information

---

# 34. Change Timeline

Potential representation:

    Repository
       |
       +-- Commit A
       |
       +-- Commit B
       |
       +-- Commit C
       |
       +-- Commit D
       |
       v
    Current State

The AI can use this information to explain how the repository evolved.

---

# 35. Phase 7 — Issue Intelligence

## Objective

Help developers understand and manage issues.

---

# 36. Issue Summarization

Given a long issue, generate:

- concise summary
- problem
- expected behavior
- actual behavior
- relevant components
- possible affected files

---

# 37. Issue Classification

Potential classifications:

    bug
    feature
    enhancement
    documentation
    security
    infrastructure
    performance
    question

Classification should remain configurable.

---

# 38. Duplicate Issue Detection

When a new issue is created:

    New Issue
        |
        v
    Semantic Search
        |
        v
    Similar Existing Issues
        |
        v
    Similarity Results

This can help reduce duplicate issues.

---

# 39. Issue-to-Code Mapping

Aevor AI should eventually identify likely affected areas.

Example:

    Issue:
    "GitHub OAuth callback fails."

Potential relevant components:

    services/auth/
    handlers/oauth.go
    middleware/auth.go
    config/oauth.go

This is a recommendation, not a guarantee.

---

# 40. Phase 8 — Code Quality Intelligence

## Objective

Provide AI-assisted software quality analysis.

---

# 41. Potential Code Quality Features

Aevor AI may identify:

- suspicious code
- duplicated logic
- overly complex functions
- potentially unreachable code
- missing error handling
- inconsistent patterns
- maintainability concerns
- documentation gaps

---

# 42. Static Analysis Integration

AI should NOT replace deterministic tools.

Preferred architecture:

    Source Code
        |
        +--------------------+
        |                    |
        v                    v
    Static Analysis       AI Analysis
        |                    |
        +---------+----------+
                  |
                  v
             Combined View

Use deterministic tools where deterministic analysis is appropriate.

Use AI where semantic reasoning adds value.

---

# 43. Documentation Intelligence

Potential capabilities:

- README suggestions
- API documentation generation
- function documentation
- architecture explanations
- changelog generation
- documentation gap detection

---

# 44. Documentation Drift Detection

Eventually detect situations such as:

    Code:
    endpoint = /api/v2/repositories

    Documentation:
    endpoint = /api/v1/repositories

The system can flag potential documentation drift.

---

# 45. Phase 9 — Advanced AI Capabilities

These are long-term possibilities.

They should NOT be implemented until the foundation is stable.

Potential capabilities:

- repository-wide reasoning
- architecture graph generation
- intelligent code navigation
- automated documentation updates
- advanced code review
- development recommendations
- intelligent issue planning
- change impact analysis
- dependency impact analysis
- AI-assisted debugging

---

# 46. Change Impact Analysis

Given a proposed change:

    Change File A

Aevor AI could identify:

    File A
       |
       +---- imports File B
       |
       +---- calls Function C
       |
       +---- affects API D
       |
       +---- tested by Test E

Potential result:

    Potentially affected:
    - File B
    - Function C
    - API D
    - Test E

This can help developers understand the impact of changes.

---

# 47. Architecture Graph

Aevor AI may eventually generate a machine-readable representation:

    Repository
       |
       +-- Service
       |     |
       |     +-- API
       |     +-- Database
       |
       +-- Service
       |     |
       |     +-- Model
       |
       +-- Infrastructure

This could power future architecture visualizations.

---

# 48. AI Observability

AI functionality itself must be observable.

Track where appropriate:

- inference latency
- request count
- failure count
- model latency
- retrieval latency
- retrieval results
- token usage
- resource usage

Do not record sensitive user/repository content unnecessarily.

---

# 49. AI Security

AI must follow strict security rules.

Never expose:

- secrets
- API keys
- passwords
- private tokens
- private credentials

The system must consider repository permissions.

A user should not receive AI-generated information from a repository they are not authorized to access.

---

# 50. Privacy

Repository content can contain sensitive information.

Therefore:

    Repository Content
          |
          v
    Permission Check
          |
          v
    Safe Processing
          |
          v
    AI Pipeline

Do not assume that all indexed content can be exposed to every user.

---

# 51. Hallucination Control

Aevor AI should prefer grounded responses.

When repository evidence is unavailable:

    "I could not find sufficient repository evidence
     to answer this confidently."

is better than inventing an implementation.

For repository questions, prefer:

    Answer
      +
    Relevant files
      +
    Relevant symbols
      +
    Evidence

where practical.

---

# 52. AI Response Confidence

Where appropriate, responses may include confidence or evidence indicators.

Example:

    Confidence: High

    Evidence:
    - services/auth/service.go
    - middleware/auth.go

Do not present confidence as mathematically accurate unless it is actually calibrated.

---

# 53. Model Independence

Aevor should not assume that one model will remain forever.

Possible future architecture:

    Aevor AI
       |
       v
    Model Interface
       |
       +---- Local Model
       |
       +---- Hosted Model
       |
       +---- Specialized Model
       |
       +---- Future Model

This allows Aevor to evolve without rewriting the entire service.

---

# 54. Feature Priority

Not every feature has equal priority.

Recommended priority:

## P0 — Foundation

    - AI API
    - health
    - readiness
    - model abstraction
    - basic inference
    - testing
    - Docker

## P1 — Repository Intelligence

    - repository ingestion
    - file classification
    - code chunking
    - embeddings
    - vector retrieval

## P1 — Semantic Search

    - natural language code search
    - semantic retrieval
    - result ranking

## P1 — Repository Assistant

    - repository Q&A
    - code explanation
    - architecture explanation
    - RAG

## P2 — Pull Request Intelligence

    - PR summary
    - diff explanation
    - risk detection
    - test awareness
    - AI review assistance

## P2 — Issue Intelligence

    - issue summary
    - classification
    - duplicate detection
    - issue-to-code mapping

## P2 — Commit Intelligence

    - commit summaries
    - historical reasoning
    - change timeline

## P3 — Code Quality

    - semantic code quality analysis
    - duplication detection
    - maintainability suggestions
    - documentation gap detection

## P3 — Advanced Intelligence

    - change impact analysis
    - architecture graph
    - advanced repository reasoning
    - intelligent development recommendations

---

# 55. What NOT to Build Yet

Do not immediately build:

- autonomous coding agents
- fully autonomous PR merging
- automatic production deployment
- unrestricted repository modification
- autonomous security decisions
- complex multi-agent systems
- distributed model infrastructure
- Kubernetes-based AI orchestration
- custom foundation models
- large-scale model training

These may be considered later.

The foundation must work first.

---

# 56. MVP Definition

The first meaningful Aevor AI MVP should be:

    Repository
       |
       v
    Ingestion
       |
       v
    Code Chunking
       |
       v
    Embeddings
       |
       v
    Vector Retrieval
       |
       v
    Repository Q&A
       |
       v
    AI Response

With:

- working API
- tests
- Docker
- documentation
- basic security
- reproducible setup

If this works reliably, Aevor already has a meaningful AI microservice.

---

# 57. MVP Example

A developer asks:

    "Where is GitHub OAuth implemented?"

Aevor AI should:

    1. Receive the question.
    2. Identify the repository context.
    3. Convert the query into a searchable representation.
    4. Retrieve relevant repository content.
    5. Provide the relevant files/functions to the model.
    6. Generate an answer.
    7. Reference the relevant repository locations.

Possible result:

    GitHub OAuth is primarily handled in:

    - auth/oauth.go
    - handlers/github_callback.go
    - services/auth/service.go

    The callback handler receives the OAuth response,
    while the authentication service handles user/session
    creation.

The exact result must come from the actual repository.

---

# 58. Development Philosophy

The AI feature roadmap should evolve based on actual usage.

Do not implement a feature simply because:

- another AI platform has it
- it looks impressive
- it is technically interesting
- an AI agent suggested it

Every feature should answer:

    What problem does this solve?

    Who uses it?

    What input does it require?

    What output does it provide?

    How will we evaluate whether it works?

---

# 59. Feature Proposal Template

Before introducing a major feature, document:

    Feature:
    
    Problem:
    
    User:
    
    Input:
    
    Output:
    
    Architecture Impact:
    
    Dependencies:
    
    Security Considerations:
    
    Performance Considerations:
    
    Testing Strategy:
    
    Success Criteria:

This prevents uncontrolled feature growth.

---

# 60. Success Metrics

Aevor AI features should eventually have measurable success criteria.

Examples:

## Semantic Search

    - relevant result rate
    - search latency
    - retrieval precision

## Repository Q&A

    - answer correctness
    - groundedness
    - retrieval quality
    - response latency

## PR Intelligence

    - useful summary rate
    - issue detection precision
    - false positive rate

## Issue Intelligence

    - duplicate detection precision
    - classification accuracy

## AI Service

    - uptime
    - inference latency
    - failure rate
    - resource consumption

---

# 61. Feature Lifecycle

Every feature follows:

    Idea
      |
      v
    Problem Definition
      |
      v
    Design
      |
      v
    Implementation
      |
      v
    Testing
      |
      v
    Evaluation
      |
      v
    Documentation
      |
      v
    PR
      |
      v
    Review
      |
      v
    Merge
      |
      v
    Monitor
      |
      v
    Improve

---

# 62. Relationship With Other Aevor Repositories

Aevor AI is one component of the larger Aevor architecture.

    Aevor
      |
      +-- platform
      |      |
      |      +-- Core application/API
      |
      +-- ai
      |      |
      |      +-- AI intelligence
      |
      +-- infra
      |      |
      |      +-- Infrastructure
      |
      +-- docs
             |
             +-- Architecture / specifications

The AI repository should remain independently deployable.

---

# 63. Platform → AI Interaction

The intended interaction is:

    Platform
       |
       | AI Request
       v
    AI Service
       |
       v
    Retrieval / AI Processing
       |
       v
    AI Result
       |
       v
    Platform
       |
       v
    User

The exact protocol and API contract should be finalized before production integration.

---

# 64. Long-Term Vision

The long-term vision is for Aevor AI to understand a software project at multiple levels:

    Level 1
    Raw Files

        ↓

    Level 2
    Code Structures

        ↓

    Level 3
    Repository Relationships

        ↓

    Level 4
    Git History

        ↓

    Level 5
    Issues / PRs / Development Activity

        ↓

    Level 6
    Semantic Understanding

        ↓

    Level 7
    Developer Intelligence

This allows Aevor to move from:

    "AI that answers questions"

toward:

    "AI that understands the software development environment."

---

# 65. Current Implementation Tracker

Update this section as development progresses.

## Phase 1

- [ ] AI service API
- [ ] Health endpoint
- [ ] Readiness endpoint
- [ ] Model abstraction
- [ ] Basic inference
- [ ] Tests
- [ ] Docker

## Phase 2

- [ ] Repository ingestion
- [ ] File classification
- [ ] Code chunking
- [ ] Embeddings
- [ ] Vector storage
- [ ] Incremental indexing

## Phase 3

- [ ] Semantic search
- [ ] Search ranking
- [ ] Search explanations

## Phase 4

- [ ] Repository Q&A
- [ ] RAG
- [ ] Code explanation
- [ ] Architecture explanation
- [ ] Dependency understanding

## Phase 5

- [ ] PR summarization
- [ ] PR diff explanation
- [ ] PR risk analysis
- [ ] Test awareness
- [ ] AI-assisted review

## Phase 6

- [ ] Commit intelligence
- [ ] Commit summaries
- [ ] Historical reasoning
- [ ] Issue summarization
- [ ] Issue classification
- [ ] Duplicate detection
- [ ] Issue-to-code mapping

## Phase 7

- [ ] Code quality intelligence
- [ ] Static analysis integration
- [ ] Documentation intelligence
- [ ] Documentation drift detection

## Phase 8

- [ ] Change impact analysis
- [ ] Architecture graph
- [ ] Advanced repository reasoning
- [ ] Developer recommendations

---

# 66. Current Priority

Only one phase should be actively implemented at a time.

The immediate priority should always be taken from:

    TODO.md

The roadmap describes the destination.

`TODO.md` describes what is currently being built.

Therefore:

    ROADMAP
       =
    What Aevor AI may become

    TODO.md
       =
    What we are implementing now

Do not confuse the two.

---

# 67. Final Principle

Aevor AI should not become a collection of random AI features.

Every feature should strengthen one central capability:

> **Understanding software and helping developers work with it.**

The progression should therefore be:

    Understand the repository
             ↓
    Search the repository
             ↓
    Explain the repository
             ↓
    Understand changes
             ↓
    Assist with development
             ↓
    Provide intelligent engineering insights

This is the long-term direction of the Aevor AI microservice.