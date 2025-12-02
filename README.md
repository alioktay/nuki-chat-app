# Real-Time Chat Application Documentation

---

## Overview

This documentation provides the system design for a real-time chat application as specified in the assignment requirements. The application supports user authentication, real-time messaging, video sharing, direct messages, and group chat rooms.

---

## Project Scope

**File**: `PROJECT_SCOPE.md`

The project scope document defines:
- **In-scope features**: All features included in the system design
- **Out-of-scope features**: Features explicitly excluded from this project
- **Scope boundaries**: What this project covers and what it does not

> **Important**: Please review `PROJECT_SCOPE.md` to understand the complete feature set and limitations of this project.

---

## Deliverables

### Deliverable 1: Architecture
**File**: `1_ARCHITECTURE.md`

Architecture documentation covering:
- System overview and high-level architecture
- Component architecture (Frontend and Backend)
- Communication patterns (REST, WebSocket, Kafka)
- Scalability and deployment architecture

**Visual Diagram**: `1_ARCHITECTURE_DIAGRAM.drawio` (Draw.io format)

### Deliverable 2: Database Schema
**File**: `2_DATABASE_SCHEMA.md`

Database schema documentation including:
- Entity Relationship Diagram
- Detailed schema for all tables
- Relationships, constraints, and indexes
- Database design principles

**Visual Diagram**: `2_DATABASE_SCHEMA.drawio` (Draw.io format)

### Deliverable 3: API Design
**File**: `3_API_DESIGN.md`

API design documentation covering:
- REST API endpoints (authentication, users, rooms, messages, files)
- WebSocket API (STOMP protocol)
- Request/response formats
- Error handling
- Authentication and authorization
- API design principles

**Swagger Specification**: `3_API_DOCUMENTATION_SWAGGER.yaml` (OpenAPI 3.0 format)

### Deliverable 4: Security Considerations
**File**: `4_SECURITY_CONSIDERATIONS.md`

Security documentation including:
- Authentication (OAuth 2.1/OpenID Connect preferred, JWT alternative, passwords)
- Authorization (RBAC, access control)
- Data validation and sanitization
- Network security (HTTPS, TLS, CORS)
- Database security
- WebSocket security
- File upload security
- Security best practices

### Deliverable 5: Technology Stack
**File**: `5_TECHNOLOGY_STACK.md`

Technology stack documentation covering:
- Frontend technologies (Angular, TypeScript, RxJS, STOMP.js)
- Backend technologies (Spring Boot, Java, Spring modules)
- Database & storage (PostgreSQL, H2, Hibernate, Liquibase)
- Message broker (Apache Kafka)
- Infrastructure (Docker, Docker Compose, Kubernetes, Nginx)
- Cloud services (AWS S3, CDN, Redis)
- Architecture patterns (Monolithic vs Microservices)
- Technology rationale and alternatives

---

## Quick Start

### Traditional Architecture (Current)

1. **Project Scope**: Read `PROJECT_SCOPE.md` to understand what features are included
2. **Architecture**: Read `1_ARCHITECTURE.md` and view `1_ARCHITECTURE_DIAGRAM.drawio`
3. **Database Design**: Read `2_DATABASE_SCHEMA.md` and view `2_DATABASE_SCHEMA.drawio`
4. **API Reference**: Read `3_API_DESIGN.md` and open `3_API_DOCUMENTATION_SWAGGER.yaml` in Swagger UI
5. **Security**: Read `4_SECURITY_CONSIDERATIONS.md`
6. **Technology Stack**: Read `5_TECHNOLOGY_STACK.md`

### AWS Serverless Architecture (Alternative)

1. **Overview**: Read [`alternative/README.md`](alternative/README.md) for AWS architecture overview
2. **Architecture**: Read [`alternative/ARCHITECTURE.md`](alternative/ARCHITECTURE.md) for detailed AWS serverless architecture
3. **Technology Stack**: Read [`alternative/TECHNOLOGY_STACK.md`](alternative/TECHNOLOGY_STACK.md) for AWS services and technologies

---

## Supporting Files

### Swagger/OpenAPI Documentation
**File**: `3_API_DOCUMENTATION_SWAGGER.yaml`

Complete REST API documentation in OpenAPI 3.0 format.

**To view in Swagger UI:**
1. Go to https://editor.swagger.io/
2. Click "File" → "Import file"
3. Select `3_API_DOCUMENTATION_SWAGGER.yaml`
4. View interactive API documentation

### Architecture Diagram
**File**: `1_ARCHITECTURE_DIAGRAM.drawio`

Visual System Architecture Diagram in Draw.io format showing:
- Three-tier architecture (Presentation, Application, Data layers)
- All system components and their relationships
- Communication flows (REST, WebSocket, Kafka)

**To view:**
1. Go to https://app.diagrams.net/ (Draw.io)
2. Click "File" → "Open from" → "Device"
3. Select `1_ARCHITECTURE_DIAGRAM.drawio`
4. View the interactive architecture diagram

### Database Schema Diagram
**File**: `2_DATABASE_SCHEMA.drawio`

Visual Entity-Relationship Diagram (ERD) in Draw.io format.

**To view:**
1. Go to https://app.diagrams.net/ (Draw.io)
2. Click "File" → "Open from" → "Device"
3. Select `2_DATABASE_SCHEMA.drawio`
4. View the interactive database schema diagram