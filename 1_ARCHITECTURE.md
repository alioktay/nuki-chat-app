# Deliverable 1: Architecture

> **Project Scope**: See `PROJECT_SCOPE.md` for feature list.

---

## Summary

This architecture provides a solid foundation for a scalable, real-time chat application. The separation of concerns, use of modern technologies, and scalable patterns ensure the system can grow with user demand while maintaining performance and reliability.

**Key Architectural Strengths**:
- Clear separation of layers
- Stateless backend design
- Real-time communication via WebSocket
- Scalable message distribution via Kafka
- Production-ready deployment strategy
- Modern technology stack
- Monolithic design suitable for current scope, with clear path to microservices if needed

---

## System Overview

Real-time chat application with:
- Direct messages (1-on-1) and group chat rooms
- Real-time messaging via WebSocket
- Video and file sharing
- Authentication via OAuth 2.1/OpenID Connect (preferred) or JWT (simpler alternative)

---

## High-Level Architecture

Three-tier architecture:

1. **Presentation Layer** (Angular Frontend)
2. **Application Layer** (Spring Boot Backend)
3. **Data Layer** (PostgreSQL & Apache Kafka)

```
┌─────────────┐
│   Client    │  (Angular Frontend)
│  (Browser)  │
└──────┬──────┘
       │ HTTP/REST + WebSocket (STOMP)
       │
┌──────▼─────────────────────────────────────┐
│         Backend API Server                 │
│      (Spring Boot Application)             │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  │
│  │   REST   │  │ WebSocket│  │  Kafka   │  │
│  │  API     │  │ Handler  │  │ Producer │  │
│  └──────────┘  └──────────┘  └────┬─────┘  │
└──────┬────────────────────────────┼────────┘
       │                            │
┌──────▼──────┐              ┌──────▼──────┐
│ PostgreSQL  │              │   Kafka     │
│  Database   │              │   Broker    │
└─────────────┘              └─────────────┘
```

**Visual Architecture Diagram**: `1_ARCHITECTURE_DIAGRAM.drawio` (Draw.io format)

For a detailed visual representation of the system architecture, including all components, communication flows, and deployment infrastructure, see the architecture diagram file.

---

## Component Architecture

### Frontend (Angular)
- **Framework**: Angular with TypeScript
- **Real-time**: STOMP over WebSocket (via SockJS for fallback support)
- **State**: RxJS Observables for reactive state management
- **Deployment**: Static assets built and served via CDN from S3

### Backend (Spring Boot)
- **Framework**: Spring Boot (Java)
- **Key Modules**: 
  - Spring Web MVC (REST API)
  - Spring WebSocket (STOMP protocol support)
  - Spring Security (Authentication & Authorization - OAuth/JWT)
  - Spring Data JPA (Database access)
  - Spring Kafka (Message broker integration)
- **Architecture**: Monolithic design (single application), can evolve to microservices

### Database
- **Production**: PostgreSQL (ACID-compliant, robust)
- **Development**: H2 (in-memory, fast startup)
- **ORM**: Hibernate (via Spring Data JPA)
- **Migrations**: Liquibase for version-controlled schema changes

### Message Broker
- **Technology**: Apache Kafka (KRaft mode - no Zookeeper dependency)
- **Purpose**: 
  - Decouple message publishing from WebSocket distribution
  - Enable horizontal scaling of backend instances
  - Ensure message delivery reliability across multiple instances
### Cache
- **Technology**: Redis (in-memory data store)
- **Purpose**: Cache frequently accessed data (user info, room metadata, recent messages)

### File Storage
- **Technology**: AWS S3 (or compatible object storage)
- **Purpose**: Store uploaded files (images, videos, documents)

---

## Communication Patterns

### REST API
- Initial data loading, CRUD operations, authentication
- Standard HTTP methods (GET, POST, PUT, DELETE)

### WebSocket
- Real-time bidirectional messaging
- Topic-based message distribution
- JWT/OAuth token authentication during handshake

### Kafka Message Distribution
- Messages published to Kafka topics
- All backend instances consume from Kafka
- Each instance broadcasts to its connected clients
- Enables horizontal scaling of WebSocket connections

---

## Scalability

### Horizontal Scaling
- **Stateless Backend**: JWT/OAuth tokens enable stateless authentication
- **Kafka as Message Bus**: All backend instances consume from same topics
- **Database Connection Pooling**: Efficient connection management
- **Load Balancing**: Multiple backend instances behind load balancer
- **CDN**: Scaling of frontend static serving

