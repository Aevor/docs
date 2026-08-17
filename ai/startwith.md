# Aevor AI Microservice — Engineering Specification

> Repository: `Aevor/ai`
>
> Service: Aevor AI
>
> Repository Type: Independent Microservice
>
> Owner: Sneha
>
> Overall Aevor Owner / Primary Reviewer: Sanjeev
>
> Architecture: Microservice Architecture
>
> Parent Project: Aevor

---

# 1. Purpose

`Aevor/ai` is the dedicated AI microservice of the Aevor platform.

This is an **independent repository and independent service**.

It must NOT become a folder inside `Aevor/platform`.

The purpose of this repository is to provide AI/ML capabilities to the Aevor ecosystem through a clean service boundary.

High-level architecture:

    AEVOR
       |
       +-----------------------+
       |                       |
       v                       v
    Aevor Platform          Aevor AI
    Repository              Repository
       |                       |
       v                       v
    Platform API            AI API
                               |
                               v
                         AI / ML Layer
                               |
                               v
                             Models

The platform should not need to understand the internal AI implementation.

---

# 2. Aevor Repository Architecture

Aevor is being developed as a collection of independent repositories/services.

Current repositories:

    Aevor/
    |
    +-- platform/
    |   +-- Core platform / API
    |
    +-- ai/
    |   +-- AI microservice
    |
    +-- infra/
    |   +-- Infrastructure
    |
    +-- docs/
    |   +-- Product requirements / architecture / documentation
    |
    +-- demo-repository/
        +-- Demo / experimentation repository

The important rule is:

    Each repository has a defined responsibility.
    Each service should remain independently maintainable.

Do not move AI implementation into `platform`.

---

# 3. Ownership

## AI Repository Owner

**Sneha**

Sneha owns the implementation and maintenance of:

    Aevor/ai

Her responsibilities include:

- AI service architecture
- AI API
- model integration
- preprocessing
- inference
- post-processing
- AI testing
- model evaluation
- AI documentation
- Docker configuration
- CI requirements
- service performance
- AI service integration contract

---

## Overall Aevor Owner

**Sanjeev**

Sanjeev is the overall Aevor project owner.

Responsibilities include:

- overall architecture
- cross-service architecture
- project direction
- major technical decisions
- service integration
- primary code review
- final review of important changes
- merge decisions

Other team members can review PRs, but Sanjeev remains the primary reviewer.

---

# 4. Current Aevor Team

    Sanjeev
    Samir Singh
    Sneha
    Sanmukh

Each member may own a different Aevor repository or service.

The goal is for every member to treat their service as if they are responsible for a real production project.

That means they are expected to:

- understand the architecture
- implement features
- write tests
- document their work
- maintain their repository
- create PRs
- respond to review comments
- fix bugs
- keep TODOs updated
- understand their service deeply

---

# 5. Service Boundary

The AI service must remain independent.

Preferred architecture:

    Aevor Platform
           |
           | HTTP / Internal API
           v
       Aevor AI
           |
           v
     AI Service Layer
           |
           v
      Inference Layer
           |
           v
          Model

The platform should communicate with the AI service through a defined contract.

The platform should NOT directly access:

- AI model internals
- model files
- preprocessing internals
- inference implementation
- AI service internal modules

---

# 6. What Sneha Should Own

Sneha should be responsible for the entire AI service lifecycle.

This includes:

    Repository
        |
        v
    Architecture
        |
        v
    API
        |
        v
    Model Integration
        |
        v
    Inference
        |
        v
    Testing
        |
        v
    Evaluation
        |
        v
    Docker
        |
        v
    CI
        |
        v
    Documentation
        |
        v
    Platform Integration

She should be able to explain every important part of the service during a technical discussion.

The goal is not simply to have code written by an AI agent.

The goal is for Sneha to understand and own the service.

---

# 7. What Sneha Should NOT Do

Do NOT:

- move AI logic into `Aevor/platform`
- directly modify another service's database
- modify unrelated repositories
- commit secrets
- commit API keys
- commit OAuth secrets
- commit passwords
- commit private credentials
- commit huge model files unnecessarily
- introduce unnecessary dependencies
- redesign the whole Aevor architecture without discussion
- delete working architecture without understanding it
- implement unrelated features
- claim something is completed without verification

---

# 8. First Step — Repository Inspection

Before implementing anything, inspect the existing repository.

