# Basboosify (WIP) — Microservices eCommerce

Basboosify is a work-in-progress eCommerce platform built using **ASP.NET Core microservices**. Each service runs in its own container, communicates via REST and asynchronous messaging, and follows an **event-driven** architecture for scalability and loose coupling.

Currently, two services are public: **Products** and **Users**. More are planned

---

## Repositories

| Service | Repo | Tech Stack | Current Features |
|---|---|---|---|
| **Products Service** | [BasboosifySolution.ProductsService](https://github.com/Aseel-Sh/BasboosifySolution.ProductsService) | C# (.NET 8), ASP.NET Core Web API, MySQL, Docker | Manages product catalog with CRUD endpoints, uses a dedicated MySQL database, containerized with Docker. Ready for RabbitMQ integration. |
| **Users Service** | [BasboosifySolution.UsersService](https://github.com/Aseel-Sh/BasboosifySolution.UsersService) | C# (.NET 8), ASP.NET Core Web API, PostgreSQL, Docker | Handles user accounts and profiles, secured endpoints, and per-service data storage. Planned event publishing on user registration/update. |

---

## Target Architecture (Planned vs Current)

- **API Gateway (Ocelot)** — *Not implemented yet*
  For routing, JWT validation, and claims forwarding.

- **Authentication & Authorization (Entra ID B2C + scopes/roles)** — *Not implemented yet*   
  For centralized authentication and per-service role-based access.

- **Event Messaging (RabbitMQ, pub/sub, idempotent consumers)** — *Not implemented yet*   
  To enable async communication and eventual consistency.

- **Resilience (Polly: retries, timeout, circuit breaker)** — *Not implemented yet*   
  For fault tolerance in inter-service calls.

- **Data Storage (DB-per-service)** — **Implemented**  
  - Products → **MySQL**  
  - Users → **PostgreSQL**  

- **Caching (Redis)** — *Not implemented yet*   
  For high-frequency reads and performance optimization.

- **Observability (structured logs, correlation IDs, tracing)** — *Not implemented yet*   
  For monitoring and diagnostics across services.

- **Documentation (OpenAPI per service; aggregated later)** — *Not implemented yet*   
  Individual service APIs will be documented and aggregated via gateway.

### Event Conventions — *Not implemented yet* 
- Append-only events
- Include `version` and `occurredAt` fields
- Consumers must be idempotent
