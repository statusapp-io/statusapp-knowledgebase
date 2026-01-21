---
title: API Reference Guide
slug: api-reference-guide
category: API
excerpt: Complete guide to using the StatusApp REST API for programmatic monitoring management.
order: 9
---

# StatusApp API Reference Guide

## Overview

The StatusApp API allows you to programmatically manage monitors, retrieve incidents, access analytics, and automate your monitoring workflow. The API is RESTful, returns JSON responses, and uses standard HTTP authentication.

**Base URL**: `https://statusapp.io/api/v1`

## Authentication

### API Keys

All API requests require authentication using an API key. Generate API keys from your dashboard at **Settings → Integrations → API Access**.

### Authentication Methods

**Option 1: X-API-Key Header** (Recommended)
```bash
curl -H "X-API-Key: sk_live_abc123xyz..." \
  https://statusapp.io/api/v1/monitors
```

**Option 2: Authorization Bearer Token**
```bash
curl -H "Authorization: Bearer sk_live_abc123xyz..." \
  https://statusapp.io/api/v1/monitors
```

### API Key Security

- **Never commit API keys** to source control
- **Use environment variables** to store keys
- **Rotate keys regularly** using expiration dates
- **Revoke unused keys** immediately from dashboard
- **Use minimal permissions** - only grant what's needed

## Rate Limits

Rate limits are based on your subscription plan:

| Plan | Requests per Hour |
|------|-------------------|
| Free | 100 |
| Starter | 1,000 |
| Professional | 5,000 |
| Business | 10,000 |
| Enterprise | 100,000 |

### Rate Limit Headers

Every API response includes rate limit information:

```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 856
X-RateLimit-Reset: 1737201600
```

When rate limit is exceeded, you'll receive a `429 Too Many Requests` response.

## Endpoints

### Monitors

#### List All Monitors

```http
GET /api/v1/monitors
```

**Response**:
```json
{
  "monitors": [
    {
      "id": "mon_abc123",
      "name": "Production API",
      "url": "https://api.example.com",
      "type": "API",
      "status": "UP",
      "interval": 300,
      "timeout": 30,
      "isActive": true,
      "isPaused": false,
      "createdAt": "2025-01-01T00:00:00Z",
      "updatedAt": "2025-01-21T10:30:00Z"
    }
  ],
  "count": 1
}
```

#### Get Monitor by ID

```http
GET /api/v1/monitors/{monitorId}
```

**Response**:
```json
{
  "id": "mon_abc123",
  "name": "Production API",
  "url": "https://api.example.com",
  "type": "API",
  "status": "UP",
  "interval": 300,
  "timeout": 30,
  "expectedStatusCode": 200,
  "httpMethod": "GET",
  "enabledRegions": ["us-east-1", "eu-west-1"],
  "avgResponseTime24h": 250,
  "uptime90d": 99.95
}
```

#### Create Monitor

```http
POST /api/v1/monitors
Content-Type: application/json
```

**Request Body**:
```json
{
  "name": "My Website",
  "url": "https://example.com",
  "type": "WEBSITE",
  "interval": 300,
  "timeout": 30,
  "expectedStatusCode": 200,
  "httpMethod": "GET",
  "enabledRegions": ["us-east-1", "eu-west-1"]
}
```

**Monitor Types**: `WEBSITE`, `API`, `GRAPHQL`, `PING`, `PORT`, `DNS`, `SSL_CERT`, `CRON`

**Optional Fields**:
- `requestHeaders` - JSON string of headers
- `requestBody` - Request body for POST/PUT
- `port` - Port number for PORT monitors
- `protocol` - TCP or UDP for PORT monitors  
- `dnsRecord` - DNS record type for DNS monitors
- `sslCertExpiryDays` - Days before SSL expiration to alert
- `gracePeriod` - Grace period for CRON monitors

**Response**:
```json
{
  "id": "mon_xyz789",
  "name": "My Website",
  "status": "UP",
  "createdAt": "2025-01-21T10:30:00Z"
}
```

#### Update Monitor

```http
PUT /api/v1/monitors/{monitorId}
Content-Type: application/json
```

All fields optional. Only provided fields will be updated.

#### Delete Monitor

```http
DELETE /api/v1/monitors/{monitorId}
```

