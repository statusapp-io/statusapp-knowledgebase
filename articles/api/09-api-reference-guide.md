---
title: API Reference Guide
slug: api-reference-guide
category: API
excerpt: Complete guide to using the StatusApp REST API for programmatic monitoring management.
order: 9
---

# StatusApp API Reference Guide

## Overview

The StatusApp API allows you to programmatically manage monitors, retrieve incidents, access analytics, configure webhooks, and automate your entire monitoring workflow. The API is RESTful, returns JSON responses, and uses API key authentication.

**Base URL**: `https://ops.statusapp.io/api/v1`

---

## Authentication

### API Keys

All API requests require authentication using an API key. Generate API keys from your dashboard at **Profile > API** tab.

API keys are stored as SHA256 hashes for security. When you create an API key, you'll see it once - make sure to copy and store it securely.

### Authentication Methods

**Option 1: X-API-Key Header** (Recommended)
```bash
curl -H "X-API-Key: your_api_key_here" \
  https://ops.statusapp.io/api/v1/monitors
```

**Option 2: Authorization Bearer Token**
```bash
curl -H "Authorization: Bearer your_api_key_here" \
  https://ops.statusapp.io/api/v1/monitors
```

### API Key Properties

When creating an API key, you can configure:

| Property | Description |
|----------|-------------|
| **Name** | A descriptive name for the key |
| **Permissions** | Comma-separated: `read`, `write`, `delete` |
| **Expiration** | Optional expiry date (keys can be permanent) |

### API Key Security Best Practices

- **Never commit API keys** to source control
- **Use environment variables** to store keys
- **Rotate keys regularly** using expiration dates
- **Revoke unused keys** immediately from dashboard
- **Use minimal permissions** - only grant what's needed
- **Monitor usage** - check `lastUsedAt` to detect unauthorized access

---

## Rate Limits

Rate limits are based on your subscription plan:

| Plan | Requests per Hour |
|------|-------------------|
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

| Header | Description |
|--------|-------------|
| `X-RateLimit-Limit` | Maximum requests allowed per hour |
| `X-RateLimit-Remaining` | Requests remaining in current window |
| `X-RateLimit-Reset` | Unix timestamp when limit resets |

When rate limit is exceeded, you'll receive a `429 Too Many Requests` response.

---

## CORS Support

