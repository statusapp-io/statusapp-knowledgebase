---
title: Complete Guide to Notification Channels
slug: notification-channels-guide
category: Alerting & Notifications
excerpt: Master StatusApp's flexible notification system with Email, Slack, Discord, Telegram, SMS, PagerDuty, Opsgenie, and Microsoft Teams integrations.
order: 4
---

# Complete Guide to Notification Channels

## Understanding Notification Channels

StatusApp uses a flexible **notification channel** system that separates where alerts are sent from which monitors trigger them. This allows you to:

- Create reusable notification configurations
- Route different monitors to different channels
- Use multiple notification types simultaneously
- Manage all alerts from a central location

### How It Works

1. **Create Notification Channels**: Configure channels for Email, Slack, Discord, etc.
2. **Assign to Monitors**: Choose which channels each monitor should use
3. **Receive Alerts**: Get notified when monitors detect issues

## Creating Notification Channels

### Accessing Notification Channels

1. Navigate to **Settings → Notification Channels**
2. Click **Create Channel**
3. Choose your channel type
4. Configure the channel settings
5. Test the channel to verify it works
6. Save the channel

### Channel Types Available

- **Email** - HTML-formatted email notifications
- **Slack** - Rich messages in Slack channels
- **Discord** - Embeds in Discord channels
- **Telegram** - Messages via Telegram bot
- **SMS** - Text message alerts (paid plans)
- **PagerDuty** - Create incidents and trigger on-call
- **Opsgenie** - Alert management with priority routing
- **Microsoft Teams** - Messages in Teams channels
- **Webhooks** - Custom HTTP endpoints for any integration

## Email Notifications

### Configuration

**Channel Name**: Give it a descriptive name (e.g., "Ops Team Email", "Critical Alerts")

**Recipient Emails**: Enter one or more email addresses, separated by commas
- Example: `ops@company.com, alerts@company.com, oncall@company.com`
- Supports distribution lists and group emails
- No limit on number of recipients

### Email Content

Emails include:
- **Subject**: Clear indication of issue and monitor name
- **Status**: Current monitor status with color coding
- **Monitor Details**: Name, URL, type
- **Incident Information**: Start time, duration, checks failed
- **Response Time**: Current and historical performance
- **Region**: Which monitoring region detected the issue
- **Actions**: Links to dashboard and monitor details
- **HTML Formatting**: Professional, branded emails

### Email Alerts For

- Monitor status changes (DOWN, UP, DEGRADED)
- Incident creation and resolution
- Performance regressions
- SSL certificate expiration warnings
- Multi-region outages

### Best Practices

- Create separate channels for different severity levels
- Use distribution lists instead of individual emails
- Test channels before assigning to production monitors
- Include both primary and backup contacts
- Document alert email addresses in runbooks

## Slack Integration

### Setup Steps

1. Create notification channel and select **Slack**
2. Click **Connect to Slack**
3. Authorize StatusApp in your Slack workspace
4. Choose the channel where alerts should be posted
5. Save the configuration

### Slack Message Features

**Rich Formatting**:
- Color-coded messages (🔴 Red for DOWN, 🟢 Green for UP, 🟠 Orange for DEGRADED)
- Monitor name, status, and URL
- Response time metrics
- Incident duration
- Quick action buttons

**Threading**:
- Initial alert creates a thread
- Updates posted as replies in the same thread
- Keeps channel organized
- Easy to track incident progression

**Mentions**:
- Configure @channel or @here mentions for critical alerts
- Mention specific users or groups
- Customize mention behavior per channel

### Channel Routing Examples

**Critical Production Alerts**:
- Channel: `#production-incidents`
- Mention: `@oncall`
- Monitors: All production services

**Staging Environment**:
- Channel: `#staging-monitoring`
- Mention: None (informational)
- Monitors: Staging services only

**Performance Alerts**:
- Channel: `#performance`
- Mention: `@performance-team`
- Monitors: Services with regression detection

### Best Practices

