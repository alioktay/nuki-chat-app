# Deliverable 2: Database Schema

> **Project Scope**: See `PROJECT_SCOPE.md` for feature list.

---

## Summary

This database schema provides a robust foundation for a real-time chat application. The design emphasizes:

- **Scalability**: UUID keys, proper indexing, partitioning-ready
- **Data Integrity**: Foreign keys, unique constraints, type safety
- **Performance**: Strategic indexes, lazy loading, efficient queries
- **Flexibility**: Support for DMs, group rooms, threading
- **Security**: UUIDs prevent enumeration, soft deletes
---

## Overview

The database schema supports a real-time chat application with the following core entities:
- **Users**: User accounts and authentication
- **Chat Rooms**: Direct messages (DMs) and group rooms
- **Messages**: Text, video, and file messages with threading support
- **Room Membership**: Many-to-many relationship between users and rooms
- **Refresh Tokens**: JWT refresh token storage

**Database**: PostgreSQL (production), H2 (development)  
**ORM**: Hibernate (via Spring Data JPA)  
**Migrations**: Liquibase  
**File Storage**: AWS S3 (or compatible object storage) for uploaded files

**Visual ERD**: `2_DATABASE_SCHEMA.drawio` (Draw.io format)

---

## Core Tables

### Users
- **Primary Key**: `id` (UUID)
- **Unique**: `username`, `email`
- **Fields**: username, email, password (hashed), enabled, created_at

### ChatRooms
- **Primary Key**: `id` (UUID)
- **Foreign Key**: `created_by` → `users.id`
- **Fields**: conversation_type (DM/ROOM), name, description, created_at

### Messages
- **Primary Key**: `id` (UUID)
- **Foreign Keys**: 
  - `sender_id` → `users.id`
  - `room_id` → `chat_rooms.id`
  - `parent_message_id` → `messages.id` (threading)
- **Fields**: content, message_type (TEXT/VIDEO/IMAGE/DOCUMENT), document_url, created_at, edited, deleted

### ChatRoomMembers
- **Primary Key**: `id` (UUID)
- **Foreign Keys**: `user_id` → `users.id`, `room_id` → `chat_rooms.id`
- **Unique**: (`user_id`, `room_id`)
- **Fields**: user_id, room_id, joined_at

### RefreshTokens
- **Primary Key**: `id` (UUID)
- **Foreign Key**: `user_id` → `users.id`
- **Unique**: `token`
- **Fields**: token, expires_at, created_at

---

## Scalability Considerations

- **Partitioning Ready**: Schema designed to support partitioning by room_id or user_id for large-scale deployments
- **Read Replicas**: Stateless design supports read replicas for scaling read operations
- **Connection Pooling**: Efficient connection management
