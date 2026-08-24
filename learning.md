# Learning Journal

## Day 1 - Project Foundation

### What We Did

* Created the Aevor GitHub organization
* Defined repository structure
* Created docs, platform, ai and infra repositories
* Initialized Git repositories
* Designed initial project architecture
* Defined V1 scope

### Key Learnings

#### Monorepo vs Multi-Repo

Monorepo:

* Single repository
* Easier dependency management
* Easier refactoring

Multi-Repo:

* Clear separation of concerns
* Independent deployments
* Better for larger teams

#### Why Documentation First?

Writing documentation before implementation helps clarify requirements, architecture and scope before code is written.

#### Why Not Start With AI?

The core value of Aevor is helping contributors discover relevant issues.

AI enhances the experience but is not required to validate the core idea.

### Questions To Explore

* How does GitHub OAuth work?
* How should recommendations be generated?
* When should microservices be introduced?

## Docker Fundamentals

### What is Docker?

Docker allows applications and their dependencies to be packaged into portable containers.

### What is Docker Compose?

Docker Compose allows multiple containers to be managed using a single configuration file.

### Why use containers?

- Consistent environments
- Easy deployment
- Simplified setup

# Day 2 - Docker and PostgreSQL

## Objective

Set up the local development environment for Aevor using Docker and PostgreSQL.

## What Was Built

* PostgreSQL container using Docker Compose
* Persistent database storage using Docker Volumes
* Local database accessible from the host machine

## Key Concepts Learned

### Docker Image

A Docker image is a read-only blueprint that contains everything required to run an application.

Examples:

* postgres:16
* golang:1.24
* redis:7

### Docker Container

A container is a running instance of an image.

Relationship:

Image → Container

Similar analogy:

Class → Object

### Port Mapping

Configuration:

5432:5432

Meaning:

Host Machine Port 5432 is mapped to PostgreSQL Port 5432 inside the container.

This allows applications running on the host machine to communicate with PostgreSQL running inside Docker.

### Docker Volumes

Containers are temporary.

Deleting a container normally removes its filesystem.

Volumes provide persistent storage that survives container recreation.

### Why Docker Was Chosen

* Consistent environments
* Easier onboarding
* Simplified deployment
* Reproducible development setup

## Outcome

A PostgreSQL 16 instance is running locally inside Docker and is ready for application integration.

## Environment Variables

Environment variables allow application configuration to be separated from source code.

Examples:

- Database Host
- Database Port
- API Keys
- Secrets

Benefits:

- Security
- Flexibility
- Environment-specific configuration

## Day 3 - Configuration Management and Database Connections

### Objective

Create a reusable configuration system and establish a PostgreSQL connection from a Go application.

### What Was Built

* Configuration loader
* Environment variable management
* PostgreSQL connection package

### Key Concepts

#### Structs

Structs are used to group related data together.

Example use cases:

* Configuration
* User models
* API responses

#### Pointers

Pointers store memory addresses.

Returning pointers avoids unnecessary copying of large objects.

#### Environment Variables

Configuration should not be hardcoded.

Environment variables allow the same application to run across multiple environments without code changes.

#### DSN (Data Source Name)

A DSN contains all information required to establish a database connection.

## Day 5 - User Domain Modeling

### Objective

Create the first business entity for Aevor and prepare the application for persistent user storage.

### What Was Built

* User domain model
* Database migration
* UUID-based identifiers
* GORM schema definitions

---

### Understanding Domain Models

A domain model represents a business entity inside the application.

Examples:

* User
* Skill
* Repository
* Issue
* Recommendation

The User model is the foundation of Aevor because every future feature depends on user identity.

---

### User Entity Design

Fields:

* ID
* GithubID
* Username
* DisplayName
* Email
* AvatarURL
* CreatedAt
* UpdatedAt

Each field represents information required by future product features.

Examples:

GithubID:

* Links users to GitHub accounts

Username:

* Used for identification inside the platform

AvatarURL:

* Used for profile presentation

---

### Why UUID Instead Of Auto Increment IDs?

Benefits:

* Globally unique
* Difficult to guess
* Better suited for distributed systems
* Safer for public APIs

Example:

Auto Increment:

