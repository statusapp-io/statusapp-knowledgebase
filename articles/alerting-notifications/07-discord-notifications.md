---
title: Discord Notifications
slug: discord-notifications
category: Alerting & Notifications
excerpt: Send real-time alerts to Discord channels for instant team visibility.
order: 7
---

# Discord Notifications

## Overview

Discord notifications send alerts directly to your Discord server. Get real-time incident notifications with color-coded messages, embeds, and mentions.

## Setting Up Discord

### Step 1: Create Discord Webhook

1. Open your Discord server
2. Right-click channel where alerts should appear
3. Select **Edit Channel**
4. Go to **Integrations** → **Webhooks**
5. Click **Create Webhook**
6. Name: `StatusApp Alerts` (or your preference)
7. Copy webhook URL
8. Click **Save**

Keep the webhook URL secret - treat it like a password.

### Step 2: Add to StatusApp

1. Go to **Notifications** → **Add Channel**
2. Select **Discord**
3. Paste webhook URL
4. Click **Test** to verify
5. Name: `Engineering Discord` (or your choice)
6. Click **Save**

## Message Format

### Incident Down Alert
```
🔴 Website Down
statusapp.io is down

Affected: Website - HTTPS Check
Last check: 2 minutes ago
Response: Connection timeout
```

### Incident Up Alert
```
🟢 Website Up
statusapp.io is back online

Affected: Website - HTTPS Check
Duration down: 15 minutes
Response: 200 OK
```

### Incident Acknowledged
```
🟡 Website Down (Acknowledged)
John Doe acknowledged this incident
Message: Working on it now
```

### Color Coding
- 🔴 **Red**: Incident started (down)
- 🟢 **Green**: Incident resolved (up)
- 🟡 **Yellow**: Incident acknowledged
- 🔵 **Blue**: Maintenance window

## Discord Channel Strategy

### By Severity

**#incidents** (Critical):
- Page alerts (requires escalation)
- Multi-region failures

**#alerts** (Production):
- Production environment incidents
- API failures
- Database issues

**#monitoring** (Informational):
- Non-critical alerts
- Maintenance windows
- Low-severity issues

### By Team

**#frontend-team**: Website, app incidents
**#backend-team**: API, database incidents
**#devops-team**: Infrastructure incidents
**#on-call**: Routed to on-call person via PagerDuty

### By Service

**#payments-service**: Payment API incidents
**#auth-service**: Authentication incidents
**#data-pipeline**: Data sync incidents

## Assigning to Monitors

### Single Discord Channel
```
Monitor: Website
Notifications: Discord (#alerts)
Incident Created: Alert sent to #alerts
Incident Resolved: Resolution sent to #alerts
```

### Multiple Channels (Different Teams)
```
Monitor: Payment API
Notifications: 
  - Discord (#backend-team)
  - Discord (#on-call)
  - Email (engineering@company.com)
Incident Created: All three notified
```

### Escalation Path
```
Level 1 (Down): Discord (#alerts) after 5 minutes down
Level 2 (Page): Discord (#on-call) after 15 minutes down
Level 3 (Escalate): SMS after 30 minutes down
```

## Best Practices

### 1. Test Your Webhook
Before using in production:

```
Send test from StatusApp:
1. Go to notification channel settings
2. Click "Test Notification"
3. Check Discord channel received message
4. Verify formatting looks good
```

### 2. Avoid Alert Fatigue
Too many channels = alerts ignored:

```
Good: 2-3 channels (critical, production, info)
Bad: 5+ channels per service

Use filtering/routing to reduce noise.
```

### 3. Use Mentions Strategically

**For Critical Incidents**:
```
@here Website Down (only on #on-call)
```

**For Production Issues**:
```
@backend-team API failing
```

Don't mention @everyone for non-critical issues.

### 4. Clear Channel Topic
Describe what each Discord channel receives:

```
#alerts: Production incidents requiring action
#monitoring: Non-critical alerts and status updates
#on-call: Critical incidents requiring immediate response
```

### 5. Archive Old Webhooks
If you change Discord channels:
1. Delete old webhook from Discord server
2. Remove notification channel from StatusApp
3. Create new channel with new webhook

