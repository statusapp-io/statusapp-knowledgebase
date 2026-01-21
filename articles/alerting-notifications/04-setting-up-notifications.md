---
title: Setting Up Notifications and Alerts
slug: setting-up-notifications
category: Alerting & Notifications
excerpt: Learn how to configure email, Slack, SMS, and webhook notifications to stay informed about service issues.
order: 4
---

# Setting Up Notifications and Alerts

## Overview

Notifications keep you informed when something goes wrong. StatusApp supports multiple notification channels including email, Slack, SMS, webhooks, and more.

## Email Notifications

### Enabling Email Alerts

1. Go to **Settings > Notifications**
2. Click **Email** section
3. Toggle **Enable Email Notifications**
4. Enter one or more email addresses
5. Click **Save**

### Email Events

Choose which events trigger email notifications:
- **Monitor Down**: When a monitor status changes to DOWN
- **Monitor Up**: When a monitor recovers from DOWN
- **Incident Created**: When a new incident is created
- **Incident Updated**: When incident status changes
- **Incident Resolved**: When incident is resolved
- **Regression Alert**: When response times degrade significantly

### Customizing Email Settings

- **Digest Mode**: Group alerts into hourly/daily digests
- **Quiet Hours**: Set times to not receive notifications
- **Alert Threshold**: Only alert for certain monitor types

### Email Format

Emails include:
- Service name and status
- Issue start time
- Current status
- Direct link to StatusApp dashboard
- Link to status page (if public)

## Slack Integration

### Connecting Slack

1. Go to **Settings > Integrations**
2. Click **Add Slack**
3. Click **Connect to Slack**
4. Authorize StatusApp in your Slack workspace
5. Select channels where alerts should be posted

### Channel Configuration

#### Alert Channels
Where incidents and outages are posted:
- Critical alerts (red)
- Major issues (orange)
- Recoveries (green)

#### Alert Routing
Direct different alert types to different channels:
- **#incidents**: Critical issues
- **#alerts**: Service changes
- **#monitoring**: General notifications
- **#regression**: Performance degradation

### Slack Message Format

Messages include:
- 🔴 **Emoji indicators**: Visual status at a glance
- **Service name**: Which monitor/incident
- **Status**: Down, Up, Degraded, Resolved
- **Affected component**: Specific part of your system
- **Quick actions**: React to acknowledge or dismiss

### Slack Threading

- Initial alert starts a thread
- Follow-up updates in same thread
- Keep conversations organized
- Easy to see full incident history

## SMS Notifications

### Setting Up Twilio Integration

