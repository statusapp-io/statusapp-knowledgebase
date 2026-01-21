---
title: SSL Certificate Monitors
slug: ssl-certificate-monitors
category: Monitors
excerpt: Track SSL/TLS certificate expiration dates and prevent HTTPS outages.
order: 7
---

# SSL Certificate Monitors

## Overview

SSL Certificate monitors track when your HTTPS certificates expire and send alerts before they become invalid. This prevents the "certificate expired" errors that break HTTPS access.

## Creating an SSL Monitor

1. Create a new monitor
2. Select **SSL Certificate** type
3. Enter domain name
4. Set alert thresholds
5. Save and test

## Configuration

### Domain
Domain with SSL certificate to monitor:
- `example.com`
- `www.example.com`
- `api.example.com`
- `mail.example.com`

### Port
HTTPS port (default: 443):
- Usually 443 for standard HTTPS
- Sometimes 8443 for custom HTTPS ports
- Leave as 443 unless using non-standard port

### Alert Thresholds
When to send alerts before expiration:

**Recommended Thresholds**:
- **30 days**: First warning - start renewal process
- **14 days**: Second warning - prioritize renewal
- **7 days**: Critical warning - immediate action
- **3 days**: Emergency - risk of outage

**Setup Multiple Monitors** for layered alerts:
```
Monitor 1: Alert when 30 days remaining
Monitor 2: Alert when 7 days remaining
Monitor 3: Alert when 1 day remaining
```

## What It Checks

### Certificate Validity
- ✓ Certificate is currently valid
- ✓ Certificate not yet expired
- ✓ Certificate domain matches
- ✓ Certificate chain is complete

### Expiration Date
- ✓ Days until expiration
- ✓ Exact expiration date/time
- ✓ Issuer information
- ✓ Certificate brand/type

### Certificate Details
- Certificate subject
- Issuer/Certificate Authority
- Certificate version
- Public key algorithm
- Signature algorithm

## Common Use Cases

### Monitor Production Domain
Main website certificate:

```
Domain: example.com
Alert Thresholds: 30, 14, 7 days
Notifications: Email + PagerDuty
```

### Monitor All Subdomains
Different subdomains might have different certs:

```
Monitor 1: example.com (wildcard cert)
Monitor 2: api.example.com (specific cert)
Monitor 3: mail.example.com (specific cert)
```

### Catch Certificate Chain Issues
Ensure full certificate chain is valid:

```
Domain: secure.example.com
Check: Intermediate certificates are valid
Alert: If chain is incomplete
```

### Track Renewals
Monitor before and after renewal:

```
Before: Old certificate
├─ Alert when 30 days before expiration
└─ Track renewal progress

After: New certificate  
├─ Verify new cert is installed
└─ Confirm expiration date is correct
```

## Best Practices

### 1. Monitor All Domains
Don't just monitor main domain:
- Main production domain
- API endpoints
- Subdomains with different certs
- Internal services with HTTPS
- Staging/development (prevent surprise expirations)

### 2. Set Multiple Alert Thresholds
Don't rely on single alert:

```
Monitor 1: 30 days - "Check renewal status"
Monitor 2: 14 days - "Start renewal if needed"
Monitor 3: 7 days - "URGENT: Renew immediately"
Monitor 4: 1 day - "CRITICAL: Emergency action"
```

### 3. Use Wildcard Certificates
Wildcard certs cover subdomains:
- `*.example.com` covers all subdomains
- `example.com` covers apex domain separately
- Monitor wildcard certificate, not each subdomain

### 4. Plan Renewal Process
Before certificate expires:
- Know your Certificate Authority's renewal process
- Have documented steps
- Test renewal in staging first
- Plan for propagation time

### 5. Multi-Region Monitoring
Monitor from multiple regions:
- Some regions might have cached old certificate
- Detects propagation issues
- Confirms certificate is valid globally

## Alert Thresholds

### 30 Days Before
**Status**: Warning - Start planning renewal
- Check certificate authority
- Review renewal process
- Test renewal in staging
- Plan timing for production renewal

### 14 Days Before
**Status**: Reminder - Begin renewal
- If not started, begin now
- Prepare required information
- Order new certificate if needed

### 7 Days Before
**Status**: Critical - Complete renewal
- Must be completed by now
- Install new certificate immediately
- Verify installation with monitor
- Test HTTPS connections

