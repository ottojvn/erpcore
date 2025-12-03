# ERP Core & Expansion Platform

**Enterprise Resource Planning system designed for evolutionary architecture: from Modular Monolith to Distributed Cloud-Native solution.**

## Project Overview

This repository hosts the implementation of an enterprise-grade ERP system focused on Supply Chain and Tax Engine modules. The project is structured in two distinct strategic phases to ensure architectural maturity before complexity scaling.

### Phase 1: The Core (Current Focus)

Development of a robust **Modular Monolith Backend** using **.NET 8** and **Clean Architecture**. This phase prioritizes:

- **Domain Modeling:** Complex business rules (DDD) for Inventory and Fiscal logic.
- **Data Integrity:** ACID transactions and Concurrency Control.
- **Performance:** Optimized SQL/Dapper queries for reporting.

### Phase 2: The Expansion (Planned Roadmap)

Evolution of the Core into a **Full Stack, Distributed System**. This phase activates only after Core stabilization and includes:

1. **Frontend Expansion:** Single Page Application (SPA) for backoffice operations.
2. **DevOps Expansion:** CI/CD Pipelines and Cloud Native infrastructure.
3. **Architecture Expansion:** Event-Driven decoupling and Data Engineering pipelines.

---

## Key Features (Core Phase)

- **Transactional Inventory:** Multi-warehouse management with immutable movement logs (Kardex).
- **Dynamic Tax Engine:** Configurable rule-based tax calculations.
- **Hybrid Data Access:** CQRS pattern using EF Core for writes and Dapper for high-performance reads.
- **Audit & Security:** Granular audit logs and optimistic concurrency control.

## Strategic Expansion Plan (Post-Core)

The following modules are specified in the [LEARNING_ROADMAP.md](./LEARNING_ROADMAP.md) and will be implemented sequentially:

| Expansion Module | Focus | Technologies |
|------------------|-------|--------------|
| **I. Web Interface** | User Experience | Angular/React, Dashboards, Error Handling |
| **II. DevOps & Cloud** | Automation | GitHub Actions, Docker (Prod), Terraform, AWS/Azure |
| **III. Data & Events** | Scalability | RabbitMQ (Messaging), Python (ETL), PowerBI, IoT Simulators |

---

## Quick Start (Core Environment)

To run the current Core Backend version:

### Prerequisites

- .NET SDK 10.0+
- Docker & Docker Compose

### Setup Steps

1. **Clone the repository:**

    ```bash
    git clone <repository-url>
    cd erpcore
    ```

2. **Start the infrastructure:**

    ```bash
    docker-compose up -d
    ```

3. **Apply database migrations:**

    ```bash
    dotnet ef database update --project src/Infrastructure
    ```

4. **Run the API:**

    ```bash
    dotnet run --project src/Api
    ```

---

## Documentation

| Document | Description |
|----------|-------------|
| [CONTRIBUTING.md](./CONTRIBUTING.md) | **Developer Guide:** Setup, architecture standards, tech stack for Core and Expansion phases. |
| [LEARNING_ROADMAP.md](./LEARNING_ROADMAP.md) | **Execution Plan:** Step-by-step task list covering Phases 0-6 (Core) and 7-9 (Expansion). |
| [PROJECT_SPEC.md](./PROJECT_SPEC.md) | **Functional Specs:** Detailed business rules and expansion requirements. |
| [docs/DOMAIN_MODEL.md](./docs/DOMAIN_MODEL.md) | **Domain Logic:** Entities, aggregates, and invariants. |

## Development Methodology

This project utilizes an "AI-Supervised" methodology. GitHub Copilot acts as the Technical Lead, ensuring adherence to the architecture defined in the roadmap. Developers must strictly follow the sequence of tasks in [LEARNING_ROADMAP.md](./LEARNING_ROADMAP.md).