Run:

    pwd
    ls -la
    find . -maxdepth 3 -type f | sort
    git status
    git branch
    git log --oneline --decorate -10

Then inspect:

    README.md
    AGENTS.md
    TODO.md

if they already exist.

The first objective is understanding the current state.

Do not immediately start rewriting files.

---

# 9. Mandatory TODO.md

The repository MUST maintain:

    TODO.md

This is the source of truth for current work.

If it does not exist, create it.

Recommended structure:

    # Aevor AI TODO

    ## Completed

    - [ ] Repository inspection
    - [ ] Architecture definition
    - [ ] Service foundation
    - [ ] API foundation
    - [ ] Model integration
    - [ ] Inference
    - [ ] Testing
    - [ ] Docker
    - [ ] CI
    - [ ] Platform integration

    ## Currently Working On

    - [ ] Define current task

    ## Blocked

    - None

    ## Next

    - [ ] Define next task

    ## Notes

    - Add important implementation decisions here.

Every meaningful implementation must update this file.

---

# 10. Mandatory AGENTS.md

The repository MUST contain:

    AGENTS.md

It should explain how developers and AI coding agents should work inside the repository.

It should contain:

- repository purpose
- architecture
- service boundaries
- directory structure
- development commands
- test commands
- environment configuration
- security rules
- Git workflow
- TODO requirements
- documentation requirements
- coding expectations
- agent workflow

If architecture changes, update `AGENTS.md`.

---

# 11. AI Agent Workflow

Whenever an AI coding agent is used, it must follow:

    1. Read AGENTS.md
    2. Read TODO.md
    3. Inspect existing implementation
    4. Identify the next unfinished task
    5. Implement only that task
    6. Run tests
    7. Update TODO.md
    8. Update AGENTS.md if necessary
    9. Report the work

The agent should NOT randomly implement future features.

---

# 12. Standard Agent Instruction

When assigning a task to an AI coding agent, use:

    Read AGENTS.md and TODO.md first.

    Inspect the current implementation before changing anything.

    Work only on the next unfinished task.

    Do not redesign unrelated parts.

    Implement the task completely.

    Run all relevant tests.

    Update TODO.md with:
    - completed work
    - current work
    - remaining work
    - blockers

    Update AGENTS.md if architecture or development rules changed.

    Do not modify unrelated repositories.

    Do not commit secrets.

    At the end report:

    1. What was implemented
    2. Files changed
    3. Tests executed
    4. Test results
    5. TODO status
    6. Remaining work
    7. Blockers
    8. Architectural concerns

---

# 13. Technology

The AI service should use technologies appropriate for AI/ML workloads.

Potential technologies include:

- Python
- FastAPI
- PyTorch
- Transformers
- scikit-learn
- model-specific libraries

The actual stack should be based on the requirements of the AI functionality.

Do not add dependencies simply because they are popular.

Every major dependency should have a reason.

---

# 14. Recommended Directory Structure

If the repository does not already have an established structure, a possible structure is:

    ai/
    |
    +-- app/
    |   |
    |   +-- api/
    |   |   +-- routes/
    |   |   +-- middleware/
    |   |
    |   +-- services/
    |   |
    |   +-- models/
    |   |
    |   +-- inference/
    |   |
    |   +-- preprocessing/
    |   |
    |   +-- config/
    |
    +-- tests/
    |
    +-- docs/
    |
    +-- scripts/
    |
    +-- Dockerfile
    +-- README.md
    +-- AGENTS.md
    +-- TODO.md
    +-- .env.example
    +-- .gitignore

This is a guideline.

If an existing architecture is already established, preserve it unless there is a clear technical reason to change it.

---

# 15. API Foundation

The service should expose a clean API.

At minimum:

    GET /health

Example response:

    {
      "status": "ok"
    }

AI functionality should use versioned routes.

Example:

    /api/v1/...

Possible future endpoints:

    GET  /health
    GET  /ready
    POST /api/v1/inference
    POST /api/v1/embeddings

Only implement endpoints that are actually required.

---

# 16. Health Endpoint

The health endpoint answers:

> Is the service process alive?

Example:

    GET /health

Response:

    {
      "status": "ok"
    }

Keep health checks lightweight.

---

# 17. Readiness Endpoint

The readiness endpoint answers:

> Is the AI service capable of handling requests?

Example:

    GET /ready

If the model is still loading:

    {
      "status": "not_ready"
    }

