# Deliverable 5: Technology Stack

> **Project Scope**: See `PROJECT_SCOPE.md` for feature list.

---

## Frontend Technologies

| Technology     | Purpose                   |
|----------------|---------------------------|
| **Angular**    | Frontend framework        |
| **TypeScript** | Type-safe JavaScript      |
| **RxJS**       | Reactive programming      |
| **STOMP.js**   | WebSocket protocol client |
| **SockJS**     | WebSocket fallback        |

---

## Backend Technologies

| Technology                     | Purpose                                               |
|--------------------------------|-------------------------------------------------------|
| **Spring Boot**                | Application framework                                 |
| **Java**                       | Programming language                                  |
| **Gradle**                     | Build tool                                            |
| **Spring Web MVC**             | REST API                                              |
| **Spring WebSocket**           | Real-time communication                               |
| **Spring Security**            | Authentication & authorization                        |
| **Spring Data JPA**            | Database access                                       |
| **Spring Kafka**               | Message broker integration                            |
| **Hibernate**                  | ORM framework                                         |
| **Liquibase**                  | Database migrations                                   |
| **OAuth 2.1 / OpenID Connect** | Externalized authentication/authorization (preferred) |
| **JWT (jjwt)**                 | Self-issued tokens (simpler alternative)              |

---

## Database & Storage

| Technology          | Purpose                                            |
|---------------------|----------------------------------------------------|
| **PostgreSQL**      | Production database                                |
| **H2**              | Development database                               |
| **Hibernate**       | ORM framework                                      |
| **Spring Data JPA** | Data access abstraction                            |
| **AWS S3**          | Object storage for uploaded files                  |
| **Redis**           | Caching layer (user data, room metadata, messages) |

---

## Message Broker

| Technology       | Purpose              |
|------------------|----------------------|
| **Apache Kafka** | Message distribution |

---

## Infrastructure

| Technology         | Purpose                                                                  |
|--------------------|--------------------------------------------------------------------------|
| **Docker**         | Containerization                                                         |
| **Docker Compose** | Orchestration for monolithic architecture (current scope)                |
| **Kubernetes**     | Container orchestration for microservices (when scope expands)           |
| **Nginx**          | Reverse proxy (production)                                               |
| **CDN**            | Frontend static assets and file delivery (AWS S3, CloudFront/Cloudflare) |

---

## Development Tools

| Technology               | Purpose                 |
|--------------------------|-------------------------|
| **Lombok**               | Code generation         |
| **Spring Boot Actuator** | Health checks & metrics |

---

## Technology Rationale

**Angular**: Component-based, TypeScript support, RxJS for async operations  
**Spring Boot**: Rapid development, production-ready, extensive ecosystem, supports both monolithic and microservices  
**PostgreSQL**: ACID-compliant, excellent performance, rich features  
**Apache Kafka**: High throughput, fault-tolerant, perfect for real-time messaging and service communication  
**Docker**: Consistent environments, easy deployment, portability  
**Docker Compose**: Simple orchestration for monolithic architecture, ideal for current scope  
**Kubernetes**: Essential for microservices orchestration, auto-scaling, service discovery, cloud deployment  
**AWS S3**: Scalable object storage, cost-effective file storage  
**Redis**: Fast in-memory caching, reduces database load  
**CDN**: Global content delivery, improved performance for static assets