The API supports Cross-Origin Resource Sharing (CORS) for browser-based applications:

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS
Access-Control-Allow-Headers: Content-Type, Authorization, X-API-Key
Access-Control-Max-Age: 86400
```

---

## Monitors API

### List All Monitors

Retrieve all monitors with optional filtering and pagination.

```http
GET /api/v1/monitors
```

**Query Parameters**:

| Parameter | Type | Description |
|-----------|------|-------------|
| `page` | integer | Page number (default: 1) |
| `limit` | integer | Results per page (max: 100, default: 50) |
| `status` | string | Filter by status: `UP`, `DOWN`, `DEGRADED` |
| `type` | string | Filter by monitor type |
| `search` | string | Search by name or URL |
| `sort` | string | Sort field (default: `createdAt`) |
| `order` | string | Sort order: `asc` or `desc` (default: `desc`) |

**Response**:
```json
{
  "monitors": [
    {
      "id": "clxyz123...",
      "name": "Production API",
      "url": "https://api.example.com",
      "type": "API",
      "interval": 300,
      "status": "UP",
      "isPaused": false,
      "isActive": true,
      "createdAt": "2025-01-01T00:00:00Z",
      "updatedAt": "2025-01-21T10:30:00Z",
      "checks": [
        {
          "id": "check_abc...",
          "status": "UP",
          "responseTime": 245,
          "statusCode": 200,
          "checkedAt": "2025-01-21T10:30:00Z",
          "workerRegion": {
            "country": "United States",
            "countryCode": "US",
            "city": "Virginia"
          }
        }
      ],
      "_count": {
        "checks": 1440,
        "incidents": 0
      }
    }
  ],
  "pagination": {
    "total": 25,
    "page": 1,
    "limit": 50,
    "totalPages": 1
  }
}
```

### Get Monitor by ID

```http
GET /api/v1/monitors/{monitorId}
```

**Response**: Full monitor object with all configuration details.

### Create Monitor

```http
POST /api/v1/monitors
Content-Type: application/json
```

**Request Body**:
```json
{
  "name": "My API Monitor",
  "url": "https://api.example.com/health",
  "type": "API",
  "interval": 300,
  "timeout": 30,
  "expectedStatusCode": 200,
  "httpMethod": "GET",
  "enabledWorkerRegionIds": ["region_us_east", "region_eu_west"],
  "regressionEnabled": true,
  "regressionThreshold": 50,
  "notifications": [
    {
      "type": "EMAIL",
      "enabled": true,
      "config": { "emails": ["alerts@example.com"] }
    }
  ]
}
```

**Monitor Types**:

| Type | Description | Required Fields |
|------|-------------|-----------------|
| `WEBSITE` | HTTP/HTTPS website monitoring | `url` |
| `API` | REST API endpoint monitoring | `url`, optionally `httpMethod`, `requestHeaders`, `requestBody` |
| `CRON` | Heartbeat/scheduled job monitoring | `url` (auto-generated heartbeatUrl) |
| `PING` | ICMP ping monitoring | `url` (hostname/IP) |
| `PORT` | TCP/UDP port monitoring | `url`, `port`, `protocol` |
| `DNS` | DNS record monitoring | `url`, `dnsRecord`, optionally `expectedIp` |
| `SSL_CERT` | SSL certificate expiry monitoring | `url` (HTTPS URL) |
| `DOMAIN` | Domain expiry & health monitoring | `url` (domain name) |

**Full Schema**:

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | **Required**. Monitor name |
| `url` | string | **Required**. URL, hostname, or IP to monitor |
| `type` | string | Monitor type (default: `WEBSITE`) |
| `interval` | integer | Check interval in seconds (min: 60, default: 300) |
| `timeout` | integer | Timeout in seconds (5-300, default: 30) |
| `port` | integer | Port number for PORT monitors (1-65535) |
| `protocol` | string | Protocol for PORT monitors: `TCP` or `UDP` |
| `expectedStatusCode` | integer | Expected HTTP status code (100-599) |
| `httpMethod` | string | HTTP method: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, `OPTIONS` |
| `requestHeaders` | string | JSON string of custom headers |
| `requestBody` | string | Request body for POST/PUT requests |
| `dnsRecord` | string | DNS record type: `A`, `AAAA`, `CNAME`, `MX`, `TXT` |
| `expectedIp` | string | Expected IP for DNS validation |
| `gracePeriod` | integer | Grace period for CRON monitors (60-86400 seconds) |
| `sslCertExpiryDays` | integer | Days before SSL expiry to alert (1-90) |
| `enabledWorkerRegionIds` | array | Worker region IDs for multi-region monitoring |
| `regressionEnabled` | boolean | Enable performance regression detection |
| `regressionThreshold` | integer | Regression threshold percentage (25-200) |
| `regressionPeriod` | integer | Regression calculation period in days (1-30) |
| `performanceTimings` | boolean | Capture detailed performance timings |
| `apiAuthConfig` | object | API authentication configuration (see below) |
| `notifications` | array | Notification channel configurations |

**API Authentication Config** (`apiAuthConfig`):

```json
{
  "type": "bearer",
  "token": "your-api-token"
}
```

| Auth Type | Required Fields |
|-----------|-----------------|
| `none` | No additional fields |
| `bearer` | `token` |
| `basic` | `username`, `password` |
| `api_key` | `apiKey`, `apiKeyHeader` or `apiKeyName`, `apiKeyLocation` (`header`/`query`) |
| `oauth2` | `oauth2TokenUrl`, `oauth2ClientId`, `oauth2ClientSecret`, `oauth2Scope`, `oauth2GrantType` |
| `jwt` | `jwtToken`, optionally `jwtHeader`, `jwtPrefix` |

**Domain Monitoring Fields**:

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `domainExpiryDays` | integer | 30 | Days before domain expiry to alert |
| `domainMonitorDns` | boolean | true | Monitor DNS record changes |
| `domainMonitorBlacklist` | boolean | true | Check domain against blacklists |
| `domainMonitorWhois` | boolean | true | Monitor WHOIS data changes |

**Response**:
```json
{
  "id": "clxyz789...",
  "name": "My API Monitor",
  "url": "https://api.example.com/health",
  "type": "API",
  "status": "UP",
  "createdAt": "2025-01-21T10:30:00Z",
  "heartbeatUrl": null,
  "monitorNotifications": [...]
}
```

### Update Monitor

```http
PUT /api/v1/monitors/{monitorId}
Content-Type: application/json
```

All fields are optional. Only provided fields will be updated.

### Delete Monitor

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

### Pause/Resume Monitor

```http
POST /api/v1/monitors/{monitorId}/pause
```

Toggles the monitor's paused state. When paused, no checks are performed.

**Response**:
```json
{
  "id": "clxyz123...",
  "isPaused": true
}
```

### Get Regression Settings

```http
GET /api/v1/monitors/{monitorId}/regression
```

**Response**:
```json
{
  "regressionEnabled": true,
  "regressionThreshold": 50,
  "regressionPeriod": 7,
  "baselineResponseTime": 250,
  "baselineCalculatedAt": "2025-01-20T00:00:00Z"
}
```

### Update Regression Settings

```http
PUT /api/v1/monitors/{monitorId}/regression
Content-Type: application/json
```

**Request Body**:
```json
{
  "regressionEnabled": true,
  "regressionThreshold": 75,
  "regressionPeriod": 14
}
```

---

## Heartbeat API

Heartbeat monitors (CRON type) use a unique URL to receive pings from your scheduled jobs. The heartbeat URL is automatically generated when you create a CRON monitor.

**Rate Limit**: Maximum 5 pings per minute per monitor.

### Send Success Ping

Report successful job completion.

```http
GET /api/v1/heartbeat/{heartbeatId}
POST /api/v1/heartbeat/{heartbeatId}
HEAD /api/v1/heartbeat/{heartbeatId}
```

All HTTP methods work identically. Use whichever is most convenient for your environment.

**Optional Body** (POST only, max 10KB):
```json
{
  "message": "Backup completed: 1.2GB processed"
}
```

**Response**:
```json
{
  "message": "OK"
}
```

### Report Job Start

Indicate a long-running job has started.

```http
GET /api/v1/heartbeat/{heartbeatId}/start
POST /api/v1/heartbeat/{heartbeatId}/start
```

**Response**:
```json
{
  "message": "OK"
}
```

### Report Job Failure

Explicitly report when a job fails.

```http
GET /api/v1/heartbeat/{heartbeatId}/fail
POST /api/v1/heartbeat/{heartbeatId}/fail
```

**Optional Body**:
```json
{
  "error": "Database connection timeout after 30s"
}
```

### Log Message

Record a log message without changing monitor status.

```http
POST /api/v1/heartbeat/{heartbeatId}/log
Content-Type: application/json
```

**Request Body**:
```json
{
  "message": "Processing batch 5 of 10..."
}
```

### Heartbeat Integration Examples

**Cron Job (Bash)**:
```bash
#!/bin/bash
# Notify start
curl -s https://ops.statusapp.io/api/v1/heartbeat/abc123/start