When the service is ready:

    {
      "status": "ready"
    }

Do not report the service as ready when the model required for inference is unavailable.

---

# 18. API Versioning

Use:

    /api/v1/

Example:

    /api/v1/inference

Avoid immediately exposing unversioned AI endpoints without a strong reason.

Versioning gives Aevor room to evolve the API.

---

# 19. Layered Architecture

Do not place all AI logic inside the HTTP route.

Bad:

    HTTP Request
         |
         v
    Load Model
         |
         v
    Preprocess
         |
         v
    Inference
         |
         v
    Response

Preferred:

    HTTP Handler
         |
         v
    Service Layer
         |
         v
    Inference Layer
         |
         v
       Model

The API layer should handle:

- request parsing
- validation
- response formatting
- HTTP-level errors

The service/inference layers should handle:

- preprocessing
- model execution
- post-processing
- AI logic

---

# 20. Model Loading

Models should normally be loaded during service initialization rather than for every request.

Bad:

    Request
       |
       v
    Load Model
       |
       v
    Inference
       |
       v
    Response

Preferred:

    Service Startup
         |
         v
      Load Model
         |
         v
      Model Ready
         |
         v
       Requests
         |
         v
      Inference

If model loading fails, the service must clearly report the failure.

---

# 21. Model Selection

Before selecting a model, document:

    Model:
    Version:
    Purpose:
    Input:
    Output:
    License:
    Resource Requirements:
    Expected Latency:
    Known Limitations:
    Reason for Selection:

The model should be selected based on Aevor requirements.

Do not select a model merely because it is popular.

---

# 22. Model Evaluation

Every production-bound model must be evaluated.

Document:

    Model:
    Dataset:
    Task:
    Metric:
    Result:
    Hardware:
    Inference Latency:
    Memory Usage:
    Known Limitations:

The existence of a model is not proof that it works sufficiently well.

---

# 23. Reproducibility

AI experiments should be reproducible.

Record:

- model version
- dependency versions
- preprocessing configuration
- dataset/source
- important parameters
- evaluation metrics
- hardware where relevant

Avoid:

> "It worked on my laptop."

---

# 24. Configuration

Use environment variables for configuration.

Example:

    APP_ENV=development
    PORT=8000
    LOG_LEVEL=info
    MODEL_NAME=
    MODEL_PATH=

Provide:

    .env.example

Never commit:

    .env

---

# 25. Secrets

NEVER commit:

- API keys
- tokens
- passwords
- OAuth secrets
- private keys
- cloud credentials
- database passwords

If a secret is accidentally committed:

1. Stop.
2. Inform Sanjeev.
3. Rotate the credential if necessary.
4. Remove it properly from Git history if required.

Simply deleting the secret from the current file may not be enough.

---

# 26. .gitignore

Ensure appropriate development and model artifacts are ignored.

Example:

    .env
    .venv/
    venv/
    __pycache__/
    *.pyc

    models/
    checkpoints/

    *.pt
    *.pth
    *.bin
    *.safetensors

Adjust this according to the actual implementation.

Do not ignore files that are actually required by the application.

---

# 27. Error Handling

Use structured API errors.

Example:

    {
      "error": {
        "code": "INVALID_INPUT",
        "message": "Input is invalid"
      }
    }

Do not expose raw stack traces to clients.

Internal logs may contain debugging information where appropriate.

---

# 28. Input Validation

All external input must be validated.

Validate:

- required fields
- data types
- supported formats
- maximum request size
- supported model inputs
- malformed requests

Invalid requests should fail early.

---

# 29. Logging

Log important service events:

- service startup
- model initialization
- model initialization failure
- request failures
- unexpected errors
- service shutdown

Never log:

- API keys
- passwords
- tokens
- private credentials

---

# 30. Testing

Testing is mandatory.

## Unit Tests

Test:

- preprocessing
- validation
- service logic
- inference logic
- post-processing
- error handling

## API Tests

Test:

    GET /health
    GET /ready

and future AI endpoints.

Test:

- valid request
- invalid request
- missing fields
- malformed request
- model unavailable
- internal failure

Do not test only the happy path.

---

# 31. Performance

Measure AI performance rather than guessing.

Important metrics:

- model loading time
- inference latency
- memory usage
- CPU usage
- GPU usage
- throughput

Do not optimize before identifying an actual bottleneck.

Priority:

    Correctness
        |
        v
    Testing
        |
        v
    Measurement
        |
        v
    Optimization

