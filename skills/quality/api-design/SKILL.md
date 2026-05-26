---
name: api-design
description: RESTful API design best practices - resource modeling, HTTP methods, status codes, versioning, authentication, rate limiting, and documentation. Build APIs developers love.
tags: [api, rest, graphql, design, documentation, versioning, authentication]
version: 1.0.0
author: Enhanced from API design best practices)
---

# API Design

## Overview

Design APIs that are intuitive, consistent, and developer-friendly.

**Why API design matters:**
- **Developer experience:** Good APIs are easy to use
- **Adoption:** Well-designed APIs get more users
- **Maintenance:** Consistent APIs are easier to maintain
- **Scalability:** Good design scales better
- **Documentation:** Clear design = clear docs

**Key principles:**
- **Consistency:** Predictable patterns
- **Simplicity:** Easy to understand
- **Flexibility:** Extensible without breaking changes
- **Security:** Secure by default
- **Performance:** Fast and efficient

## When to Use

**Use for:**
- New API development
- API redesign/refactoring
- Adding new endpoints
- API versioning
- Public API launches

**Critical for:**
- Public APIs (external developers)
- Microservices (internal APIs)
- Mobile apps (backend APIs)
- Third-party integrations
- Platform APIs

## The 7-Pillar API Design Framework

```
┌─────────────────────────────────────────────────────────┐
│              API DESIGN FRAMEWORK                        │
└─────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │   PILLAR 1   │  Resource Modeling
    │  RESOURCES   │  - Nouns not verbs
    │              │  - Hierarchical
    └──────┬───────┘  - Collections
           │
           ▼
    ┌──────────────┐
    │   PILLAR 2   │  HTTP Methods
    │   METHODS    │  - GET, POST, PUT
    │              │  - PATCH, DELETE
    └──────┬───────┘  - Idempotency
           │
           ▼
    ┌──────────────┐
    │   PILLAR 3   │  Status Codes
    │    STATUS    │  - 2xx Success
    │              │  - 4xx Client error
    └──────┬───────┘  - 5xx Server error
           │
           ▼
    ┌──────────────┐
    │   PILLAR 4   │  Versioning
    │  VERSIONING  │  - URL versioning
    │              │  - Header versioning
    └──────┬───────┘  - Breaking changes
           │
           ▼
    ┌──────────────┐
    │   PILLAR 5   │  Authentication
    │     AUTH     │  - API keys
    │              │  - OAuth 2.0
    └──────┬───────┘  - JWT tokens
           │
           ▼
    ┌──────────────┐
    │   PILLAR 6   │  Error Handling
    │    ERRORS    │  - Consistent format
    │              │  - Helpful messages
    └──────┬───────┘  - Error codes
           │
           ▼
    ┌──────────────┐
    │   PILLAR 7   │  Documentation
    │     DOCS     │  - OpenAPI/Swagger
    │              │  - Examples
    └──────────────┘  - Interactive docs
```

## Pillar 1: Resource Modeling

### Goal: Design intuitive resource structure

**Resource Naming:**

```markdown
## Use Nouns, Not Verbs

**Good (nouns):**
```
GET    /users
GET    /users/123
POST   /users
PUT    /users/123
DELETE /users/123
```

**Bad (verbs):**
```
GET    /getUsers
GET    /getUserById/123
POST   /createUser
PUT    /updateUser/123
DELETE /deleteUser/123
```

## Use Plural Nouns

**Good (plural):**
```
GET /users
GET /products
GET /orders
```

**Bad (singular):**
```
GET /user
GET /product
GET /order
```

## Hierarchical Resources

**Good (hierarchical):**
```
GET /users/123/orders           # User's orders
GET /users/123/orders/456       # Specific order
GET /users/123/orders/456/items # Order items
```

**Bad (flat):**
```
GET /orders?userId=123
GET /orderItems?orderId=456
```

## Collections vs Single Resources

**Collection (plural):**
```
GET /users              # List all users
POST /users             # Create new user
```

**Single resource (singular ID):**
```
GET /users/123          # Get specific user
PUT /users/123          # Update user
DELETE /users/123       # Delete user
```
```

**URL Structure Best Practices:**

```markdown
## URL Structure

**Format:**
```
https://api.example.com/v1/resources/id/sub-resources
```

**Examples:**
```
# Users
GET    /v1/users
GET    /v1/users/123
POST   /v1/users
PUT    /v1/users/123
PATCH  /v1/users/123
DELETE /v1/users/123

