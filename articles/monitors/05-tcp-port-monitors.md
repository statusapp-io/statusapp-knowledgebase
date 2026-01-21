---
title: TCP & Port Monitors
slug: tcp-port-monitors
category: Monitors
excerpt: Monitor database servers, mail servers, and network services by checking port connectivity.
order: 5
---

# TCP & Port Monitors

## Overview

TCP/Port monitors check if a specific port on a host is accepting connections. Perfect for databases, mail servers, SSH, and other network services.

## Creating a TCP Monitor

1. Create a new monitor
2. Select **TCP/Port** type
3. Enter hostname or IP address
4. Enter port number
5. Save and test

## Configuration

### Host
IP address or domain name of the service:
- `db.example.com`
- `mail.example.com`
- `192.168.1.100`
- `services.internal`

### Port
Port number to check (1-65535):

**Common Ports**:
- Database services
- SSH: 22
- MySQL: 3306
- PostgreSQL: 5432
- MongoDB: 27017
- Redis: 6379
- Microsoft SQL Server: 1433

**Mail Services**:
- SMTP: 25, 587, 465
- IMAP: 143, 993
- POP3: 110, 995

**Other Services**:
- FTP: 21
- DNS: 53
- HTTP: 80
- HTTPS: 443
- Custom application ports

### Timeout
Maximum time to wait for connection (seconds):
- Default: 10 seconds
- Databases: 5-10 seconds
- Remote services: 10-15 seconds
- Unreliable networks: 20-30 seconds

## What TCP Monitors Test

**What They Check**:
- ✓ Port is open and accepting connections
- ✓ Service is listening
- ✓ Network connectivity to host
- ✓ Firewall allows access

**What They Don't Check**:
- ✗ Service is working correctly
- ✗ Service will respond to actual requests
- ✗ Service returns valid responses
- ✗ Application-level functionality

**Tip**: TCP monitors are good for basic connectivity. For application-level testing, combine with HTTP/API monitors.

## Common Use Cases

### Database Availability
Monitor database server is accessible:

```
Host: db.example.com
Port: 5432 (PostgreSQL)
Alert if: Connection fails for 3 consecutive checks
```

### Mail Server Health
Check mail server is accepting connections:

```
Host: mail.example.com
Port: 587 (SMTP with TLS)
Alert if: Port unavailable
```

### Custom Application Port
Monitor internal service:

```
Host: services.internal
Port: 8080 (custom application)
Alert if: Unreachable
```

### Database Failover
Monitor both primary and standby:

```
Monitor 1:
Host: db-primary.example.com
Port: 3306

Monitor 2:
Host: db-standby.example.com
Port: 3306
```

## Best Practices

### 1. Use Appropriate Ports
- Verify you're checking the correct port
- Services may listen on non-standard ports
- Confirm port with documentation or network team

### 2. Combine with Application Monitors
TCP alone only confirms port is open:
- **TCP Monitor**: Database port is reachable
- **API Monitor**: Database queries are working
- Together: Complete coverage

### 3. Firewall Considerations
- Ensure monitoring region IPs are whitelisted
- Some firewalls block unusual ports
- Verify firewall rules allow monitoring traffic
- Work with network team if monitors keep failing

### 4. Multi-Region Monitoring
Monitor from multiple regions:
- Detects regional network issues
- Validates routing/connectivity from different paths
- Requires 2+ regions for better detection

### 5. Appropriate Intervals
- Critical databases: 1-5 minutes
- Production services: 5-15 minutes
- Non-critical: 15-60 minutes

## Example: Database Monitoring

### Basic Database Check
```
Host: db.company.com
Port: 5432
Timeout: 10s
Alert: Immediate (confirm 1 check)
Interval: 5 minutes
Regions: 2 (home region + backup region)
```

### With Notification Escalation
```
TCP Monitor: db.company.com:5432
├─ Slack: #database-alerts
├─ Email: dba@company.com
└─ PagerDuty: Database-On-Call
```

## Troubleshooting

### Connection Refused
**Cause**: Port not open or service not listening

**Solutions**:
- Verify port number is correct
- Confirm service is running
- Check if service needs restart
- Verify process is listening on expected port

**Test Manually**:
```bash
# Linux/Mac
telnet example.com 5432

# Or use nc
nc -zv example.com 5432
```

### Connection Timeout
**Cause**: Host unreachable or firewall blocking

**Solutions**:
- Verify host is online and reachable
- Whitelist StatusApp monitoring IPs in firewall
- Increase timeout if network is slow
- Check routing between monitor region and host

### False Positives
Monitor alerts frequently but service works fine

**Solutions**:
- Increase timeout for flaky networks
- Use higher confirmation threshold (2-3 failures before alert)
- Require multi-region confirmation (2+ regions must fail)
- Check if service is slowly responding

### Port Open But Service Not Working
Monitor passes but application is broken

**Add HTTP/API Monitor**:
- TCP confirms port is open
- HTTP/API monitor confirms service responds correctly
- Combine for comprehensive monitoring

## Advanced Scenarios

### Database Cluster Monitoring
Monitor multiple database nodes:

```
Monitor 1: db-node-1.internal:5432
Monitor 2: db-node-2.internal:5432
Monitor 3: db-node-3.internal:5432
```

### Service Availability
Monitor critical internal services:

```
Monitor 1: api-internal.internal:8080
Monitor 2: cache.internal:6379
Monitor 3: worker-queue.internal:5672
```

### Port Scan (Multiple Ports on Same Host)
Monitor multiple services on one host:

```
Monitor 1: services.internal:22 (SSH)
Monitor 2: services.internal:3306 (MySQL)
Monitor 3: services.internal:6379 (Redis)
Monitor 4: services.internal:8080 (App)
```

## Combining with Other Monitors

### TCP + HTTP for Complete Coverage
```
TCP Monitor: db.example.com:5432
└─ Confirms database port is open

HTTP Monitor: https://example.com/health
└─ Confirms application is working
└─ Confirms database queries work
```

### TCP + API for API Services
```
TCP Monitor: api.example.com:443
└─ Confirms HTTPS port is open

API Monitor: https://api.example.com/health
└─ Confirms API responds correctly
└─ Confirms authentication works
```

## Next Steps

- **[HTTP Monitors](/articles/monitors/http-website-monitors)** - Monitor websites and web apps
- **[API Monitors](/articles/monitors/api-monitors)** - Monitor REST APIs
- **[DNS Monitors](/articles/monitors/dns-monitors)** - Monitor domain name resolution
- **[Notifications](/articles/alerting-notifications/notification-channels-overview)** - Set up alerts
