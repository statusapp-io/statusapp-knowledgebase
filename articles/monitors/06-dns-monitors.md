---
title: DNS Monitors
slug: dns-monitors
category: Monitors
excerpt: Verify domain name resolution and monitor DNS records for changes or misconfigurations.
order: 6
---

# DNS Monitors

## Overview

DNS monitors verify that domain names are resolving correctly to the expected IP addresses or values. Use them to catch DNS misconfigurations, detect unauthorized changes, and track record updates.

## Creating a DNS Monitor

1. Create a new monitor
2. Select **DNS** type
3. Enter domain name
4. Select record type (A, AAAA, CNAME, MX, etc.)
5. (Optional) Enter expected value
6. Save and test

## Configuration

### Domain
Domain name to resolve:
- `example.com`
- `www.example.com`
- `api.example.com`
- `mail.example.com`

### Record Type

#### A Record (IPv4 Address)
Maps domain to IPv4 address:

```
Domain: example.com
Record Type: A
Expected Value: 192.0.2.1
```

**Use For**: Any service with a static IP

#### AAAA Record (IPv6 Address)
Maps domain to IPv6 address:

```
Domain: example.com
Record Type: AAAA
Expected Value: 2001:db8::1
```

**Use For**: IPv6-enabled services

#### CNAME Record (Alias)
Points domain to another domain:

```
Domain: www.example.com
Record Type: CNAME
Expected Value: cdn.example.com
```

**Use For**: CDN aliases, service aliases, subdomains

#### MX Record (Mail Exchange)
Specifies mail servers:

```
Domain: example.com
Record Type: MX
Expected Value: mail.example.com (or mail1.example.com)
```

**Use For**: Verify mail routing configuration

#### TXT Record (Text)
Stores arbitrary text data:

```
Domain: example.com
Record Type: TXT
Expected Value: v=spf1 include:sendgrid.net ~all
```

**Use For**: SPF, DKIM, domain verification

#### NS Record (Name Server)
Specifies authoritative name servers:

```
Domain: example.com
Record Type: NS
Expected Value: ns1.provider.com
```

**Use For**: Detect unauthorized nameserver changes

#### SRV Record (Service)
Specifies service location:

```
Domain: _ldap._tcp.example.com
Record Type: SRV
Expected Value: ldap.example.com:389
```

**Use For**: LDAP, SIP, and other service records

### Expected Value (Optional)
What the record should resolve to:
- Leave blank to just check resolution works
- Specify value to verify it's correct
- Great for catching unauthorized changes

## Common Use Cases

### Verify DNS Records
Ensure domain points to correct IP:

```
Domain: example.com
Record Type: A
Expected Value: 203.0.113.50
Alert: If resolves to different IP
```

### Monitor DNS Propagation
After DNS change, verify propagation:

```
Domain: new-server.example.com
Record Type: A
Expected Value: 198.51.100.1
Alert: If still resolving to old IP
```

### Catch DNS Hijacking
Detect unauthorized DNS changes:

```
Domain: example.com
Record Type: A
Expected Value: 192.0.2.1
Alert: If changes to unexpected IP
```

### Monitor Mail Server Configuration
Verify MX records are correct:

```
Domain: example.com
Record Type: MX
Expected Value: mail.example.com
Alert: If MX record is wrong
```

### Verify CDN Configuration
Ensure CNAME points to CDN:

```
Domain: cdn.example.com
Record Type: CNAME
Expected Value: d123.cloudfront.net
Alert: If CNAME is incorrect
```

### SPF/DKIM Configuration
Verify email authentication records:

```
Domain: example.com
Record Type: TXT
Expected Value: Contains "v=spf1"
Alert: If SPF record missing or wrong
```

## Multi-Region Monitoring

DNS monitors can query from multiple regions to detect:
- Regional DNS issues
- Global DNS propagation
- DNS resolver differences
- Regional blocking

**Setup**:
1. Edit monitor
2. Select multiple regions
3. Each region queries independently
4. Compare results across regions

## Best Practices

### 1. Monitor Critical Domains
Focus on:
- Main production domain
- API endpoints
- Mail servers
- CDN aliases

### 2. Verify Both Old and New
During DNS migrations:
- Monitor old IP to ensure it eventually expires
- Monitor new IP to confirm it's resolving
- Use this during transitions

### 3. Set Expected Values
Don't just check if it resolves - verify it's correct:

```
Good: Monitor resolves to 192.0.2.1
Better: Monitor resolves to 192.0.2.1 (your IP)
```

### 4. Multi-Region Monitoring
Monitor from multiple regions:
- Catches regional DNS issues
- Detects propagation problems
- Validates global consistency
- Requires 2+ regions to be sure

### 5. Alerts on Changes
Set alerts for unexpected values:
- Immediately alerts to unauthorized changes
- Helps detect DNS hijacking
- Catches misconfiguration before users affected

## Example: Complete DNS Monitoring

### Basic Setup
```
Monitor 1: example.com A record → 192.0.2.1
Monitor 2: www.example.com CNAME → cdn.example.com
Monitor 3: example.com MX record → mail.example.com
Monitor 4: example.com TXT record → contains "v=spf1"
```

### During Migration
```
Monitor 1 (Old): example.com A record → 192.0.2.1 (being deprecated)
Monitor 2 (New): example.com A record → 198.51.100.1 (new server)
Monitor 3 (Verify): www.example.com resolves correctly
```

### With Notifications
```
Each Monitor:
├─ Email: dba@company.com
├─ Slack: #dns-alerts
└─ Alert: If resolves to unexpected value
```

## Troubleshooting

### DNS Not Resolving
**Cause**: Domain doesn't exist or DNS not configured

**Solutions**:
- Verify domain name is correct
- Confirm DNS records are created
- Check with external DNS lookup tool

**Test Manually**:
```bash
# Linux/Mac
nslookup example.com
dig example.com

# Or online
Visit: nslookup.io or dnschecker.org
```

### Different Resolutions in Different Regions
**Cause**: Normal - DNS can vary by region

**Solutions**:
- Verify each region resolves to expected value
- Check if using geolocation-based DNS
- Confirm all regional values are intentional

**Check Across Regions**:
- Use online tool like MXToolbox or DNSChecker
- Select multiple locations
- Compare results

### Expected Value Mismatch
**Cause**: DNS changed (intentional or not)

**Solutions**:
- Verify change was intentional
- Update expected value if correct
- Investigate if unauthorized change

### Propagation Delay
**Cause**: DNS change not yet propagated globally

**Solutions**:
- Wait 24-48 hours for full propagation
- Monitor multiple regions to track progress
- Lower alert threshold during migration period

### CNAME Not Resolving Correctly
**Cause**: CNAME points to non-existent domain

**Solutions**:
- Verify target domain exists
- Check CNAME doesn't point to another CNAME
- Confirm target has A record

## DNS for Security

### Detect Hijacking
```
Domain: example.com
Record Type: NS
Alert: If nameservers change unexpectedly
```

### Verify DNSSEC
Monitor DNSSEC status:
```
Domain: example.com
Record Type: DNSKEY
Alert: If DNSSEC fails validation
```

### Unauthorized Subdomain Detection
```
Monitor: *.example.com
Alert: If unexpected subdomains appear
```

## Next Steps

- **[HTTP Monitors](/help/knowledge-base/http-website-monitors)** - Monitor websites after DNS verification
- **[SSL Monitors](/help/knowledge-base/ssl-certificate-monitors)** - Monitor HTTPS certificates
- **[Notifications](/help/knowledge-base/notification-channels-overview)** - Set up alerts
