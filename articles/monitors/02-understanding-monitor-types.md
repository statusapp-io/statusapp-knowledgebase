---
title: Understanding Monitor Types
slug: understanding-monitor-types
category: Monitors
excerpt: Learn about all monitor types available in StatusApp and when to use each one.
order: 2
---

# Understanding Monitor Types

StatusApp supports multiple monitor types to help you track different aspects of your infrastructure. Each monitor type is optimized for specific use cases and provides specialized configuration options.

## WEBSITE (HTTP/HTTPS) Monitors

### What It Monitors
HTTP and HTTPS monitors check if your website or web application is responding correctly. This is the most commonly used monitor type for basic website availability checks.

### Configuration Options
- **URL**: The full URL to monitor (including https://)
- **HTTP Method**: GET, POST, PUT, DELETE, PATCH, HEAD
- **Expected Status Code**: Verify specific response codes (200, 301, 404, etc.)
- **Response Time Threshold**: Alert if response is slower than expected
- **Custom Headers**: Add authentication, user-agent, or any custom headers
- **Request Body**: Send POST/PUT data for form submissions or API calls
- **SSL Certificate Validation**: Verify SSL certificates are valid
- **Redirect Following**: Choose to follow or reject HTTP redirects
- **Timeout**: Configure maximum wait time (default: 30 seconds)

### Common Use Cases
- Monitor website availability
- Check web application health
- Verify page load times
- Monitor after deployment
- Test SSL certificate installation
- Check HTTP → HTTPS redirects

### Best Practices
- Set realistic timeout values based on your service's performance
- Use multi-region monitoring for global services
- Configure appropriate expected status codes (some APIs return 201 or 202 intentionally)
- Add custom headers for services behind authentication

## API Endpoint Monitors

### What It Monitors
Advanced HTTP monitoring specifically designed for RESTful APIs with support for authentication, schema validation, assertions, and multi-step workflows.

### Advanced Features

#### Authentication Types
- **Bearer Token**: JWT tokens, API tokens
- **Basic Authentication**: Username and password
- **API Key**: Custom API key in headers or query parameters
- **OAuth 2.0**: OAuth token-based authentication
- **JWT**: JSON Web Token authentication

#### Schema Validation
Validate API responses against JSON schemas:
- Define expected response structure
- Catch breaking API changes
- Ensure data integrity
- Validate required fields and data types

#### Response Assertions
Create custom validation rules:
- Status code assertions
- Response time thresholds
- JSON path validations
- Header validations
- Body content checks

#### Multi-Step API Workflows
Test complex API sequences:
- Login → Get Token → Make Authenticated Request
- Create Resource → Update Resource → Verify
- Extract variables from one request to use in the next
- Test complete user journeys

#### Performance Profiling
Track detailed timing metrics:
- DNS resolution time
- TCP connection time
- TLS handshake time
- Time to first byte (TTFB)
- Download time
- Total request time

### Common Use Cases
- Monitor REST API availability
- Test authenticated API endpoints
- Validate API response schemas
- Monitor API performance
- Test multi-step workflows (e.g., authentication flows)
- Detect breaking changes in API contracts

## GraphQL API Monitors

### What It Monitors
Specialized monitoring for GraphQL endpoints with support for queries, mutations, variables, and response validation.

### Configuration Options
- **GraphQL Query**: Define your GraphQL query or mutation
- **Variables**: Pass dynamic variables to your queries
- **Authentication**: Support for Bearer tokens, API keys, OAuth
- **Response Assertions**: Validate GraphQL response data
- **Performance Profiling**: Track query execution times
- **Error Detection**: Monitor GraphQL-specific errors

### Common Use Cases
- Monitor GraphQL API availability
- Test complex GraphQL queries
- Validate data graph integrity
- Monitor query performance
- Test authenticated GraphQL endpoints

## PORT (TCP/UDP) Monitors

### What It Monitors
TCP and UDP monitors check if a specific port is accepting connections. Perfect for monitoring databases, mail servers, and other network services.

### Configuration
- **Host**: IP address or domain name
- **Port**: Port number to monitor (1-65535)
- **Protocol**: TCP or UDP
- **Timeout**: Maximum wait time for connection

### Common TCP Ports
**Databases**:
- MySQL/MariaDB: 3306
- PostgreSQL: 5432  
- MongoDB: 27017
- Redis: 6379
- Microsoft SQL Server: 1433

**Mail Services**:
- SMTP: 25, 587, 465
- IMAP: 143, 993
- POP3: 110, 995

**Other Services**:
- SSH: 22
- FTP: 21
- DNS: 53
- HTTPS: 443
- Custom application ports

### Common Use Cases
- Monitor database server availability
- Check mail server connectivity
- Verify SSH access
- Monitor custom application ports
- Test firewall rules

### Important Notes
- Only tests if port is open, not application functionality
- Some firewalls may block connection attempts
- UDP monitoring is less reliable than TCP
- Consider using application-specific monitors (HTTP/API) for better validation

## PING (ICMP) Monitors

### What It Monitors
Ping monitors use ICMP packets to check if a host is reachable and measure round-trip response times.

### Configuration
- **Host**: IP address or domain name
- **Packet Count**: Number of ping packets to send (default: 5)
- **Timeout**: Maximum wait time for response
- **Packet Loss Threshold**: Alert if packet loss exceeds threshold

### Metrics Tracked
- Round-trip time (min/avg/max)
- Packet loss percentage
- Network latency
- Jitter (variance in response time)

### Common Use Cases
- Check basic server connectivity
- Monitor network latency
- Diagnose network issues
- Verify server is online
- Monitor network quality

### Important Limitations
- **Firewall Blocking**: Many servers block ICMP packets for security
- **Not Application-Level**: Only tests network connectivity, not service functionality
- **Alternative**: Use TCP or HTTP monitors if ICMP is blocked
- **False Positives**: Network issues may cause alerts even when service is fine

### Best Practices
- Combine with HTTP or PORT monitors for comprehensive monitoring
- Don't rely solely on PING for production services
- Use for network infrastructure monitoring
- Enable multi-region monitoring to detect routing issues

## DNS Monitors

### What It Monitors
DNS monitors verify that domain names are resolving correctly and returning expected IP addresses or records.

### Configuration
- **Domain**: Domain name to resolve (e.g., example.com)
- **Record Type**: A, AAAA, MX, NS, TXT, CNAME, SOA, SRV
- **Expected Value**: The IP address or value you expect (optional)
- **Name Server**: Specific DNS server to query (optional, uses system default if not specified)

### DNS Record Types

**A Record** (IPv4 Address):
- Maps domain to IPv4 address
- Example: example.com → 192.0.2.1

**AAAA Record** (IPv6 Address):
- Maps domain to IPv6 address  
- Example: example.com → 2001:db8::1

**CNAME Record** (Alias):
- Creates domain alias
- Example: www.example.com → example.com

**MX Record** (Mail Exchange):
- Specifies mail servers
- Example: example.com → mail.example.com (priority 10)

**TXT Record** (Text):
- Stores arbitrary text
- Used for SPF, DKIM, domain verification

**NS Record** (Name Server):
- Specifies authoritative name servers
- Example: example.com → ns1.provider.com

### Common Use Cases
- Verify DNS configuration changes
- Monitor DNS propagation  
- Check if domain points to correct IP
- Monitor MX records for email delivery
- Verify CNAME configurations for CDNs
- Monitor DNS server health
- Detect unauthorized DNS changes

### Best Practices
- Monitor critical domains from multiple regions
- Set expected values to detect unauthorized changes
- Monitor both A and AAAA records for dual-stack services
- Check MX records to prevent email delivery issues
- Monitor NS records to detect DNS hijacking

## SSL Certificate Monitors

### What It Monitors
SSL certificate monitors track SSL/TLS certificate expiration dates and validity, sending alerts before certificates expire.

### Configuration
- **Domain**: Domain with SSL certificate to monitor
- **Days Before Expiration**: Alert threshold (default: 30 days)
- **Port**: HTTPS port (default: 443)
- **Check Validity**: Verify certificate is properly configured

### Monitored Attributes
- Certificate expiration date
- Days until expiration
- Certificate issuer
- Certificate validity
- Certificate chain completeness
- Domain name match

### Common Use Cases
- Prevent certificate expiration outages
- Monitor multiple domain certificates
- Track wildcard certificate expiration
- Monitor certificate renewals
- Ensure HTTPS is properly configured

### Best Practices
- Set alert threshold to 30+ days for manual renewals
- Monitor 7 days before expiration as final warning
- Monitor all production domains
- Include subdomains and wildcard certificates
- Monitor staging/development certificates too

### Alert Thresholds
Recommended alert timing:
- **30 days**: First warning - start renewal process
- **14 days**: Second warning - prioritize renewal
- **7 days**: Critical warning - immediate action required
- **3 days**: Emergency - risk of outage

## CRON / Heartbeat Monitors

### What It Monitors
Heartbeat monitoring works in reverse - your scheduled tasks ping StatusApp when they run. If we don't receive a ping within the expected timeframe, we alert you.

### How It Works
1. Create a CRON/Heartbeat monitor in StatusApp
2. StatusApp generates a unique heartbeat URL for you
3. Your scheduled task pings this URL when it runs successfully
4. If we don't receive a ping within the grace period, we send an alert

### Configuration
- **Name**: Descriptive name for your job
- **Expected Interval**: How often the job should run
- **Grace Period**: Extra time allowed before alerting (default: 5 minutes)
- **Heartbeat URL**: Generated unique URL to ping

### Setting Up

**Step 1**: Create the monitor and copy your heartbeat URL  
Example URL: `https://statusapp.io/api/v1/heartbeat/abc123xyz`

**Step 2**: Add to your cron job or script

**Bash Script**:
```bash
#!/bin/bash
# Your backup script
/usr/local/bin/backup.sh

# Ping StatusApp on success
curl -X POST https://statusapp.io/api/v1/heartbeat/abc123xyz
```

**Python Script**:
```python
import requests
import sys

try:
    # Your scheduled task
    run_backup()
    
    # Ping StatusApp on success
    requests.post('https://statusapp.io/api/v1/heartbeat/abc123xyz')
except Exception as e:
    # Optional: Ping failure endpoint
    requests.post('https://statusapp.io/api/v1/heartbeat/abc123xyz/fail')
    sys.exit(1)
```

**Cron Entry**:
```bash
# Daily backup at 2 AM with heartbeat
0 2 * * * /usr/local/bin/backup.sh && curl -X POST https://statusapp.io/api/v1/heartbeat/abc123xyz
```

### Common Use Cases
- Monitor nightly backups
- Track data synchronization jobs
- Monitor report generation
- Verify cleanup scripts run
- Monitor database maintenance tasks
- Track ETL processes
- Monitor scheduled data imports/exports

### Best Practices
- Set grace period longer than typical job duration
- Use the `/fail` endpoint to report job failures
- Add logging to capture why jobs fail
- Monitor both success and failure states
- Test heartbeat URLs before deploying
- Document expected run times for on-call teams

### Troubleshooting
**Not receiving pings?**
- Verify script is calling the correct URL
- Check script has internet access
- Ensure firewall allows outbound HTTPS
- Verify cron job is actually running
- Check script logs for errors

## Multi-Region Monitoring

All monitor types support multi-region monitoring, allowing you to check your services from multiple geographic locations.

### Available Regions
- **North America**: United States (East, West, Central), Canada
- **Europe**: United Kingdom, Germany, France, Ireland, Netherlands
- **Asia Pacific**: Singapore, Tokyo, Sydney, Mumbai
- **South America**: Brazil (São Paulo)
- **Africa**: South Africa (Johannesburg)

### Benefits
- **Detect Regional Outages**: Identify issues affecting specific geographic areas
- **CDN Performance**: Monitor CDN effectiveness across regions
- **Global Service Verification**: Ensure worldwide accessibility
- **Reduce False Positives**: Multiple regions provide confirmation before alerting
- **Latency Tracking**: Compare response times across regions

### Configuration
1. Edit your monitor
2. Navigate to the "Regions" section
3. Select multiple regions to monitor from
4. Save your changes

### Best Practices
- Use 2-3 regions minimum for critical services
- Choose regions where your users are located
- Monitor from regions near your CDN edge locations
- For global services, use 5+ regions for comprehensive coverage

## Choosing the Right Monitor Type

| Service/Infrastructure | Recommended Monitor | Alternative |
|------------------------|---------------------|-------------|
| Website/Web App | WEBSITE (HTTP/HTTPS) | PING |
| REST API | API Endpoint | WEBSITE |
| GraphQL API | GraphQL | API Endpoint |
| Database Server | PORT (TCP) | CRON for query checks |
| Mail Server | PORT (TCP 25, 587) | WEBSITE for webmail |
| SSH Server | PORT (TCP 22) | PING |
| DNS Records | DNS | None |
| SSL Certificates | SSL_CERT | WEBSITE |
| Scheduled Jobs | CRON (Heartbeat) | External script monitoring |
| CDN | WEBSITE with multi-region | DNS |
| Load Balancer | WEBSITE or API | PORT |
| Kubernetes Service | WEBSITE or API | PORT |
| Microservices | API Endpoint | WEBSITE |

## Monitor Configuration Best Practices

### 1. Set Appropriate Timeouts
- **Fast APIs**: 5-10 seconds
- **Standard Websites**: 15-30 seconds
- **Slow Services**: 30-60 seconds
- **Database Ports**: 10-15 seconds
- **International Services**: 30-45 seconds

### 2. Configure Check Intervals Based on Criticality
- **Mission Critical**: 1-2 minutes
- **Production Services**: 5 minutes
- **Staging/Development**: 10-15 minutes
- **Background Services**: 15-30 minutes
- **Batch Jobs**: Match job schedule

### 3. Use Multiple Monitor Types
Combine different monitor types for comprehensive monitoring:
- **WEBSITE** + **SSL_CERT** = Complete web monitoring
- **PORT** + **API** = Database connectivity + application health
- **DNS** + **WEBSITE** = DNS resolution + service availability
- **PING** + **PORT** = Network + service layer monitoring

### 4. Validate Responses
Don't just check status codes:
- Validate response content
- Check API response schemas
- Verify critical data is present
- Use assertions for business logic validation

### 5. Plan for Maintenance
- Pause monitors during planned maintenance
- Use maintenance windows (when available)
- Document expected downtime
- Notify teams before major changes

## Troubleshooting Monitors

### Monitor Shows False Positives

**Symptoms**: Frequent DOWN alerts but service is working

**Common Causes**:
- Timeout set too low
- Firewall blocking StatusApp IPs
- Rate limiting triggered
- Response validation too strict
- Network latency from monitoring region

**Solutions**:
- Increase timeout to 30-60 seconds
- Whitelist StatusApp monitoring IPs
- Adjust check interval to avoid rate limits
- Review and relax validation rules
- Test from multiple regions

### Monitor Not Detecting Actual Outages

**Symptoms**: Service is down but monitor shows UP

**Common Causes**:
- Monitoring wrong endpoint
- Not validating response content
- Load balancer returning cached responses
- Service degraded but not completely down

**Solutions**:
- Verify URL is correct and current
- Add response validation (status codes, content checks)
- Monitor actual application endpoints, not just load balancer
- Configure DEGRADED state thresholds
- Add assertions to validate functionality

### Inconsistent Results Across Regions

**Symptoms**: Some regions show DOWN, others show UP

**Possible Causes**:
- Actual regional outage (CDN, routing, etc.)
- Regional firewall rules
- Geographic rate limiting
- DNS propagation issues

**Solutions**:
- Review which regions are affected
- Check CDN and routing configuration
- Verify firewall rules allow all regions
- Confirm DNS has propagated globally

## Next Steps

- **[Advanced Monitor Configuration](/articles/monitors/advanced-monitor-configuration)** - Learn about advanced features and settings
- **[Setting Up Notifications](/articles/alerting-notifications/notification-channels-guide)** - Configure alerts for your monitors
- **[Understanding Analytics](/articles/analytics/understanding-analytics)** - Interpret performance metrics
- **[Creating Status Pages](/articles/status-pages/creating-status-pages)** - Share monitor status with customers
- **[Incident Management](/articles/incidents/incident-management-basics)** - Handle detected incidents effectively