---

# 32. Docker

The AI service must eventually be independently containerized.

Expected architecture:

    Aevor Platform Container
             |
             | HTTP
             v
      Aevor AI Container
             |
             v
          AI Model

The AI container must be independently buildable and runnable.

It should not require the platform container merely to start.

---

# 33. Microservice Independence

The AI service must be independently deployable.

It should not require:

- Platform source code
- Platform internal packages
- Platform database

to function.

Service boundaries must remain explicit.

---

# 34. Database Rules

If the AI service eventually requires persistent data, define ownership first.

Do NOT directly access another service's database.

Bad:

    AI Service
         |
         v
    Platform Database

Preferred:

    AI Service
         |
         v
    AI-owned data

or:

    AI Service
         |
         v
    Platform API

depending on the actual requirement.

---

# 35. Platform Integration

The eventual architecture should be:

    Aevor Platform
           |
           | HTTP / Internal API
           v
        Aevor AI
           |
           v
          Model

The platform should know:

- AI service URL
- endpoint
- request schema
- response schema
- error schema
- timeout behavior

The platform should NOT need to know:

- model internals
- preprocessing internals
- model loading implementation
- inference implementation

---

# 36. AI API Contract

Before platform integration, document the API contract.

Example request:

    {
      "input": "...",
      "options": {}
    }

Example response:

    {
      "result": "...",
      "metadata": {}
    }

Example failure:

    {
      "error": {
        "code": "MODEL_UNAVAILABLE",
        "message": "AI model is currently unavailable"
      }
    }

The actual schema must be based on the implemented AI functionality.

---

# 37. Frontend Boundary

The frontend should generally communicate with the platform rather than directly calling internal AI infrastructure.

Preferred:

    Frontend
       |
       v
    Platform API
       |
       v
    AI Service

This keeps the internal microservice architecture behind the platform API.

---

# 38. Security

Follow secure development practices.

Never:

- expose credentials
- log secrets
- trust arbitrary input
- execute arbitrary user-provided code
- expose internal stack traces
- bypass authentication
- download arbitrary model files without validation

Security-sensitive decisions should be documented.

---

# 39. Model Security

Only use trusted model sources.

Document:

    Model Source:
    Model Version:
    Model Format:
    Model Provenance:

Do not blindly load arbitrary serialized model files.

---

# 40. Resource Requirements

AI workloads may consume significant resources.

Document:

    CPU:
    RAM:
    GPU:
    VRAM:
    Disk:
    Model Size:
    Expected Latency:

Do not assume GPU infrastructure is necessary before measuring actual requirements.

---

# 41. Avoid Premature Infrastructure

Do NOT immediately introduce:

- Kubernetes
- service mesh
- distributed inference
- GPU clusters
- message queues
- complex caching
- model orchestration

unless Aevor actually requires them.

Development order:

    Correct
       |
       v
    Tested
       |
       v
    Documented
       |
       v
    Containerized
       |
       v
    Integrated
       |
       v
    Measured
       |
       v
    Optimized

---

# 42. Documentation

The repository should maintain:

    README.md
    AGENTS.md
    TODO.md

README should explain:

- what the service does
- architecture
- setup
- installation
- environment variables
- local development
- testing
- API endpoints
- model information
- Docker usage

---

# 43. Git Workflow

The AI repository is an independent GitHub repository.

Sneha should work on feature/fix branches.

Examples:

    main
      |
      +-- feat/ai-inference
      |
      +-- feat/model-loader
      |
      +-- fix/input-validation
      |
      +-- test/inference

Workflow:

    Create branch
         |
         v
    Implement
         |
         v
    Test
         |
         v
    Update TODO
         |
         v
    Update documentation
         |
         v
    Create PR
         |
         v
    Team Review
         |
         v
    Sanjeev Primary Review
         |
         +---- Changes requested
         |
         +---- Approval
                  |
                  v
                Merge

Sanjeev handles the final merge according to repository rules.

---

# 44. Review Ownership

Aevor is a team project.

Other contributors can review PRs.

However:

**Sanjeev is the primary reviewer and project owner.**

The intended flow is:

    Contributor
         |
         v
    Pull Request
         |
         v
    Optional Team Reviews
         |
         v
    Sanjeev Review
         |
         v
       Approval
         |
         v
       Merge

The purpose is not to block contributors.

The purpose is to maintain architectural consistency across Aevor.

