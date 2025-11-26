# Enterprise Resource Planning (ERP) Core - Supply Chain Module

## Visão Geral do Projeto
Este repositório contém a implementação de um núcleo de ERP (Enterprise Resource Planning) focado no módulo de Supply Chain (Cadeia de Suprimentos) e Regras Fiscais.

O objetivo deste projeto é demonstrar a capacidade de engenharia de software na construção de soluções corporativas de alta complexidade, robustez e escalabilidade. O sistema foi desenhado para simular desafios reais enfrentados por grandes players do mercado de software de gestão (como Totvs e LG Lugar de Gente), priorizando integridade transacional, performance de banco de dados e arquitetura desacoplada.

## Metodologia de Desenvolvimento
Este projeto utiliza uma metodologia de "IA Supervisionada". O GitHub Copilot atua como Tech Lead, definindo tarefas, revisando implementações e garantindo a aderência aos padrões arquiteturais definidos. O desenvolvedor atua como Engenheiro de Software, responsável pela implementação técnica, tomada de decisão arquitetural e resolução de problemas complexos.

## Arquitetura e Engenharia ("Strategic Overengineering")
O projeto adota intencionalmente uma arquitetura sofisticada para demonstrar domínio técnico sobre padrões de design enterprise, justificando a complexidade pela necessidade de manutenção a longo prazo e testabilidade.

### Stack Tecnológica
* **Core:** .NET 8 (C#)
* **Database:** SQL Server (Linux/Docker container)
* **Orm/Data Access:** Abordagem Híbrida
    * *Entity Framework Core:* Para operações de comando (Create, Update, Delete) e mapeamento de domínio.
    * *Dapper / Raw SQL:* Para consultas de alta performance e relatórios complexos, simulando a exigência de mercado por otimização de queries.

### Padrões Adotados
* **Clean Architecture:** Separação estrita de responsabilidades em camadas (Domain, Application, Infrastructure, Presentation).
* **Domain-Driven Design (DDD):** Uso de Entidades Ricas, Value Objects, Aggregates e Domain Events para encapsular regras de negócio complexas (ex: validação fiscal, bloqueio de estoque).
* **CQRS (Command Query Responsibility Segregation):** Separação lógica entre operações de leitura e escrita para otimização de performance.
* **Repository & Unit of Work Patterns:** Abstração da persistência de dados.
* **Result Pattern:** Tratamento de erros funcional, evitando o uso excessivo de exceções para fluxo de controle.

## Funcionalidades do Módulo
1.  **Gestão de Estoque Transacional:** Controle de movimentações com concorrência otimista e pessimista.
2.  **Motor de Regras Fiscais:** Cálculo dinâmico de impostos baseados em parametrização, simulando a complexidade tributária brasileira.
3.  **Relatórios de Inteligência de Negócio:** Consultas analíticas otimizadas (Curva ABC, Giro de Estoque) executadas via SQL nativo.

## Configuração do Ambiente

### Pré-requisitos
* .NET SDK 8.0+
* Docker & Docker Compose
* SQL Server Management Studio (SSMS) ou Azure Data Studio

### Execução
1.  Clone o repositório.
2.  Suba a infraestrutura de banco de dados:
    ```bash
    docker-compose up -d
    ```
3.  Aplique as migrações iniciais (se aplicável) ou execute os scripts de init do banco:
    ```bash
    dotnet ef database update
    ```
4.  Execute a aplicação:
    ```bash
    dotnet run --project src/ErpCore.Api
    ```

## Licença
Este projeto é destinado a fins de portfólio acadêmico e profissional.