**Response**:
```json
{
  "success": true,
  "message": "Monitor deleted successfully"
}
```

#### Pause/Resume Monitor

```http
POST /api/v1/monitors/{monitorId}/pause
POST /api/v1/monitors/{monitorId}/resume
```

**Response**:
```json
{
  "id": "mon_abc123",
  "isPaused": true
}
```

### Heartbeat Endpoints

Heartbeat monitors allow you to ping StatusApp when scheduled jobs run.

#### Ping Heartbeat (Success)

```http
POST /api/v1/heartbeat/{monitorId}
GET /api/v1/heartbeat/{monitorId}
HEAD /api/v1/heartbeat/{monitorId}
```

All methods accept the heartbeat ping. Use whichever is most convenient.

**Response**:
```json
{
  "success": true,
  "monitorId": "mon_abc123",
  "timestamp": "2025-01-21T10:30:00Z"
}
```

#### Report Heartbeat Failure

```http
POST /api/v1/heartbeat/{monitorId}/fail
```

Use this to explicitly report when a job fails.

**Optional Body**:
```json
{
  "error": "Database backup failed: disk full"
}
```

#### Start Heartbeat Job

```http
POST /api/v1/heartbeat/{monitorId}/start
```

Report when a long-running job starts. Useful for tracking job duration.

#### Log Heartbeat Message

```http
POST /api/v1/heartbeat/{monitorId}/log
Content-Type: application/json
```

**Request**:
```json
{
  "message": "Backup completed: 1.2GB backed up successfully"
}
```

Add context to heartbeat pings without failing the check.

### Incidents

#### List Incidents

```http
GET /api/v1/incidents
GET /api/v1/incidents?monitorId={monitorId}
GET /api/v1/incidents?status=active
```

**Query Parameters**:
- `monitorId` - Filter by specific monitor
- `status` - Filter by status: `active`, `resolved`, or `all`
- `limit` - Number of results (default: 50, max: 100)
- `offset` - Pagination offset

**Response**:
```json
{
  "incidents": [
    {
      "id": "inc_xyz789",
      "monitorId": "mon_abc123",
      "monitorName": "Production API",
      "startedAt": "2025-01-21T10:00:00Z",
      "resolvedAt": "2025-01-21T10:15:00Z",
      "duration": 900,
      "isResolved": true,
      "severity": "HIGH",
      "status": "resolved",
      "checksFailed": 3,
      "checksTotal": 3,
      "mttr": 900
    }
  ],
  "count": 1,
  "hasMore": false
}
```

#### Get Incident by ID

```http
GET /api/v1/incidents/{incidentId}
```

**Response**:
```json
{
  "id": "inc_xyz789",
  "monitorId": "mon_abc123",
  "monitorName": "Production API",
  "startedAt": "2025-01-21T10:00:00Z",
  "resolvedAt": "2025-01-21T10:15:00Z",
  "duration": 900,
  "isResolved": true,
  "severity": "HIGH",
  "status": "resolved",
  "rootCause": "Database connection timeout",
  "rootCauseCategory": "DATABASE_ISSUE",
  "affectedRegions": ["us-east-1", "us-west-2"],
  "checksFailed": 3,
  "checksTotal": 3
}
```

### Status Pages

#### List Status Pages

```http
GET /api/v1/status-pages
```

**Response**:
```json
{
  "statusPages": [
    {
      "id": "sp_abc123",
      "name": "Public Status",
      "slug": "status",
      "customDomain": "status.example.com",
      "isPublic": true,
      "monitorCount": 5
    }
  ]
}
```

#### Get Status Page

```http
GET /api/v1/status-pages/{statusPageId}
```

Returns detailed status page information including monitors.

### Worker Regions

#### List Available Regions

```http
GET /api/v1/worker-regions
```

**Response**:
```json
{
  "regions": [
    {
      "id": "region_us_east_1",
      "city": "Virginia",
      "country": "United States",
      "countryCode": "US",
      "latitude": 37.926868,
      "longitude": -78.024902,
      "isActive": true
    }
  ]
}
```

Use this to get valid region codes for monitor configuration.

### Health Check

#### API Health

```http
GET /api/v1/health
```

**Response**:
```json
{
  "status": "ok",
  "version": "1.0.0",
  "timestamp": "2025-01-21T10:30:00Z"
}
```

