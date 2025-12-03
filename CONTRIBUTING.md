# Contributing to ERP Core

Thank you for your interest in contributing to ERP Core. This document serves as the comprehensive Developer Guide, covering setup, architecture standards, technology stack, and contribution workflow.

## Development Environment Setup

### Prerequisites

- **.NET SDK:** Version 8.0 or higher
- **IDE:** Any C#-compatible IDE or editor
- **Docker & Docker Compose:** For running the database container
- **SQL Client:** Any SQL client capable of connecting to SQL Server

### Database Setup

The project uses SQL Server running in a Docker container.

1. **Start the database container:**
   ```bash
   docker-compose up -d
   ```

2. **Connection Details:**
   - See `docker-compose.yml` for local development configuration
   - Database is created via EF Core migrations
   
   > **Note:** The credentials in `docker-compose.yml` are for local development only. Do not use them in production environments.

3. **Apply database migrations:**
   ```bash
   dotnet ef database update --project src/Infrastructure
   ```

### Running the Application

```bash
# Build the solution
dotnet build

# Run the API
dotnet run --project src/Api

# Run tests
dotnet test
```

## Architecture

This project follows **Clean Architecture** with **Domain-Driven Design (DDD)** principles, prioritizing maintainability, testability, and performance.

### Project Structure

```
src/
├── Api/              # Presentation layer - Controllers, Middlewares, DI
├── Application/      # Use Cases - Commands, Queries, Handlers, DTOs
├── Domain/           # Core - Entities, Value Objects, Domain Services
└── Infrastructure/   # External - EF Core, Dapper, Repositories

tests/
└── Tests/            # Unit and Integration tests
```

### Layer Responsibilities

| Layer | Responsibility | Dependencies |
|-------|----------------|--------------|
| **Domain** | Entities, Value Objects, Enums, Repository Interfaces, Domain Exceptions | None (zero external framework dependencies) |
| **Application** | Use Cases, orchestration, input validation, transaction coordination. Implements CQRS pattern. | Domain |
| **Infrastructure** | Repository implementations, EF Core configuration, Dapper queries, external service integrations | Domain, Application |
| **Api** | RESTful Controllers, Middlewares, Error handling, Dependency Injection configuration | Application, Infrastructure |

### Key Architectural Principles

1. **Dependencies point inward** - Domain has no external dependencies
2. **Rich Domain Models** - Entities contain business logic, not just data (no anemic models)
3. **CQRS** - Commands (EF Core for writes) separated from Queries (Dapper/Raw SQL for reads)
4. **Result Pattern** - Use Result objects instead of exceptions for flow control

### Design Patterns

The following patterns are mandatory:

- **Repository Pattern:** Abstraction for data access
- **Unit of Work:** Atomic transactions across multiple repositories
- **Domain Events (Notification Pattern):** Decoupled side effects
- **Result Pattern:** Error handling without exceptions for validation

## Technology Stack

### Core Technologies

| Component | Technology |
|-----------|------------|
| Platform | .NET 8 (LTS) |
| Language | C# 12 |
| Database | SQL Server (containerized) |
| ORM (Commands) | Entity Framework Core 8 (Fluent API only, no Data Annotations) |
| Queries (Reports) | Dapper / Raw SQL |
| Migrations | EF Core Migrations |

### Libraries

| Purpose | Library |
|---------|---------|
| Mediator | MediatR |
| Validation | FluentValidation |
| Logging | Serilog |
| Mapping | AutoMapper (restricted to simple query projections) |

### Testing

| Component | Technology |
|-----------|------------|
| Framework | xUnit |
| Mocking | Moq |
| Data Generation | Bogus |

## Code Standards

### Language

- **Code:** All code (variables, classes, methods, comments) must be in **English**
- **Documentation:** Business documentation and commits may be in **Portuguese**

### Naming Conventions

- **Classes/Methods:** PascalCase
- **Variables/Parameters:** camelCase
- **Constants:** UPPER_SNAKE_CASE or PascalCase

### Async/Await

All I/O operations (database, file, network) must use async/await patterns.

### Null Safety

Nullable Reference Types are enabled (`<Nullable>enable</Nullable>`). Handle nulls explicitly.

### Code Quality

- No "magic numbers" or "magic strings" - use constants or enums
- No business logic in Controllers
- No database access from Domain layer
- Use projections for read queries; avoid excessive `Include()` in EF Core

## Testing Requirements

- **Unit Tests:** Required for all domain logic and business rules
- **Integration Tests:** Required for repositories and SQL queries
- **Scope:**
  - Unit Tests: Focus on business rules and entities
  - Integration Tests: Focus on repositories and SQL queries

## Pull Request Process

1. Create a feature branch from `main`
2. Implement your changes following the architecture guidelines
3. Ensure all tests pass: `dotnet test`
4. Ensure code compiles without warnings
5. Submit PR with clear description of changes

## Learning Path

If you're new to the project, follow the implementation phases defined in [LEARNING_ROADMAP.md](./LEARNING_ROADMAP.md).

## Documentation

- [PROJECT_SPEC.md](./PROJECT_SPEC.md) - Business requirements
- [docs/DOMAIN_MODEL.md](./docs/DOMAIN_MODEL.md) - Domain model documentation
- [LEARNING_ROADMAP.md](./LEARNING_ROADMAP.md) - Implementation phases and task sequence
