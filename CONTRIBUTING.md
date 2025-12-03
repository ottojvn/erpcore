# Contributing to ERP Core & Expansion

Thank you for your interest in contributing to ERP Core. This document serves as the comprehensive Developer Guide.

**Project Phases:**

1. **Core Phase (Current):** Focus on building the modular monolith backend (.NET 10).
2. **Expansion Phase (Future):** Evolution into a Full Stack, Cloud-Native, and Event-Driven system.

> **Important:** Contributions to the **Expansion Phase** should only commence after the Core Phase meets its "Definition of Done" as outlined in [LEARNING_ROADMAP.md](./LEARNING_ROADMAP.md).

---

## 1. Development Environment Setup

### 1.1. Core Phase Prerequisites (Required Immediately)

- **.NET SDK:** Version 10.0 or higher
- **IDE:** Visual Studio 2026, VS Code, or Rider
- **Docker & Docker Compose:** For running SQL Server
- **SQL Client:** SSMS, DBeaver, or Visual Studio Code with MSSQL extension.

### 1.2. Expansion Phase Prerequisites (Future Use)

- **Node.js & npm:** LTS Version (for Frontend Expansion)
- **Python:** 3.14+ (for Data/IoT Expansion)
- **Cloud CLI:** AWS CLI or Azure CLI (for DevOps Expansion)
- **Terraform:** For Infrastructure as Code

### 1.3. Database Setup (Core)

The project uses SQL Server running in a Docker container.

1. **Start the database container:**

    ```bash
    docker-compose up -d
    ```

2. **Apply database migrations:**

    ```bash
    dotnet ef database update --project src/Infrastructure
    ```

### 1.4. Running the Application

```bash
# Build the solution
dotnet build

# Run the API (Core)
dotnet run --project src/Api

# Run Tests
dotnet test
```

## 2. Architecture Standards

The project follows **Clean Architecture** with **Domain-Driven Design (DDD)**. In the Expansion Phase, this architecture evolves to support Event-Driven patterns.

### 2.1. Core Architecture (Monolith)

| Layer | Responsibility | Dependencies |
| ----- | -------------- | ------------ |
| **Domain** | Entities, Value Objects, Logic | None |
| **Application** | Use Cases (CQRS), Orchestration | Domain |
| **Infrastructure** | EF Core, Dapper, External Services | Domain, Application |
| **Api** | Controllers | Middlewares |

### 2.2. Expansion Architecture (Distributed)

During the Expansion Phase, the system introduces:

- **Frontend Layer:** SPA (Single Page Application) communicating via REST.
- **Worker Services:** Background services for asynchronous processing (e.g., Tax Calculation).
- **Data Pipeline:** Python scripts for ETL and Analytics.

## 3. Technology Stack

### 3.1. Core Stack (Backend)

| Component | Technology |
| --------- | ---------- |
| Platform | .NET 10 (LTS) |
| Language | C# 14 |
| ORM (Write) | Entity Framework Core 10 |
| ORM (Read) | Dapper |
| Database | SQL Server (Containerized) |
| Validation | FluentValidation |
| Logging | Serilog |

### 3.2. Expansion Stack (Planned)

| Expansion Module | Technology | Purpose |
| ---------------- | ---------- | ------- |
| Frontend | Angular or React | Web Interface / Backoffice |
| DevOps | GitHub Actions | CI/CD Pipelines |
| Infrastructure | Terraform / Docker | IaC and Cloud Provisioning |
| Messaging | RabbitMQ / Kafka | Asynchronous Event Bus
| Data/IoT | Python 3.x | ETL Scripts and IoT Simulation |

## 4. Code Standards

### 4.1. General Rules

- **Language:** Code must be in **English**. Commits/Docs may be in Portuguese.
- **Null Safety:** Enable ```<Nullable>enable</Nullable>```. Handle nulls explicitly.
- **Async/Await:** Mandatory for all I/O operations.

### 4.2. C# Conventions (Core)

- **Classes/Methods:** PascalCase
- **Variables:** camelCase
- **Interfaces:** IPascalCase

### 4.3. Expansion Conventions (Future)

- **TypeScript (Frontend):** Follow strict typing. Use defined ESLint rules.
- **Python (Data):** Follow PEP 8 standards.
- **Terraform:** Follow HashiCorp recommended naming conventions.

## 5. Testing Strategy

### 5.1. Core Testing

- **Unit Tests (xUnit):** Required for all Domain logic.
- **Integration Tests:** Required for Repositories and custom SQL queries.

### 5.2. Expansion Testing

- **Frontend:** Unit tests for Components; E2E tests for critical flows (Cypress/Playwright).
- **Infrastructure:** ```terraform validate``` and plan review.
- **Data:** Unit tests for Python ETL logic.

## 6. Pull Request & Workflow

1. **Branching:** Create feature branches from main.
2. **Phase Check:** Ensure you are working on tasks allowed by the current phase in LEARNING_ROADMAP.md.
3. **Verification:**

- Code compiles without warnings.
- ```dotnet test``` passes (Core).
- Relevant linters pass (Expansion).
- **Submission:** Submit PR with a clear description of changes.

# 7. Documentation References

- [PROJECT_SPEC.md](/PROJECT_SPEC.md) - Functional specs (Core & Expansion).
- [docs/DOMAIN_MODEL.md](/docs/DOMAIN_MODEL.md) - Core Domain business rules.
- [LEARNING_ROADMAP.md](LEARNING_ROADMAP.md) - Task sequence and Phasing.
