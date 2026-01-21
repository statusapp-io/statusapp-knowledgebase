---
title: GraphQL Monitors
slug: graphql-monitors
category: Monitors
excerpt: Monitor GraphQL queries, mutations, and performance with custom assertions.
order: 9
---

# GraphQL Monitors

## Overview

GraphQL monitors test your GraphQL endpoints with custom queries and mutations. Perfect for API health checks, authentication validation, and performance monitoring of your GraphQL schema.

## Creating a GraphQL Monitor

1. Create a new monitor
2. Select **GraphQL** type
3. Enter GraphQL endpoint URL
4. Write your query or mutation
5. (Optional) Add variables
6. Configure assertions
7. Save and test

## Configuration

### Endpoint URL
Your GraphQL endpoint:
```
https://api.example.com/graphql
https://api.example.com/v1/graphql
https://graphql.example.com
```

Typically ends in `/graphql` but verify with your API docs.

### HTTP Method
GraphQL typically uses:
- **POST**: Standard for queries and mutations
- **GET**: Some APIs support GET for queries only

### Authentication
Add headers for authentication:

**Bearer Token**:
```
Authorization: Bearer YOUR_TOKEN_HERE
```

**API Key in Header**:
```
X-API-Key: your-api-key-123
```

**Custom Headers**:
```
X-Custom-Auth: value
X-Request-ID: unique-id
```

## Writing GraphQL Queries

### Simple Query
```graphql
query {
  user {
    id
    name
    email
  }
}
```

### Query with Arguments
```graphql
query {
  user(id: "123") {
    id
    name
    email
    posts {
      title
      published
    }
  }
}
```

### Query with Variables
```graphql
query GetUser($userId: ID!) {
  user(id: $userId) {
    id
    name
    email
    posts {
      id
      title
      published
    }
  }
}
```

Variables (JSON):
```json
{
  "userId": "123"
}
```

### Mutation
```graphql
mutation CreatePost($title: String!, $content: String!) {
  createPost(title: $title, content: $content) {
    id
    title
    content
    createdAt
  }
}
```

Variables:
```json
{
  "title": "New Post",
  "content": "Post content here"
}
```

### Authentication Check
```graphql
query {
  me {
    id
    name
    email
    role
  }
}
```

Returns current authenticated user (confirms auth works).

### Health Check
```graphql
query {
  __schema {
    types {
      name
    }
  }
}
```

Very lightweight - just checks schema is accessible.

## Response Assertions

### Status Code
- **200**: Success (default)
- **400**: Bad request (usually query syntax)
- **401**: Unauthorized
- **403**: Forbidden
- **500**: Server error

### JSON Path Assertions

Verify specific fields in response:

**Path Syntax**:
```
$.data.user.id
$.data.user.posts[0].title
$.data.createPost.id
$.errors[0].message
```

**Common Assertions**:

Check field exists and has value:
```
$.data.user.id exists
```

Check field equals value:
```
$.data.user.role = "admin"
$.data.user.active = true
```

Check field contains text:
```
$.data.post.title contains "important"
```

Check field is not empty:
```
$.data.user.email is_not_empty
```

### Error Checking
GraphQL often returns 200 with errors in response:

```graphql
{
  "data": {
    "user": null
  },
  "errors": [
    {
      "message": "User not found",
      "extensions": {
        "code": "NOT_FOUND"
      }
    }
  ]
}
```

Assert no errors:
```
$.errors is_empty
```

Assert specific error:
```
$.errors[0].message contains "not found"
```

### Response Time
Ensure queries are performant:

```
Response time < 500ms
```

Good thresholds:
- Health checks: < 100ms
- Simple queries: < 500ms
- Complex queries: < 2000ms
- Mutations: < 1000ms

## Common Use Cases

### API Health Check
Simple query that exercises authentication and data retrieval:

```
Endpoint: https://api.example.com/graphql
Query: 
  query {
    me {
      id
      name
    }
  }
Assertions:
  - Status: 200
  - $.data.me.id exists
  - $.errors is_empty
```

### Authentication Validation
Verify bearer token is working:

```
Endpoint: https://api.example.com/graphql
Headers: Authorization: Bearer YOUR_TOKEN
Query:
  query {
    me {
      id
      email
      role
    }
  }
Assertions:
  - $.data.me.role = "admin"
  - $.data.me.email exists
```

### Data Availability
Check database is responding:

```
Query:
  query {
    users(limit: 1) {
      id
      name
    }
  }
Assertions:
  - $.data.users exists
  - $.data.users is_not_empty
```

### Mutation Testing
Test create/update operations work:

```
Query:
  mutation {
    createPost(title: "Test", content: "Testing") {
      id
      title
    }
  }
Assertions:
  - $.data.createPost.id exists
  - $.data.createPost.title = "Test"
```

### Performance Monitoring
Track query performance over time:

```
Query: Complex user profile query
Assertions:
  - Response time < 800ms
  - $.data.user.profile exists
```

## Advanced Topics

### Subscriptions
Subscriptions (real-time) are not supported by monitors.

