# Project Scope
---

## Overview

This document defines the features and scope of the real-time chat application. The scope outlines what functionality is included in the system design and implementation, as well as what is explicitly out of scope for this project.

---

## In-Scope Features

### 1. User Authentication & Authorization

#### Authentication
- User registration with username, email, and password
- User login with username and password
- OAuth 2.1/OpenID Connect (preferred) or JWT-based authentication (self-issued tokens)
- Access tokens and refresh tokens
- Token refresh mechanism
- Password hashing with BCrypt
- Session management

#### Authorization
- Role-based access control (RBAC) - basic implementation
- Room membership-based access control
- Message ownership verification
- Room creator permissions

### 2. Real-Time Messaging

#### Core Messaging
- Send and receive text messages in real-time
- Real-time message delivery via WebSocket (STOMP protocol)
- Message persistence in database
- Message history retrieval
- Pagination for message history
- Message timestamps

#### Message Operations
- Edit messages (only by sender)
- Delete messages (soft delete, only by sender)
- Message threading/replies (reply to specific messages)

#### Message Types
- Text messages
- Video messages (via URL)
- File attachments (images, documents)
- Message metadata (sender, timestamp, edit status)

### 3. Chat Rooms

#### Room Types
- Direct Messages (DM) - 1-on-1 conversations
- Group Rooms - multi-user chat rooms

#### Room Management
- Create direct message conversations
- Create group chat rooms
- Room membership management
- Room metadata (name, description)
- Room deletion (by creator only)
- List user's direct messages
- List group rooms (paginated)

### 4. User Management

#### User Operations
- User registration
- User profile information (username, email)
- Get current user information
- List all users
- User account status (enabled/disabled)

### 5. API Design

#### REST API
- RESTful API endpoints
- API versioning (`/api/v1/`)
- Standard HTTP methods and status codes
- JSON request/response format
- Error handling with consistent error responses
- Request validation
- Pagination support

#### WebSocket API
- WebSocket connection with STOMP protocol
- Real-time message broadcasting
- Room-based message subscriptions
- User-specific notifications
- Room event notifications

### 6. Database Schema

#### Core Entities
- Users table
- Chat rooms table
- Messages table
- Room memberships table
- Refresh tokens table

#### Database Features
- UUID primary keys
- Foreign key relationships
- Unique constraints
- Indexes for performance
- Soft deletes for messages

### 7. Security

#### Authentication Security
- Secure password storage (BCrypt)
- JWT token security
- OAuth 2.1/OpenID Connect support
- Token expiration and refresh
- Secure token storage

#### Authorization Security
- Access control enforcement
- Room membership verification
- Message ownership verification
- Role-based permissions

#### Data Security
- Input validation and sanitization
- XSS prevention
- SQL injection prevention
- File upload security
- URL validation

#### Network Security
- HTTPS/TLS in production
- WSS for WebSocket in production
- CORS configuration
- CSRF protection

### 8. Scalability & Performance

#### Scalability Features
- Stateless backend design
- Horizontal scaling support
- Message broker integration (Kafka)
- Database connection pooling
- Load balancing ready

#### Performance Features
- Database indexing strategy
- Lazy loading for relationships
- Pagination for large datasets
- Caching support (application-level)

### 9. Technology Stack

#### Frontend
- Angular framework
- TypeScript
- RxJS for reactive programming
- STOMP.js for WebSocket communication
- CDN deployment (AWS S3, Cloudfront/Cloudflare) for static assets

#### Backend
- Java programming language
- Spring Boot framework
- Spring Security for authentication/authorization
- Spring Data JPA for database access
- Spring WebSocket for real-time communication
- Spring Kafka for message broker integration

#### Database & Infrastructure
- PostgreSQL (production)
- H2 (development)
- Apache Kafka (message broker)
- Redis for caching
- Docker containerization
- **Docker Compose** for monolithic architecture (current scope)
- **Kubernetes** for microservices orchestration (when scope expands)
- AWS S3 for file storage
- CDN for frontend and file delivery

---

## Out-of-Scope Features

- Advanced User Features
- Advanced Messaging Features
- Advanced Room Features
- Push Notifications
- Advanced Security Features like 2FA, SSO etc
- Analytics & Reporting
- Integration Features
- Mobile Applications
- Advanced File Features
- Advanced Search
- Administration
- Compliance & Legal

---

## Scope Boundaries

### What This Project Covers

This project focuses on:
1. **Core chat functionality**: Real-time messaging, rooms, and user management
2. **System design**: Architecture, database schema, API design, and security considerations
3. **Architecture approach**: Monolithic design suitable for current scope, with guidance for microservices evolution if feature scope expands
4. **Technology recommendations**: Technology stack selection and rationale
5. **Scalability considerations**: Design patterns for horizontal scaling
6. **Security fundamentals**: Authentication, authorization, and data protection

### What This Project Does NOT Cover

This project does NOT include:
1. **Full implementation**: This is a design document, not a complete implementation
2. **UI/UX design**: Frontend design and user experience details
3. **Testing strategy**: Unit tests, integration tests, E2E tests
4. **DevOps pipeline**: CI/CD, deployment automation
5. **Monitoring & observability**: Logging, metrics, tracing implementation
6. **Performance testing**: Load testing, stress testing results
7. **Production deployment**: Actual production deployment configuration