# User's orders
GET    /v1/users/123/orders
GET    /v1/users/123/orders/456
POST   /v1/users/123/orders

# Products
GET    /v1/products
GET    /v1/products/789
POST   /v1/products
PUT    /v1/products/789
DELETE /v1/products/789

# Product reviews
GET    /v1/products/789/reviews
POST   /v1/products/789/reviews
```

## Query Parameters

**Filtering:**
```
GET /users?status=active
GET /users?role=admin&status=active
```

**Sorting:**
```
GET /users?sort=created_at
GET /users?sort=-created_at  # Descending
GET /users?sort=name,created_at
```

**Pagination:**
```
GET /users?page=2&limit=20
GET /users?offset=40&limit=20
```

**Field selection:**
```
GET /users?fields=id,name,email
GET /users/123?fields=id,name,email,orders
```

**Search:**
```
GET /users?q=john
GET /products?search=laptop
```
```

---

## Pillar 2: HTTP Methods

### Goal: Use HTTP methods correctly

**HTTP Methods:**

```markdown
## GET - Retrieve Resource

**Purpose:** Fetch data, no side effects

**Examples:**
```http
GET /users              # List all users
GET /users/123          # Get specific user
GET /users/123/orders   # Get user's orders
```

**Response:**
```json
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com"
}
```

**Status codes:**
- 200 OK - Success
- 404 Not Found - Resource doesn't exist

## POST - Create Resource

**Purpose:** Create new resource

**Examples:**
```http
POST /users
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com"
}
```

**Response:**
```json
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2026-05-26T00:42:25Z"
}
```

**Status codes:**
- 201 Created - Resource created
- 400 Bad Request - Invalid data
- 409 Conflict - Resource already exists

**Headers:**
```
Location: /users/123
```

## PUT - Replace Resource

**Purpose:** Replace entire resource (all fields)

**Examples:**
```http
PUT /users/123
Content-Type: application/json

{
  "name": "John Smith",
  "email": "john.smith@example.com",
  "phone": "+1234567890"
}
```

**Response:**
```json
{
  "id": 123,
  "name": "John Smith",
  "email": "john.smith@example.com",
  "phone": "+1234567890",
  "updated_at": "2026-05-26T00:42:25Z"
}
```

**Status codes:**
- 200 OK - Updated
- 404 Not Found - Resource doesn't exist

**Idempotent:** Multiple identical requests = same result

## PATCH - Partial Update

**Purpose:** Update specific fields only

**Examples:**
```http
PATCH /users/123
Content-Type: application/json

{
  "email": "newemail@example.com"
}
```

**Response:**
```json
{
  "id": 123,
  "name": "John Doe",
  "email": "newemail@example.com",
  "phone": "+1234567890",
  "updated_at": "2026-05-26T00:42:25Z"
}
```

**Status codes:**
- 200 OK - Updated
- 404 Not Found - Resource doesn't exist

## DELETE - Remove Resource

**Purpose:** Delete resource

**Examples:**
```http
DELETE /users/123
```

**Response:**
```json
{
  "message": "User deleted successfully"
}
```

**Or no content:**
```
204 No Content
```

**Status codes:**
- 200 OK - Deleted (with response body)
- 204 No Content - Deleted (no response body)
- 404 Not Found - Resource doesn't exist

**Idempotent:** Multiple deletes = same result (404 after first)
```

**Idempotency:**

```markdown
## Idempotent Methods

**Idempotent:** Same request multiple times = same result

**Idempotent methods:**
- GET - Always returns same data
- PUT - Replaces with same data
- DELETE - Already deleted = 404
- PATCH - Updates to same value

**Not idempotent:**
- POST - Creates multiple resources

**Example:**
```http
# First POST - creates user 123
POST /users → 201 Created, id=123

# Second POST - creates user 124 (different!)
POST /users → 201 Created, id=124

# First PUT - updates user 123
PUT /users/123 → 200 OK

# Second PUT - same result
PUT /users/123 → 200 OK (idempotent)
```
```

---

## Pillar 3: Status Codes

### Goal: Use appropriate HTTP status codes

**Status Code Categories:**