Use **Webhook** monitors instead if you need to test GraphQL subscriptions.

### Introspection
Query schema introspection:

```graphql
query {
  __schema {
    types {
      name
      description
    }
  }
}
```

Useful to verify schema is accessible and hasn't changed.

### Multiple Operations
Some GraphQL servers support multiple operations:

```graphql
query GetUser {
  user(id: "123") {
    id
    name
  }
}

query GetPosts {
  posts(limit: 10) {
    id
    title
  }
}
```

StatusApp will use the first operation by default.

### Nested Variables
Complex variable structures:

```graphql
query {
  search(filters: { status: ACTIVE, role: ADMIN }) {
    id
    name
  }
}
```

Variables (JSON):
```json
{
  "filters": {
    "status": "ACTIVE",
    "role": "ADMIN"
  }
}
```

## Best Practices

### 1. Keep Queries Simple
Avoid overly complex queries in monitors:

```graphql
# Good: Simple, fast
query {
  me {
    id
    name
  }
}

# Bad: Complex, might timeout
query {
  user(id: "123") {
    posts(limit: 100) {
      comments(limit: 50) {
        author {
          followers(limit: 20) {
            id
          }
        }
      }
    }
  }
}
```

### 2. Use Lightweight Queries
For frequent monitoring (every minute):

```graphql
# Good: Minimal data transfer
query {
  health {
    status
  }
}

# Better: Just schema check
query {
  __typename
}
```

### 3. Test with Real Data
Use actual user IDs and data that exists:

```graphql
# Good: Real user ID that always exists
query {
  user(id: "system") {
    id
    status
  }
}

# Bad: Hard-coded ID that might not exist
query {
  user(id: "random-id-123") {
    id
  }
}
```

### 4. Handle Authentication Properly
Store tokens securely:

```
Use Custom Header for Bearer tokens:
Authorization: Bearer <your-token>

Never expose tokens in:
- GitHub commits
- Monitor names or descriptions
- Logs
```

### 5. Monitor Key Operations
Focus on critical paths:

- Authentication (login/me queries)
- Core data retrieval (most-used queries)
- Important mutations (create/update operations)
- Performance-critical queries

### 6. Set Appropriate Timeouts
Account for query complexity:

```
Simple query: 1-5 seconds
Medium query: 5-15 seconds
Complex query: 15-30 seconds

Default: 10 seconds
```

### 7. Use Variables for Reusability
Makes queries more maintainable:

```graphql
# Good: Reusable query
query GetUser($id: ID!) {
  user(id: $id) {
    id
    name
  }
}

# Variables: { "id": "123" }

# Less flexible: Hard-coded values
query {
  user(id: "123") {
    id
    name
  }
}
```

## Troubleshooting

### Query Returns 200 but Error in Response

GraphQL often returns successful HTTP status with errors in response:

```json
{
  "errors": [
    {
      "message": "Authentication required",
      "extensions": {
        "code": "UNAUTHENTICATED"
      }
    }
  ]
}
```

**Solution**: 
- Check if authentication header is correct
- Verify token hasn't expired
- Ensure bearer token format: `Bearer YOUR_TOKEN`

### Query Timeout

Complex queries might exceed time limit:

```
Solution:
- Simplify query (reduce nested fields)
- Increase monitor timeout to 20-30 seconds
- Use pagination to limit results
- Query less frequently (every 5 min vs every min)
```

### JSON Path Assertions Not Matching

Verify response structure:

```
Query returns:
{
  "data": {
    "user": {
      "id": "123",
      "email": null
    }
  }
}

Assertion $.data.user.email exists - FAILS (null is technically "empty")
Fix: Use $.data.user.email is_not_empty
```

### Wrong Data Type in Response

Ensure type matching:

```
Response: {"count": "100"}  (string, not number)

Assertion count = 100 - FAILS
Fix: count = "100" (string comparison)
```

## Response Time Metrics

GraphQL monitors track:
- **DNS time**: Domain resolution
- **Connect time**: TCP connection
- **TLS time**: SSL/HTTPS handshake
- **Request time**: Query execution
- **Response time**: Total (sum of above)

Use `Response time < Xms` assertion to enforce performance SLAs.

## Comparing GraphQL to HTTP/API Monitors

| Aspect | GraphQL | HTTP/API |
|--------|---------|----------|
| Queries | Single endpoint | Multiple endpoints |
| Configuration | Complex query language | Simple URL + method |
| Authentication | Via headers | Via headers |
| Assertions | JSON path in response | Status + body |
| Use case | Test GraphQL APIs | Test REST/generic HTTP |

**Use GraphQL monitors when**: Testing GraphQL endpoints, complex assertion needs
**Use HTTP/API monitors when**: Testing REST APIs, simpler checks

## Next Steps

- **[API Monitors](/articles/monitors/api-monitors)** - HTTP/REST API monitoring alternative
- **[Notifications](/articles/alerting-notifications/notification-channels-overview)** - Set up alerts
- **[Analytics](/articles/analytics/understanding-analytics)** - Track query performance
