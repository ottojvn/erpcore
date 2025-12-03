# ERP Core - Supply Chain Module

Enterprise Resource Planning system focused on Supply Chain and Tax Engine modules.

## Overview

This repository implements an enterprise-grade ERP core designed to handle complex supply chain operations, multi-warehouse inventory management, and dynamic tax calculations. The system prioritizes transactional integrity, database performance, and decoupled architecture.

### Key Features

- **Transactional Inventory Management:** Stock movements with concurrency control
- **Dynamic Tax Engine:** Rule-based tax calculation
- **Business Intelligence Reports:** Optimized analytical queries using native SQL

## Quick Start

### Prerequisites

- .NET SDK 8.0+
- Docker & Docker Compose

### Setup

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd erpcore
   ```

2. **Start the database:**
   ```bash
   docker-compose up -d
   ```

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

For detailed setup instructions and architecture guidelines, see [CONTRIBUTING.md](./CONTRIBUTING.md).

## Documentation

| Document | Description |
|----------|-------------|
| [CONTRIBUTING.md](./CONTRIBUTING.md) | Developer guide: setup, architecture, technology stack, and contribution guidelines |
| [docs/DOMAIN_MODEL.md](./docs/DOMAIN_MODEL.md) | Domain entities, aggregates, and business rules |
| [PROJECT_SPEC.md](./PROJECT_SPEC.md) | Functional and non-functional requirements specification |
| [LEARNING_ROADMAP.md](./LEARNING_ROADMAP.md) | Implementation phases and task sequence |

## Development Methodology

This project uses an "AI-Supervised" methodology where GitHub Copilot acts as a Tech Lead, defining tasks, reviewing implementations, and ensuring adherence to architectural standards.

## License

This project is licensed under the Apache License 2.0 - see [LICENSE](./LICENSE) for details.