Use for uptime monitoring of the StatusApp API itself.

## Error Handling

### Error Response Format

All errors return a consistent JSON format:

```json
{
  "error": "Error message description",
  "code": "ERROR_CODE",
  "statusCode": 400
}
```

### Common Error Codes

| HTTP Status | Error Code | Description |
|-------------|------------|-------------|
| 400 | `BAD_REQUEST` | Invalid request parameters |
| 401 | `UNAUTHORIZED` | Missing or invalid API key |
| 403 | `FORBIDDEN` | Insufficient permissions |
| 404 | `NOT_FOUND` | Resource not found |
| 429 | `RATE_LIMIT_EXCEEDED` | Too many requests |
| 500 | `INTERNAL_SERVER_ERROR` | Server error |

## Code Examples

### Node.js / JavaScript

```javascript
const STATUSAPP_API_KEY = process.env.STATUSAPP_API_KEY;
const BASE_URL = 'https://statusapp.io/api/v1';

async function createMonitor() {
  const response = await fetch(`${BASE_URL}/monitors`, {
    method: 'POST',
    headers: {
      'X-API-Key': STATUSAPP_API_KEY,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      name: 'My API',
      url: 'https://api.example.com',
      type: 'API',
      interval: 300,
      expectedStatusCode: 200
    })
  });
  
  const data = await response.json();
  console.log('Monitor created:', data);
}
```

### Python

```python
import os
import requests

STATUSAPP_API_KEY = os.environ['STATUSAPP_API_KEY']
BASE_URL = 'https://statusapp.io/api/v1'

def create_monitor():
    headers = {
        'X-API-Key': STATUSAPP_API_KEY,
        'Content-Type': 'application/json'
    }
    
    payload = {
        'name': 'My API',
        'url': 'https://api.example.com',
        'type': 'API',
        'interval': 300,
        'expectedStatusCode': 200
    }
    
    response = requests.post(
        f'{BASE_URL}/monitors',
        headers=headers,
        json=payload
    )
    
    print('Monitor created:', response.json())
```

### cURL

```bash
curl -X POST https://statusapp.io/api/v1/monitors \
  -H "X-API-Key: sk_live_abc123xyz..." \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My API",
    "url": "https://api.example.com",
    "type": "API",
    "interval": 300,
    "expectedStatusCode": 200
  }'
```

### Go

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "net/http"
    "os"
)

func createMonitor() {
    apiKey := os.Getenv("STATUSAPP_API_KEY")
    baseURL := "https://statusapp.io/api/v1"
    
    payload := map[string]interface{}{
        "name":               "My API",
        "url":                "https://api.example.com",
        "type":               "API",
        "interval":           300,
        "expectedStatusCode": 200,
    }
    
    jsonData, _ := json.Marshal(payload)
    req, _ := http.NewRequest("POST", baseURL+"/monitors", bytes.NewBuffer(jsonData))
    req.Header.Set("X-API-Key", apiKey)
    req.Header.Set("Content-Type", "application/json")
    
    client := &http.Client{}
    resp, _ := client.Do(req)
    defer resp.Body.Close()
    
    var result map[string]interface{}
    json.NewDecoder(resp.Body).Decode(&result)
    fmt.Println("Monitor created:", result)
}
```

## Webhooks

### Receiving Webhooks from StatusApp

StatusApp can send webhooks to your endpoints when events occur. This is different from the API - webhooks push data to you instead of you pulling data from the API.

Set up webhooks in **Settings → Notification Channels → Create Channel → Webhook**.

### Webhook Payload

```json
{
  "event": "monitor.down",
  "timestamp": "2025-01-21T10:30:00Z",
  "monitor": {
    "id": "mon_abc123",
    "name": "Production API",
    "url": "https://api.example.com",
    "type": "API",
    "status": "DOWN"
  },
  "incident": {
    "id": "inc_xyz789",
    "startedAt": "2025-01-21T10:25:00Z",
    "duration": 300,
    "checksFailed": 3
  },
  "check": {
    "statusCode": 500,
    "responseTime": 1250,
    "error": "Internal Server Error",
    "region": "us-east-1"
  }
}
```

### Webhook Events

- `monitor.down` - Monitor went down
- `monitor.up` - Monitor recovered
- `monitor.degraded` - Performance degraded
- `regression.detected` - Performance regression
- `regression.resolved` - Regression resolved
- `ssl.expiring` - SSL certificate expiring soon

## Best Practices

### 1. Use Environment Variables

Never hardcode API keys in your code:

```javascript
const API_KEY = process.env.STATUSAPP_API_KEY;
```

### 2. Handle Rate Limits

Check rate limit headers and implement backoff:

```javascript
if (response.status === 429) {
  const resetTime = response.headers.get('X-RateLimit-Reset');
  const waitSeconds = resetTime - Math.floor(Date.now() / 1000);
  await new Promise(resolve => setTimeout(resolve, waitSeconds * 1000));
  // Retry request
}
```

### 3. Error Handling

Always handle errors gracefully:

```javascript
try {
  const response = await fetch(url, options);
  if (!response.ok) {
    const error = await response.json();
    console.error('API Error:', error.error);
  }
} catch (error) {
  console.error('Network Error:', error);
}
```

### 4. Pagination

Handle pagination for large result sets:

```javascript
let offset = 0;
const limit = 100;
let hasMore = true;

