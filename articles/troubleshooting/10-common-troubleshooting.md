---
title: Common Troubleshooting Issues
slug: common-troubleshooting
category: Troubleshooting
excerpt: Solutions to common StatusApp problems and frequently encountered issues.
order: 10
---

# Common Troubleshooting Issues

## Monitor Issues

### Monitor Shows Down But Service is Up

**Causes:**
- StatusApp can't reach your service from monitoring regions
- Firewall blocking monitors
- Service responding slowly
- DNS resolution issues

**Solutions:**

1. **Check Service Status**
   - Verify service is actually running
   - Check locally: `curl https://yoursite.com`

2. **Check Firewall Rules**
   - Allow StatusApp IP ranges
   - Check WAF (Web Application Firewall)
   - Disable rate limiting temporarily

3. **Verify Monitor Configuration**
   - Check URL is correct
   - Verify expected status code
   - Check timeout settings
   - Try simple HTTP request without validation

4. **Test from Different Regions**
   - Issue may be regional
   - Configure multi-region monitoring
   - See which regions have connectivity

5. **Check DNS**
   - Verify domain resolves
   - Use nslookup or dig
   - Check for DNS propagation issues

### Monitor Keeps Timing Out

**Causes:**
- Service responding slowly
- Timeout threshold too low
- Network connectivity issues
- Overloaded server

**Solutions:**

1. **Increase Timeout**
   - Go to monitor settings
   - Increase timeout (default 10s)
   - Try 30 seconds
   - Monitor for issues

2. **Check Server Performance**
   - Monitor server CPU/memory
   - Check database performance
   - Look for slow queries
   - Review application logs

3. **Optimize Service**
   - Add caching
   - Use CDN
   - Optimize database queries
   - Scale infrastructure

4. **Check Network**
   - Ping server from regions
   - Measure network latency
   - Check for packet loss
   - Contact your ISP if needed

### Wrong Status Code Alert

**Example**: Getting 503 instead of 200

**Causes:**
- Service is down
- Maintenance mode enabled
- Load balancer misconfigured
- Rate limiting triggering

**Solutions:**

1. **Verify Actual Status Code**
   - Test manually
   - Check with curl: `curl -i https://yoursite.com`

2. **Check for Redirects**
   - Monitor settings: allow or disallow redirects
   - Check redirect chains
   - Verify final destination

3. **Check Rate Limiting**
   - StatusApp checks from multiple regions
   - Might trigger rate limits
   - Whitelist StatusApp IPs
   - Increase rate limit thresholds

4. **Review Application Logs**
   - Check application error logs
   - Look for exceptions
   - Check database connectivity
   - Review recent deployments

## Notification Issues

### Not Receiving Email Alerts

**Causes:**
- Email misconfigured
- Alerts disabled
- Email in spam folder
- Notification rules don't match

**Solutions:**

1. **Verify Email Setting**
   - Go to Settings > Notifications
   - Email section enabled?
   - Correct email address?

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