### 1 Day Before
**Status**: Emergency - Immediate action
- If not renewed, severe outage imminent
- Drop everything to resolve
- Emergency on-call response
- Manual verification required

## Example: Complete Certificate Monitoring

### Basic Setup
```
Monitor 1: example.com - Alert at 30 days
Monitor 2: example.com - Alert at 7 days
Monitor 3: api.example.com - Alert at 30 days
Monitor 4: mail.example.com - Alert at 30 days
```

### With Notifications
```
Each Monitor:
├─ Slack: #security-alerts
├─ Email: devops@company.com
└─ PagerDuty: Cert-Renewal team (for critical threshold)
```

### During Renewal
```
Before Renewal:
├─ Old cert: example.com expires in 7 days
├─ Alert: URGENT - Renewal needed
└─ Action: Begin renewal process

After Renewal:
├─ New cert: example.com expires in 365 days
├─ Verify: New certificate installed correctly
└─ Monitor: Expiration reset to future date
```

## Troubleshooting

### Certificate Not Being Checked
**Cause**: Domain not reachable or HTTPS not working

**Solutions**:
- Verify domain is online
- Ensure HTTPS is working with browser
- Check port is correct (usually 443)
- Confirm SSL is enabled on domain

**Test Manually**:
```bash
# Check certificate
openssl s_client -connect example.com:443

# View expiration date
echo | openssl s_client -connect example.com:443 | grep "not"
```

### Wrong Certificate Detected
**Cause**: Domain is using different certificate than expected

**Solutions**:
- Verify you're checking right domain
- Confirm certificate is correct one
- Check if using wildcard certificate for subdomains
- Verify CNAME pointing to correct destination

### Certificate Chain Invalid
**Cause**: Missing intermediate certificates

**Solutions**:
- Contact Certificate Authority
- Request intermediate certificate
- Install complete chain on server
- Verify with SSL checker tool

### Alerts Not Firing
**Cause**: Monitor not configured correctly

**Solutions**:
- Verify domain is correct
- Confirm alert thresholds are set
- Test monitor manually
- Check notification channels are assigned

## SSL Certificate Workflow

### Step 1: Initial Setup (365 days before expiration)
- Set up SSL certificate monitor
- Configure 30-day alert threshold
- Add notifications

### Step 2: 30 Days Before Expiration
- Monitor sends alert
- Review certificate renewal requirements
- Plan renewal timing
- Test in staging

### Step 3: 14 Days Before
- Begin renewal process
- Order new certificate if needed
- Prepare documentation/approvals

### Step 4: 7 Days Before
- Install new certificate
- Test HTTPS access
- Verify browser shows valid certificate
- Monitor detects new certificate

### Step 5: 1 Day Before Old Certificate Expires
- Confirm new certificate is live
- Verify expiration date updated
- Plan for monitoring of new certificate

### Step 6: After Expiration
- Old certificate no longer needs monitoring
- Remove old certificate monitor
- Continue monitoring new certificate (30-day alert)

## Certificate Types

### Single Domain Certificate
Covers one domain only:
- `*.example.com` or `www.example.com`
- Need separate certificates for different domains

### Wildcard Certificate
Covers domain and all subdomains:
- `*.example.com` = covers `api.example.com`, `mail.example.com`, etc.
- Doesn't cover apex domain separately (need both `*.example.com` and `example.com`)

### Multi-Domain (SAN) Certificate
Covers multiple specific domains:
- `example.com`, `www.example.com`, `api.example.com` (all on one cert)
- Need to list each domain when renewing

### EV Certificate
Extended Validation (green bar):
- Organization validated
- More trust indicators in browser
- Same monitoring as standard certificates

## Integration with HTTPS Monitoring

Combine with HTTP monitors:

```
SSL Certificate Monitor: example.com
└─ Ensures certificate is valid and not expiring

HTTP Monitor: https://example.com
└─ Ensures website loads over HTTPS
└─ Ensures certificate is installed correctly

Together: Complete HTTPS monitoring
```

## Next Steps

- **[HTTP Monitors](/articles/monitors/http-website-monitors)** - Combine with HTTP monitoring
- **[Notifications](/articles/alerting-notifications/notification-channels-overview)** - Set up alerts
- **[Analytics](/articles/analytics/understanding-analytics)** - Track certificate metrics