# Run backup
if /usr/local/bin/backup.sh; then
  # Success
  curl -s https://ops.statusapp.io/api/v1/heartbeat/abc123
else
  # Failure
  curl -s -X POST https://ops.statusapp.io/api/v1/heartbeat/abc123/fail \
    -d '{"error": "Backup script failed"}'
fi
```

**Node.js Scheduled Task**:
```javascript
const fetch = require('node-fetch');
const HEARTBEAT_URL = 'https://ops.statusapp.io/api/v1/heartbeat/abc123';

async function runScheduledTask() {
  // Signal start
  await fetch(`${HEARTBEAT_URL}/start`);

  try {
    await performTask();
    // Signal success
    await fetch(HEARTBEAT_URL);
  } catch (error) {
    // Signal failure
    await fetch(`${HEARTBEAT_URL}/fail`, {
      method: 'POST',
      body: JSON.stringify({ error: error.message })
    });
  }
}
```

**Python Script**:
```python
import requests

HEARTBEAT_URL = "https://ops.statusapp.io/api/v1/heartbeat/abc123"

def run_job():
    # Signal start
    requests.get(f"{HEARTBEAT_URL}/start")

    try:
        do_work()
        # Signal success
        requests.get(HEARTBEAT_URL)
    except Exception as e:
        # Signal failure
        requests.post(f"{HEARTBEAT_URL}/fail", json={"error": str(e)})
```

---

## Incidents API

### List Incidents

```http
GET /api/v1/incidents
```

**Query Parameters**:

| Parameter | Type | Description |
|-----------|------|-------------|
| `page` | integer | Page number (default: 1) |
| `limit` | integer | Results per page (max: 100, default: 50) |
| `monitorId` | string | Filter by monitor ID |
| `status` | string | Filter by status: `open` or `resolved` |
| `sort` | string | Sort field (default: `startedAt`) |
| `order` | string | Sort order: `asc` or `desc` |

**Response**:
```json
{
  "incidents": [
    {
      "id": "inc_xyz789",
      "incidentNumber": "INC-2025-0042",
      "monitorId": "mon_abc123",
      "startedAt": "2025-01-21T10:00:00Z",
      "resolvedAt": "2025-01-21T10:15:00Z",
      "isResolved": true,
      "severity": "high",
      "status": "resolved",
      "rootCause": "Database connection timeout",
      "rootCauseCategory": "DATABASE_ISSUE",
      "affectedRegions": ["us-east-1", "us-west-2"],
      "checksFailed": 3,
      "checksTotal": 3,
      "mttr": 900
    }
  ],
  "pagination": {
    "total": 42,
    "page": 1,
    "limit": 50,
    "totalPages": 1
  }
}
```

### Get Incident by ID

```http
GET /api/v1/incidents/{incidentId}
```

Returns full incident details including:
- Status update history
- Affected regions
- Root cause analysis
- MTTR (Mean Time To Recovery)
- Associated check results

**Root Cause Categories**:
- `DNS_FAILURE`
- `NETWORK_TIMEOUT`
- `SERVER_ERROR`
- `APPLICATION_ERROR`
- `DATABASE_ISSUE`
- `SSL_CERTIFICATE`
- `CONFIGURATION_ERROR`
- `DEPLOYMENT_ISSUE`
- `THIRD_PARTY_SERVICE`
- `INFRASTRUCTURE`
- `DDOS_ATTACK`
- `MAINTENANCE`
- `UNKNOWN`

---

## Status Pages API

### List Status Pages

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
      "description": "Current system status",
      "customDomain": "status.example.com",
      "isPublic": true,
      "brandColor": "#22c55e",
      "createdAt": "2025-01-01T00:00:00Z"
    }
  ]
}
```