while (hasMore) {
  const response = await fetch(
    `${BASE_URL}/monitors?limit=${limit}&offset=${offset}`
  );
  const data = await response.json();
  
  // Process data.monitors
  
  offset += limit;
  hasMore = data.hasMore;
}
```

### 5. Verify Webhook Signatures

Validate webhook authenticity (if signature header provided):

```javascript
const crypto = require('crypto');

function verifyWebhook(payload, signature, secret) {
  const expectedSignature = crypto
    .createHmac('sha256', secret)
    .update(JSON.stringify(payload))
    .digest('hex');
    
  return signature === expectedSignature;
}
```

## Common Use Cases

### Automated Monitor Creation

Create monitors automatically when deploying new services:

```javascript
async function deployService(serviceName, serviceUrl) {
  // Deploy service...
  
  // Create monitor
  await createMonitor({
    name: `${serviceName} - Production`,
    url: serviceUrl,
    type: 'API',
    interval: 300,
    enabledRegions: ['us-east-1', 'eu-west-1']
  });
}
```

### Incident Reporting Integration

Pull incidents into your incident management system:

```javascript
async function syncIncidents() {
  const response = await fetch(`${BASE_URL}/incidents?status=active`);
  const { incidents } = await response.json();
  
  for (const incident of incidents) {
    await createTicketInJira(incident);
  }
}
```

### Custom Dashboard

Build custom monitoring dashboards:

```javascript
async function getDashboardData() {
  const monitors = await fetch(`${BASE_URL}/monitors`).then(r => r.json());
  const incidents = await fetch(`${BASE_URL}/incidents?status=active`).then(r => r.json());
  
  return {
    totalMonitors: monitors.count,
    upMonitors: monitors.monitors.filter(m => m.status === 'UP').length,
    activeIncidents: incidents.count
  };
}
```

### Heartbeat Integration

Ping monitors from cron jobs or scheduled tasks:

```bash
# In crontab
0 2 * * * /usr/local/bin/backup.sh && curl -X POST https://statusapp.io/api/v1/heartbeat/mon_abc123
```

## Troubleshooting

### 401 Unauthorized

- Verify API key is correct
- Check API key is active (not expired/revoked)
- Ensure proper header format

### 429 Rate Limit Exceeded

- Check your plan's rate limits
- Implement exponential backoff
- Cache responses when possible
- Upgrade plan if needed

### 403 Forbidden

- Verify API key has required permissions
- Check account is active (not suspended)

### Slow API Responses

- Use pagination for large datasets
- Cache responses when appropriate
- Monitor from regions closer to API servers

## Support

Need help with the API?

- **Documentation**: Full API docs at https://statusapp.io/docs/api
- **Support**: Email api-support@statusapp.io
- **Status**: Check API status at https://status.statusapp.io

## Next Steps

- **[Getting Started](/help/knowledge-base/getting-started-with-statusapp)** - Set up your first monitor
- **[Understanding Monitor Types](/help/knowledge-base/monitor-types-overview)** - Learn about monitor configuration
- **[Setting Up Notifications](/help/knowledge-base/notification-channels-overview)** - Configure alerts
- **[Team Collaboration](/help/knowledge-base/team-collaboration)** - Manage team access and API keys