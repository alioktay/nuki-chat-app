# Deliverable 3: API Design

> **Project Scope**: See `PROJECT_SCOPE.md` for feature list.

---

## Summary

This API design provides a comprehensive, RESTful, and real-time communication interface for the chat application. The design emphasizes:

- **RESTful Conventions**: Standard HTTP methods and status codes
- **Real-time Communication**: WebSocket (STOMP) for live updates
- **Security**: OAuth 2.1/OpenID Connect (preferred) or JWT authentication and authorization
- **Scalability**: Stateless design enables horizontal scaling
- **Documentation**: Complete Swagger/OpenAPI specification
- **Error Handling**: Consistent and informative error responses
- **Pagination**: Efficient data retrieval for large datasets
- **File Storage**: AWS S3 integration for scalable file storage
---

## Overview

Two API types:
1. **REST API**: CRUD operations, authentication, initial data loading
2. **WebSocket API (STOMP)**: Real-time bidirectional messaging

**Base URL**: `http://localhost:8080/api/v1` (dev) or `https://api.example.com/api/v1` (prod)  
**WebSocket**: `ws://localhost:8080/ws` (dev) or `wss://api.example.com/ws` (prod)

**Swagger Spec**: `3_API_DOCUMENTATION_SWAGGER.yaml`

---

## REST API Endpoints

### Authentication

**Register**
```
POST /api/v1/auth/register
Body: { "username": "string", "email": "string", "password": "string" }
Response: 201 - User info
```

**Login**
```
POST /api/v1/auth/login
Body: { "username": "string", "password": "string" }
Response: 200 - { "token": "jwt", "refreshToken": "jwt" }
```

**Refresh Token**
```
POST /api/v1/auth/refresh
Body: { "refreshToken": "string" }
Response: 200 - New tokens
```

### Users

**Get Current User**
```
GET /api/v1/users/me
Response: 200 - User info
```

**Get All Users**
```
GET /api/v1/users
Response: 200 - List of users
```

### Chat Rooms

**Get Direct Messages**
```
GET /api/v1/rooms/dms
Response: 200 - List of DMs
```

**Create Direct Message**
```
POST /api/v1/rooms/dms?userId={uuid}
Response: 201 - DM room
```

**Get Group Rooms**
```
GET /api/v1/rooms/rooms?page=0&size=20
Response: 200 - Paginated rooms
```


**Create Group Room**
```
POST /api/v1/rooms
Body: { "name": "string", "description": "string" }
Response: 201 - Group room
```

**Get Room**
```
GET /api/v1/rooms/{roomId}
Response: 200 - Room details
```

**Delete Room**
```
DELETE /api/v1/rooms/{roomId}
Response: 204
```

### Messages

**Create Message**
```
POST /api/v1/messages
Body: { "roomId": "uuid", "content": "string", "parentMessageId": "uuid" (optional) }
Response: 201 - Message object
```

**Create Document Message**
```
POST /api/v1/messages/document
Body: { "roomId": "uuid", "fileUrl": "string" }
Response: 201 - Document message
```

**Get Messages**
```
GET /api/v1/messages/rooms/{roomId}?page=0&size=50
Response: 200 - Paginated messages
```

**Update Message**
```
PUT /api/v1/messages/{messageId}
Body: { "content": "string" }
Response: 200 - Updated message
```

**Delete Message**
```
DELETE /api/v1/messages/{messageId}
Response: 204
```

**Get Message Replies**
```
GET /api/v1/messages/{messageId}/replies
Response: 200 - Paginated messages
```

### Files

**Upload File**
```
POST /api/v1/files/upload
Content-Type: multipart/form-data
Body: file (binary)
Response: 200 - { "fileUrl": "string" }
```

**Storage**: Files uploaded to AWS S3, URLs returned for CDN delivery  
**Allowed Types**: Images (.jpg, .png, .gif, etc.), Videos (.mp4, .webm, etc.), Documents (.pdf, .doc, etc.)

---

## WebSocket API

**Connection**: `ws://localhost:8080/ws` (or `wss://` in production)

### Subscribe to Room Messages
**Destination**: `/topic/room/{roomId}/messages`

**Message Format**:
```json
{
  "id": "uuid",
  "content": "string",
  "senderId": "uuid",
  "roomId": "uuid",
  "messageType": "TEXT|VIDEO|IMAGE|DOCUMENT",
  "parentMessageId": "uuid", // (optional, for threading)
  "createdAt": "2024-01-01T12:00:00Z",
  "documentUrl": "string",
  "edited": false,
  "deleted": false
}
```

### Send Message
**Destination**: `/app/room/{roomId}/send`


**Body**:
```json
{
  "content": "string",
  "parentMessageId": "uuid" // (optional, for threading)
}
```

**Response**: Message is saved to database, published to Kafka, and broadcast to all subscribers

### Room Events
**Destination**: `/topic/room/{roomId}/events`

**Subscription**: Client subscribes to receive room-level events

**Event Types**:
- `USER_JOINED`: User joined the room
- `USER_LEFT`: User left the room
- `ROOM_UPDATED`: Room metadata changed (name, description)

**Event Format**:
```json
{
  "eventType": "USER_JOINED|USER_LEFT|ROOM_UPDATED",
  "roomId": "uuid",
  "userId": "uuid", // (for USER_JOINED/USER_LEFT)
  "timestamp": "2024-01-01T12:00:00Z",
  "data": {} // (additional event-specific data)
}
```

---

## API Design Principles

1. **RESTful**: Standard HTTP methods (GET, POST, PUT, DELETE) and status codes
2. **Versioning**: `/api/v1/` prefix for future API compatibility
3. **Pagination**: All list endpoints support pagination (page, size parameters)
4. **Error Handling**: Consistent error response format with meaningful messages
5. **Authentication**: OAuth 2.1/OpenID Connect (preferred) or JWT Bearer tokens
6. **Content**: JSON request/response bodies
7. **Validation**: Request body validation
8. **Stateless**: No server-side sessions, enables horizontal scaling

---

## Authentication & Authorization

**Authentication**: 
- **Preferred**: OAuth 2.1/OpenID Connect access token in `Authorization: Bearer {token}` header
- **Alternative**: Self-issued JWT token in `Authorization: Bearer {token}` header
- All endpoints except `/api/v1/auth/register` and `/api/v1/auth/login` require authentication

**Authorization**:
- **Room Access**: Users can only access rooms they are members of
- **Message Operations**: Users can only edit/delete their own messages
- **Room Management**: Only room creators can delete rooms

**WebSocket**: 
- Token validated during WebSocket handshake
- Room membership verified before allowing subscriptions
- Same authentication method (OAuth/JWT) as REST API