```markdown
## 2xx Success

**200 OK**
- GET: Resource retrieved
- PUT: Resource updated
- PATCH: Resource updated
- DELETE: Resource deleted (with body)

**201 Created**
- POST: Resource created
- Include Location header

**202 Accepted**
- Request accepted, processing async
- Return job ID or status URL

**204 No Content**
- DELETE: Resource deleted (no body)
- PUT/PATCH: Updated (no body)

## 3xx Redirection

**301 Moved Permanently**
- Resource permanently moved
- Update bookmarks

**302 Found**
- Temporary redirect

**304 Not Modified**
- Cached version still valid
- Use with ETag/If-None-Match

## 4xx Client Errors

**400 Bad Request**
- Invalid request format
- Validation errors
- Missing required fields

**401 Unauthorized**
- Missing or invalid authentication
- Need to log in

**403 Forbidden**
- Authenticated but not authorized
- Insufficient permissions

**404 Not Found**
- Resource doesn't exist
- Invalid endpoint

**405 Method Not Allowed**
- HTTP method not supported
- e.g., DELETE on read-only resource

**409 Conflict**
- Resource already exists
- Concurrent modification conflict

**422 Unprocessable Entity**
- Valid format but semantic errors
- Business logic validation failed

**429 Too Many Requests**
- Rate limit exceeded
- Include Retry-After header

## 5xx Server Errors

**500 Internal Server Error**
- Unexpected server error
- Generic error

**502 Bad Gateway**
- Upstream service error
- Gateway/proxy issue

**503 Service Unavailable**
- Server temporarily unavailable
- Maintenance mode
- Include Retry-After header

**504 Gateway Timeout**
- Upstream service timeout
```

**Error Response Format:**

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request data",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      },
      {
        "field": "age",
        "message": "Must be at least 18"
      }
    ],
    "request_id": "req_abc123",
    "timestamp": "2026-05-26T00:42:25Z"
  }
}
```

---

## Pillar 4: Versioning

### Goal: Manage API changes without breaking clients

**Versioning Strategies:**

```markdown
## URL Versioning (Recommended)

**Format:**
```
https://api.example.com/v1/users
https://api.example.com/v2/users
```

**Pros:**
- Clear and visible
- Easy to test different versions
- Cacheable

**Cons:**
- URL changes

**Example:**
```
# Version 1
GET /v1/users/123
{
  "id": 123,
  "name": "John Doe"
}

# Version 2 (added email)
GET /v2/users/123
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com"
}
```

## Header Versioning

**Format:**
```http
GET /users/123
Accept: application/vnd.example.v1+json
```

**Pros:**
- Clean URLs
- RESTful

**Cons:**
- Less visible
- Harder to test

## Query Parameter Versioning

**Format:**
```
GET /users/123?version=1
GET /users/123?api_version=2
```

**Pros:**
- Simple

**Cons:**
- Pollutes query params
- Not RESTful

## Breaking vs Non-Breaking Changes

**Non-breaking (no version bump):**
- Adding new endpoints
- Adding optional fields
- Adding new response fields
- Relaxing validation

**Breaking (version bump required):**
- Removing endpoints
- Removing fields
- Changing field types
- Renaming fields
- Changing authentication
- Changing error formats
```

**Version Lifecycle:**

```markdown
## Version Lifecycle

**Active:** Current version, fully supported
**Deprecated:** Old version, still works, will be removed
**Sunset:** Removed, no longer works

**Example timeline:**
```
2026-01-01: v2 released (v1 active, v2 active)
2026-06-01: v1 deprecated (v1 deprecated, v2 active)
2026-12-01: v1 sunset (v1 removed, v2 active)
```

**Deprecation headers:**
```http
HTTP/1.1 200 OK
Deprecation: true
Sunset: Sat, 01 Dec 2026 00:00:00 GMT
Link: </v2/users/123>; rel="successor-version"
```

**Deprecation notice:**
```json
{
  "data": { ... },
  "meta": {
    "deprecated": true,
    "sunset_date": "2026-12-01",
    "migration_guide": "https://docs.example.com/migration/v1-to-v2"
  }
}
```
```

---

## Pillar 5: Authentication

### Goal: Secure API access

**Authentication Methods:**

```markdown
## API Keys

**Simple, good for server-to-server:**

```http
GET /users
Authorization: Bearer sk_live_abc123xyz789
```

**Or custom header:**
```http
GET /users
X-API-Key: sk_live_abc123xyz789
```

