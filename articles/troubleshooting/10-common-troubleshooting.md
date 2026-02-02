---
title: Common Troubleshooting Issues
slug: common-troubleshooting
category: Troubleshooting
excerpt: Solutions to common StatusApp problems and frequently encountered issues.
order: 10
---

# Common Troubleshooting Issues
# Common Troubleshooting

Use this guide to resolve frequent issues with monitors, notifications, incidents, and status pages.

## Monitors

### Monitor shows Down but service is Up
- **Firewall blocks probes:** allow StatusApp monitor IPs (see Monitoring IPs in Settings)
- **HTTPS/SSL errors:** expired cert, wrong host, or missing chain; test with `openssl s_client -connect domain:443 -servername domain`
- **Redirect loops or 30x limits:** enable "Follow redirects" or fix redirect target
- **Wrong expected status:** align the expected status code/range with your endpoint
- **Auth required:** add headers/tokens; ensure tokens are valid
- **Geo-specific failures:** check region-level results; some regions may be blocked

### Timeouts or very slow responses
- Increase monitor timeout slightly to confirm behavior (e.g., 10s)
- Check server load, DB latency, or upstream dependencies
- Verify large payloads or long-running queries
- Compare by region to find network-specific slowness

### DNS resolution failed
- Confirm A/AAAA/CNAME records are correct
- Wait for propagation after DNS changes (can take up to 24h)
- Avoid CNAMEs pointing to records with low availability

### TLS/Certificate errors
- Renew expired certificates; install intermediate chain
- Ensure `Common Name`/SAN matches the hostname
- For custom domains behind proxies, present the correct cert to monitors

### Wrong status code
- Enable/disable redirect following as needed
- Add authentication headers or query params required by the endpoint
- Verify the endpoint itself with `curl -I` from an external network

### Webhook/Cron (Heartbeat) monitors not recovering
- Ensure your service calls the heartbeat URL within the expected interval
- Check for clock drift or paused jobs
- Confirm you are using the latest heartbeat URL after regenerating it

## Notifications

### Email not received
- Check spam/junk; allowlist `statusapp.io`
- Verify the email is listed in the notification rule
- Send a test notification from Settings → Notifications

### Slack not receiving alerts
- Reconnect the Slack integration if the app was removed
- Ensure the target channel still exists and the app has permission
- Check if incident/monitor is included in the Slack rule

### SMS/Phone not received
- Confirm the phone number and country code are correct
- Check plan includes SMS/voice; exhausted credits can block delivery
- Carriers may filter unknown callers; try SMS if voice is blocked (or vice versa)

### Webhook issues
- Verify the endpoint returns 2xx within timeout
- Check signature/secret validation if enabled
- Review retry logs; StatusApp retries on non-2xx responses

## Incidents

### Incident did not auto-close
- Ensure the linked monitor is back to UP in all regions
- Confirm no ongoing maintenance window overlapping the incident
- If needed, manually resolve and add a postmortem note

### Status page not showing incident
- Confirm the incident is published (not private/internal)
- Ensure the affected monitors/components are attached to the status page

## Status Pages

### Custom domain not working
- Add `CNAME` pointing to your StatusApp host
- Wait for DNS propagation; avoid CDN caching stale DNS
- Ensure SSL issuance completed (usually a few minutes after DNS is correct)

### Metrics missing on status page
- Monitors must be linked to the page and have recent data
- Some metrics require paid plans; verify plan includes public metrics

### Embedded page or widgets not updating
- Browser/caching issues: hard refresh or disable aggressive CDN caching
- Confirm the embed uses HTTPS and correct domain

## API

### 401/403 Unauthorized
- Use the latest API key; keys are account-scoped
- Check if the API key has been revoked
- Send the key in the `X-API-Key` header OR `Authorization: Bearer <key>` header
- Verify the API key has the required permissions

### Rate Limit Errors (429)
- Check your plan's rate limit (Starter: 1,000/hr, Professional: 5,000/hr, Business: 10,000/hr, Enterprise: 100,000/hr)
- Implement exponential backoff in your client
- Check `X-RateLimit-Remaining` header for current quota
- Batch requests where possible
- Consider upgrading if consistently hitting limits

## Billing/Plan Limits

- Monitor count exceeded: remove unused monitors or upgrade plan
- Notification credits exhausted: add credits or wait for renewal
- Seat limits reached: remove inactive teammates or upgrade

## Still stuck?

Contact support with these details for faster help:
- Monitor type and target URL/host
- Exact error message and timestamp (UTC)
- Recent config changes (deploys, DNS, certs, firewall)
- Status page URL (if relevant)
2. **Check Notification Rules**
   - What events trigger email?
   - Monitor status matches?
   - Quiet hours enabled?

3. **Send Test Email**
   - Click "Send Test"
   - Check inbox and spam
   - Verify you receive it

4. **Check Email Provider**
   - StatusApp uses Resend
   - Check spam/junk folder
   - Add statusapp@resend.dev to contacts
   - Check email provider blocks

### Slack Messages Not Appearing

**Causes:**
- Slack not properly authorized
- Bot not member of channel
- Channel permissions

**Solutions:**

1. **Re-authorize Slack**
   - Go to Settings > Integrations
   - Remove Slack integration
   - Click Connect Slack
   - Re-authorize in workspace

2. **Check Channel Access**
   - Verify StatusApp bot is member
   - Check channel permissions
   - Try public channel
   - Invite bot: @statusapp

3. **Test Integration**
   - Trigger a test alert
   - Manual test from settings
   - Check bot permissions

