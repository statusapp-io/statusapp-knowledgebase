---
title: Monitor Types Overview
slug: monitor-types-overview
category: Monitors
excerpt: Choose the right monitor type for your infrastructure - HTTP, API, TCP, DNS, SSL, Heartbeat, Domain, and more.
order: 2
---

# Monitor Types Overview

StatusApp supports multiple specialized monitor types for different use cases. Choose the right type to accurately test your services.

## Monitor Types at a Glance

| Type | Best For | Key Feature |
|------|----------|-------------|
| **[WEBSITE/HTTP](/help/knowledge-base/http-website-monitors)** | Websites, web apps | Simple availability check |
| **[API](/help/knowledge-base/api-monitors)** | REST APIs | Auth, assertions, schema validation |
| **[PORT/TCP](/help/knowledge-base/tcp-port-monitors)** | Databases, services | Check port availability |
| **[DNS](/help/knowledge-base/dns-monitors)** | Domain resolution | Verify DNS records |
| **[SSL_CERT](/help/knowledge-base/ssl-certificate-monitors)** | HTTPS sites | Track certificate expiration |
| **[CRON/Heartbeat](/help/knowledge-base/heartbeat-monitors)** | Scheduled jobs | Reverse monitoring via pings |
| **[DOMAIN](/help/knowledge-base/domain-monitors)** | Domain health | Expiry, WHOIS, blacklists |
| **[GRAPHQL](/help/knowledge-base/graphql-monitors)** | GraphQL APIs | Query/mutation validation |
| **PING** | Network connectivity | ICMP latency checks |
| **SERVER** | Infrastructure | CPU, memory, disk metrics |

---

## Quick Selector

**Not sure which type?**

- **Is it a website or web app?** → [WEBSITE/HTTP](/help/knowledge-base/http-website-monitors)
- **Is it a REST API?** → [API](/help/knowledge-base/api-monitors)
- **Is it a GraphQL endpoint?** → [GRAPHQL](/help/knowledge-base/graphql-monitors)
- **Is it a database or service port?** → [PORT/TCP](/help/knowledge-base/tcp-port-monitors)
- **Is it a domain name you want to track?** → DOMAIN
- **Need DNS record validation?** → [DNS](/help/knowledge-base/dns-monitors)
- **Checking SSL certificates?** → [SSL_CERT](/help/knowledge-base/ssl-certificate-monitors)
- **Is it a scheduled job or cron?** → [CRON/Heartbeat](/help/knowledge-base/heartbeat-monitors)
- **Testing network connectivity?** → PING
- **Monitoring server resources?** → SERVER

---

## Monitor Type Details

### WEBSITE / HTTP / HTTPS

Monitor any HTTP or HTTPS endpoint for availability.

**Configuration Options**:
- URL to monitor
- Expected HTTP status code (e.g., 200, 301, 302)
- HTTP method (GET, POST, PUT, etc.)
- Request headers (JSON format)
- Request body (for POST/PUT)
- Timeout settings

**Use Cases**:
- Public websites
- Web applications
- Landing pages
- CDN endpoints
- Load balancer health checks

### API

Monitor REST API endpoints with advanced validation.

**Configuration Options**:
- URL to monitor
- HTTP method
- Request headers and body
- Expected status code
- Authentication configuration:
  - Bearer Token
  - Basic Auth (username/password)
  - API Key (header or query parameter)
  - OAuth 2.0 (client credentials, password, refresh token)
  - JWT (with custom prefix)
- Performance timing capture
- Response assertions

**Authentication Config Example**:
```json
{
  "type": "bearer",
  "token": "your-api-token"
}
```

**Use Cases**:
- REST API endpoints
- Authenticated APIs
- API health endpoints
- Third-party API integrations
- Microservice endpoints

### CRON / Heartbeat

Reverse monitoring for scheduled jobs - your job pings StatusApp.

**How It Works**:
1. Create a CRON monitor - you get a unique heartbeat URL
2. Your scheduled job pings the URL when it runs
3. If no ping is received within the grace period, an incident is triggered

**Configuration Options**:
- Grace period (60 seconds to 24 hours)
- Expected ping frequency

**Heartbeat Signals**:
- **Success**: `GET /api/v1/heartbeat/{id}` - Job completed successfully
- **Start**: `GET /api/v1/heartbeat/{id}/start` - Job started (for long jobs)
- **Fail**: `POST /api/v1/heartbeat/{id}/fail` - Job failed (include error in body)
- **Log**: `POST /api/v1/heartbeat/{id}/log` - Log message without status change

