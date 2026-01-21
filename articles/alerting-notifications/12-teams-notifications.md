---
title: Microsoft Teams Notifications
slug: teams-notifications
category: Alerting & Notifications
excerpt: Send StatusApp alerts to Microsoft Teams for team collaboration and incident visibility.
order: 12
---

# Microsoft Teams Notifications

## Overview

Microsoft Teams notifications deliver alerts directly to your Teams channels. Keep your whole team informed with color-coded cards and action buttons.

## Setting Up Microsoft Teams

### Step 1: Configure Incoming Webhook

1. Open Microsoft Teams
2. Click channel name where alerts should appear
3. Select **⋯ More options** (top right)
4. Click **Connectors**
5. Search for **Incoming Webhook**
6. Click **Configure**
7. Name: `StatusApp Alerts` (or your choice)
8. Optionally upload icon image
9. Click **Create**
10. Copy webhook URL
11. Click **Done**

**Keep the webhook URL secret** - treat it like a password.

### Step 2: Add to StatusApp

1. Go to **Notifications** → **Add Channel**
2. Select **Microsoft Teams**
3. Paste webhook URL
4. Click **Test** to verify
5. Name: `Teams #incidents` (or your choice)
6. Click **Save**

## Message Format

### Incident Down Alert

```
🔴 Website Down

Service: Website - HTTPS Check
Status: Down
Time Down: 2 minutes ago
Last Response: Connection timeout
Severity: Critical

Affected Regions: US-East, EU-West
Monitor URL: [View in StatusApp]
Acknowledge: [Button]
```

### Incident Up Alert

```
🟢 Website Up

Service: Website - HTTPS Check
Status: Resolved
Duration Down: 15 minutes
Response Time: 150ms
Resolution Time: 2 minutes

Healthy Regions: All
Monitor URL: [View in StatusApp]
```

### Incident Acknowledged

```
🟡 Acknowledged by John Doe

Website Down
Status: Being investigated
Time Acknowledged: Just now
Incident started: 30 minutes ago

[View in StatusApp]
```

## Channels and Strategy

### By Service Criticality

**#production-alerts** (Critical):
- Revenue-impacting incidents
- Database/API outages
- Payment system issues
- Requires immediate response

**#alerts** (Production):
- Production feature breakage
- API endpoint failures
- Database performance
- Typical response time: 15 min

**#monitoring** (Informational):
- Non-critical alerts
- Performance degradation
- Maintenance windows
- No immediate action needed

**#on-call** (Private):
- Only on-call team sees alerts
- Private channel for escalation
- Real-time incident management
- Fast communication

### By Team

**#engineering-alerts**: All technical incidents
**#devops-alerts**: Infrastructure and deployment issues
**#frontend-team**: UI/website issues
**#backend-team**: API and database issues
**#security-alerts**: Security and compliance issues

### By Environment

**#prod-alerts**: Production only
**#staging-alerts**: Staging environment
**#dev-monitoring**: Development environment

Use separate webhooks for each environment to avoid waking team for dev issues.

## Assigning to Monitors

### Single Teams Channel
```
Monitor: Production API
Notifications: Teams (#production-alerts)
Incident Down: Alert sent to channel
Incident Up: Resolution sent to channel
```

### Multiple Teams Channels
```
Monitor: Payment Service
Notifications:
  - Teams (#production-alerts)
  - Teams (#on-call-private)
  - Email (formal log)
  - PagerDuty (escalation)

Incident Down: All channels notified
```

### Escalation Path
```
5 min down   → Teams #alerts (awareness)
15 min down  → Teams #on-call (escalation)
30 min down  → SMS (urgent)
60 min down  → Phone call (critical)
```

## Best Practices

### 1. Test Your Webhook
Before using in production:

```
1. Go to notification channel settings
2. Click "Test Notification"
3. Verify message appears in Teams channel
4. Check formatting looks good
5. Confirm no errors
```

### 2. Reduce Alert Fatigue
Too many channels = alerts ignored:

```
Good: 2-3 channels max
- Production (urgent)
- Staging/Dev (informational)
- Archive/Logs (reference)

Bad: 10+ channels
- Creates notification chaos
- Important alerts missed
- Team disengages from alerts
```

### 3. Use Threads for Context
After incident alert:

```
Initial Alert:
└─ Website Down (posted in channel)

Team Discussion:
├─ Reply 1: "Restarting deployment..."
├─ Reply 2: "Database is fine, checking app logs"
├─ Reply 3: "Found memory leak in new version"
├─ Reply 4: "Rolling back to previous version"
└─ Resolution: "Website back online"

Threads keep context organized.
```

### 4. Archive Alert History
Keep channels clean:

```
After incident resolved:
- Review incident in StatusApp
- Post summary to #incidents-archive channel
- Keep alert channel focused on current issues
- Document lessons learned separately
```

### 5. Set Clear Channel Topics
Describe what each channel contains:

```
#production-alerts
Topic: Revenue-impacting incidents requiring immediate response

#staging-alerts
Topic: Staging environment issues for testing and validation

#on-call-private
Topic: Active incident coordination (private, team only)

#monitoring-archive
Topic: Historical alerts and resolved incidents (reference only)
```

### 6. Mention Appropriately
Use @mentions strategically:

```
@channel or @here:
- Only for critical incidents
- Only during business hours
- Risk of overuse = alerts ignored

@oncall or specific person:
- For normal production incidents
- Doesn't disturb entire team

No mention:
- Informational alerts
- Scheduled maintenance
- Non-urgent updates
```

## Common Patterns

### Production Incident Routing

```
Tier 1 Alert (5 min down):
├─ Teams: #production-alerts (no mention)
├─ Email: ops@company.com
└─ Time: Post immediately

Tier 2 Alert (15 min down):
├─ Teams: #on-call (with @oncall mention)
├─ SMS: on-call number
└─ Time: Escalate if not acknowledged

Tier 3 Alert (30 min down):
├─ Phone call to escalation lead
├─ All hands notification
└─ War room initiated
```

### Multi-Team Development
```
Monitor: API Endpoint

Service Team:
└─ Teams #backend-team (implementation details)

Operations Team:
└─ Teams #production-alerts (service status)

Executives:
└─ Teams #exec-summary (impact summary only)

Each team gets relevant level of detail.
```

### Shift Handoff
```
Before End of Shift:
├─ Post summary to #handoff channel
├─ Highlight: Critical issues, ongoing investigations
├─ Include: Runbooks, contacts, escalation info
├─ Alert: Incoming team to check message

New Shift Arrives:
└─ Reviews handoff summary before starting
```

## Troubleshooting

### Message Not Appearing

**Verify Webhook URL**:
```
1. Check webhook URL in StatusApp
2. Compare with Team's connector settings
3. Copy fresh webhook URL if changed
4. Verify URL is complete and correct
```

**Check Channel Permissions**:
```
1. Right-click Teams channel
2. Select "Manage channel"
3. Check bot can post (Connector permissions)
4. Verify you have access to channel
5. Check if channel is archived
```

**Test Webhook Manually**:
```bash
curl -X POST <WEBHOOK_URL> \
  -H 'Content-Type: application/json' \
  -d '{"text":"Test message"}'
```

Should see message appear in Teams channel.

**Webhook Might Be Expired**:
```
If messages stopped working:
1. Delete old connector
2. Create new webhook
3. Update URL in StatusApp
4. Test notification
```

### Formatting Issues

**Card Not Rendering**:
- Teams sometimes needs refresh
- Try switching channels and back
- Check Teams app is updated
- Try on web version

**Colors Not Showing**:
- Check Teams theme (Dark vs Light)
- Colors render differently per theme
- Message should still be readable

**Truncated Messages**:
- Teams limits character length
- StatusApp auto-truncates long messages
- View full details in StatusApp web UI

### Duplicate Messages

**Check Settings**:
```
1. Go to monitor settings
2. Verify Teams channel assigned once
3. Check no global/default notifications
4. Verify no duplicate notification routing
```

### Too Many Notifications

**Reduce Frequency**:
```
1. Increase check interval (5 to 15 minutes)
2. Require more failed checks before alerting
3. Filter non-critical services from Teams
4. Send only P1/P2 to Teams
```

**Route by Severity**:
```
P1 (Critical) → Teams + SMS + Phone
P2 (High) → Teams + Email
P3 (Medium) → Email + Dashboard only
P4 (Low) → Dashboard only
```

## Security

### Protect Your Webhook

**Do Not**:
- Share URL in email or chat
- Commit to GitHub
- Log to files
- Include in error messages
- Put in documentation

**Do**:
- Store in StatusApp secure storage
- Treat like password
- Rotate if exposed
- Delete old webhooks

### Regenerate Webhook

To rotate/replace webhook:

1. In Teams: Create new connector with new webhook
2. Update StatusApp: Use new URL
3. Test: Send test notification
4. Delete old connector in Teams
5. Done: Old webhook no longer works

Old webhooks are deactivated immediately.

## Webhook vs Teams Bot

**Incoming Webhook** (Used by StatusApp):
- Simple, one-way notifications
- Posts to channel as webhook
- No interactive features
- Good for: Status updates, alerts

**Teams Bot** (Alternative):
- More complex, two-way communication
- Can respond to commands
- Interactive cards with buttons
- StatusApp uses webhooks, not bots

For StatusApp, webhooks are sufficient and simpler.

## Integration with Slack

Some teams use both Teams and Slack:

```
Primary Notification: Teams #production
Secondary: Slack #production-alerts
Backup: Email

Each platform notifies via StatusApp notification channel
```

Use different notification channels to route to each platform.

## Next Steps

- **[Slack](/help/knowledge-base/slack-notifications)** - Slack alternative
- **[Discord](/help/knowledge-base/alerting-notifications/discord-notifications)** - Discord alternative
- **[Notification Overview](/help/knowledge-base/notification-channels-overview)** - All channels