1. Sign up for [Twilio](https://www.twilio.com)
2. Get your Account SID and Auth Token
3. In StatusApp, go to **Settings > Integrations**
4. Click **Add Twilio**
5. Enter credentials and phone numbers
6. Test the connection

### SMS Alert Rules

Configure when SMS alerts are sent:
- **Critical Alerts Only**: Only for major outages
- **All Alerts**: Every status change
- **Custom Rules**: By monitor or severity

### SMS Best Practices

- Use sparingly - SMS can be expensive
- Reserve for critical services
- Set up only for on-call staff
- Configure quiet hours
- Test delivery before relying on it

## Webhooks

### Creating Webhook Integrations

1. Go to **Settings > Integrations**
2. Click **Add Webhook**
3. Enter webhook URL
4. Choose events to trigger
5. Add custom headers if needed
6. Test webhook

### Webhook Events

```json
{
  "event": "monitor.down",
  "timestamp": "2024-01-21T10:30:00Z",
  "monitor": {
    "id": "mon_123",
    "name": "API Server",
    "status": "DOWN",
    "region": "US East"
  },
  "incident": {
    "id": "inc_456",
    "status": "ACTIVE",
    "duration_minutes": 5
  }
}
```

### Event Types

- **monitor.down**: Monitor status changed to DOWN
- **monitor.up**: Monitor recovered to UP
- **incident.created**: New incident created
- **incident.updated**: Incident status changed
- **incident.resolved**: Incident resolved
- **incident.regression**: Performance regression detected

### Webhook Signatures

All webhook requests include HMAC SHA-256 signature:

```
X-StatusApp-Signature: sha256=abc123...
```

Verify the signature:
1. Get your webhook secret from settings
2. Create HMAC SHA-256 hash of request body
3. Compare with X-StatusApp-Signature header
4. Reject if signatures don't match

### Popular Integrations

#### Microsoft Teams
1. Get webhook URL from Teams channel
2. In StatusApp, create webhook with Teams URL
3. Configure JSON format:

```json
{
  "@type": "MessageCard",
  "@context": "https://schema.org/extensions",
  "summary": "{monitor_name} is {status}",
  "themeColor": "{color}"
}
```

#### PagerDuty
1. Set up PagerDuty integration in your account
2. Get integration key from PagerDuty
3. Use webhook to send events to PagerDuty
4. Create incidents directly from StatusApp

#### Opsgenie
1. Configure Opsgenie webhook receiver
2. Set up StatusApp to send to Opsgenie
3. Route alerts to on-call engineers
4. Create alerts and incidents automatically

## API Keys for Custom Integration

### Creating API Keys

1. Go to **Settings > API Keys**
2. Click **Generate New Key**
3. Enter key name
4. Select permissions (read, write, admin)
5. Set expiration (optional)
6. Click **Generate**

### Using API Keys

Include in request header:
```
Authorization: Bearer sk_live_abc123...
```

### API Permissions

- **Read**: View monitors, incidents, data
- **Write**: Create incidents, update monitors
- **Admin**: Delete items, manage settings

### Webhook via API

Send custom incident updates:
```bash
curl -X POST https://api.statusapp.io/incidents \
  -H "Authorization: Bearer sk_live_abc123" \
  -d '{"title":"Custom Incident","status":"investigating"}'
```

## Alert Thresholds and Rules

### Response Time Alerts

1. Go to monitor settings
2. Set **Response Time Threshold**
3. Example: Alert if > 2 seconds
4. Configure notifications

### Status Code Alerts

1. Define expected status codes
2. Example: Alert on anything other than 200
3. Alert on timeouts
4. Alert on SSL certificate errors

### Custom Rules

- Alert only during business hours
- Different thresholds per service
- Escalation paths (first alert, then escalate)
- Group related alerts

## Notification Preferences

### Quiet Hours

1. Go to **Settings > Notifications**
2. Enable **Quiet Hours**
3. Set time range (e.g., 10 PM - 7 AM)
4. Choose behavior:
   - Silence all notifications
   - Critical alerts only
   - Digest mode

### Notification Digest

- Batch notifications into summaries
- Send hourly, daily, or weekly digests
- Reduce notification fatigue
- Still get critical alerts immediately

### Do Not Disturb

- Temporarily mute all notifications
- Set duration (1 hour, 1 day, 1 week)
- Quick toggle during maintenance
- Resume normal notifications after period

## Testing Notifications

### Send Test Notifications

1. Go to **Settings > Notifications**
2. Click **Send Test** for each channel
3. Verify you receive test messages
4. Check message format and content

### Troubleshooting Delivery

#### Email Not Received
- Check spam/junk folder
- Verify email address is correct
- Check notification settings are enabled
- Verify event filters match

#### Slack Messages Not Appearing
- Verify Slack workspace is authorized
- Check channel permissions
- Verify bot is member of channel
- Test with @StatusApp command

#### Webhooks Not Firing
- Verify webhook URL is correct
- Check firewall/network access
- Review webhook logs
- Test with a simple webhook service (webhook.site)
- Verify event types are configured

## Best Practices

### Notification Strategy

1. **Email for Documentation**: Keep records of alerts
2. **Slack for Team Visibility**: Organize as a team
3. **SMS for Critical Issues**: Only for major outages
4. **Webhooks for Automation**: Trigger automated responses

### Alert Fatigue Prevention

- Don't configure alerts for everything
- Use thresholds appropriately
- Group related alerts
- Set up digest mode
- Configure quiet hours
- Regular review and adjustment

### Team Communication

1. Assign notification responsibilities
2. Define escalation procedures
3. Train team members
4. Document procedures
5. Regular review meetings

## Next Steps

- Learn about [incident management](/articles/incidents/incident-management-basics)
- Set up [status pages](/articles/status-pages/creating-status-pages)
- Explore [analytics](/articles/analytics/understanding-analytics)
- Review [API documentation](/articles/api/api-overview)