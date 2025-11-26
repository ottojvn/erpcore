# Definição de Stack Tecnológica e Padrões de Arquitetura

## 1. Visão Técnica
Este projeto segue uma abordagem de "Engenharia de Software Corporativa", priorizando manutenibilidade, testabilidade e performance em detrimento de velocidade de desenvolvimento inicial. A arquitetura foi desenhada para resolver problemas de domínios complexos (DDD) e alta volumetria de dados.

## 2. Core Technologies

### 2.1. Runtime e Linguagem
* **Plataforma:** .NET 8 (LTS).
* **Linguagem:** C# 12.
* **Paradigma:** Orientação a Objetos estrita para modelagem de domínio, com uso de recursos funcionais (Records, Pattern Matching) onde apropriado para imutabilidade.

### 2.2. Banco de Dados e Persistência
* **SGBD:** SQL Server 2022 (Executado via Docker Container).
* **Estratégia de Acesso a Dados (Híbrida):**
    * **Command Stack (Escrita/Domínio):** *Entity Framework Core 8*. Utilizado exclusivamente para mapeamento objeto-relacional (ORM) do Domínio e persistência de transações de negócio. Configuração via *Fluent API* (proibido Data Annotations nas entidades).
    * **Query Stack (Leitura/Relatórios):** *Dapper* ou *ADO.NET Puro*. Utilizado para consultas de alta performance, relatórios gerenciais e dashboards. O uso de SQL cru (Raw SQL) é obrigatório para demonstrar otimização de queries.
* **Gerenciamento de Migrações:** EF Core Migrations para estrutura DDL.

## 3. Arquitetura de Software

### 3.1. Padrão Arquitetural
O projeto adota a **Clean Architecture** (Arquitetura Cebola/Hexagonal), com dependências apontando para o centro (Domínio).

* **Layer 1 - Domain:** Núcleo do projeto. Contém Entidades, Value Objects, Enums, Interfaces de Repositório e Exceções de Domínio. Dependência zero de frameworks externos.
* **Layer 2 - Application:** Casos de Uso (Use Cases). Orquestra o fluxo de dados, validação de entrada e coordenação de transações. Implementa o padrão CQRS (Command Query Responsibility Segregation).
* **Layer 3 - Infrastructure:** Implementação de interfaces (Repositórios, Serviços de E-mail, Integrações). Contém a configuração do EF Core e Dapper.
* **Layer 4 - Presentation (API):** Controllers RESTful, Middlewares de tratamento de erro e configuração de DI (Injeção de Dependência).

### 3.2. Design Patterns Obrigatórios
* **Repository Pattern:** Para abstração de acesso a dados.
* **Unit of Work:** Para garantir atomicidade de transações que envolvem múltiplos repositórios.
* **Notification Pattern (Domain Events):** Para desacoplamento de efeitos colaterais (ex: enviar e-mail após confirmar pedido).
* **Result Pattern:** Substituição do lançamento de Exceções para controle de fluxo de erros de validação (utilizando lib *FluentResults* ou implementação própria).

## 4. Bibliotecas e Ferramentas Auxiliares

### 4.1. Bibliotecas do Ecossistema
* **MediatR:** Implementação do padrão Mediator para desacoplar a camada de API da camada de Aplicação (Handlers).
* **FluentValidation:** Validação robusta de DTOs e Commands fora das entidades de domínio.
* **Serilog:** Logging estruturado (Sink para Console/File/Seq).
* **AutoMapper:** Mapeamento entre Entidades de Domínio e ViewModels/DTOs (uso restrito a consultas simples).

### 4.2. Testes Automatizados
* **Framework:** xUnit.
* **Mocking:** Moq.
* **Data Generation:** Bogus (para geração de massa de dados realista nos testes de carga).
* **Escopo:**
    * Testes de Unidade: Focados nas Regras de Negócio e Entidades.
    * Testes de Integração: Focados nos Repositórios e Consultas SQL.

## 5. Convenções de Código (Style Guide)
* **Idioma:** Código (Variáveis, Classes, Comentários) em **Inglês**. Documentação de negócio (Commits, Readmes) em **Português**.
* **Async/Await:** Obrigatório em todas as operações de I/O (Banco de Dados, Arquivos).
* **Tratamento de Nulos:** *Nullable Reference Types* habilitado (`<Nullable>enable</Nullable>`).
* **Clean Code:** Proibido "Magic Numbers" ou "Magic Strings". Uso extensivo de Enums e Constantes.

## 6. Ambiente de Desenvolvimento
* **Docker Compose:** Orquestração do SQL Server e ferramentas de observabilidade.
* **IDE:** Visual Studio 2022 ou JetBrains Rider ou Visual Studio Code.
