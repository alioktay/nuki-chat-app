# Deliverable 4: Security Considerations

---

> **Project Scope**: See `PROJECT_SCOPE.md` for feature list.

---

## Summary

The application implements multiple layers of security to protect user data and prevent unauthorized access. While the current implementation covers essential security requirements, there are opportunities for enhancement, particularly in areas like rate limiting, 2FA, and comprehensive audit logging.

**Key Security Strengths**:
- Strong authentication (OAuth 2.1/OpenID Connect preferred, JWT as alternative)
- Secure password storage (BCrypt)
- Input validation and sanitization
- SQL injection prevention
- XSS prevention
- File upload security with S3
- HTTPS/TLS in production

**Areas to consider**:
- Rate limiting
- Two-factor authentication
- Comprehensive audit logging
- End-to-end encryption (for sensitive conversations)

---

## Authentication

### OAuth 2.1 / OpenID Connect (Preferred)

**Why Prefer OAuth/OIDC**:
- Enterprise SSO integration (Keycloak, Auth0, Azure AD, Okta)
- Multi-application ecosystems
- Centralized identity management, MFA, auditing

**Flow** (Authorization Code with PKCE):
1. Frontend redirects user to IdP authorization endpoint
2. User authenticates with IdP (password, MFA, SSO, etc.)
3. IdP redirects back with authorization code
4. Frontend exchanges code (with PKCE) for access token (+ optional ID token and refresh token)
5. Access token sent to backend in `Authorization: Bearer {token}` header
6. Backend validates token signature and claims using IdP's JWKS endpoint

**Integration**: 
- **Spring Security**: Configure `spring-security-oauth2-resource-server` for token validation

### JWT (Self-Issued Alternative)

Simpler alternative for smaller systems where backend issues its own tokens.

**Token Structure**:
- **Access Token**: Short-lived (1 hour), contains user identity
- **Refresh Token**: Long-lived, stored in database, used to obtain new access tokens
- **Algorithm**: HS256 (HMAC-SHA256)
- **Secret**: Configurable via environment variable, minimum 256 bits

**Token Lifecycle**:
1. User logs in → receives access token and refresh token
2. Access token used for API requests
3. When access token expires → use refresh token to get new access token
4. Refresh token stored in database, can be revoked

---

## Authorization

**Access Control**:
- Room access: Only members can access rooms
- Message operations: Users can only edit/delete their own messages
- Room management: Only creators can delete rooms

**Implementation**: `@PreAuthorize` annotations, service-level validation

---

## Data Validation

**Input Sanitization**:
- HTML Escaping: All user-generated content escaped before storage and display
- Content length limits (messages: 5000 chars, usernames: 3-50 chars)
- File type whitelist and size limits

**Output Encoding**: 
- Proper JSON serialization prevents injection attacks
- Security headers set appropriately (Content-Type, X-Content-Type-Options)

---

## Network Security

**HTTPS/TLS**: All production communication over HTTPS/WSS  
**CORS**: Configurable allowed origins  
**CSRF Protection**: Enabled for state-changing operations

---

## Database Security

**Connection Security**:
- SSL/TLS for production database connections
- Credentials in environment variables (ideally secret manager)
- Connection pooling

**Data Protection**:
- Passwords hashed with BCrypt
- Parameterized queries (JPA/Hibernate)
- Audit logging for security events

---

## WebSocket Security

**Authentication**: Token validated during handshake  
**Authorization**: Room membership verified before subscriptions  
**Message Validation**: All messages validated, size limits enforced

---

## File Upload Security

**Storage**: AWS S3 (or compatible object storage) with proper IAM policies

**Validation & File scanning**:
- File type whitelist (images, videos, documents)
- MIME type validation
- Path traversal prevention
- UUID filenames
- 3rd party service for virus scanning

**S3 Security**:
- Private bucket with signed URLs for file access
- IAM policies restrict access to application only
- Versioning enabled for file recovery
- Lifecycle policies for old file cleanup

---

## Security Best Practices

- Principle of Least Privilege  
- Defense in Depth  
- Input Validation  
- SQL Injection Prevention (parameterized queries)  
- XSS Prevention (input sanitization)  
- HTTPS/TLS in production  
- Secure password storage (BCrypt)

---

## Future Improvements

- Rate limiting
- Two-factor authentication (2FA)
- Comprehensive audit logging
- End-to-end encryption
- Security headers (CSP, HSTS)