### Get Status Page

```http
GET /api/v1/status-pages/{statusPageId}
```

Returns full status page configuration including:
- Monitor groups
- Associated monitors
- Display settings
- Custom branding

### Create Status Page

```http
POST /api/v1/status-pages
Content-Type: application/json
```

**Request Body**:
```json
{
  "name": "API Status",
  "slug": "api-status",
  "description": "Real-time API service status",
  "isPublic": true,
  "brandColor": "#3b82f6",
  "logoUrl": "https://example.com/logo.png"
}
```

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | **Required**. Status page name |
| `slug` | string | **Required**. URL slug (must be unique) |
| `description` | string | Page description |
| `isPublic` | boolean | Whether page is publicly accessible |
| `brandColor` | string | Hex color for branding |
| `logoUrl` | string | URL to logo image |
| `faviconUrl` | string | URL to favicon |
| `customDomain` | string | Custom domain (requires DNS setup) |

### Update Status Page

```http
PUT /api/v1/status-pages/{statusPageId}
Content-Type: application/json
```

### Delete Status Page

```http
DELETE /api/v1/status-pages/{statusPageId}
```

---

## Notification Channels API

### List Notification Channels

```http
GET /api/v1/notifications
```

**Response**:
```json
{
  "channels": [
    {
      "id": "nc_abc123",
      "name": "Engineering Slack",
      "type": "SLACK",
      "isActive": true,
      "createdAt": "2025-01-01T00:00:00Z"
    }
  ]
}
```

### Get Notification Channel

```http
GET /api/v1/notifications/{channelId}
```

### Create Notification Channel

```http
POST /api/v1/notifications
Content-Type: application/json
```

**Notification Types & Configuration**:

**Email**:
```json
{
  "name": "Team Email",
  "type": "EMAIL",
  "config": {
    "emails": ["team@example.com", "oncall@example.com"]
  }
}
```

**SMS** (requires plan with SMS feature):
```json
{
  "name": "On-Call SMS",
  "type": "SMS",
  "config": {
    "phoneNumbers": ["+1234567890"]
  }
}
```

**Slack**:
```json
{
  "name": "Engineering Slack",
  "type": "SLACK",
  "config": {
    "webhookUrl": "https://hooks.slack.com/services/...",
    "channel": "#alerts"
  }
}
```

**Discord**:
```json
{
  "name": "Discord Alerts",
  "type": "DISCORD",
  "config": {
    "webhookUrl": "https://discord.com/api/webhooks/..."
  }
}
```

**Telegram**:
```json
{
  "name": "Telegram Bot",
  "type": "TELEGRAM",
  "config": {
    "botToken": "123456:ABC...",
    "chatId": "-1001234567890"
  }
}
```

**PagerDuty**:
```json
{
  "name": "PagerDuty",
  "type": "PAGERDUTY",
  "config": {
    "integrationKey": "your-pagerduty-integration-key",
    "severity": "critical"
  }
}
```

**OpsGenie**:
```json
{
  "name": "OpsGenie",
  "type": "OPSGENIE",
  "config": {
    "apiKey": "your-opsgenie-api-key",
    "priority": "P1"
  }
}
```

**Microsoft Teams**:
```json
{
  "name": "Teams Channel",
  "type": "MICROSOFT_TEAMS",
  "config": {
    "webhookUrl": "https://outlook.office.com/webhook/..."
  }
}
```

**Custom Webhook**:
```json
{
  "name": "Custom Webhook",
  "type": "WEBHOOK",
  "config": {
    "url": "https://api.example.com/webhook",
    "headers": { "Authorization": "Bearer token" }
  }
}
```

### Update Notification Channel

```http
PUT /api/v1/notifications/{channelId}
Content-Type: application/json
```

### Delete Notification Channel

