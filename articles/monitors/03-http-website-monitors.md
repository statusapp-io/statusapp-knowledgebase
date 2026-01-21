---
title: HTTP & Website Monitors
slug: http-website-monitors
category: Monitors
excerpt: Monitor website availability, response times, and HTTP status codes with custom validation.
order: 3
---

# HTTP & Website Monitors

## Overview

HTTP/Website monitors check if your website or web application is responding correctly. This is the most commonly used monitor type and supports HTTP and HTTPS URLs.

## Creating an HTTP Monitor

1. Create a new monitor
2. Select **HTTP/Website** type
3. Enter the target URL
4. Configure optional settings
5. Save and test

## Basic Configuration

### URL
- Full URL including protocol (http:// or https://)
- Examples:
  - `https://example.com`
  - `https://api.example.com/health`
  - `https://app.example.com/status`

### HTTP Method
- **GET** (default) - Simple fetch
- **POST** - Send data
- **PUT** - Update data
- **DELETE** - Delete resource
- **PATCH** - Partial update
- **HEAD** - Like GET but no response body

### Expected Status Code
Define which status codes indicate success:

**Common Success Codes**:
- **200** - OK (standard success)
- **201** - Created (after POST)
- **202** - Accepted (request processing)
- **204** - No Content (DELETE success)
- **301/302** - Redirect (if following redirects)

**Other Codes**:
- **400** - Bad Request
- **401** - Unauthorized
- **403** - Forbidden
- **404** - Not Found
- **500** - Server Error
- **503** - Service Unavailable

**Tip**: Some APIs intentionally return 201 or 202 - verify what your API returns before setting expectations.

### Timeout
Maximum time to wait for response before failing:

**Recommendations**:
- Fast APIs: 5-10 seconds
- Standard websites: 15-30 seconds
- Slow services: 30-60 seconds
- International: 30-45 seconds

## Advanced Options

### Custom Headers
Add headers needed for authentication or specific requests:

```
Authorization: Bearer eyJhbGc...
User-Agent: StatusApp Monitor
X-Custom-Header: value
```

**Common Headers**:
- **Authorization**: For API authentication
- **User-Agent**: Identify the request source
- **X-Custom-Header**: Application-specific headers
- **Content-Type**: For POST/PUT requests

### Request Body
For POST/PUT requests, include body data:

**JSON Example**:
```json
{
  "action": "health_check",
  "version": "1.0"
}
```

**Form Data**:
```
username=admin&password=secret
```

### Response Validation
Verify the response contains expected content:

**Keyword Search**:
- Include: Check if response contains specific text
- Exclude: Check if response does NOT contain text

**Examples**:
- Include: "status": "ok"
- Exclude: "error"
- Include: "healthy"

### SSL Certificate Validation
- **Enabled** (default): Verify SSL certificate is valid
- **Disabled**: Allow self-signed or expired certificates (development only)

**Tip**: For staging/development with self-signed certs, disable validation. Always enable for production.

### Redirect Following
- **Follow** (default): Automatically follow HTTP redirects
- **Don't Follow**: Fail if redirects occur

**Example**:
- URL: `http://example.com`
- Redirects to: `https://www.example.com`
- Follow enabled: Returns final page
- Follow disabled: Fails (redirect not followed)

## Common Use Cases

### Website Availability
Check if your website is up:

```
URL: https://example.com
Method: GET
Status: 200
```

### API Health Check
Monitor REST API endpoint:

```
URL: https://api.example.com/health
Method: GET
Status: 200
Validate Response Contains: "status": "ok"
```

### Post-Deployment Check
Verify deployment was successful:

```
URL: https://app.example.com/version
Method: GET
Status: 200
Validate Response Contains: "version": "2.5.0"
```

### HTTP to HTTPS Redirect
Ensure HTTP redirects to HTTPS:

```
URL: http://example.com
Method: GET
Status: 301 or 302
Follow Redirects: enabled
Validate Final URL: https://...
```

### Form Submission
Test login or form endpoint:

```
URL: https://example.com/login
Method: POST
Status: 200
Body: username=test&password=pass
Validate Contains: "login successful"
```

## Response Metrics

Each monitor tracks:

- **Response Time**: Total time from request to response
- **Status Code**: HTTP response code returned
- **Content Size**: Size of response body
- **DNS Time**: Time to resolve domain
- **Connect Time**: Time to establish connection
- **SSL Time**: Time for SSL/TLS handshake
- **Transfer Time**: Time to receive response

View these in Analytics dashboard.

## Best Practices

### 1. Monitor Key User Paths
Don't just monitor home page:
- Monitor login endpoint
- Monitor API endpoints
- Monitor search functionality
- Monitor payment/checkout

### 2. Use Realistic Expectations
- Set status codes your app actually returns
- Don't expect 200 if API returns 201
- Allow redirects if your app uses them
- Set timeouts based on actual performance

### 3. Validate Response Content
- Don't just check status code
- Verify important content is present
- Check for error indicators in response
- Validate API returns expected data

### 4. Multi-Region Monitoring
- Monitor from regions where users are
- Catch regional CDN issues
- Detect routing problems
- Reduce false positives with 2+ regions

### 5. Appropriate Intervals
- Critical services: 1-5 minutes
- Production services: 5-15 minutes
- Non-critical: 15-60 minutes

## Troubleshooting

### Monitor Shows False Positives (False DOWN alerts)

**Increase Timeout**:
- Extend timeout if response is slow
- Account for network latency to regions

**Adjust Status Code**:
- Verify your app actually returns expected code
- Some services return 202 (Accepted) instead of 200

**Disable SSL Validation**:
- If using self-signed certificate
- Only for non-production monitoring

**Check Custom Headers**:
- Verify headers are correct format
- Confirm authentication isn't failing

### Monitor Not Detecting Real Issues

**Add Response Validation**:
- Don't just check status code
- Validate content contains expected text
- Add keyword or JSON validation

**Use Different Endpoint**:
- Monitor actual application endpoint, not just load balancer
- Check cache headers not returning stale content

**Lower Confirmation Threshold**:
- Alert after 1 failure instead of 2-3
- Catch issues faster

### Timeout Errors

**Increase Timeout**:
- Extend to 30-60 seconds for slow services
- Account for network distance in different regions

**Check Endpoint**:
- Verify URL is still correct
- Confirm service isn't actually down
- Check if endpoint requires authentication

## Multi-Region Monitoring

Test website from multiple geographic locations:

**Setup**:
1. Edit monitor
2. Go to "Regions" section
3. Select multiple regions
4. Save monitor

**Result**: Monitor checks from each region independently, helping detect:
- Regional outages
- CDN issues
- Network routing problems
- Regional rate limiting

## SSL Certificate Monitoring

While HTTP monitors check website availability, also set up [SSL Certificate Monitors](/help/knowledge-base/ssl-certificate-monitors) to track certificate expiration.

Combined approach:
- **HTTP Monitor**: Website availability (every 5 min)
- **SSL Monitor**: Certificate expiration (daily)

## Next Steps

- **[API Monitors](/help/knowledge-base/api-monitors)** - For advanced REST API testing
- **[Heartbeat Monitors](/help/knowledge-base/heartbeat-monitors)** - For scheduled job monitoring
- **[SSL Certificates](/help/knowledge-base/ssl-certificate-monitors)** - Monitor certificate expiration
- **[Notifications](/help/knowledge-base/notification-channels-overview)** - Set up alerts
- **[Analytics](/help/knowledge-base/understanding-analytics)** - Track performance over time