- Use dedicated channels for monitoring alerts (don't mix with team chat)
- Create escalation channels for critical vs informational alerts
- Test Slack integration before deploying
- Document which Slack channels receive which alerts
- Use thread replies to discuss incidents without cluttering the channel

## Discord Integration

### Setup

1. Create a webhook in your Discord server:
   - Server Settings → Integrations → Webhooks → New Webhook
   - Choose the channel for alerts
   - Copy the webhook URL
2. In StatusApp, create notification channel
3. Select **Discord** type
4. Paste webhook URL
5. Test the integration

### Discord Alert Features

**Rich Embeds**:
- Color-coded embeds based on status
- Monitor information and metrics
- Timestamp and duration
- Direct links to dashboard
- Thumbnail icons for status

**Embed Colors**:
- 🔴 Red (#DC2626): Monitor DOWN
- 🟢 Green (#16A34A): Monitor UP/Recovered
- 🟠 Orange (#EA580C): Monitor DEGRADED
- 🟣 Purple (#9333EA): Performance regression
- 🔵 Blue (#2563EB): Regression resolved

### Best Practices

- Create dedicated Discord channels for monitoring (e.g., `#monitoring`, `#incidents`)
- Use role mentions (@Ops) for critical alerts
- Test webhooks before production use
- Keep webhook URLs secret (regenerate if exposed)
- Use different channels for different environments

## Telegram Integration

### Setup Steps

1. Create a Telegram bot via @BotFather:
   - Message `/newbot` to @BotFather
   - Follow prompts to create your bot
   - Copy the bot token
2. Add bot to your group or channel
3. Get your Chat ID:
   - Send a message to your bot or group
   - Visit: `https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates`
   - Find your `chat_id` in the response
4. In StatusApp, create notification channel
5. Enter bot token and chat ID
6. Test the connection

### Telegram Message Features

**Markdown Formatting**:
- Bold status indicators
- Monitor names and URLs as links
- Code blocks for technical details
- Inline mentions and hashtags

**Message Content**:
- Status emoji (🔴🟢🟠)
- Monitor name and type
- Current status and duration
- Response time metrics
- Links to dashboard

### Use Cases

- Mobile-first alerting
- International teams
- Personal notifications
- Quick status updates
- On-call rotations

### Best Practices

- Use group chats for team notifications
- Use private chats for personal alerts
- Set up bot commands for common actions
- Mute non-critical notification channels
- Keep bot tokens secure

## SMS Notifications

*Available on Starter plans and above*

### Configuration

1. Go to **Settings → Notification Channels**
2. Create new channel, select **SMS**
3. Enter phone number with country code (e.g., +1234567890)
4. Verify phone number (verification code sent via SMS)
5. Configure which alerts trigger SMS

### SMS Content

SMS messages are concise and contain:
- Monitor name
- Status (DOWN/UP)
- Duration if applicable
- Short link to dashboard

Example:
```
[StatusApp] CRITICAL: Production API is DOWN for 5min. 
Check: https://app.statusapp.io/m/abc123
```

### SMS Best Practices

**Use Sparingly**:
- Only for critical, actionable alerts
- Avoid SMS for DEGRADED or informational alerts
- Don't send SMS for every monitor

**Configure Wisely**:
- Use SMS as escalation, not primary notification
- Send SMS only if Slack/Email not acknowledged
- Limit to production/critical monitors only

**Cost Management**:
- SMS notifications incur costs per message
- Review SMS usage in billing dashboard
- Optimize alert thresholds to reduce volume

### Alert Thresholds

Configure SMS to only fire after:
- 2+ consecutive failed checks
- 5+ minutes of downtime
- Multiple regions reporting DOWN
- Previous alerts not acknowledged

## PagerDuty Integration

*Available on Professional plans and above*

### Setup

1. In PagerDuty, create an integration:
   - Services → Your Service → Integrations → Add Integration
   - Select "Events API V2"
   - Copy the Integration Key
2. In StatusApp, create notification channel
3. Select **PagerDuty**
4. Enter Integration Key
5. Test the integration

### PagerDuty Features

**Incident Creation**:
- Automatic incident creation in PagerDuty
- Triggers on-call escalation policies
- Includes all monitor context

**Auto-Resolution**:
- Incidents automatically resolve when monitor recovers
- No manual cleanup required
- Complete incident lifecycle tracking

**Incident Details**:
- Monitor name and URL
- Status and duration
- Response time metrics
- Links back to StatusApp dashboard
- Region information

### Integration Benefits

- **On-Call Management**: Automatic escalation to on-call engineers
- **Incident Response**: Centralized incident management
- **Escalation Policies**: Automatic escalation if not acknowledged
- **Mobile Alerts**: Push notifications via PagerDuty mobile app
- **Incident Analytics**: Track MTTA and MTTR in PagerDuty

### Best Practices

- Create separate PagerDuty services for different teams/services
- Use PagerDuty schedules for on-call rotations
- Configure escalation policies for critical monitors
- Integrate with communication tools (Slack, Teams)
- Review PagerDuty incidents weekly for patterns

## Opsgenie Integration

*Available on Professional plans and above*

### Setup

1. In Opsgenie, create an API integration:
   - Settings → Integrations → Add Integration → API
   - Copy the API Key
2. In StatusApp, create notification channel
3. Select **Opsgenie**
4. Enter API Key
5. Configure priority mapping (optional)
6. Test the integration

### Opsgenie Features

**Alert Creation**:
- Creates alerts in Opsgenie with full context
- Includes monitor details and metrics
- Triggers notification rules

**Priority Mapping**:
- Map monitor severity to Opsgenie priorities (P1-P5)
- Critical monitors → P1
- Standard monitors → P3
- Informational → P5

**Auto-Resolution**:
- Alerts automatically close when monitors recover
- Updates alert status throughout incident lifecycle

### Alert Details Sent

- Monitor name and type
- Current status
- Response time
- Duration
- Error messages
- Links to StatusApp dashboard
- Tags for filtering

### Best Practices

- Configure Opsgenie schedules for on-call management
- Use alert policies for intelligent routing
- Set up escalation rules for unacknowledged alerts
- Use Opsgenie's mobile app for mobile alerts
- Tag alerts by team, environment, or service

## Microsoft Teams Integration

*Available on Professional plans and above*

### Setup

1. In Microsoft Teams channel:
   - Click ⋯ → Connectors → Incoming Webhook
   - Give it a name (e.g., "StatusApp Alerts")
   - Copy the webhook URL
2. In StatusApp, create notification channel
3. Select **Microsoft Teams**
4. Paste webhook URL
5. Test the integration

### Teams Message Features

**Adaptive Cards**:
- Professional, formatted messages
- Color-coded status indicators
- Monitor details and metrics
- Action buttons linking to dashboard
- Timeline updates in same card

**Message Content**:
- Monitor name and status
- Response time and uptime
- Incident duration
- Error details
- Quick action buttons

### Best Practices

- Create dedicated Teams channel for monitoring
- Use @mentions for critical alerts
- Test webhook before production deployment
- Keep webhook URLs secure
- Document webhook configurations

## Custom Webhooks

### When to Use Webhooks

- Integrate with custom internal systems
- Send alerts to unsupported platforms
- Trigger automation workflows
- Feed monitoring data into data warehouses
- Custom alert processing logic

### Configuration

1. Create notification channel
2. Select **Webhook**
3. Enter webhook URL (must be HTTPS)
4. Choose HTTP method (POST recommended)
5. Add custom headers if needed (e.g., Authorization)
6. Configure payload format
7. Test webhook

### Webhook Payload

StatusApp sends a JSON payload with complete alert information:

```json
{
  "event": "monitor.down",
  "timestamp": "2025-01-21T10:30:00Z",
  "monitor": {
    "id": "mon_abc123",
    "name": "Production API",
    "url": "https://api.example.com",
    "type": "API",
    "status": "DOWN"
  },
  "incident": {
    "id": "inc_xyz789",
    "startedAt": "2025-01-21T10:25:00Z",
    "duration": 300,
    "checksFailed": 3,
    "checksTotal": 3
  },
  "check": {
    "statusCode": 500,
    "responseTime": 1250,
    "error": "Internal Server Error",
    "region": "us-east-1"
  }
}
```

### Webhook Events

- `monitor.down` - Monitor status changed to DOWN
- `monitor.up` - Monitor recovered
- `monitor.degraded` - Monitor performance degraded
- `regression.detected` - Performance regression detected
- `regression.resolved` - Performance returned to normal
- `ssl.expiring` - SSL certificate expiring soon

### Security Best Practices

- Always use HTTPS URLs
- Implement webhook signature verification
- Use authentication headers (Bearer tokens, API keys)
- Validate payloads on receiving end
- Rate limit webhook endpoints
- Log webhook requests for debugging
- Rotate webhook URLs/keys regularly

### Testing Webhooks

StatusApp provides a "Test" button that:
- Sends a sample payload to your endpoint
- Displays the response status and body
- Shows any errors encountered
- Validates endpoint is reachable

## Assigning Channels to Monitors

### Via Monitor Creation/Edit

1. Create or edit a monitor
2. Navigate to **Notifications** section
3. Select notification channels from dropdown
4. Choose multiple channels if needed
5. Save monitor

### Bulk Assignment

1. Go to **Monitors** list
2. Select multiple monitors (checkboxes)
3. Click **Bulk Actions → Assign Notification Channels**
4. Choose channels to add/remove
5. Apply changes

### Channel Assignment Strategies

**By Environment**:
- Production monitors → Critical alerts channel (PagerDuty, SMS)
- Staging monitors → Team Slack channel (informational)
- Development monitors → Email only

**By Team**:
- Frontend monitors → Frontend team Slack
- Backend monitors → Backend team Discord
- Database monitors → DBA team PagerDuty

**By Severity**:
- Critical services → Multiple channels (Email + Slack + PagerDuty)
- Standard services → Email + Slack
- Non-critical → Email only

## Managing Notification Channels

### Editing Channels

1. Go to **Settings → Notification Channels**
2. Click on channel to edit
3. Update configuration
4. Test changes
5. Save

**Note**: Changes to channels immediately affect all monitors using that channel.

### Deleting Channels

1. Go to channel list
2. Click delete icon
3. Confirm deletion
4. Monitors using this channel will no longer send alerts via it

**Warning**: Check which monitors use a channel before deleting (shown in channel details).

### Testing Channels

Every channel has a **Test** button:
- Sends a sample alert
- Verifies configuration is correct
- Confirms connectivity
- Shows any errors

**Test Early and Often**:
- Test when creating new channels
- Test after editing channel configuration
- Test periodically to ensure continued functionality
- Test after changing service credentials

## Notification Events

### Monitor State Changes

**DOWN Event**:
- Triggered when monitor fails health check
- Sent after configured number of failed checks (usually 1-3)
- Includes error details and metrics

**UP Event**:
- Triggered when monitor recovers from DOWN state
- Includes downtime duration
- Shows recovery metrics

**DEGRADED Event**:
- Triggered when monitor is partially functional
- Example: High response times, packet loss, intermittent failures
- Alerts without creating full incident

### Performance Events

**Regression Detected**:
- Response time increased significantly (50%+ by default)
- Performance degraded compared to baseline
- Helps catch performance issues early

**Regression Resolved**:
- Performance returned to normal levels
- Automatically sent when regression clears
- Confirms issue is resolved

### Certificate Events

**SSL Expiring**:
- Triggered when SSL certificate approaching expiration
- Default: 30 days, 14 days, 7 days before expiration
- Customizable alert thresholds

### Incident Events

**Incident Created**:
- New incident opened
- Sent when multiple checks fail
- Includes incident details

**Incident Updated**:
- Status updates posted to incident
- New information added
- Severity changed

**Incident Resolved**:
- Incident marked as resolved
- Includes resolution time (MTTR)
- Includes incident summary

## Alert Thresholds and Filtering

### Confirmation Checks

Configure how many consecutive failures before alerting:

**Single Check** (sensitivity: high):
- Alert immediately on first failure
- Higher false positive rate
- Use for critical services only

**2-3 Checks** (sensitivity: medium):
- Alert after 2-3 consecutive failures
- Reduces false positives
- Standard for production services

**5+ Checks** (sensitivity: low):
- Alert after extended failure period
- Minimal false positives
- Use for non-critical or flaky services

### Multi-Region Thresholds

Only alert if multiple regions report issues:

- **1 region**: Possible region-specific problem or false positive
- **2+ regions**: Likely real outage
- **All regions**: Definite outage

Configure in monitor settings: "Alert if DOWN in X regions"

## Notification Best Practices

### 1. Layer Your Alerting

**Tier 1 - Informational**:
- Email notifications
- Slack channels (no mentions)
- Low-priority monitors

**Tier 2 - Action Required**:
- Slack with @mentions
- Telegram
- Standard production monitors

**Tier 3 - Critical/Urgent**:
- PagerDuty or Opsgenie
- SMS alerts
- Mission-critical services only

### 2. Reduce Alert Fatigue

- Don't send every alert to everyone
- Use appropriate check intervals
- Configure confirmation checks (2-3 failures before alerting)
- Set up maintenance windows for planned downtime
- Tune alert thresholds based on historical data

### 3. Test Everything

- Test channels when created
- Test after configuration changes
- Periodic testing (monthly)
- Include test runs in on-call rotation changes

### 4. Document Your Alerting

- Document which channels receive which alerts
- Maintain runbooks for common alerts
- Document escalation procedures
- Keep contact lists up to date

### 5. Review and Optimize

- Weekly: Review alert volume and actionability
- Monthly: Check for channels not being used
- Quarterly: Review and update notification strategies
- Annually: Audit all notification configurations

### 6. Redundancy is Important

- Use multiple channels for critical alerts
- Don't rely on single notification method
- Have backup contacts for all channels
- Test failover procedures

## Troubleshooting Notifications

### Not Receiving Notifications

**Check Channel Configuration**:
- Verify channel is active/enabled
- Test the channel
- Check credentials haven't expired
- Verify URLs/keys are correct

**Check Monitor Assignment**:
- Confirm monitor has channels assigned
- Verify monitor is active (not paused)
- Check if monitor is actually failing

**Check Service Status**:
- Slack workspace not blocking app
- Discord webhook not deleted
- Email not going to spam
- SMS number verified

### Delayed Notifications

**Possible Causes**:
- High alert volume causing queue backlog
- External service (Slack, Discord) experiencing delays
- Email delivery delays
- Network connectivity issues

**Solutions**:
- Check StatusApp status page
- Verify third-party service status
- Reduce alert frequency if overwhelming system
- Contact support if persistent

### Duplicate Notifications

**Causes**:
- Monitor assigned to same channel multiple times
- Multiple monitors for same service triggering
- Channel misconfiguration

**Fix**:
- Review monitor channel assignments
- Remove duplicate channel assignments
- Consolidate similar monitors

## Advanced Notification Scenarios

### On-Call Rotations

**Setup**:
1. Create PagerDuty or Opsgenie integration
2. Configure on-call schedules in those platforms
3. Assign critical monitors to on-call channel
4. Alerts automatically route to current on-call person

### Escalation Chains

**Scenario**: Alert not acknowledged in 5 minutes

**Solution**:
1. Primary: Slack notification to team channel
2. After 5 min: PagerDuty creates incident (alerts on-call)
3. After 15 min: SMS to backup on-call
4. After 30 min: Escalate to management

**Implementation**:
- Configure escalation in PagerDuty/Opsgenie
- Use their built-in escalation policies
- StatusApp creates initial alert, escalation platform handles rest

### Maintenance Windows

**During Planned Maintenance**:
1. Pause affected monitors
2. Notifications automatically stop
3. Resume monitors after maintenance
4. No false alerts during planned work

**Alternative**: Create maintenance window (if supported by your plan)

## Integration with Incident Management

### How Notifications Trigger Incidents

1. Monitor detects failure
2. StatusApp creates incident automatically
3. Notifications sent via configured channels
4. Incident tracked in Incidents dashboard
5. Updates sent to same channels as alerts progress
6. Resolution notification when incident resolves

### Incident Updates in Notifications

- Initial alert when incident created
- Updates when incident status changes
- Updates when comments/notes added
- Final notification when resolved
- Includes MTTR and incident summary

## Notification Analytics

### Viewing Notification History

1. Go to **Settings → Notification Channels**
2. Click on a channel
3. View **Recent Notifications** tab

See:
- Timestamp of each notification
- Which monitor triggered it
- Success/failure status
- Response time from service
- Error messages if failed

### Notification Metrics

Track:
- Total notifications sent
- Notifications per channel
- Delivery success rate
- Failed notification count
- Most frequently alerting monitors

Use this data to:
- Identify noisy monitors
- Optimize alert routing
- Find misconfigured channels
- Reduce alert fatigue

## Next Steps

- **[Incident Management](/articles/incidents/incident-management-basics)** - Learn how to manage incidents when they occur
- **[Understanding Analytics](/articles/analytics/understanding-analytics)** - Track notification effectiveness and incident response times
- **[Team Collaboration](/articles/team-collaboration/team-collaboration)** - Coordinate incident response across your team
- **[API Reference](/articles/api/api-reference-guide)** - Manage notifications programmatically via API