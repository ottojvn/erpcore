# Roadmap de Implementação e Plano de Tarefas

## Visão Geral
Este documento define a sequência lógica de implementação do ERP Core. O GitHub Copilot deve utilizar este roadmap para alocar tarefas (tasks) de forma incremental. O avanço para a próxima fase só é permitido após a conclusão satisfatória de todos os itens da fase atual (Definition of Done).

---

## FASE 0: Estruturação e Infraestrutura Inicial
**Objetivo:** Estabelecer a fundação da Clean Architecture e o ambiente de desenvolvimento.

1.  **Task 0.1:** Configurar a Solução `.sln` e os Projetos seguindo a estrutura de camadas:
    * `ErpCore.Domain` (Class Library)
    * `ErpCore.Application` (Class Library)
    * `ErpCore.Infrastructure` (Class Library)
    * `ErpCore.Api` (Web API)
    * `ErpCore.Tests` (xUnit)
2.  **Task 0.2:** Configurar `docker-compose.yml` para subir o container do banco de dados.
3.  **Task 0.3:** Configurar injeção de dependência básica e Swagger na API.

---

## FASE 1: Modelagem de Domínio (Pure DDD)
**Objetivo:** Implementar o núcleo do negócio sem dependências externas (Zero Database, Zero API). Foco em lógica pura.

1.  **Task 1.1 - Entidades Base:** Criar classes base abstratas (`Entity`, `AggregateRoot`, `ValueObject`) para suportar igualdade e validação.
2.  **Task 1.2 - Agregado de Produto:**
    * Implementar entidade `Product` e Value Objects (`Sku`, `Dimensions`).
    * Implementar validações de invariantes (ex: SKU não pode ser vazio, Peso > 0).
3.  **Task 1.3 - Agregado de Estoque:**
    * Implementar entidade `StockTransaction` (Kardex) e `Warehouse`.
    * Lógica de movimentação (Entrada/Saída) garantindo imutabilidade das transações passadas.
4.  **Task 1.4 - Testes de Unidade (Domain):** Criar testes para validar as regras de negócio acima (ex: impedir estoque negativo se configurado).

---

## FASE 2: Persistência e Infraestrutura de Dados
**Objetivo:** Mapear o domínio para o banco de dados utilizando EF Core para escrita e preparar o terreno para alta performance.

1.  **Task 2.1 - Mapeamento ORM:** Implementar `IEntityTypeConfiguration` para cada entidade do domínio.
    * *Restrição:* Usar Fluent API estrita. Definir tipos de dados, índices e chaves estrangeiras explicitamente.
2.  **Task 2.2 - Contexto e Migrations:** Configurar `ErpContext`, gerar e aplicar a primeira Migration.
3.  **Task 2.3 - Repositórios (Write Side):** Implementar o padrão Repository (`IProductRepository`, `IStockRepository`) usando EF Core.
4.  **Task 2.4 - Unit of Work:** Implementar controle transacional.

---

## FASE 3: Application Layer & CQRS (Command Stack)
**Objetivo:** Implementar os Casos de Uso que alteram o estado do sistema.

1.  **Task 3.1 - Handlers de Cadastro:** Implementar Commands para `CreateProduct` e `UpdateProductPrice` usando MediatR.
2.  **Task 3.2 - Pipeline de Validação:** Configurar FluentValidation para validar os DTOs de entrada antes de atingir o domínio.
3.  **Task 3.3 - Motor de Movimentação:** Implementar o Use Case `RegisterStockMovement`.
    * Este Handler deve coordenar a regra de negócio complexa de verificação de saldo antes da baixa.
4.  **Task 3.4 - API Endpoints (Escrita):** Criar Controllers para expor os Commands via HTTP POST/PUT.

---

## FASE 4: O Motor Fiscal (Complexidade de Engenharia)
**Objetivo:** Implementar o cálculo dinâmico de impostos.

1.  **Task 4.1 - Entidades Fiscais:** Modelar `TaxRule` (Regra Fiscal), `TaxZone` (Estado/Região) e `Ncm`.
2.  **Task 4.2 - Serviço de Domínio (Tax Calculator):** Criar um `Domain Service` que recebe um Pedido e calcula os impostos baseados nas regras vigentes.
    * *Desafio:* Implementar Pattern Strategy ou Chain of Responsibility para seleção de regras.
3.  **Task 4.3 - Persistência do Snapshot:** Garantir que o pedido salvo contenha os valores calculados imutáveis.

---

## FASE 5: Query Stack & Relatórios (SQL Performance)
**Objetivo:** Implementar a leitura de dados focada em performance extrema (Dapper).

1.  **Task 5.1 - Stack de Leitura:** Configurar Dapper na camada de Infraestrutura.
2.  **Task 5.2 - Relatório Kardex:** Criar query SQL crua (Raw SQL) para extrair o extrato de movimentação de um produto, paginado no banco.
3.  **Task 5.3 - Dashboard de Vendas (Curva ABC):** Implementar query complexa utilizando Window Functions para classificar produtos.
4.  **Task 5.4 - API Endpoints (Leitura):** Expor os relatórios via HTTP GET.

---

## FASE 6: Resiliência e Auditoria (Toque Final)
**Objetivo:** Adicionar características de software Enterprise.

1.  **Task 6.1 - Log de Auditoria:** Implementar interceptor no EF Core para gravar quem alterou o quê (`AuditLog`).
2.  **Task 6.2 - Tratamento de Concorrência:** Implementar controle de versão (`RowVersion`) para evitar que dois usuários alterem o mesmo estoque simultaneamente (Optimistic Concurrency).
3.  **Task 6.3 - Middleware de Erros:** Implementar Global Exception Handler para retornar problemas de validação no padrão `ProblemDetails`.

---

## Critérios de Aceite por Fase (DoD)
Para considerar uma fase concluída, o Copilot deve verificar:
1.  O código compila sem warnings.
2.  Os testes automatizados relacionados à fase estão passando.
3.  A estrutura de pastas respeita a Clean Architecture.
4.  Nenhuma "Magic String" ou regra de negócio vazada na API.