**Pros:**
- Simple
- Easy to implement

**Cons:**
- No user context
- Hard to revoke selectively

## OAuth 2.0

**Standard for user authorization:**

**Authorization Code Flow (web apps):**
```
1. User clicks "Login with Example"
2. Redirect to: https://auth.example.com/authorize?
   client_id=abc123&
   redirect_uri=https://app.com/callback&
   response_type=code&
   scope=read write

3. User approves

4. Redirect back: https://app.com/callback?code=xyz789

5. Exchange code for token:
   POST https://auth.example.com/token
   {
     "grant_type": "authorization_code",
     "code": "xyz789",
     "client_id": "abc123",
     "client_secret": "secret",
     "redirect_uri": "https://app.com/callback"
   }

6. Response:
   {
     "access_token": "eyJhbGc...",
     "token_type": "Bearer",
     "expires_in": 3600,
     "refresh_token": "refresh_abc123"
   }

7. Use token:
   GET /users/me
   Authorization: Bearer eyJhbGc...
```

**Client Credentials Flow (server-to-server):**
```http
POST https://auth.example.com/token
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&
client_id=abc123&
client_secret=secret&
scope=read
```

## JWT (JSON Web Tokens)

**Self-contained tokens:**

**Structure:**
```
header.payload.signature
```

**Example:**
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

**Decoded payload:**
```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022,
  "exp": 1516242622,
  "scope": "read write"
}
```

**Usage:**
```http
GET /users/me
Authorization: Bearer eyJhbGc...
```

**Pros:**
- Stateless
- Contains user info
- Can be verified without database

**Cons:**
- Can't revoke before expiry
- Larger than API keys
```

**Rate Limiting:**

```markdown
## Rate Limiting

**Protect API from abuse:**

**Headers:**
```http
HTTP/1.1 200 OK
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1622505600
```

**Rate limit exceeded:**
```http
HTTP/1.1 429 Too Many Requests
Retry-After: 3600

{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Rate limit exceeded. Try again in 1 hour.",
    "limit": 1000,
    "remaining": 0,
    "reset_at": "2026-05-26T01:42:25Z"
  }
}
```

**Rate limit strategies:**
- **Per API key:** 1000 requests/hour
- **Per user:** 100 requests/minute
- **Per IP:** 10 requests/second
- **Per endpoint:** Different limits for different endpoints
```

---

## Pillar 6: Error Handling

### Goal: Provide helpful error messages

**Error Response Format:**

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request data",
    "details": [
      {
        "field": "email",
        "code": "INVALID_FORMAT",
        "message": "Invalid email format"
      },
      {
        "field": "age",
        "code": "OUT_OF_RANGE",
        "message": "Must be at least 18"
      }
    ],
    "request_id": "req_abc123",
    "timestamp": "2026-05-26T00:42:25Z",
    "documentation_url": "https://docs.example.com/errors/validation"
  }
}
```

**Error Codes:**

```markdown
## Standard Error Codes

**Authentication:**
- `UNAUTHORIZED` - Missing or invalid credentials
- `FORBIDDEN` - Insufficient permissions
- `TOKEN_EXPIRED` - Access token expired
- `INVALID_TOKEN` - Malformed token

**Validation:**
- `VALIDATION_ERROR` - Request validation failed
- `MISSING_FIELD` - Required field missing
- `INVALID_FORMAT` - Field format invalid
- `OUT_OF_RANGE` - Value out of acceptable range

**Resources:**
- `NOT_FOUND` - Resource doesn't exist
- `ALREADY_EXISTS` - Resource already exists
- `CONFLICT` - Concurrent modification conflict

**Rate Limiting:**
- `RATE_LIMIT_EXCEEDED` - Too many requests

**Server:**
- `INTERNAL_ERROR` - Unexpected server error
- `SERVICE_UNAVAILABLE` - Service temporarily unavailable
- `GATEWAY_TIMEOUT` - Upstream service timeout
```

---

## Pillar 7: Documentation

### Goal: Make API easy to understand and use

**OpenAPI/Swagger:**

