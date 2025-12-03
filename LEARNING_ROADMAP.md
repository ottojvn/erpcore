# Roadmap de Implementação e Plano de Tarefas

## Visão Geral e Estratégia
Este documento define a sequência lógica de implementação do **ERP Core** e seu roteiro de **Expansão**. O desenvolvimento está estruturado em duas etapas macro:

1.  **Etapa Core (Fases 0 a 6):** Construção do backend monolítico, focado em regras de negócio, integridade e performance.
2.  **Etapa Expansão (Fases 7 a 9):** Evolução para Full Stack, DevOps (Cloud) e Arquitetura Distribuída.

> **Regra de Execução:** O avanço para a **Etapa Expansão** só é permitido após a conclusão satisfatória (Definition of Done) de todas as tarefas da **Etapa Core**.

---

# PARTE I: ERP CORE (Backend & Regras de Negócio)

## FASE 0: Estruturação e Infraestrutura Inicial
**Objetivo:** Estabelecer a fundação da Clean Architecture e o ambiente de desenvolvimento.

1.  **Task 0.1:** Configurar a Solução `.sln` e Projetos (Domain, Application, Infrastructure, Api, Tests).
2.  **Task 0.2:** Configurar `docker-compose.yml` para banco de dados SQL Server.
3.  **Task 0.3:** Configurar injeção de dependência básica e Swagger.

## FASE 1: Modelagem de Domínio (Pure DDD)
**Objetivo:** Implementar o núcleo do negócio sem dependências externas.

1.  **Task 1.1 - Entidades Base:** Classes abstratas `Entity`, `AggregateRoot`, `ValueObject`.
2.  **Task 1.2 - Agregado de Produto:** Entidade `Product` e Value Objects (`Sku`, `Dimensions`).
3.  **Task 1.3 - Agregado de Estoque:** `StockTransaction` (Kardex) e `Warehouse` com lógica de imutabilidade.
4.  **Task 1.4 - Testes de Unidade (Domain):** Validação de invariantes de negócio.

## FASE 2: Persistência e Infraestrutura de Dados
**Objetivo:** Mapear o domínio para o banco de dados via EF Core.

1.  **Task 2.1 - Mapeamento ORM:** Configuração via Fluent API estrita.
2.  **Task 2.2 - Contexto e Migrations:** `ErpContext` e aplicação da primeira migration.
3.  **Task 2.3 - Repositórios (Write Side):** Implementação do padrão Repository.
4.  **Task 2.4 - Unit of Work:** Controle transacional.

## FASE 3: Application Layer & CQRS (Command Stack)
**Objetivo:** Implementar os Casos de Uso de escrita.

1.  **Task 3.1 - Handlers de Cadastro:** Commands via MediatR (`CreateProduct`, `UpdateProductPrice`).
2.  **Task 3.2 - Pipeline de Validação:** Configuração do FluentValidation.
3.  **Task 3.3 - Motor de Movimentação:** Handler complexo `RegisterStockMovement`.
4.  **Task 3.4 - API Endpoints (Escrita):** Controllers para exposição dos Commands.

## FASE 4: O Motor Fiscal (Complexidade de Engenharia)
**Objetivo:** Implementar o cálculo dinâmico de impostos.

1.  **Task 4.1 - Entidades Fiscais:** `TaxRule`, `TaxZone` e `Ncm`.
2.  **Task 4.2 - Serviço de Domínio (Tax Calculator):** Domain Service com Strategy Pattern para cálculo de impostos.
3.  **Task 4.3 - Persistência do Snapshot:** Gravação de valores calculados imutáveis no pedido.

## FASE 5: Query Stack & Relatórios (SQL Performance)
**Objetivo:** Implementar leitura de alta performance via Dapper.

1.  **Task 5.1 - Stack de Leitura:** Configuração do Dapper.
2.  **Task 5.2 - Relatório Kardex:** Query Raw SQL para extrato de movimentação.
3.  **Task 5.3 - Dashboard de Vendas (Curva ABC):** Query com Window Functions.
4.  **Task 5.4 - API Endpoints (Leitura):** Exposição dos relatórios via GET.

## FASE 6: Resiliência e Auditoria (Finalização Core)
**Objetivo:** Adicionar características de software Enterprise.

1.  **Task 6.1 - Log de Auditoria:** Interceptor EF Core para rastreamento de mudanças.
2.  **Task 6.2 - Tratamento de Concorrência:** Implementação de Optimistic Concurrency (`RowVersion`).
3.  **Task 6.3 - Middleware de Erros:** Global Exception Handler com padrão `ProblemDetails`.

---

# PARTE II: EXPANSÃO ESTRATÉGICA (Pós-Core)

## FASE 7: Expansão Frontend (Full Stack)
**Objetivo:** Transformar a API em uma solução completa com interface visual (Angular/React).

1.  **Task 7.1 - Setup SPA:** Inicializar projeto frontend e configurar comunicação HTTP com a API Core.
2.  **Task 7.2 - Módulos de Cadastro:** Implementar telas de CRUD conectadas aos Endpoints da Fase 3.
3.  **Task 7.3 - Dashboard Executivo:** Implementar visualização gráfica dos dados da Fase 5 (Curva ABC e Giro de Estoque).
4.  **Task 7.4 - Integração de Erros:** Implementar interceptor no frontend para ler e exibir notificações baseadas no `ProblemDetails` da Fase 6.

## FASE 8: Expansão Infraestrutura (DevOps & Cloud)
**Objetivo:** Automatizar o ciclo de vida e migrar para ambiente produtivo em nuvem.

1.  **Task 8.1 - Pipeline de CI:** Criar workflow no GitHub Actions para Build e Testes Automatizados a cada Pull Request.
2.  **Task 8.2 - Containerização de Produção:** Otimizar `Dockerfile` para ambiente produtivo (Multi-stage build).
3.  **Task 8.3 - Pipeline de CD:** Configurar deploy automatizado para provedor de nuvem (AWS/Azure) via Terraform ou CLI.

## FASE 9: Expansão Arquitetural (Dados & Eventos)
**Objetivo:** Desacoplar serviços críticos e implementar inteligência de dados.

1.  **Task 9.1 - Refatoração Event-Driven:** Implementar RabbitMQ. Refatorar o fluxo de "Pedido Aprovado" para publicar um evento, desacoplando o cálculo fiscal (processamento assíncrono).
2.  **Task 9.2 - Engenharia de Dados (ETL):** Criar script Python para extração noturna de dados do SQL Server e conversão para formato Parquet.
3.  **Task 9.3 - Simulação IoT:** Implementar script Python/MQTT para simular entrada automatizada de estoque via sensores.

---

## Critérios de Aceite Gerais (DoD)
Para considerar qualquer fase (Core ou Expansão) concluída:
1.  O código compila sem warnings.
2.  Os testes automatizados relacionados à fase estão passando.
3.  A estrutura respeita os padrões arquiteturais definidos (Clean Arch para Core, Padrões de Componentes para Frontend).
4.  Nenhuma credencial ou "Magic String" hardcoded.