```http
DELETE /api/v1/notifications/{channelId}
```

---

## Webhooks API

Configure webhooks to receive event notifications at your endpoints.

### List Webhooks

```http
GET /api/v1/webhooks
```

**Response**:
```json
{
  "webhooks": [
    {
      "id": "wh_abc123",
      "name": "Production Webhook",
      "url": "https://api.example.com/statusapp-events",
      "events": "monitor.down,monitor.up,incident.created,incident.resolved",
      "isActive": true,
      "lastStatus": 200,
      "lastTriggeredAt": "2025-01-21T10:30:00Z"
    }
  ]
}
```

### Get Webhook

```http
GET /api/v1/webhooks/{webhookId}
```

### Create Webhook

```http
POST /api/v1/webhooks
Content-Type: application/json
```

**Request Body**:
```json
{
  "name": "Production Webhook",
  "url": "https://api.example.com/statusapp-events",
  "events": "monitor.down,monitor.up,incident.created,incident.resolved",
  "secret": "your-webhook-secret"
}
```

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | **Required**. Webhook name |
| `url` | string | **Required**. Endpoint URL |
| `events` | string | **Required**. Comma-separated event types |
| `secret` | string | Optional secret for signature verification |

**Available Events**:
- `monitor.down` - Monitor detected as down
- `monitor.up` - Monitor recovered
- `incident.created` - New incident created
- `incident.resolved` - Incident resolved

### Update Webhook

```http
PUT /api/v1/webhooks/{webhookId}
Content-Type: application/json
```

### Delete Webhook

```http
DELETE /api/v1/webhooks/{webhookId}
```

### Test Webhook

Send a test payload to verify your webhook endpoint.

```http
POST /api/v1/webhooks/{webhookId}/test
```

**Response**:
```json
{
  "success": true,
  "statusCode": 200,
  "responseTime": 245
}
```

### Get Webhook Deliveries

View delivery history for a webhook.

```http
GET /api/v1/webhooks/{webhookId}/deliveries
```

**Response**:
```json
{
  "deliveries": [
    {
      "id": "wd_abc123",
      "event": "monitor.down",
      "statusCode": 200,
      "attempts": 1,
      "deliveredAt": "2025-01-21T10:30:00Z"
    }
  ]
}
```

### Webhook Payload Format

When an event occurs, StatusApp sends a POST request to your webhook URL:

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
    "incidentNumber": "INC-2025-0042",
    "startedAt": "2025-01-21T10:25:00Z",
    "severity": "high"
  },
  "check": {
    "statusCode": 500,
    "responseTime": 1250,
    "error": "Internal Server Error",
    "region": "us-east-1"
  }
}
```

### Webhook Signature Verification

If you provided a secret when creating the webhook, verify authenticity using HMAC-SHA256:

```javascript
const crypto = require('crypto');

function verifyWebhookSignature(payload, signature, secret) {
  const expectedSignature = crypto
    .createHmac('sha256', secret)
    .update(JSON.stringify(payload))
    .digest('hex');

  return crypto.timingSafeEqual(
    Buffer.from(signature),
    Buffer.from(expectedSignature)
  );
}

