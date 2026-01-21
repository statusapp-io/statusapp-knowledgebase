---
title: API Reference and Integration Guide
slug: api-reference-guide
category: API
excerpt: Complete guide to StatusApp API for programmatic monitor and incident management.
order: 9
---

# API Reference and Integration Guide

## Getting Started with the API

### Authentication

All API requests require an API key in the Authorization header:

```bash
curl -H "Authorization: Bearer sk_live_abc123..." \
  https://api.statusapp.io/monitors
```

### API Key Management

1. Go to **Settings > API Keys**
2. Click **Create New Key**
3. Enter key name
4. Select permissions:
   - **read**: View data only
   - **write**: Create and update data
   - **admin**: Full access
5. Copy and save securely

### Rate Limits

- **Free Plan**: 100 requests/hour
- **Starter Plan**: 1,000 requests/hour
- **Professional Plan**: 10,000 requests/hour
- **Business Plan**: 50,000 requests/hour
- **Enterprise**: Custom limits

## Endpoints

### Monitors

#### List Monitors

```bash
GET /api/monitors
```

Parameters:
- `status`: Filter by status (UP, DOWN, DEGRADED)
- `category`: Filter by monitor type
- `limit`: Results per page (default: 20, max: 100)
- `page`: Page number (default: 1)

#### Get Monitor Details

```bash
GET /api/monitors/{id}
```

#### Create Monitor

```bash
POST /api/monitors

{
  "name": "API Health Check",
  "type": "http",
  "url": "https://api.example.com/health",
  "interval": 60,
  "timeout": 10
}
```

#### Update Monitor

```bash
PUT /api/monitors/{id}

{
  "name": "Updated Name",
  "status": "PAUSED"
}
```

#### Delete Monitor

```bash
DELETE /api/monitors/{id}
```

### Incidents

#### List Incidents

```bash
GET /api/incidents

Parameters:
- status: Filter by status (INVESTIGATING, IDENTIFIED, MONITORING, RESOLVED)
- limit: Results per page
- page: Page number
```

#### Create Incident

```bash
POST /api/incidents

{
  "title": "Database Maintenance",
  "description": "Planned maintenance on primary database",
  "status": "INVESTIGATING",
  "affectedComponents": ["mon_123", "mon_456"],
  "impact": "high"
}
```

#### Update Incident

```bash
PUT /api/incidents/{id}

{
  "status": "RESOLVED",
  "update": "Issue resolved. All systems operational."
}
```

#### Add Incident Update

```bash
POST /api/incidents/{id}/updates

{
  "message": "Investigating database connectivity issues",
  "notifySubscribers": true
}
```

### Status Pages

#### List Status Pages

```bash
GET /api/status-pages
```

#### Get Status Page

```bash
GET /api/status-pages/{id}
```

#### Update Status Page

```bash
PUT /api/status-pages/{id}

{
  "name": "Updated Name",
  "description": "Updated description"
}
```

### Webhooks

#### Event Types

- `monitor.down`: Monitor status changed to DOWN
- `monitor.up`: Monitor recovered to UP
- `incident.created`: New incident created
- `incident.updated`: Incident status changed
- `incident.resolved`: Incident resolved

#### Webhook Payload

```json
{
  "event": "monitor.down",
  "timestamp": "2024-01-21T10:30:00Z",
  "monitor": {
    "id": "mon_123",
    "name": "API Server",
    "status": "DOWN",
    "lastCheck": "2024-01-21T10:29:58Z"
  },
  "incident": {
    "id": "inc_456",
    "status": "ACTIVE"
  }
}
```

#### Verifying Webhook Signatures

```python
import hmac
import hashlib

def verify_signature(payload, signature, secret):
    expected = hmac.new(
        secret.encode(),
        payload.encode(),
        hashlib.sha256
    ).hexdigest()
    return hmac.compare_digest(signature, expected)
```

## Error Handling

### HTTP Status Codes

- `200 OK`: Request successful
- `201 Created`: Resource created
- `400 Bad Request`: Invalid parameters
- `401 Unauthorized`: Invalid API key
- `403 Forbidden`: Insufficient permissions
- `404 Not Found`: Resource not found
- `429 Too Many Requests`: Rate limit exceeded
- `500 Internal Server Error`: Server error

### Error Response

```json
{
  "error": {
    "code": "invalid_request",
    "message": "Invalid monitor URL",
    "details": "URL must be valid HTTP/HTTPS"
  }
}
```

## Integration Examples

### Python Integration

```python
import requests

api_key = "sk_live_abc123"
headers = {"Authorization": f"Bearer {api_key}"}

# Get all monitors
response = requests.get(
    "https://api.statusapp.io/monitors",
    headers=headers
)
monitors = response.json()

# Create incident
incident_data = {
    "title": "Service Degradation",
    "status": "INVESTIGATING",
    "affectedComponents": ["mon_123"]
}
response = requests.post(
    "https://api.statusapp.io/incidents",
    headers=headers,
    json=incident_data
)
```

### JavaScript Integration

```javascript
const apiKey = "sk_live_abc123";

// Get all incidents
const response = await fetch(
  "https://api.statusapp.io/incidents",
  {
    headers: {
      "Authorization": `Bearer ${apiKey}`
    }
  }
);
const incidents = await response.json();

// Create monitor
const monitorData = {
  name: "Website Check",
  type: "http",
  url: "https://example.com",
  interval: 60
};

const createResponse = await fetch(
  "https://api.statusapp.io/monitors",
  {
    method: "POST",
    headers: {
      "Authorization": `Bearer ${apiKey}`,
      "Content-Type": "application/json"
    },
    body: JSON.stringify(monitorData)
  }
);
```

## Common Use Cases

### Automated Incident Creation

Create incidents from external monitoring systems:

```bash
curl -X POST https://api.statusapp.io/incidents \
  -H "Authorization: Bearer sk_live_abc123" \
  -d '{
    "title": "External Alert",
    "description": "Alert from monitoring system",
    "status": "INVESTIGATING"
  }'
```

### Bulk Monitor Management

Programmatically create multiple monitors:

```python
for url in ["https://api1.example.com", "https://api2.example.com"]:
    requests.post(
        "https://api.statusapp.io/monitors",
        headers=headers,
        json={
            "name": f"Monitor for {url}",
            "type": "http",
            "url": url,
            "interval": 60
        }
    )
```

## Pagination

API responses with multiple results are paginated:

```json
{
  "data": [/* items */],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 150,
    "pages": 8
  }
}
```

Access next page:

```bash
GET /api/monitors?page=2&limit=20
```

## SDKs

Official SDKs available:
- Python: `pip install statusapp`
- Node.js: `npm install @statusapp/sdk`
- Go: `go get github.com/statusapp-io/go-sdk`

## API Documentation

Full interactive API documentation: https://api.statusapp.io/docs

- Try endpoints in browser
- See request/response examples
- Test with your API key
- View real data

## Support

- Email: api-support@statusapp.io
- Documentation: https://docs.statusapp.io
- Issues: https://github.com/statusapp-io/api-issues