---
title: API Monitors
slug: api-monitors
category: Monitors
excerpt: Monitor REST APIs with authentication, assertions, schema validation, and multi-step workflows.
order: 4
---

# API Monitors

## Overview

API monitors are specialized for testing REST APIs. They support authentication, response validation, multi-step workflows, and detailed performance profiling.

## Creating an API Monitor

1. Create a new monitor
2. Select **API** type
3. Enter endpoint URL
4. Configure authentication (if needed)
5. Set up assertions and validation
6. Save and test

## Basic Setup

### URL
Full API endpoint URL:
- `https://api.example.com/users`
- `https://api.example.com/status`
- `https://api.example.com/v2/health`

### HTTP Method
- **GET** - Retrieve data (default)
- **POST** - Create resource
- **PUT** - Replace resource
- **PATCH** - Update resource
- **DELETE** - Delete resource

### Expected Status Code
HTTP response code that indicates success:
- **200** - OK (GET, POST, PUT, PATCH)
- **201** - Created (POST returning created object)
- **202** - Accepted (async operation started)
- **204** - No Content (DELETE, no response body)

## Authentication

StatusApp supports multiple authentication types for API monitors:

### None
No authentication required (public endpoints).

### Bearer Token
For JWT or OAuth token-based authentication:

**Setup**:
1. In **Authentication** section, select **Bearer**
2. Enter your token
3. Automatically added as `Authorization: Bearer <token>`

**Example**:
```
Token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### Basic Authentication
Username and password:

**Setup**:
1. Select **Basic**
2. Enter username
3. Enter password
4. Automatically encoded in header

**Behind the scenes**: Converts to `Authorization: Basic <base64>`

### API Key
Custom header or query parameter authentication:

**Setup**:
1. Select **API Key**
2. Enter header name (e.g., `X-API-Key`)
3. Enter key value
4. Added as custom header

**Example**:
```
Header: X-API-Key
Value: sk_live_abc123xyz
```

### OAuth 2.0
For OAuth 2.0 flows:

**Supported Grant Types**:
- Client Credentials
- Password (Resource Owner)
- Refresh Token

**Setup**:
1. Select **OAuth2**
2. Enter Token URL
3. Enter Client ID and Client Secret
4. Select grant type
5. StatusApp automatically fetches and refreshes tokens

### JWT (Custom)
For custom JWT authentication with a specific prefix:

**Setup**:
1. Select **JWT**
2. Enter your JWT token
3. Optionally specify custom header prefix

### Custom Headers
For any other authentication needs, add custom headers:

```
Authorization: Signature keyId="12",algorithm="hmac-sha256",signature="..."
X-Auth-Token: custom-token-here
X-Tenant-ID: tenant-123
```

## Request Body

For POST/PUT/PATCH requests:

**JSON**:
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "status": "active"
}
```

**Form Data**:
```
name=John+Doe&email=john@example.com&role=admin
```

**XML** (if API supports):
```xml
<user>
  <name>John Doe</name>
  <email>john@example.com</email>
</user>
```

## Response Validation

### Status Code Check
Verify expected HTTP status:
- ✓ 200 OK for successful GET
- ✓ 201 Created for successful POST
- ✓ 202 Accepted for async operations

### Keyword Validation
Check response contains or excludes text:

**Contains** (success if present):
- `"status": "ok"`
- `"user_id": 12345`
- `healthy`

**Exclude** (success if NOT present):
- `"error"`
- `"exception"`
- `"failed"`

### JSON Path Validation
Assert specific JSON fields exist and have expected values:

**JSON Response**:
```json
{
  "status": "ok",
  "data": {
    "user_id": 123,
    "name": "John",
    "role": "admin"
  }
}
```

**JSON Path Assertions**:
- `$.status = "ok"` ✓
- `$.data.user_id = 123` ✓
- `$.data.role = "admin"` ✓

### Response Time Threshold
Alert if API is too slow:
- Fast APIs: < 200ms
- Standard APIs: < 500ms
- Acceptable: < 1000ms
- Warning: > 2000ms

### Full Response Schema Validation
Validate entire response structure against JSON schema:

Ensures response hasn't changed unexpectedly and catches breaking API changes.

## Common API Tests

### Health Check Endpoint
Test basic API availability:

```
URL: https://api.example.com/health
Method: GET
Status: 200
Validate Contains: "status": "ok"
```

### Authenticated Endpoint
Monitor endpoint requiring authentication:

```
URL: https://api.example.com/users/me
Method: GET
Auth: Bearer Token (your token)
Status: 200
Validate: $.user_id exists
```

