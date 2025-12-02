# ERP Core - Supply Chain Module

Enterprise Resource Planning system focused on Supply Chain and Tax Engine modules.

## Overview

This repository implements an enterprise-grade ERP core designed to handle complex supply chain operations, multi-warehouse inventory management, and dynamic tax calculations. The system demonstrates real-world challenges faced by enterprise software vendors, prioritizing transactional integrity, database performance, and decoupled architecture.

### Key Features

- **Transactional Inventory Management:** Stock movements with optimistic/pessimistic concurrency control
- **Dynamic Tax Engine:** Rule-based tax calculation simulating Brazilian tax complexity
- **Business Intelligence Reports:** Optimized analytical queries (ABC Curve, Inventory Turnover) using native SQL

## Quick Start

### Prerequisites

- .NET SDK 8.0+
- Docker & Docker Compose
- SQL Server Management Studio (SSMS) or Azure Data Studio

### Setup

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd erpcore
   ```

2. **Start SQL Server (Docker):**
   ```bash
   docker-compose up -d
   ```
   
   This starts SQL Server 2022 accessible at `localhost,1433`.
   
   > **Note:** Default credentials for local development are defined in `docker-compose.yml`. Do not use these credentials in production.

3. **Apply database migrations:**
   ```bash
   dotnet ef database update --project src/Infrastructure
   ```

4. **Run the application:**
   ```bash
   dotnet run --project src/Api
   ```

5. **Run tests:**
   ```bash
   dotnet test
   ```

## Architecture

This project follows **Clean Architecture** with **Domain-Driven Design (DDD)** principles.

```
src/
├── Api/              # Presentation - Controllers, Middlewares, DI
├── Application/      # Use Cases - Commands, Queries, Handlers
├── Domain/           # Core - Entities, Value Objects, Domain Services
└── Infrastructure/   # External - EF Core, Dapper, Repositories
```

### Technology Stack

| Layer | Technology |
|-------|------------|
| Runtime | .NET 8 / C# 12 |
| Database | SQL Server 2022 (Docker) |
| ORM (Commands) | Entity Framework Core 8 |
| Query (Reports) | Dapper / Raw SQL |
| Validation | FluentValidation |
| Mediator | MediatR |
| Testing | xUnit, Moq |

### Design Patterns

- **CQRS:** Separate read/write stacks for performance optimization
- **Repository & Unit of Work:** Data access abstraction
- **Result Pattern:** Functional error handling without exceptions
- **Domain Events:** Decoupled side effects

## Documentation

| Document | Description |
|----------|-------------|
| [CONTRIBUTING.md](./CONTRIBUTING.md) | Development setup and contribution guidelines |
| [docs/DOMAIN_MODEL.md](./docs/DOMAIN_MODEL.md) | Domain entities, aggregates, and business rules |
| [PROJECT_SPEC.md](./PROJECT_SPEC.md) | Functional requirements specification |
| [TECH_STACK.md](./TECH_STACK.md) | Technical architecture decisions |
| [LEARNING_ROADMAP.md](./LEARNING_ROADMAP.md) | Implementation phases and task sequence |

## Development Methodology

This project uses an "AI-Supervised" methodology where GitHub Copilot acts as a Tech Lead, defining tasks, reviewing implementations, and ensuring adherence to architectural standards. The developer acts as the Software Engineer, responsible for technical implementation and architectural decisions.

## License

This project is licensed under the Apache License 2.0 - see [LICENSE](./LICENSE) for details.