1
2
3

UUID:

550e8400-e29b-41d4-a716-446655440000

---

### Struct Tags

Struct tags provide metadata that external libraries can use.

Example:

gorm:"not null"

This tells GORM how to create the database schema.

---

### Database Migrations

Database schemas evolve over time.

Migrations provide a controlled mechanism to create and update database structures safely.

For Aevor V1 we are using GORM AutoMigrate.

Future versions may use:

* Goose
* Atlas
* Flyway
* Liquibase

for stricter migration management.

## Day 7 - First Persistent Database Table

### Objective

Create the first persistent database table in Aevor.

### What Was Built

* User model
* User migration
* PostgreSQL users table

### Verification

Database inspection:

\dt

Result:

users table created successfully.

### Key Learning

A struct inside an application is not the same as a database table.

Migrations are responsible for translating application models into database schema.

### Engineering Principle

Never assume migrations succeeded.

Always verify directly in the database.

## Repository Layer

### Purpose

The repository layer is responsible for all database operations.

Responsibilities:

* Create records
* Retrieve records
* Update records
* Delete records

Repositories should not contain HTTP logic or business rules.

### Benefits

* Cleaner architecture
* Easier testing
* Centralized database access
* Reduced code duplication

### Dependency Injection

Repositories receive database connections from outside rather than creating them internally.

This approach improves flexibility and testability.

## GORM Hooks

GORM supports lifecycle hooks.

Examples:

* BeforeCreate
* AfterCreate
* BeforeUpdate
* AfterUpdate

These hooks execute automatically during database operations.

In Aevor, BeforeCreate is used to generate UUIDs before inserting users.


# Day 9 - Querying Users Using Alternate Keys

## Objective

Today we extended the Users module by introducing support for querying users using their GitHub ID in addition to the internal UUID.

New endpoint:

GET /users/github/:id

This endpoint will become a critical part of the OAuth login flow because GitHub identifies users using GitHub IDs, not Aevor UUIDs.

---

## Understanding User Identification

A user can have multiple identifiers.

Example:

User:

* Name: Sanjeev Kumar
* Username: sanjeev
* GitHub ID: 12345
* Internal UUID: 34660b45-47fe-4502-9da9-45c83338ec71

Different systems use different identifiers.

GitHub:

* Uses GitHub ID

Aevor:

* Uses UUID

During authentication we need a way to map external identities to internal identities.

---

## Primary Key vs Alternate Key

### Primary Key

The primary key uniquely identifies a record inside a database.

In Aevor:

UUID

Example:

34660b45-47fe-4502-9da9-45c83338ec71

Advantages:

* Globally unique
* Hard to guess
* Independent from third-party services

---

### Alternate Key

An alternate key is another unique field that can also identify a record.

Examples:

* GitHub ID
* Email
* Username

In our case:

github_id

Example:

12345

---

## Why Not Use GitHub ID As Primary Key?

Because Aevor should own its identity system.

If tomorrow we support:

* GitLab
* Bitbucket
* Azure DevOps

we still keep the same internal UUID.

External providers may change, but internal identity remains stable.

---

## Repository Layer Implementation

Function added:

func (r *Repository) GetByGitHubID(githubID int64) (*User, error)

Responsibilities:

* Query PostgreSQL
* Return matching user
* Return error if user does not exist

Repository should only contain database logic.

No business rules.

No HTTP code.

---

## Service Layer Responsibility

Function:

func (s *Service) GetUserByGitHubID(githubID int64)

Responsibilities:

* Business validation
* Future business rules
* Delegation to repository

The service layer sits between handlers and repositories.

---

## Handler Layer Responsibility

Endpoint:

GET /users/github/:id

Responsibilities:

* Read path parameter
* Convert string → int64
* Call service
* Return JSON response

Handlers should never talk directly to PostgreSQL.

---

## How OAuth Will Use This

Future flow:

User clicks Login with GitHub
↓
GitHub returns profile
↓
GitHub ID extracted
↓
GetUserByGitHubID()
↓
User found?
/      
Yes      No
↓        ↓
Login    Create User

This is why today's endpoint is important.

It is the foundation for authentication.

---

## Key Backend Engineering Concepts Learned