// In your webhook handler
app.post('/webhook', (req, res) => {
  const signature = req.headers['x-webhook-signature'];
  if (!verifyWebhookSignature(req.body, signature, WEBHOOK_SECRET)) {
    return res.status(401).send('Invalid signature');
  }
  // Process webhook...
});
```

---

## Team Management API

### List Team Members

```http
GET /api/v1/team/members
```

**Response**:
```json
{
  "members": [
    {
      "id": "tm_abc123",
      "email": "developer@example.com",
      "name": "Jane Developer",
      "role": "MEMBER",
      "status": "ACCEPTED",
      "acceptedAt": "2025-01-15T00:00:00Z"
    }
  ]
}
```

### Get Team Member

```http
GET /api/v1/team/members/{memberId}
```

### Add Team Member

```http
POST /api/v1/team/members
Content-Type: application/json
```

**Request Body**:
```json
{
  "email": "newmember@example.com",
  "name": "New Team Member",
  "role": "MEMBER"
}
```

**Roles**:

| Role | Description |
|------|-------------|
| `OWNER` | Full access (account creator) |
| `ADMIN` | Manage team and all resources |
| `MEMBER` | Create and manage monitors |
| `VIEWER` | Read-only access |

### Update Team Member

```http
PUT /api/v1/team/members/{memberId}
Content-Type: application/json
```

**Request Body**:
```json
{
  "role": "ADMIN"
}
```

### Remove Team Member

```http
DELETE /api/v1/team/members/{memberId}
```

### List Pending Invitations

```http
GET /api/v1/team/invitations
```

---

## Account API

### Get Account Details

```http
GET /api/v1/account
```

**Response**:
```json
{
  "id": "user_abc123",
  "email": "user@example.com",
  "name": "John Doe",
  "image": "https://example.com/avatar.jpg",
  "accountStatus": "ENABLED",
  "plan": "PROFESSIONAL",
  "phoneNumber": "+1234567890",
  "smsNotificationsEnabled": true,
  "language": "en",
  "timezone": "America/New_York",
  "createdAt": "2025-01-01T00:00:00Z"
}
```

### Update Account

```http
PUT /api/v1/account
Content-Type: application/json
```

**Request Body**:
```json
{
  "name": "Jane Doe",
  "phoneNumber": "+0987654321",
  "smsNotificationsEnabled": true,
  "timezone": "Europe/London"
}
```

### Get API Usage

```http
GET /api/v1/account/usage
```

**Query Parameters**:

| Parameter | Type | Description |
|-----------|------|-------------|
| `period` | string | Time period: `1h`, `24h`, `7d`, `30d` |

**Response**:
```json
{
  "period": "24h",
  "totalRequests": 1542,
  "requestsByEndpoint": {
    "/api/v1/monitors": 850,
    "/api/v1/incidents": 412,
    "/api/v1/heartbeat": 280
  },
  "rateLimit": {
    "limit": 5000,
    "used": 1542,
    "remaining": 3458
  }
}
```

### List API Keys

```http
GET /api/v1/account/api-keys
```

**Response**:
```json
{
  "apiKeys": [
    {
      "id": "key_abc123",
      "name": "Production API Key",
      "prefix": "sk_live_abc...",
      "permissions": "read,write",
      "lastUsedAt": "2025-01-21T10:30:00Z",
      "expiresAt": null,
      "isActive": true,
      "createdAt": "2025-01-01T00:00:00Z"
    }
  ]
}
```

---

## Analytics API

### Get Dashboard Analytics

```http
GET /api/v1/analytics
```

**Response**:
```json
{
  "monitors": {
    "total": 25,
    "active": 23,
    "paused": 2,
    "up": 22,
    "down": 1,
    "degraded": 0
  },
  "incidents": {
    "open": 1,
    "resolvedToday": 3,
    "resolvedThisWeek": 12,
    "avgMttr": 845
  },
  "performance": {
    "avgResponseTime": 245,
    "p95ResponseTime": 520,
    "overallUptime": 99.95
  }
}
```

### Get Monitor Analytics

```http
GET /api/v1/analytics/monitors/{monitorId}
```

**Query Parameters**:

| Parameter | Type | Description |
|-----------|------|-------------|
| `period` | string | Time period: `1h`, `24h`, `7d`, `30d` |

**Response**:
```json
{
  "monitorId": "mon_abc123",
  "period": "7d",
  "uptime": 99.95,
  "avgResponseTime": 245,
  "p50ResponseTime": 180,
  "p95ResponseTime": 520,
  "p99ResponseTime": 890,
  "totalChecks": 2016,
  "checksUp": 2015,
  "checksDown": 1,
  "incidents": 1,
  "responseTimeByRegion": {
    "us-east-1": 180,
    "eu-west-1": 245,
    "ap-southeast-1": 380
  }
}
```

---

## Worker Regions API

### List Available Regions

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
    },
    {
      "id": "region_eu_west_1",
      "city": "Dublin",
      "country": "Ireland",
      "countryCode": "IE",
      "latitude": 53.349805,
      "longitude": -6.26031,
      "isActive": true
    }
  ]
}
```

Use region IDs when creating monitors with multi-region monitoring enabled.

---

## Health & Server API

### API Health Check

```http
GET /api/v1/health
```

**Response**:
```json
{
  "status": "healthy",
  "version": "1.0.0",
  "uptime": 864000,
  "checks": {
    "database": "healthy",
    "redis": "healthy"
  },
  "timestamp": "2025-01-21T10:30:00Z"
}
```

**Status Values**:
- `healthy` - All systems operational
- `degraded` - Some services impacted
- `unhealthy` - Critical issues

### Get API Version

```http
GET /api/v1/server/version
```

**Response**:
```json
{
  "version": "1.0.0",
  "apiVersion": "v1"
}
```

---

## Error Handling

### Error Response Format

All errors return a consistent JSON format:

```json
{
  "error": "Error message description",
  "code": "ERROR_CODE",
  "details": "Additional context if available"
}
```

### Common Error Codes

| HTTP Status | Error Code | Description |
|-------------|------------|-------------|
| 400 | `BAD_REQUEST` | Invalid request parameters |
| 401 | `UNAUTHORIZED` | Missing or invalid API key |
| 403 | `FORBIDDEN` | Insufficient permissions or account not active |
| 404 | `NOT_FOUND` | Resource not found |
| 429 | `RATE_LIMIT_EXCEEDED` | Too many requests |
| 500 | `INTERNAL_SERVER_ERROR` | Server error |