**Use Cases**:
- Cron jobs
- Scheduled backups
- ETL processes
- Batch jobs
- Periodic scripts
- Queue workers

### PORT / TCP

Monitor TCP or UDP port availability.

**Configuration Options**:
- Hostname or IP address
- Port number (1-65535)
- Protocol (TCP or UDP)
- Timeout

**Use Cases**:
- Database servers (MySQL 3306, PostgreSQL 5432, MongoDB 27017)
- Mail servers (SMTP 25/587, IMAP 143/993)
- SSH access (port 22)
- FTP servers (port 21)
- Redis (port 6379)
- Custom services on specific ports

### DNS

Monitor DNS record resolution and validate expected values.

**Configuration Options**:
- Domain name
- Record type (A, AAAA, CNAME, MX, TXT)
- Expected IP or value (optional validation)
- Timeout

**Record Types**:
| Type | Description | Example Use |
|------|-------------|-------------|
| A | IPv4 address | Validate server IP |
| AAAA | IPv6 address | IPv6 validation |
| CNAME | Canonical name | CDN/alias records |
| MX | Mail exchange | Email server |
| TXT | Text records | SPF, DKIM verification |

**Use Cases**:
- Verify DNS propagation
- Monitor DNS changes
- Validate CDN configuration
- Check mail server records
- Monitor for DNS hijacking

### SSL_CERT

Monitor SSL/TLS certificate expiration.

**Configuration Options**:
- HTTPS URL
- Days before expiry to alert (1-90 days, default: 30)

**What's Monitored**:
- Certificate validity
- Days until expiration
- Certificate issuer
- Certificate chain

**Use Cases**:
- Production websites
- API endpoints
- Internal services
- Third-party dependencies
- Wildcard certificates

### DOMAIN

Comprehensive domain health monitoring.

**Configuration Options**:
- Domain name (e.g., `example.com`)
- Days before domain expiry to alert (1-365 days)
- Monitor DNS changes (on/off)
- Monitor WHOIS changes (on/off)
- Monitor blacklists (on/off)

**What's Monitored**:
- Domain expiration date
- WHOIS data changes
- DNS record modifications
- Blacklist status
- Registrar information
- Nameserver configuration

**Use Cases**:
- Critical domain assets
- Customer domains (for agencies)
- Domains with auto-renewal concerns
- Domain portfolio management
- Security monitoring

### GRAPHQL

Monitor GraphQL endpoints with query validation.

**Configuration Options**:
- GraphQL endpoint URL
- Query string
- Variables (JSON)
- Authentication (same options as API monitors)
- Expected response validation

**Example Query**:
```graphql
query HealthCheck {
  health {
    status
    version
  }
}
```

**Use Cases**:
- GraphQL API health checks
- Schema validation
- Query performance monitoring
- Apollo/GraphQL server monitoring

### PING

ICMP ping monitoring for network connectivity.

**Configuration Options**:
- Hostname or IP address
- Timeout

**What's Measured**:
- Connectivity (up/down)
- Latency (response time)
- Packet loss

**Use Cases**:
- Network device availability
- Server reachability
- VPN endpoint checking
- ISP connectivity validation

### SERVER

Server infrastructure monitoring (requires agent installation).

**Configuration Options**:
- CPU threshold (default: 90%)
- Memory threshold (default: 85%)
- Disk threshold (default: 90%)
- Load average threshold

**What's Monitored**:
- CPU usage and load average
- Memory usage (RAM and swap)
- Disk usage and I/O
- Network bandwidth
- Process count
- System uptime
- OS and kernel version

**Use Cases**:
- Production servers
- Database servers
- Application servers
- Containerized workloads

---

## Multi-Region Monitoring

All monitor types support checking from multiple geographic regions:

**Available Regions**:
- North America (US East, US West, US Central, Canada)
- Europe (UK, Germany, France, Ireland, Netherlands)
- Asia Pacific (Singapore, Tokyo, Sydney, Mumbai)
- South America (Brazil)
- Africa (South Africa)

**Benefits**:
- Detect regional outages
- Verify global accessibility
- Monitor CDN performance
- Reduce false positives (require 2+ regions to agree)
- Track region-specific latency

**Configuration**:
Select regions when creating/editing a monitor using `enabledWorkerRegionIds`.