```yaml
openapi: 3.0.0
info:
  title: Example API
  version: 1.0.0
  description: API for managing users and orders

servers:
  - url: https://api.example.com/v1

paths:
  /users:
    get:
      summary: List users
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/User'
                  meta:
                    $ref: '#/components/schemas/Pagination'
    
    post:
      summary: Create user
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUserRequest'
      responses:
        '201':
          description: Created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'

  /users/{id}:
    get:
      summary: Get user
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '404':
          description: Not found

components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        email:
          type: string
          format: email
        created_at:
          type: string
          format: date-time
    
    CreateUserRequest:
      type: object
      required:
        - name
        - email
      properties:
        name:
          type: string
        email:
          type: string
          format: email
    
    Pagination:
      type: object
      properties:
        page:
          type: integer
        limit:
          type: integer
        total:
          type: integer

  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

security:
  - bearerAuth: []
```

**Documentation Best Practices:**

```markdown
## Documentation Checklist

- [ ] Overview and getting started guide
- [ ] Authentication instructions
- [ ] Complete endpoint reference
- [ ] Request/response examples
- [ ] Error codes and meanings
- [ ] Rate limiting details
- [ ] Versioning policy
- [ ] Changelog
- [ ] SDKs and client libraries
- [ ] Interactive API explorer (Swagger UI)
- [ ] Postman collection
- [ ] Code examples in multiple languages

## Example Documentation Structure

**Getting Started**
- Authentication
- Making your first request
- Rate limits
- Versioning

**API Reference**
- Users
  - List users
  - Get user
  - Create user
  - Update user
  - Delete user
- Orders
  - List orders
  - Get order
  - Create order
  - Update order
  - Cancel order

**Guides**
- Pagination
- Filtering and sorting
- Error handling
- Webhooks
- Best practices

**Resources**
- Changelog
- SDKs
- Postman collection
- Status page
- Support
```

---

## API Design Checklist

**Resource Modeling:**
- [ ] Use nouns, not verbs
- [ ] Use plural nouns
- [ ] Hierarchical structure
- [ ] Consistent naming

**HTTP Methods:**
- [ ] GET for retrieval
- [ ] POST for creation
- [ ] PUT for full update
- [ ] PATCH for partial update
- [ ] DELETE for removal
- [ ] Idempotent where appropriate

**Status Codes:**
- [ ] 2xx for success
- [ ] 4xx for client errors
- [ ] 5xx for server errors
- [ ] Appropriate codes for each scenario

**Versioning:**
- [ ] Version strategy chosen (URL recommended)
- [ ] Breaking changes require version bump
- [ ] Deprecation policy defined
- [ ] Migration guides provided

**Authentication:**
- [ ] Authentication method chosen
- [ ] API keys or OAuth 2.0
- [ ] Rate limiting implemented
- [ ] Secure token storage

**Error Handling:**
- [ ] Consistent error format
- [ ] Helpful error messages
- [ ] Error codes defined
- [ ] Request IDs for debugging

**Documentation:**
- [ ] OpenAPI/Swagger spec
- [ ] Getting started guide
- [ ] Complete endpoint reference
- [ ] Code examples
- [ ] Interactive API explorer

---

## Common API Design Mistakes

### Mistake 1: Using Verbs in URLs
**Problem:** `/getUsers`, `/createUser`

**Solution:** Use nouns + HTTP methods: `GET /users`, `POST /users`

### Mistake 2: Inconsistent Naming
**Problem:** `/users`, `/product`, `/order-items`

**Solution:** Consistent plural nouns: `/users`, `/products`, `/order-items`

### Mistake 3: Wrong Status Codes
**Problem:** Returning 200 OK for errors

**Solution:** Use appropriate codes: 400 for validation, 404 for not found, 500 for server errors

### Mistake 4: No Versioning
**Problem:** Breaking changes break all clients

**Solution:** Version from day 1: `/v1/users`

### Mistake 5: Unclear Error Messages
**Problem:** `{"error": "Error"}`

**Solution:** Detailed errors with codes and field-level details

### Mistake 6: No Rate Limiting
**Problem:** API abuse, DDoS

**Solution:** Implement rate limiting with clear headers

---

## Integration with Other Skills

**Use before API design:**
- `architecture-blueprint` - System design
- `security-review-owasp` - Security considerations

**Use during API design:**
- `test-driven-development` - API tests
- `requesting-code-review` - Review API changes

**Use after API design:**
- `performance-optimization` - Optimize API performance
- `user-research` - Developer feedback

---

**Remember:** Good API design is about developer experience. Make it intuitive, consistent, and well-documented. Think like a developer using your API. Test with real developers. Iterate based on feedback.