1. Primary Keys
2. Alternate Keys
3. UUIDs
4. Repository Pattern
5. Service Pattern
6. Handler Pattern
7. API Design
8. Identity Mapping
9. OAuth Preparation

---

## Interview Questions

Q1. What is a Primary Key?

A primary key uniquely identifies each record in a table.

Example:
users.id

---

Q2. What is an Alternate Key?

A unique field that can also identify a record.

Examples:

* Email
* Username
* GitHub ID

---

Q3. Why use UUIDs instead of auto-increment IDs?

Advantages:

* Globally unique
* Better for distributed systems
* Harder to enumerate
* Safer for public APIs

---

Q4. Why not query the database directly inside handlers?

Because handlers should manage HTTP concerns only.

Database logic belongs inside repositories.

---

Q5. Why introduce a service layer?

To isolate business logic from transport and persistence layers.

This improves maintainability and testability.

# Day 10 - OAuth Login Initialization

## Objective

Implemented the first stage of GitHub OAuth authentication.

Endpoint:

GET /auth/github/login

This endpoint redirects users to GitHub's authorization page.

---

## OAuth Flow Overview

User
↓
Aevor Login Endpoint
↓
GitHub Authorization Page
↓
User Grants Access
↓
GitHub Callback
↓
Access Token
↓
User Profile

Today we implemented only the first step.

---

## Why OAuth?

OAuth allows users to authenticate using trusted third-party providers without sharing passwords directly with the application.

Benefits:

* Better security
* Faster onboarding
* Reduced password management burden

---

## Components Added

* OAuth Configuration
* Auth Service
* Auth Handler
* GitHub User DTO

---

## Engineering Lesson

Authentication systems should be built incrementally.

Before processing callbacks or issuing JWTs, verify that login redirection works correctly.

---

# Day 11 - GORM Column Naming Pitfalls and Time-Bomb Tests

## Objective

Fix the OAuth re-login failure (`column excluded.github_access_token does not exist`) and the suddenly-red JWT test suite, then make both classes of bug impossible to reintroduce.

---

## What Happened

The `User` model declared `GitHubAccessToken *string` without an explicit column tag. GORM's default naming strategy split the field name at every case boundary it recognized and produced the column `git_hub_access_token` — not `github_access_token`. The repository's ON CONFLICT clause hand-wrote `github_access_token`. PostgreSQL therefore rejected every re-login upsert.

Separately, four JWT tests issued tokens at a hardcoded timestamp (2026-08-15) with a 7-day TTL, then parsed them with live wall-clock validation. Exactly one week after that date, the suite began failing permanently through no code change.

---

## Why It Matters

* Hand-written column strings drift silently from struct-derived schema. The compiler cannot catch the mismatch; only PostgreSQL at runtime can.
* Tests that depend on wall-clock progress are deferred failures — they pass for days, then rot.
* "Tests passed" is not evidence of correctness when fakes replace real SQL.

---

## How Aevor Uses It

* The model now pins `gorm:"column:git_hub_access_token"` explicitly — intent lives next to the field, immune to naming-strategy changes.
* Upsert conflict/assignment columns live in package-level vars validated by unit tests against the parsed GORM schema (`repository_upsert_test.go`), so any drift fails `go test` instead of production.
* An opt-in integration test (`AEVOR_TEST_DATABASE_DSN`) proves the real ON CONFLICT SQL against live PostgreSQL while keeping default runs hermetic.
* The claim-inspection test helper parses with `jwt.WithoutClaimsValidation()`; validity semantics stay covered by `Verify`/middleware tests against injected clocks.

---

## Common Mistakes

* Assuming GORM turns `GitHubX` into `github_x`; initialism handling is limited and produces surprising splits like `git_hub_x`.
* Writing raw column names inside clause builders instead of deriving or validating them.
* Validating expiry in helpers whose purpose is inspecting claim values.
* Treating a green fake-based suite as full coverage of persistence behavior.

---

## Production Considerations

* Prefer explicit column tags on any field whose Go name does not map obviously.
* Integration-test critical write paths (upserts, migrations) at least optionally, gated by environment configuration.
* Inject clocks into time-sensitive components so deterministic tests never race the calendar.
