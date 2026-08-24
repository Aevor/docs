# Interview Notes

## Git

### What is Git?

Git is a distributed version control system used to track changes in source code.

### What is the difference between Git and GitHub?

Git:

* Version control tool

GitHub:

* Platform that hosts Git repositories

### What is a commit? 

A commit is a snapshot of changes in a repository.

### What is a branch?

A branch is an independent line of development.

### What is the difference between merge and rebase?

Merge:

* Preserves history
* Creates merge commits

Rebase:

* Rewrites history
* Produces a cleaner commit graph

## Repository Design

### What is a monorepo?

A repository containing multiple applications and services.

### What is a multi-repo architecture?

Each service or application is maintained in a separate repository.

### Advantages of Multi-Repo

* Independent deployments
* Better ownership boundaries

### Advantages of Monorepo

* Easier code sharing
* Easier refactoring
* Simpler dependency management

## Docker

### What is Docker?

Docker is a containerization platform that packages applications and dependencies together.

### Difference Between VM and Container

VM:
- Includes guest operating system

Container:
- Shares host OS kernel
- Lightweight
- Faster startup

### What is Docker Compose?

Docker Compose manages multiple containers through a YAML configuration file.

# Docker and PostgreSQL Interview Notes

## What is Docker?

Docker is a containerization platform that packages applications and their dependencies into portable containers.

## What is the difference between a Docker Image and a Docker Container?

Image:

* Blueprint
* Read-only
* Used to create containers

Container:

* Running instance of an image
* Has its own processes and networking

## What is Docker Compose?

Docker Compose allows multiple containers to be defined and managed using a single YAML configuration file.

Example:

* PostgreSQL
* Redis
* API Service

can all be started using a single command.

## What problem do Docker Volumes solve?

Containers are ephemeral.

Volumes persist application data independently of the container lifecycle.

Without volumes:

Container deleted → Data lost

With volumes:

Container deleted → Data preserved

## Why use PostgreSQL inside Docker instead of installing it locally?

* Consistent versions across environments
* Simplified setup
* Easier deployment
* Reduced dependency conflicts

## What does 5432:5432 mean?

Format:

HOST_PORT:CONTAINER_PORT

Example:

5432:5432

Requests to localhost:5432 are forwarded to PostgreSQL running inside the container.

## What happens if another application is already using port 5432?

Docker cannot bind the port.

A different host port must be selected, such as:

5433:5432

## What is the difference between a Virtual Machine and a Container?

Virtual Machine:

* Includes a full guest operating system
* Higher resource consumption

Container:

* Shares the host operating system kernel
* Faster startup
* Lower resource usage

## Environment Variables

### Why use environment variables?

Environment variables allow configuration to be changed without modifying application code.

Examples:

- Database credentials
- API keys
- Service URLs

### Why should secrets not be hardcoded?

Hardcoded secrets can be exposed through source control and are difficult to rotate.

## Go Fundamentals

### What is a Struct?

A struct is a composite data type used to group related fields together.

### What is a Pointer?

A pointer stores the memory address of another value.

### Why use pointers?

* Avoid copying large objects
* Improve performance
* Allow mutation of original data

### What is a DSN?

DSN stands for Data Source Name.

It contains the information required for a database connection such as:

* Host
* Port
* Username
* Password
* Database Name

### Why should configuration not be hardcoded?

Hardcoded configuration makes deployments difficult and increases security risks.

Environment variables provide flexibility and security.


## User Domain and Database Design

### What is a Domain Model?

A domain model represents a core business entity within an application.

Examples:

* User
* Product
* Repository
* Issue

Domain models define the structure and behavior of business data.

---

### What is a Struct in Go?

A struct is a composite data type that groups related fields together.

Example:

type User struct {
Username string
Email string
}

Structs are commonly used to represent domain models.

---

### What are Struct Tags?

Struct tags are metadata attached to struct fields.

Example:

Username string `gorm:"not null"`

Common uses:

* ORM configuration
* JSON serialization
* Validation rules

---

### UUID vs Auto Increment IDs

UUID Advantages:

* Globally unique
* Harder to enumerate
* Suitable for distributed systems

Auto Increment Advantages:

* Smaller size
* Better index locality
* Simpler debugging

Tradeoff:

Choose UUIDs when uniqueness and security are more important than storage efficiency.

---

### What is a Database Migration?

A migration is a controlled schema change applied to a database.

Examples:

* Creating tables
* Adding columns
* Creating indexes

Benefits:

* Versioned database changes
* Reproducible deployments
* Safer schema evolution

---

### What is AutoMigrate?

AutoMigrate is a GORM feature that automatically creates or updates database schema based on struct definitions.

Advantages:

* Fast development
* Reduced boilerplate

Disadvantages:

* Less control over schema evolution
* Not ideal for large production systems


## Repository Pattern

### What is a Repository?

A repository is a layer responsible for database interactions.

Examples:

* Create User
* Update User
* Find User
* Delete User

### Why Use Repositories?

Benefits:

* Separation of concerns
* Easier testing
* Cleaner business logic
* Reduced database coupling

### Application Architecture

HTTP Request
↓
Handler
↓
Service
↓
Repository
↓
Database

Each layer has a specific responsibility.

## Repository Pattern

### What Is Repository Pattern?

Repository Pattern abstracts database access behind a dedicated layer.

Application Flow:

Handler
↓
Service
↓
Repository
↓
Database

### Why Use Repositories?

Benefits:

* Separation of concerns
* Easier testing
* Better maintainability
* Database independence

### What Is Dependency Injection?

Dependencies are provided to components rather than created inside them.

Example:

Repository receives a database connection during initialization.

### Why Not Query Database Directly From Handlers?

Handlers should focus on HTTP concerns.

Database logic belongs inside repositories.

## ORM Hooks

ORM hooks allow logic to run automatically during entity lifecycle events.

Examples:

* BeforeCreate
* AfterCreate
* BeforeUpdate

Common uses:

* UUID generation
* Auditing
* Validation
* Logging

## Why are we implementing OAuth before Skills?
Skills belong to a user.
Without authentication there is no trusted identity.
Therefore user identity must exist before any user-owned domain such as skills, repositories, preferences or recommendations.

# OAuth Fundamentals

## What is OAuth?

OAuth is an authorization framework that allows applications to obtain limited access to user resources without handling user passwords.

---

## OAuth Flow

User
↓
Authorization Request
↓
Provider Login
↓
Authorization Code
↓
Access Token
↓
Protected Resources

---

## Why Not Use Username And Password?

Reasons:

* Security risks
* Password storage complexity
* Compliance burden

OAuth delegates identity verification to trusted providers.

---

## What Is An Authorization Code?

A short-lived code issued by the OAuth provider.

The application exchanges this code for an access token.

The authorization code itself does not grant API access.