### Performance Optimizations

**Database**:
- Strategic indexing on foreign keys and frequently queried fields (room_id, created_at, user_id)
- Lazy loading for JPA relationships to reduce initial query load
- Pagination for all list endpoints to handle large datasets efficiently

**Caching**:
- **Redis**: In-memory caching for frequently accessed data
  - User information and profiles
  - Room metadata and member lists
  - Recent messages per room
  - Reduces database load and improves response times

**Content Delivery**:
- **CDN**: Frontend static assets (Angular app) served via CDN for global distribution
- **File Delivery**: Uploaded files (images, videos, documents) delivered via CDN from S3
- Reduces latency and server load

---

## Deployment

### Current Scope (Monolithic Architecture)

**Development & Simple Deployments**:
- **Docker Compose**: Orchestrates all services (Frontend, Backend, PostgreSQL, Kafka, Redis)
- Suitable for monolithic application deployment
- Simple local development setup
- Single deployment unit

### Production (Cloud Deployment)

**For Monolithic**:
- **Docker Compose**: Can be used for simple production deployments
- **Docker Swarm**: Alternative for simple orchestration
- **Single Container**: Deploy monolithic application as single container

**For Microservices** (when scope expands):
- **Kubernetes**: Recommended orchestration platform (AWS EKS, GKE, AKS)
- **Container Orchestration**: Manages multiple services, auto-scaling, service discovery
- **Service Mesh**: Optional (Istio, Linkerd) for advanced microservices communication

**Application Layer**:
- **Reverse Proxy**: Nginx/ALB (SSL termination, load balancing)
- **Application Servers**: Multiple Spring Boot instances (auto-scaling)
- **Database**: PostgreSQL primary + replicas (managed service: RDS, Cloud SQL)
- **Message Broker**: Kafka cluster (managed service: MSK, Confluent Cloud)
- **Cache**: Redis cluster (managed service: ElastiCache, Cloud Memorystore)

**Storage & CDN**:
- **File Storage**: AWS S3 (or compatible object storage) for uploaded files
- **CDN**: CloudFront/Cloudflare for frontend static assets and file delivery


**Benefits**:
- Easy cloud deployment with Kubernetes
- Scalable file storage with S3
- Global content delivery via CDN
- Managed services reduce operational overhead


## Data Flow Examples

### Message Creation Flow

```
1. User types message in UI
   │
   ▼
2. Frontend: ChatService.createMessage()
   │
   ▼
3. HTTP POST /api/v1/messages
   │
   ▼
4. Backend: MessageController.createMessage()
   │
   ▼
5. Backend: MessageService.createMessage()
   │
   ├──► 6a. Save to Database (PostgreSQL)
   │         MessageRepository.save()
   │
   └──► 6b. Publish to Kafka
            KafkaTemplate.send("chat-events", messageDto)
   │
   ▼
7. Kafka Consumer (in same or different backend instance)
   │
   ▼
8. WebSocketController.handleMessage()
   │
   ▼
9. Broadcast via WebSocket to subscribed clients
   SimpMessagingTemplate.convertAndSend("/topic/room/{roomId}/messages", messageDto)
   │
   ▼
10. Frontend: WebSocketService receives message
   │
   ▼
11. UI updates with new message
```

### Authentication Flow

```
1. User submits login form
   │
   ▼
2. Frontend: AuthService.login()
   │
   ▼
3. HTTP POST /api/v1/auth/login
   │
   ▼
4. Backend: AuthController.login()
   │
   ▼
5. Backend: AuthService.login()
   │
   ├──► 6a. Validate credentials
   │         UserRepository.findByUsername()
   │         PasswordEncoder.matches()
   │
   ├──► 6b. Generate JWT tokens
   │         JwtTokenProvider.generateAccessToken()
   │         JwtTokenProvider.generateRefreshToken()
   │
   └──► 6c. Save refresh token to database
            RefreshTokenRepository.save()
   │
   ▼
7. Return JwtResponse (access token + refresh token)
   │
   ▼
8. Frontend: Store tokens in memory/localStorage
   │
   ▼
9. Frontend: Add token to subsequent requests
         AuthInterceptor adds "Authorization: Bearer {token}"
```

---