---

# 45. Pull Request Requirements

Every PR should contain:

    ## What changed?

    ## Why?

    ## How was it tested?

    ## Breaking changes?

    ## API changes?

    ## Documentation updated?

    ## TODO updated?

    ## Follow-up work?

Keep PRs focused.

Do not combine unrelated work.

---

# 46. Commit Messages

Use meaningful commit messages.

Good examples:

    feat: add AI inference service
    feat: add model loader
    feat: add inference endpoint
    test: add inference API tests
    fix: handle model initialization failure
    docs: document AI API
    chore: add Docker configuration

Avoid:

    update
    changes
    stuff
    final
    final2
    working
    new

---

# 47. Branch Naming

Use:

    feat/...
    fix/...
    refactor/...
    test/...
    docs/...
    chore/...

Examples:

    feat/ai-inference-api
    feat/model-loader
    test/inference-service
    fix/input-validation
    docs/ai-api

---

# 48. Development Phases

## Phase 0 — Repository Understanding

- [ ] Inspect repository
- [ ] Read README
- [ ] Read AGENTS.md
- [ ] Read TODO.md
- [ ] Inspect Git history
- [ ] Understand current implementation
- [ ] Confirm technology stack

---

## Phase 1 — Service Foundation

- [ ] Define service responsibility
- [ ] Define architecture
- [ ] Define directory structure
- [ ] Define configuration
- [ ] Define logging
- [ ] Implement health endpoint
- [ ] Create/update TODO.md
- [ ] Create/update AGENTS.md

---

## Phase 2 — API

- [ ] Define API contract
- [ ] Implement API server
- [ ] Implement validation
- [ ] Implement response schemas
- [ ] Implement error handling
- [ ] Add API tests

---

## Phase 3 — AI / Model

- [ ] Select model
- [ ] Document model selection
- [ ] Implement model loader
- [ ] Implement preprocessing
- [ ] Implement inference
- [ ] Implement post-processing
- [ ] Add model tests

---

## Phase 4 — Evaluation

- [ ] Define evaluation dataset
- [ ] Define metrics
- [ ] Evaluate model
- [ ] Measure latency
- [ ] Measure resource usage
- [ ] Document results

---

## Phase 5 — Production Readiness

- [ ] Docker
- [ ] Environment configuration
- [ ] Health checks
- [ ] Readiness checks
- [ ] Logging
- [ ] Error handling
- [ ] Tests
- [ ] CI
- [ ] Documentation

---

## Phase 6 — Platform Integration

- [ ] Define platform → AI contract
- [ ] Implement integration endpoint
- [ ] Configure service communication
- [ ] Test platform → AI communication
- [ ] Test failure handling
- [ ] Document integration

---

# 49. Definition of Done

A task is complete only when appropriate requirements are satisfied.

Checklist:

- [ ] Implementation exists
- [ ] Code is understandable
- [ ] Tests exist where appropriate
- [ ] Tests pass
- [ ] Error cases are handled
- [ ] Documentation is updated
- [ ] TODO.md is updated
- [ ] AGENTS.md is updated if necessary
- [ ] No secrets were committed
- [ ] No unrelated files were changed
- [ ] API contract is documented if applicable
- [ ] PR is ready for review

Code existing does NOT automatically mean the task is complete.

---

# 50. TODO Discipline

After every meaningful implementation:

1. Update TODO.md.
2. Mark completed work.
3. Add newly discovered work.
4. Record blockers.
5. Record the next task.

Example:

    # Current Status

    ## Completed

    - [x] API server
    - [x] Health endpoint

    ## Current

    - [ ] Implement inference endpoint

    ## Blocked

    - None

    ## Next

    - [ ] Add inference tests

This allows the entire team to track progress.

---

# 51. Agent Completion Report

Every AI coding agent session should end with:

    ## Implementation Summary

    ### Completed
    - ...

    ### Files Changed
    - ...

    ### Tests
    - ...

    ### Test Results
    - ...

    ### TODO Status
    - ...

    ### Remaining Work
    - ...

    ### Blockers
    - ...

    ### Architectural Concerns
    - ...

Never simply report:

    Done.

---

# 52. Do Not Claim Unverified Work

Agents must distinguish between:

    Implemented

and:

    Verified

Example:

    Implemented the inference endpoint.
    Tests have not yet been executed.

is acceptable.

Do not say:

    Production-ready

without verification.