Old webhooks no longer work.

### 6. Separate Environments
Different webhooks for dev/staging/prod:

```
Webhook 1: Production (#prod-alerts)
Webhook 2: Staging (#staging-alerts)
Webhook 3: Development (#dev-alerts)
```

Each environment has its own channel and webhook.

## Common Patterns

### Production Incident Routing

```
Tier 1 Alert:
├─ Discord: #prod-alerts (after 1 min down)
├─ Email: team@company.com

Tier 2 Alert:
├─ Discord: #on-call (after 5 min down)
├─ SMS: oncall number (after 5 min down)
├─ PagerDuty: Create incident

Tier 3 Alert:
├─ Call: Escalation number (after 15 min down)
└─ SMS: Manager (after 15 min down)
```

### Multi-Team Setup

```
Service: Payment Processing

Alert Path 1 (Default):
└─ Discord #payments-team

Alert Path 2 (Production):
└─ Discord #on-call (during business hours)

Alert Path 3 (After-hours):
└─ Discord #on-call + SMS alert (during on-call hours)
```

### Integration with Slack

Some teams use both Slack and Discord:

```
Primary: Slack #alerts (team works there)
Secondary: Discord #alerts (secondary channel)
SMS: High-severity only (pay attention to multiple alerts)
```

Use different notification channels to route to each.

## Troubleshooting

### Message Not Appearing in Discord

**Verify Webhook URL**:
```
1. Check webhook URL in StatusApp
2. Go to Discord channel → Integrations → Webhooks
3. Compare URLs match
4. Try copying fresh webhook URL
```

**Check Channel Permissions**:
```
1. Go to Discord channel
2. Right-click → View Details
3. Check webhook has permission to post
4. Verify you have access to channel
```

**Test Webhook Manually**:
```bash
# Replace WEBHOOK_URL with your actual webhook
curl -X POST https://discordapp.com/api/webhooks/YOUR_WEBHOOK \
  -H "Content-Type: application/json" \
  -d '{"content":"Test message"}'

# Should see message appear in channel
```

**Webhook Might Be Old**:
```
If messages stopped working:
1. Create new webhook in Discord
2. Update webhook URL in StatusApp
3. Delete old webhook
```

### Formatting Issues

**Message Too Long**:
- Discord has character limits
- StatusApp truncates long messages automatically
- Check status page or incident details in web UI

**Unicode Issues**:
- Emoji usually work fine
- Special characters may not render
- Report formatting issues in StatusApp

### Too Many Duplicate Messages

**Check Notification Settings**:
```
1. Go to monitor settings
2. Verify Discord channel assigned once
3. Check no global/default notifications
4. Verify incident routing isn't creating duplicates
```

### Webhook Expired or Removed

Messages stop working if:
- Discord webhook was deleted
- Bot permissions changed
- Server moved to different channel

**Solution**:
1. Create new webhook in Discord
2. Update in StatusApp with new URL
3. Test notification

## Webhook Security

### Protect Your Webhook URL

**Do Not**:
- Commit to GitHub
- Share in Slack or email
- Log to files
- Put in error messages

**Do**:
- Store in environment variables
- Use StatusApp's secure storage
- Rotate if accidentally exposed
- Recreate webhook if compromised

## Webhook Rotation

**To Rotate Webhooks**:

1. Create new webhook in Discord:
   - Channel → Integrations → Webhooks → Create
   
2. Add new webhook to StatusApp:
   - Notifications → Add Channel → Discord
   - Paste new URL
   
3. Test new webhook works:
   - Send test notification
   - Verify message in Discord
   
4. Update existing monitors:
   - Change monitors from old to new channel
   
5. Delete old webhook:
   - Discord channel → Integrations → Webhooks → Delete old one

## Next Steps

- **[Slack](/help/knowledge-base/slack-notifications)** - Slack alternative
- **[SMS](/help/knowledge-base/sms-notifications)** - SMS for critical alerts
- **[Notification Overview](/help/knowledge-base/notification-channels-overview)** - All channels
