# Contributing to ERP Core

Thank you for your interest in contributing to ERP Core. This document provides guidelines for contributing to this enterprise ERP project.

## Development Environment Setup

### Prerequisites

- **.NET SDK:** Version 8.0 or higher
- **IDE:** Visual Studio 2022 Professional, JetBrains Rider, or Visual Studio Code with C# extension
- **Docker & Docker Compose:** For running SQL Server
- **SQL Client:** SQL Server Management Studio (SSMS) or Azure Data Studio

### Database Setup

The project uses SQL Server running in a Docker container.

1. **Start the SQL Server container:**
   ```bash
   docker-compose up -d
   ```

2. **Connection Details:**
   - **Host:** `localhost,1433`
   - **Credentials:** See `docker-compose.yml` for local development credentials
   - **Database:** Created via EF Core migrations
   
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

## Architecture Guidelines

This project follows **Clean Architecture** with **Domain-Driven Design (DDD)** principles.

### Project Structure

```
src/
├── Api/              # Presentation layer - Controllers, Middlewares
├── Application/      # Use Cases, Commands, Queries, DTOs
├── Domain/           # Entities, Value Objects, Domain Services
└── Infrastructure/   # EF Core, Dapper, External Services

tests/
└── Tests/            # Unit and Integration tests
```

### Key Principles

1. **Dependencies point inward** - Domain has no external dependencies
2. **Rich Domain Models** - Entities contain business logic, not just data
3. **CQRS** - Commands (EF Core) separated from Queries (Dapper/Raw SQL)
4. **Result Pattern** - Use Result objects instead of exceptions for flow control

For detailed technical specifications, see [TECH_STACK.md](./TECH_STACK.md).

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

Nullable Reference Types are enabled. Handle nulls explicitly.

### Code Quality

- No "magic numbers" or "magic strings" - use constants or enums
- No business logic in Controllers
- No database access from Domain layer

## Testing Requirements

- **Unit Tests:** Required for all domain logic and business rules
- **Integration Tests:** Required for repositories and SQL queries
- **Test Framework:** xUnit with Moq for mocking

## Pull Request Process

1. Create a feature branch from `main`
2. Implement your changes following the architecture guidelines
3. Ensure all tests pass: `dotnet test`
4. Ensure code compiles without warnings
5. Submit PR with clear description of changes

## Learning Path

If you're new to the project, follow the implementation phases defined in [LEARNING_ROADMAP.md](./LEARNING_ROADMAP.md).

## Questions?

Review the following documentation:
- [PROJECT_SPEC.md](./PROJECT_SPEC.md) - Business requirements
- [TECH_STACK.md](./TECH_STACK.md) - Technical specifications
- [docs/DOMAIN_MODEL.md](./docs/DOMAIN_MODEL.md) - Domain model documentation