---

## Common Configuration Options

All monitor types include:

### Check Interval

How often to test (depends on your plan):
- **Starter**: 60 seconds minimum
- **Professional**: 30 seconds minimum
- **Business**: 10 seconds minimum
- **Enterprise**: 5 seconds minimum

**Recommendations**:
- Critical services: 30-60 seconds
- Production services: 2-5 minutes
- Non-critical: 5-15 minutes

### Timeout

Maximum wait for response before marking as failed:
- Minimum: 5 seconds
- Maximum: 300 seconds (5 minutes)
- Default: 30 seconds

**Recommendations**:
- Fast services: 5-10 seconds
- Standard: 15-30 seconds
- Slow/international: 30-60 seconds

### Mute Alerts Before

Number of consecutive failures before alerting:
- 1 check: Immediate alert (high sensitivity)
- 2-3 checks: Balanced (reduce false positives)
- 5+ checks: Low sensitivity (for flaky services)

### Notifications

Assign notification channels to each monitor:
- Email
- Slack
- Discord
- Telegram
- SMS
- PagerDuty
- OpsGenie
- Microsoft Teams
- Custom Webhooks

---

## Performance Regression Detection

Available for HTTP, API, and GraphQL monitors.

**Configuration**:
- `regressionEnabled`: Enable/disable detection
- `regressionThreshold`: Percentage increase to trigger alert (25-200%, default: 50%)
- `regressionPeriod`: Baseline calculation period in days (1-30, default: 7)

**How It Works**:
1. StatusApp calculates baseline response time from recent checks
2. If response time exceeds baseline by threshold percentage, alert triggers
3. Alert resolves when performance returns to normal

---

## Performance Timings

Enable detailed timing breakdown for HTTP-based monitors:

| Timing | Description |
|--------|-------------|
| DNS | DNS resolution time |
| TCP | TCP connection time |
| TLS | TLS handshake time |
| First Byte | Time to first byte |
| Download | Content download time |
| Total | Total response time |

Enable with `performanceTimings: true` when creating monitors.

---

## Choosing the Right Monitor Type

**For Accuracy**:
- Use the monitor type that closest matches how users access the service
- HTTP for websites, API for APIs, PORT for databases

**For Coverage**:
- Combine monitor types for comprehensive testing
- WEBSITE + SSL_CERT = Full web monitoring
- API + PORT = Complete service testing
- DNS + HTTP = Domain + availability

**For Scale**:
- Start simple, add complexity as needed
- Monitor critical services from multiple regions
- Use different intervals for different criticality levels

---

## Monitoring Common Infrastructure

| Service | Recommended Monitor(s) |
|---------|----------------------|
| Website/Web App | WEBSITE + SSL_CERT |
| REST API | API (with auth) |
| GraphQL API | GRAPHQL |
| Database | PORT (check connection port) |
| Mail Server | PORT (SMTP, IMAP ports) |
| Scheduled Job | CRON/Heartbeat |
| DNS Records | DNS |
| Domain Health | DOMAIN |
| CDN | HTTP from multiple regions |
| Load Balancer | HTTP/API from multiple regions |
| Microservice | API |
| VPN Endpoint | PING |
| Server | SERVER (with agent) |

---

## Best Practices

### Set Realistic Timeouts
- Match your service's typical response time
- Add buffer for network latency
- Test locally to establish baseline
- Increase for international regions

### Start Conservative, Then Optimize
- Begin with longer intervals and higher confirmation thresholds
- Reduce false positives first
- Shorten intervals once baseline is stable
- Tune thresholds based on incident history

### Document Your Setup
- Record what each monitor tests
- Document why you chose that monitor type
- Keep runbooks for common alerts
- Update as services change

### Review Regularly
- Monthly: Check false positive rate
- Quarterly: Review intervals and thresholds
- When services change: Update monitors
- Annually: Full audit of all monitors

---

## Next Steps

- **[HTTP/Website Monitors](/help/knowledge-base/http-website-monitors)** - Detailed HTTP monitoring guide
- **[API Monitors](/help/knowledge-base/api-monitors)** - REST API monitoring with auth
- **[Heartbeat Monitors](/help/knowledge-base/heartbeat-monitors)** - Monitor scheduled jobs
- **[Notifications](/help/knowledge-base/notification-channels-overview)** - Set up alerts
- **[API Reference](/help/knowledge-base/api-reference-guide)** - Create monitors via API