### Validation Errors

For validation errors (400), the response includes field-specific issues:

```json
{
  "error": [
    {
      "code": "invalid_type",
      "expected": "string",
      "received": "undefined",
      "path": ["name"],
      "message": "Required"
    }
  ]
}
```

---

## Code Examples

### Node.js / JavaScript

```javascript
const STATUSAPP_API_KEY = process.env.STATUSAPP_API_KEY;
const BASE_URL = 'https://ops.statusapp.io/api/v1';

// Helper function for API calls
async function statusAppAPI(endpoint, options = {}) {
  const response = await fetch(`${BASE_URL}${endpoint}`, {
    ...options,
    headers: {
      'X-API-Key': STATUSAPP_API_KEY,
      'Content-Type': 'application/json',
      ...options.headers
    }
  });

  if (!response.ok) {
    const error = await response.json();
    throw new Error(error.error || 'API request failed');
  }

  return response.json();
}

// Create a monitor
async function createMonitor(data) {
  return statusAppAPI('/monitors', {
    method: 'POST',
    body: JSON.stringify(data)
  });
}

// Get all monitors
async function getMonitors(params = {}) {
  const queryString = new URLSearchParams(params).toString();
  return statusAppAPI(`/monitors${queryString ? '?' + queryString : ''}`);
}

// Example usage
const monitor = await createMonitor({
  name: 'Production API',
  url: 'https://api.example.com/health',
  type: 'API',
  interval: 300,
  expectedStatusCode: 200
});
console.log('Created monitor:', monitor.id);
```

### Python

```python
import os
import requests
from typing import Optional, Dict, Any

STATUSAPP_API_KEY = os.environ['STATUSAPP_API_KEY']
BASE_URL = 'https://ops.statusapp.io/api/v1'

class StatusAppClient:
    def __init__(self, api_key: str):
        self.api_key = api_key
        self.session = requests.Session()
        self.session.headers.update({
            'X-API-Key': api_key,
            'Content-Type': 'application/json'
        })

    def _request(self, method: str, endpoint: str, **kwargs) -> Dict[Any, Any]:
        response = self.session.request(method, f'{BASE_URL}{endpoint}', **kwargs)
        response.raise_for_status()
        return response.json()

    def list_monitors(self, **params) -> Dict:
        return self._request('GET', '/monitors', params=params)

    def create_monitor(self, data: Dict) -> Dict:
        return self._request('POST', '/monitors', json=data)

    def get_monitor(self, monitor_id: str) -> Dict:
        return self._request('GET', f'/monitors/{monitor_id}')

    def delete_monitor(self, monitor_id: str) -> Dict:
        return self._request('DELETE', f'/monitors/{monitor_id}')

    def ping_heartbeat(self, heartbeat_id: str, message: Optional[str] = None) -> Dict:
        if message:
            return self._request('POST', f'/heartbeat/{heartbeat_id}',
                               json={'message': message})
        return self._request('GET', f'/heartbeat/{heartbeat_id}')

# Example usage
client = StatusAppClient(STATUSAPP_API_KEY)

# Create a monitor
monitor = client.create_monitor({
    'name': 'Production API',
    'url': 'https://api.example.com/health',
    'type': 'API',
    'interval': 300
})
print(f"Created monitor: {monitor['id']}")

# List all monitors
monitors = client.list_monitors(status='UP', limit=10)
for m in monitors['monitors']:
    print(f"  - {m['name']}: {m['status']}")
```

### Go

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io"
    "net/http"
    "os"
)

const baseURL = "https://ops.statusapp.io/api/v1"

type StatusAppClient struct {
    apiKey string
    client *http.Client
}

func NewClient(apiKey string) *StatusAppClient {
    return &StatusAppClient{
        apiKey: apiKey,
        client: &http.Client{},
    }
}

func (c *StatusAppClient) request(method, endpoint string, body interface{}) ([]byte, error) {
    var reqBody io.Reader
    if body != nil {
        jsonData, err := json.Marshal(body)
        if err != nil {
            return nil, err
        }
        reqBody = bytes.NewBuffer(jsonData)
    }

    req, err := http.NewRequest(method, baseURL+endpoint, reqBody)
    if err != nil {
        return nil, err
    }

    req.Header.Set("X-API-Key", c.apiKey)
    req.Header.Set("Content-Type", "application/json")

    resp, err := c.client.Do(req)
    if err != nil {
        return nil, err
    }
    defer resp.Body.Close()

    return io.ReadAll(resp.Body)
}