### SMS Not Received

**Causes:**
- Twilio not configured
- Phone number incorrect
- Carrier blocking
- Payment issue

**Solutions:**

1. **Check Twilio Setup**
   - Verify Twilio credentials
   - Test Twilio account directly
   - Check account balance

2. **Verify Phone Number**
   - Correct format: +1-555-0123
   - Area code correct?
   - Try different number
   - Check carrier supports SMS

3. **Check SMS Rules**
   - Only alerts critical incidents?
   - Quiet hours enabled?
   - Alert type triggers SMS?

4. **Enable SMS Delivery Logs**
   - Check Twilio logs
   - See delivery status
   - View error messages

## Status Page Issues

### Status Page Not Updating

**Causes:**
- Automation disabled
- Components not linked
- Status page paused

**Solutions:**

1. **Check Automation**
   - Status page settings
   - "Automatically update" enabled?
   - Disable and re-enable

2. **Verify Components**
   - Components linked to monitors?
   - Monitor status actually changing?
   - Refresh page (Ctrl+F5)

3. **Check Status Page Settings**
   - Status page published?
   - Visibility settings correct?
   - Domain configured?

### Custom Domain Not Working

**Causes:**
- DNS not configured
- DNS propagation not complete
- SSL certificate not issued

**Solutions:**

1. **Verify DNS Configuration**
   - Copy CNAME from StatusApp
   - Add to DNS provider
   - Verify with `nslookup`
   - Wait for propagation (15m-48h)

2. **Check DNS Propagation**
   - Use mxtoolbox.com
   - Search for your domain
   - Shows propagation status
   - Check from multiple locations

3. **Verify SSL Certificate**
   - Let's Encrypt certificate auto-issued
   - Takes 15-30 minutes
   - Wait if recently configured
   - Check browser console for errors

4. **Clear Cache**
   - Browser cache
   - DNS cache (ipconfig /flushdns)
   - Try incognito window

## Analytics Issues

### No Data in Analytics

**Causes:**
- Monitor too new (no data)
- Monitor disabled
- No checks running

**Solutions:**

1. **Verify Monitor Running**
   - Check monitor status
   - See recent checks in log
   - Run manual check
   - Verify interval

2. **Wait for Data**
   - New monitors need time
   - At least one check needed
   - Data appears within minutes
   - Wait 1 hour for trends

3. **Check Monitor Configuration**
   - Correct URL?
   - Correct status code?
   - Response validation settings?

### Analytics Showing Wrong Uptime

**Causes:**
- False positives from validation
- Monitor misconfiguration
- Network issues

**Solutions:**

1. **Review Failed Checks**
   - Click uptime percentage
   - See what failed
   - Check error messages
   - Identify pattern

2. **Adjust Thresholds**
   - Check timeout settings
   - Review response validation
   - Disable strict checking if needed
   - Enable grace period

3. **Investigate Failures**
   - Manual test of URL
   - Check actual service
   - Review server logs
   - Check for false positives

## Team and Access Issues

### Team Member Can't Access Account

**Causes:**
- Not invited properly
- Invitation expired
- Wrong email
- Permission issue

**Solutions:**

1. **Verify Invitation**
   - Go to Settings > Team
   - Check team members list
   - Is user listed as "Invited"?
   - Resend invitation if expired

2. **Check Email
   - Verify correct email
   - Check spam folder
   - Ask to confirm receipt
   - Try different email

3. **Verify Permissions**
   - Check user role
   - Sufficient permissions?
   - Change role if needed

### Lost Access to Admin Account

**Solutions:**

1. **Use Password Reset**
   - Login page > Forgot Password
   - Check email for link
   - Reset password
   - Login with new password

2. **Two-Factor Authentication**
   - Lost backup codes?
   - Use recovery email
   - Contact support
   - Verify identity

3. **Contact Support**
   - Email support@statusapp.io
   - Explain situation
   - Verify account ownership
   - Account recovery assistance

## Billing Issues

### Payment Declined

**Causes:**
- Card expired
- Insufficient funds
- Fraud detection
- Wrong billing info

**Solutions:**

1. **Verify Card Details**
   - Check expiration date
   - Verify billing address
   - Check CVV
   - Try different card

2. **Contact Your Bank**
   - Fraud alert?
   - Request approval
   - Verify merchant
   - Resolve with bank

3. **Try Different Payment Method**
   - Different card
   - Bank transfer
   - Wire transfer
   - Contact sales

### Invoice Not Received

**Causes:**
- Email misconfigured
- Spam folder
- Billing address wrong

**Solutions:**

1. **Check Billing Email**
   - Go to Billing settings
   - Is email correct?
   - Update if needed

2. **View in Dashboard**
   - Go to Billing > Invoices
   - Download PDF
   - Print if needed

3. **Request Manual Invoice**
   - Email support
   - Request invoice
   - Provide details

## Contacting Support

### When to Contact Support

- Can't resolve issue with troubleshooting
- Technical issues
- Billing disputes
- Feature requests
- Account issues

### Support Channels

- **Email**: support@statusapp.io
- **Chat**: Available in dashboard
- **Phone**: Enterprise customers only
- **Docs**: https://docs.statusapp.io

### Information to Provide

- Detailed description of issue
- Steps to reproduce
- Screenshots/logs
- Affected monitors/incidents
- Current plan type
- Browser/environment info

## Next Steps

- Check [knowledge base articles](/articles)
- Visit [documentation](https://docs.statusapp.io)
- Email support: support@statusapp.io