---

# 53. Long-Term AI Direction

Possible future capabilities may include:

- intelligent code assistance
- repository understanding
- semantic search
- embeddings
- code analysis
- recommendations
- anomaly detection
- developer assistance
- AI-powered workflows

These are future possibilities, not immediate requirements.

Every feature must have a clear relationship to Aevor.

---

# 54. Current Priority

The immediate objective is NOT:

> Build the most advanced AI possible.

The immediate objective is:

> Build a clean, reliable, independently deployable AI microservice that Aevor can integrate with.

Priority:

    1. Understand repository
    2. Establish architecture
    3. Define service boundary
    4. Define API
    5. Build service foundation
    6. Build model/inference layer
    7. Test
    8. Document
    9. Containerize
    10. Integrate
    11. Measure
    12. Optimize

---

# 55. Communication With Sanjeev

Major decisions should be discussed before implementation when they affect:

- architecture
- service boundaries
- API contracts
- model selection
- database access
- security
- infrastructure
- major dependencies
- external services

Do not silently make major architectural changes.

---

# 56. Engineering Mindset

Aevor should be treated as a real open-source engineering project.

The goal is not:

> "I wrote some code."

The goal is:

> "Another engineer can understand, run, test, review, maintain, and integrate my service."

Every contributor should build the habit of:

    Understand
        |
        v
    Plan
        |
        v
    Implement
        |
        v
    Test
        |
        v
    Document
        |
        v
    Review
        |
        v
    Improve

---

# 57. Ownership Mindset

Sneha should treat `Aevor/ai` as her own service.

That means she should be able to answer:

- Why does this service exist?
- What problem does it solve?
- Why was this architecture chosen?
- Why was this model chosen?
- What are its API endpoints?
- How does inference work?
- What happens when the model fails?
- How is it tested?
- How is it containerized?
- How does Platform communicate with it?
- What are the current limitations?
- What is the next feature?
- What technical debt exists?

If she cannot answer these questions, the service is not yet fully owned.

---

# 58. Cross-Repository Rule

Do not assume that changes in one repository automatically belong in another.

For example:

    Aevor/platform

owns platform functionality.

    Aevor/ai

owns AI functionality.

    Aevor/infra

owns infrastructure.

    Aevor/docs

owns project documentation and specifications.

If a change affects multiple repositories, define the interface between them rather than simply copying code across repositories.

---

# 59. No Monolith Creep

Aevor is intentionally being developed toward a microservice architecture.

Avoid turning:

    platform

into:

    platform
    + AI
    + infrastructure
    + every future feature

Instead:

    Platform
       |
       +---- AI
       |
       +---- Infrastructure
       |
       +---- Future Services

Services communicate through explicit interfaces.

---

# 60. Current Status

This section must be updated continuously.

## Completed

- [ ] Repository inspection
- [ ] Architecture definition
- [ ] Service foundation
- [ ] API foundation
- [ ] Model selection
- [ ] Model integration
- [ ] Inference
- [ ] Testing
- [ ] Docker
- [ ] CI
- [ ] Platform integration

## Currently Working On

- [ ] Define current task

## Blocked

- None currently known

## Next

- [ ] Define next implementation task

## Notes

- Update this section after meaningful work.
- Do not mark work complete unless it has actually been implemented and verified.

---

# 61. Final Mandatory Workflow

Before starting ANY task:

    READ AGENTS.md
          |
          v
    READ TODO.md
          |
          v
    INSPECT EXISTING CODE
          |
          v
    IDENTIFY NEXT TASK
          |
          v
    IMPLEMENT ONE TASK
          |
          v
    RUN TESTS
          |
          v
    UPDATE TODO.md
          |
          v
    UPDATE AGENTS.md IF NECESSARY
          |
          v
    CREATE PR
          |
          v
    TEAM REVIEW
          |
          v
    SANJEEV PRIMARY REVIEW
          |
          v
        MERGE

This workflow applies to both human contributors and AI coding agents.

---

# 62. Final Rule

The most important rule for the Aevor AI repository is:

> **Do not optimize for writing code quickly. Optimize for building a service that another engineer can understand, test, review, deploy, and maintain.**

Aevor is a team project.

Sneha owns the AI service.

Samir and Sanmukh own their respective services/repositories.

Sanjeev owns the overall architecture and acts as the primary reviewer.

The repositories remain independent, but they form one coherent Aevor microservice platform.