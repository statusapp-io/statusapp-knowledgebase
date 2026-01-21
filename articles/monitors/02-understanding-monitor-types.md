---
title: Understanding Monitor Types
slug: understanding-monitor-types
category: Monitors
excerpt: Learn about different monitor types available in StatusApp and when to use each one.
order: 2
---

# Understanding Monitor Types

StatusApp supports multiple monitor types to help you track different aspects of your infrastructure. Choose the right monitor type for your specific monitoring needs.

## HTTP/HTTPS Monitors

### What It Monitors
HTTP and HTTPS monitors check if your website or API is responding correctly and within expected performance parameters.

### Configuration Options
- **URL**: The full URL to monitor
- **Method**: GET, POST, PUT, DELETE, PATCH, HEAD
- **Status Code Check**: Verify the response code (e.g., 200, 301)
- **Response Time**: Alert if slower than expected
- **Custom Headers**: Add authentication or custom headers
- **Request Body**: Send POST/PUT data
- **Response Validation**: Check for specific text in response
- **Redirect Following**: Follow or reject redirects

### Common Use Cases
- Monitor web applications
- Track API health
- Check third-party service availability
- Validate SSL certificate installation

## TCP/UDP Monitors

### What It Monitors
TCP and UDP monitors check if a specific port is accepting connections, useful for monitoring databases, mail servers, and other network services.

### Configuration
- **Host**: IP address or domain name
- **Port**: Port number to monitor (1-65535)
- **Protocol**: TCP or UDP
- **Response Timeout**: How long to wait for a response

### Common Use Cases
- Database server availability (MySQL port 3306, PostgreSQL port 5432)
- Mail server monitoring (SMTP port 25, 587)
- SSH availability (port 22)
- Custom application ports

## Ping (ICMP) Monitors

### What It Monitors
Ping monitors use ICMP packets to check if a host is reachable and measure round-trip response times.

### Configuration
- **Host**: IP address or domain name
- **Packet Count**: Number of ping packets to send
- **Timeout**: Maximum time to wait for response

### Important Notes
- Some hosts may block ICMP packets (firewalls)
- Ping is not available on all servers
- Use TCP/HTTP monitors as alternatives if ICMP is blocked

### Common Use Cases
- Check if servers are online
- Monitor network connectivity
- Diagnose network issues
- Track latency to remote servers

## DNS Monitors

### What It Monitors
DNS monitors verify that your DNS records are resolving correctly and returning expected results.

### Configuration
- **Domain**: Domain name to resolve
- **Record Type**: A, AAAA, MX, NS, TXT, CNAME, etc.
- **Expected Value**: The value you expect to receive
- **DNS Server**: Which nameserver to query (optional)

### Common Use Cases
- Verify DNS configuration
- Monitor DNS propagation
- Check domain is pointing to correct IP
- Monitor MX records for email
- Verify CNAME setup

## CRON Monitors

### What It Monitors
CRON monitors track periodic jobs and background tasks. They use a webhook to verify that your cron jobs are running on schedule.

### How It Works
1. Create a CRON monitor in StatusApp
2. Get a unique webhook URL
3. Integrate the webhook into your cron job
4. StatusApp tracks job execution

### Configuration
- **Expected Interval**: How often your job should run (e.g., daily at 2 AM)
- **Grace Period**: Allow extra time beyond expected interval
- **Timeout**: Alert if job doesn't complete in time

### Common Use Cases
- Monitor database backups
- Track report generation
- Monitor cleanup jobs
- Verify scheduled tasks are executing

## API Monitors (HTTP with Advanced Validation)

### Advanced Features

#### Response Validation
- **Schema Validation**: Verify JSON response matches expected schema
- **Text Search**: Check for specific text in response
- **Status Code**: Verify specific HTTP status codes

#### Custom Authentication
- **Basic Auth**: Username and password
- **Bearer Token**: JWT or API token
- **Custom Headers**: Any custom authorization method

#### Performance Monitoring
- **Response Time Assertions**: Alert if slower than threshold
- **Timeout Configuration**: Custom timeouts per check

## Multi-Region Monitoring

All monitor types support checking from multiple geographic regions:
- **North America**: United States, Canada
- **Europe**: Germany, France, UK, Ireland
- **Asia Pacific**: Singapore, Tokyo, Sydney
- **Additional Regions**: Brazil, South Africa, and more

### Benefits
- Identify regional outages
- Test global CDN performance
- Detect location-specific issues
- Better representative monitoring

## Choosing the Right Monitor Type

| Service | Recommended Monitor | Alternative |
|---------|-------------------|-------------|
| Website | HTTP/HTTPS | Ping |
| API | HTTP/HTTPS | TCP |
| Database | TCP | CRON |
| DNS Records | DNS | N/A |
| Mail Server | TCP (port 25, 587) | Ping |
| Cron Job | CRON | HTTP webhook |
| Server Uptime | Ping | TCP or HTTP |

## Best Practices

1. **Use Multiple Monitors**: Track both connectivity (Ping) and functionality (HTTP)
2. **Appropriate Intervals**: Don't overload services with excessive checks
3. **Geographic Diversity**: Monitor from multiple regions for critical services
4. **Timeout Configuration**: Set realistic timeouts based on historical data
5. **Response Validation**: Always validate responses when possible

## Troubleshooting Monitors

### Monitor Shows Red/Down
1. Check if the service is actually running
2. Verify firewall allows monitoring from StatusApp regions
3. Check monitor configuration (URL, port, host)
4. Review recent changes to the service

### False Positives
1. Increase timeout settings
2. Adjust response validation rules
3. Check for rate limiting issues
4. Verify network connectivity from all regions

## Next Steps
- Learn about [advanced monitor configuration](/articles/monitors/advanced-monitor-configuration)
- Set up [incident management](/articles/incidents/incident-management-basics)
- Create [status pages](/articles/status-pages/creating-your-first-status-page)