func (c *StatusAppClient) CreateMonitor(data map[string]interface{}) (map[string]interface{}, error) {
    respBody, err := c.request("POST", "/monitors", data)
    if err != nil {
        return nil, err
    }

    var result map[string]interface{}
    err = json.Unmarshal(respBody, &result)
    return result, err
}

func main() {
    client := NewClient(os.Getenv("STATUSAPP_API_KEY"))

    monitor, err := client.CreateMonitor(map[string]interface{}{
        "name":     "Production API",
        "url":      "https://api.example.com/health",
        "type":     "API",
        "interval": 300,
    })

    if err != nil {
        fmt.Printf("Error: %v\n", err)
        return
    }

    fmt.Printf("Created monitor: %v\n", monitor["id"])
}
```

### cURL Examples

**List Monitors**:
```bash
curl -X GET https://ops.statusapp.io/api/v1/monitors \
  -H "X-API-Key: your_api_key"
```

**Create Monitor**:
```bash
curl -X POST https://ops.statusapp.io/api/v1/monitors \
  -H "X-API-Key: your_api_key" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My API",
    "url": "https://api.example.com",
    "type": "API",
    "interval": 300
  }'
```

**Send Heartbeat**:
```bash
curl https://ops.statusapp.io/api/v1/heartbeat/your_heartbeat_id
```

**Get Incidents**:
```bash
curl -X GET "https://ops.statusapp.io/api/v1/incidents?status=open" \
  -H "X-API-Key: your_api_key"
```

---

## Best Practices

### 1. Use Environment Variables

Never hardcode API keys:

```javascript
const API_KEY = process.env.STATUSAPP_API_KEY;
```

### 2. Handle Rate Limits

Implement exponential backoff:

```javascript
async function apiRequestWithRetry(fn, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (error.status === 429 && i < maxRetries - 1) {
        const resetTime = error.headers?.get('X-RateLimit-Reset');
        const waitMs = resetTime
          ? Math.max(0, resetTime - Date.now())
          : Math.pow(2, i) * 1000;
        await new Promise(r => setTimeout(r, waitMs));
        continue;
      }
      throw error;
    }
  }
}
```

### 3. Cache Responses

Cache read-only data to reduce API calls:

```javascript
const cache = new Map();
const CACHE_TTL = 60000; // 1 minute

async function getCachedMonitors() {
  const cached = cache.get('monitors');
  if (cached && Date.now() - cached.timestamp < CACHE_TTL) {
    return cached.data;
  }

  const data = await getMonitors();
  cache.set('monitors', { data, timestamp: Date.now() });
  return data;
}
```

### 4. Use Pagination

Handle large result sets properly:

```javascript
async function getAllMonitors() {
  const allMonitors = [];
  let page = 1;
  let hasMore = true;

  while (hasMore) {
    const { monitors, pagination } = await getMonitors({ page, limit: 100 });
    allMonitors.push(...monitors);
    hasMore = page < pagination.totalPages;
    page++;
  }

  return allMonitors;
}
```

### 5. Error Handling

Always handle errors gracefully:

```javascript
try {
  const monitors = await getMonitors();
  // Process monitors
} catch (error) {
  if (error.status === 401) {
    console.error('Invalid API key');
  } else if (error.status === 429) {
    console.error('Rate limited - retry later');
  } else if (error.status >= 500) {
    console.error('Server error - check StatusApp status page');
  } else {
    console.error('API error:', error.message);
  }
}
```

---

## Troubleshooting

### 401 Unauthorized

- Verify API key is correct and complete
- Check API key is active (not expired or revoked)
- Ensure proper header format (`X-API-Key` or `Authorization: Bearer`)
- Confirm key has not expired

### 403 Forbidden

- Verify API key has required permissions (`read`, `write`, `delete`)
- Check account status is `ENABLED`
- Confirm you're accessing resources you own (or are a team member of)

### 429 Rate Limit Exceeded

- Check your plan's rate limits
- Implement exponential backoff
- Cache responses when possible
- Upgrade plan if needed

### 500 Internal Server Error

- Check StatusApp status page
- Retry with exponential backoff
- Contact support if persistent

---

## Support

Need help with the API?

- **Documentation**: Full API docs at https://ops.statusapp.io/docs/api
- **Support**: Email support@statusapp.io
- **Status**: Check API status at https://status.statusapp.io

---

## Next Steps

- **[Getting Started](/help/knowledge-base/getting-started-with-statusapp)** - Set up your first monitor
- **[Understanding Monitor Types](/help/knowledge-base/monitor-types-overview)** - Learn about monitor configuration
- **[Setting Up Notifications](/help/knowledge-base/notification-channels-overview)** - Configure alerts
- **[Team Collaboration](/help/knowledge-base/team-collaboration)** - Manage team access
