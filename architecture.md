# Aevor Architecture

## V1 Goal

Help developers discover open-source issues that match their skills.

## System Overview

User
  ↓
Web Application
  ↓
API Service
  ↓
PostgreSQL

## Components

### Web

Responsibilities:

- Authentication
- Profile Management
- Skill Management
- Repository Discovery
- Issue Discovery

### API

Responsibilities:

- User Management
- GitHub Integration
- Repository Management
- Issue Management
- Recommendation Engine

### Database

Responsibilities:

- Store users
- Store skills
- Store repositories
- Store issues
- Store recommendations

## Future Components

- AI Service
- Vector Database
- Repository Intelligence
- Contribution Planner