### Data Creation (POST)
Test creating resource:

```
URL: https://api.example.com/events
Method: POST
Body: {"event": "test", "source": "monitor"}
Status: 201
Validate: $.event_id exists
```

### Update Operation (PUT)
Test updating resource:

```
URL: https://api.example.com/settings/1
Method: PUT
Body: {"theme": "dark", "notifications": true}
Status: 200
Validate: $.updated_at exists
```

### Deletion (DELETE)
Test deletion:

```
URL: https://api.example.com/events/123
Method: DELETE
Status: 204 (or 200)
```

## Multi-Step Workflows

Test sequences of API calls:

**Example Workflow**:
1. Login → Get authentication token
2. Use token to fetch user data
3. Update user profile
4. Verify changes

**Setup**:
- Configure each step as separate monitor (for now)
- Or use external script that calls your heartbeat endpoint with result

## Performance Profiling

API monitor automatically tracks:
- **DNS Resolution Time**: Domain lookup
- **TCP Connection Time**: Socket connection
- **SSL/TLS Handshake**: Certificate exchange
- **Time to First Byte (TTFB)**: Server processing
- **Download Time**: Response transfer
- **Total Response Time**: End-to-end

**View in Analytics**: Compare performance across time and regions

## Common Use Cases

### Monitor API Availability
```
URL: https://api.myapp.com/status
Method: GET
Status: 200
Alert if: Any failure
```

### Monitor Critical Endpoint
```
URL: https://api.myapp.com/users/current
Method: GET
Auth: Bearer Token
Status: 200
Validate: $.user_id exists
Alert if: Down or missing user_id
```

### Track API Performance
```
Same as above, plus:
Response Time Threshold: 500ms
Alert if: Slower than 500ms consistently
```

### Verify Authentication Works
```
URL: https://api.myapp.com/auth
Method: POST
Body: {"username": "monitor", "password": "test"}
Status: 200
Validate: $.token exists
```

## Best Practices

### 1. Use Realistic Test Data
- Don't create production records during monitoring
- Use test/mock accounts for authentication
- Use dedicated monitoring endpoints if available
- Clean up any test data created

### 2. Check Multiple Aspects
- Status code (is it responding?)
- Response content (is it correct data?)
- Response time (is it fast enough?)
- Authentication (does auth work?)

### 3. Validate, Don't Just Check Status
- Different status code might still indicate failure
- API could return 200 with error in JSON
- Check actual data returned, not just "success"

### 4. Multi-Region Testing
- Monitor from regions near users
- Catch regional API issues
- Test CDN/routing effectiveness
- Use 2+ regions to reduce false positives

### 5. Appropriate Intervals
- Critical APIs: 1-5 minutes
- Production: 5-15 minutes
- Non-critical: 15-60 minutes

## Troubleshooting

### Authentication Failing

**Check Token**:
- Verify token is current and not expired
- Some tokens only valid for specific scopes
- Refresh token if available

**Check Header Format**:
- `Authorization: Bearer <token>` (with space)
- Not `AuthorizationBearer` (no space)
- Not `Bearer: <token>` (wrong order)

**Check API Requirements**:
- Verify API uses Bearer auth (not API Key)
- Check if token needs specific scopes

### Validation Failing

**Check Response Format**:
- Verify API returns JSON (not XML/HTML)
- Check response structure matches expectations
- Test endpoint manually with curl

**Check Assertions**:
- Use correct JSON Path syntax
- Verify field names (case-sensitive)
- Test assertions manually with response

### Slow API

**Identify Bottleneck**:
- Look at performance profiling metrics
- Longest phase indicates where slowness is:
  - TTFB high = server processing slow
  - Download time high = large payload/slow network

**Solutions**:
- Optimize server code if TTFB high
- Reduce response payload if download high
- Check network between monitor and API

### Status Code Unexpected

**Verify Expectation**:
- What status does your API actually return?
- Test manually with curl to confirm
- Some APIs return 202 (Accepted) for async
- Update monitor to match actual response

## Next Steps

- **[HTTP Monitors](/help/knowledge-base/http-website-monitors)** - For website monitoring
- **[Heartbeat Monitors](/help/knowledge-base/heartbeat-monitors)** - For scheduled jobs
- **[TCP Monitors](/help/knowledge-base/tcp-port-monitors)** - For database/service monitoring
- **[Notifications](/help/knowledge-base/notification-channels-overview)** - Set up alerts
- **[Analytics](/help/knowledge-base/understanding-analytics)** - Track performance
