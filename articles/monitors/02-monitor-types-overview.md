---
title: Monitor Types Overview
slug: monitor-types-overview
category: Monitors
excerpt: Choose the right monitor type for your infrastructure - HTTP, API, TCP, DNS, SSL, and Heartbeat.
order: 2
---

# Monitor Types Overview

StatusApp supports multiple specialized monitor types for different use cases. Choose the right type to accurately test your services.

## Monitor Types at a Glance

| Type | Best For | Key Feature |
|------|----------|-------------|
| **[HTTP/Website](/articles/monitors/http-website-monitors)** | Websites, web apps | Simple availability check |
| **[API](/articles/monitors/api-monitors)** | REST APIs | Auth, assertions, multi-step |
| **[TCP/Port](/articles/monitors/tcp-port-monitors)** | Databases, services | Check port availability |
| **[DNS](/articles/monitors/dns-monitors)** | Domain resolution | Verify DNS records |
| **[SSL Certificate](/articles/monitors/ssl-certificate-monitors)** | HTTPS sites | Track expiration dates |
| **[Heartbeat/Cron](/articles/monitors/heartbeat-monitors)** | Scheduled jobs | Reverse monitoring |
| **[GraphQL](/articles/monitors/graphql-monitors)** | GraphQL APIs | Query/mutation validation |

## Quick Selector

**Not sure which type?**

- **Is it a website?** → [HTTP/Website](/articles/monitors/http-website-monitors)
- **Is it an API?** → [API](/articles/monitors/api-monitors)
- **Is it a database or service port?** → [TCP/Port](/articles/monitors/tcp-port-monitors)
- **Is it a domain name?** → [DNS](/articles/monitors/dns-monitors)
- **Is it an HTTPS certificate?** → [SSL Certificate](/articles/monitors/ssl-certificate-monitors)
- **Is it a scheduled job?** → [Heartbeat](/articles/monitors/heartbeat-monitors)
- **Is it a GraphQL endpoint?** → [GraphQL](/articles/monitors/graphql-monitors)

## Multi-Region Monitoring

All monitor types support checking from multiple geographic regions:

**Available Regions**:
- North America (US East/West/Central, Canada)
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

## Common Configuration

All monitor types include:

**Check Interval**: How often to test (1-60 minutes)
- Critical services: 1-5 minutes
- Production: 5-15 minutes
- Non-critical: 15-60 minutes

**Timeout**: Maximum wait for response
- Fast services: 5-10 seconds
- Standard: 15-30 seconds
- Slow/international: 30-60 seconds

**Confirmation Checks**: Failed checks before alerting
- 1 check: Immediate alert (high sensitivity)
- 2-3 checks: Balanced (standard)
- 5+ checks: Low sensitivity (fewer false positives)

**Notifications**: Where to send alerts
- Email, Slack, SMS, PagerDuty, etc.
- Assign multiple channels per monitor

## Choosing Wisely

**For Accuracy**:
- Use the monitor type closest to how users access the service
- HTTP monitors for websites
- API monitors for APIs
- Port monitors for databases, etc.

**For Coverage**:
- Combine monitor types for comprehensive testing
- Website + SSL Certificate = Full web monitoring
- API + Port = Complete service testing
- DNS + HTTP = Domain + availability

**For Scale**:
- Start simple, add complexity as needed
- Monitor critical services from multiple regions
- Use different intervals for different criticality levels

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

## Monitoring Common Infrastructure

**Website/Web App** → HTTP/Website + SSL Certificate

**REST API** → API Endpoint (with auth/assertions)

**GraphQL API** → GraphQL Monitor

**Database** → TCP/Port + optional API health check

**Mail Server** → TCP/Port (check SMTP/IMAP ports)

**Scheduled Job** → Heartbeat/Cron

**DNS Records** → DNS Monitor

**CDN** → HTTP from multiple regions

**Load Balancer** → HTTP/API from multiple regions

**Microservice** → API Endpoint

## Next Steps

- **[HTTP/Website Monitors](/articles/monitors/http-website-monitors)** - Monitor websites and web apps
- **[API Monitors](/articles/monitors/api-monitors)** - Monitor REST APIs with auth and assertions
- **[Heartbeat Monitors](/articles/monitors/heartbeat-monitors)** - Monitor scheduled jobs
- **[Notifications](/articles/alerting-notifications/notification-channels-overview)** - Set up alerts
- **[Analytics](/articles/analytics/understanding-analytics)** - Track performance metrics
