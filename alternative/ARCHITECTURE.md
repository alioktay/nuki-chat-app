# Deliverable 1: Architecture (AWS Cloud-First)

> **Project Scope**: See `PROJECT_SCOPE.md` for feature list.

---

## Summary

This architecture provides a cloud-first, serverless foundation for a scalable, real-time chat application. The architecture leverages AWS managed services to minimize operational overhead while providing enterprise-grade scalability, reliability, and security.

**Key Architectural Strengths**:
- Fully serverless architecture (no servers to manage)
- Cloud-native design with AWS managed services
- Automatic scaling based on demand
- Pay-per-use cost model
- Built-in high availability and fault tolerance
- Event-driven architecture with EventBridge
- SAM-based deployment for Infrastructure as Code

---

## System Overview

Real-time chat application with:
- Direct messages (1-on-1) and group chat rooms
- Real-time messaging via WebSocket (API Gateway WebSocket API)
- Video and file sharing
- Authentication via AWS Cognito (OAuth 2.1/OpenID Connect)

---

## High-Level Architecture

Serverless, event-driven architecture:

1. **Presentation Layer** (Angular Frontend on S3/CloudFront)
2. **API Layer** (API Gateway - REST & WebSocket)
3. **Application Layer** (AWS Lambda Functions)
4. **Data Layer** (DynamoDB, S3, EventBridge)

```
┌─────────────────────────────────────────────────────────┐
│                    Client (Browser)                     │
│              (Angular Frontend via CDN)                 │
└──────────────┬──────────────────────────────┬───────────┘
               │                              │
               │ HTTPS                        │ WSS
               │                              │
┌──────────────▼──────────────┐   ┌───────────▼───────────┐
│   API Gateway (REST API)    │   │ API Gateway WebSocket │
│   - Authentication          │   │ - Real-time messaging │
│   - CRUD Operations         │   │ - Connection mgmt     │
└──────────────┬──────────────┘   └───────────┬───────────┘
               │                              │
               │                              │
┌──────────────▼──────────────────────────────▼──────────┐
│              Lambda Functions (Serverless)             │
│      ┌──────────┐  ┌───────────┐  ┌───────────┐        │
│      │ Message  │  │   Room    │  │   Auth    │        │
│      │ Handler  │  │  Handler  │  │  Handler  │        │
│      └────┬─────┘  └─────┬─────┘  └─────┬─────┘        │
│           │              │              │              │
│      ┌────▼──────────────▼──────────────▼─────┐        │
│      │      WebSocket Connection Handler      │        │
│      └────┬───────────────────────────────────┘        │
└───────┼────────────────────────────────────────────────┘
        │
        ├──────────────┬──────────────┬──────────────┐
        │              │              │              │
┌───────▼──────┐ ┌─────▼───────┐ ┌────▼──────┐ ┌─────▼──────┐
│  DynamoDB    │ │  EventBridge│ │    S3     │ │  Cognito   │
│  (NoSQL DB)  │ │  (Events)   │ │  (Files)  │ │  (Auth)    │
└──────────────┘ └─────────────┘ └───────────┘ └────────────┘
```
---

## Component Architecture

### Frontend (Angular)
- **Framework**: Angular with TypeScript
- **State**: RxJS Observables for reactive state management
- **Deployment**: Static assets built and served via CloudFront from S3
- **Authentication**: AWS Amplify or Cognito JavaScript SDK

### API Gateway
- **REST API**: HTTP API or REST API for CRUD operations
  - Authentication via Cognito Authorizer
  - Request validation
  - Rate limiting and throttling
  - CORS configuration
- **WebSocket API**: Real-time bidirectional messaging
  - Connection management
  - Route selection (connect, disconnect, sendMessage)
  - Integration with Lambda functions

### Backend (AWS Lambda)
- **Runtime**: Node.js
- **Architecture**: Serverless
- **Key Functions**:
  - `MessageHandler`: Create, read, update, delete messages
  - `RoomHandler`: Room management, membership
  - `WebSocketHandler`: Connection management, message broadcasting
  - `FileUploadHandler`: S3 presigned URL generation, file processing
  - `AuthHandler`: Cognito integration, user management
- **Deployment**: AWS SAM (Serverless Application Model)

### Database
- **Technology**: Amazon DynamoDB (NoSQL)
- **Design**: Single-table or multi-table design based on access patterns

### Event-Driven Architecture
- **Technology**: Amazon EventBridge
- **Purpose**:
  - Decouple message publishing from WebSocket distribution
  - Enable event-driven workflows
  - Integrate with other AWS services
  - Support custom business rules
- **Event Types**:
  - `message.created`
  - `message.updated`
  - `message.deleted`
  - `room.created`
  - `user.joined`

### Authentication & Authorization
- **Technology**: AWS Cognito
- **Features**:
  - User pools for user management
  - OAuth 2.1/OpenID Connect support
  - Social identity providers (Google, Facebook, etc.)
  - Multi-factor authentication (MFA)
  - Password policies
  - User attributes and custom claims
- **Authorization**: IAM roles and policies, Cognito groups

### File Storage
- **Technology**: Amazon S3
- **Purpose**: Store uploaded files (images, videos, documents)
- **Features**:
  - Presigned URLs for secure uploads
  - Lifecycle policies for cost optimization
  - Versioning for file recovery
  - CloudFront integration for CDN delivery
  - Server-side encryption

---

## Communication Patterns

### REST API
- Initial data loading, CRUD operations
- Standard HTTP methods (GET, POST, PUT, DELETE)
- Cognito JWT token authentication
- API Gateway request/response transformation

### WebSocket (API Gateway)
- Real-time bidirectional messaging
- Connection lifecycle management
- Route-based message routing
- Automatic connection cleanup

### EventBridge
- Event-driven message distribution
- Lambda functions subscribe to events
- WebSocket handlers broadcast to connected clients
- Decoupled architecture enables horizontal scaling

---

## Scalability

### Automatic Scaling
- **Lambda**: Automatically scales from 0 to thousands of concurrent executions
- **API Gateway**: Handles millions of requests per second
- **DynamoDB**: Auto-scales based on traffic patterns
- **S3**: Unlimited storage capacity
- **CloudFront**: Global edge locations for low latency

### Performance Optimizations

**DynamoDB**:
- Single-table design for efficient queries
- Global Secondary Indexes (GSI) for flexible access patterns
- DynamoDB Streams for real-time processing
- Batch operations for bulk writes
- Query and scan optimization

**Content Delivery**:
- **CloudFront**: Frontend static assets (Angular app) served via CDN
- **S3 + CloudFront**: Uploaded files delivered via CDN
- Reduces latency and server load globally

**Lambda Optimization**:
- Provisioned concurrency for consistent performance
- Lambda layers for shared code
- Connection pooling for external resources
- Optimal memory allocation