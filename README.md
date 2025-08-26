# Basboosify (WIP) — Microservices eCommerce

Basboosify is a work-in-progress eCommerce platform built using **ASP.NET Core microservices**. Each service runs in its own container, communicates via REST and asynchronous messaging, and follows an **event-driven** architecture for scalability and loose coupling.

Currently, two services are public: **Products** and **Users**. More are planned

---

## Repositories

| Service | Repo | Tech Stack | Current Features |
|---|---|---|---|
| **Products Service** | [BasboosifySolution.ProductsService](https://github.com/Aseel-Sh/BasboosifySolution.ProductsService) | C# (.NET 8), ASP.NET Core Web API, **MySQL**, Docker | Manages product catalog with CRUD endpoints, dedicated MySQL database, containerized with Docker, seeded with demo data. |
| **Users Service** | [BasboosifySolution.UsersService](https://github.com/Aseel-Sh/BasboosifySolution.UsersService) | C# (.NET 8), ASP.NET Core Web API, **PostgreSQL**, Docker | Handles user accounts and profiles, dedicated Postgres database, containerized with Docker. |
| **Orders Service** | [BasboosifySolution.OrdersService](https://github.com/Aseel-Sh/BasboosifySolution.OrdersService) | C# (.NET 8), ASP.NET Core Web API, **MongoDB**, Docker | **WIP.** API project and layered structure in place with a Dockerfile to containerize the service. |

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
  - Orders → **MongoDB**
  
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

## How to Run (Users/Recruiters)

You **do not** need the source code. This repo contains a `docker-compose.yml` that pulls prebuilt images from Docker Hub and starts the databases with schema/seed.

### Prerequisites
- Install [Docker](https://docs.docker.com/get-docker/) (includes Docker Compose)
- Clone this repo:
  ```bash
  git clone https://github.com/Aseel-Sh/basboosify-hub.git
  cd basboosify-hub
  ```

### Repo structure

After cloning `basboosify-hub`, you should see:

```
basboosify-hub/
├─ docker-compose.yml              # Orchestration file
├─ mysql-init/                     # MySQL init scripts
│  └─ db.sql                       # Creates Products DB + seeds 12 sample products
├─ postgres-init/                  # PostgreSQL init scripts
│  ├─ sql1.sql                     # Creates BasboosifyUsers database
│  └─ sql2.sql                     # Creates Users table
└─ README.md                       # Project overview + run instructions
```

- `mysql-init/` and `postgres-init/` are automatically mounted into the DB containers on startup.  
- The `.sql` scripts run only **the first time** the containers create fresh volumes.  

### Start
```bash
docker compose up
```

### What this does
- Pulls microservice images from Docker Hub:
  - `mxku/basboosify-products-microservice:v1.0`
  - `mxku/basboosify-users-microservice:v1.0`
  - `mxku/basboosify-orders-microservice:v1.0` *(when ready)*
- Starts official DB images and applies init scripts on first run:
  - **MySQL** ← `./mysql-init/*.sql` (creates `basboosifyproductsdatabase`, `Products` table, + sample rows)
  - **PostgreSQL** ← `./postgres-init/*.sql` (creates `BasboosifyUsers`, `Users` table)
  - **MongoDB (Orders)** — init/seed will be added when Orders is published

### URLs
- Products API → http://localhost:8080/swagger  
- Users API → http://localhost:9090/swagger  
- Orders API → *(WIP)*   

### Stop
```bash
docker compose down
```
