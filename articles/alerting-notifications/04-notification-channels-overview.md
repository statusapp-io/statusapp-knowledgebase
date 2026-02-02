---
title: Notification Channels Overview
slug: notification-channels-overview
category: Alerting & Notifications
excerpt: Route alerts to the right people via email, Slack, SMS, PagerDuty, and more.
order: 4
---

# Notification Channels Overview

## How It Works

StatusApp's notification system separates **where alerts are sent** from **which monitors trigger them**. This lets you:

- Create reusable notification configurations
- Route different monitors to different destinations
- Use multiple channels simultaneously
- Manage alerts from a central location

**Process**:
1. Create notification channels (Email, Slack, SMS, etc.)
2. Assign channels to monitors
3. Receive alerts when monitors detect issues

## Available Channels

### Communication Channels

- **[Email Notifications](/help/knowledge-base/email-notifications)** - HTML-formatted emails with full context
- **[Slack](/help/knowledge-base/slack-notifications)** - Real-time alerts in Slack with threading
- **[Discord](/help/knowledge-base/discord-notifications)** - Rich embeds in Discord channels
- **[Telegram](/help/knowledge-base/telegram-notifications)** - Mobile-first messaging

### Escalation & Incident Management

- **[PagerDuty](/help/knowledge-base/pagerduty-integration)** - On-call management and escalation (Pro+)
- **[Opsgenie](/help/knowledge-base/opsgenie-integration)** - Alert management with routing (Pro+)
- **[Microsoft Teams](/help/knowledge-base/teams-notifications)** - Teams adaptive cards (Pro+)

### Direct Alerts

- **[SMS](/help/knowledge-base/sms-notifications)** - Text message alerts to phones

### Custom Integration

- **[Webhooks](/help/knowledge-base/webhook-notifications)** - Custom HTTP endpoints for any integration

## Creating a Channel

1. Go to **Settings → Notification Channels**
2. Click **Create Channel**
3. Select channel type
4. Enter configuration (email, webhook URL, etc.)
5. Click **Test** to verify setup
6. Click **Save**

## Assigning Channels to Monitors

### Add to New Monitor

1. Create a monitor
2. Go to **Notifications** section
3. Select channels from dropdown
4. Save monitor

### Add to Existing Monitor

1. Edit the monitor
2. Go to **Notifications** section
3. Select or change channels
4. Save changes

### Bulk Assignment

1. Go to **Monitors** list
2. Select multiple monitors (checkboxes)
3. Click **Bulk Actions → Assign Notification Channels**
4. Choose channels to add/remove
5. Apply changes

## Recommended Alerting Layers

Build a layered approach based on severity:

### Tier 1 - Informational

**Purpose**: Awareness, not immediate action

**Channels**:
- Email (daily digest)
- Slack (general channel, no mentions)

**Monitors**: Non-critical services, monitoring-as-record

### Tier 2 - Action Required

**Purpose**: Team should investigate

**Channels**:
- Slack (team channel with mentions)
- Email (immediate)

**Monitors**: Standard production services

### Tier 3 - Critical/Urgent

**Purpose**: Immediate escalation to on-call

**Channels**:
- PagerDuty or Opsgenie (automatic escalation)
- Slack (critical channel with mentions)
- SMS (on-call backup)

**Monitors**: Mission-critical services, revenue-impacting

## Alert Thresholds

Configure when alerts trigger:

### Confirmation Checks

How many consecutive failures before alerting:
- **1 check**: Immediate alert (high sensitivity)
- **2-3 checks**: Balanced approach (standard)
- **5+ checks**: Low sensitivity (fewer false positives)

### Multi-Region Requirements

Only alert if failures occur in multiple regions:
- **1 region**: Catch regional issues
- **2+ regions**: Only real outages (fewer false alerts)

### Event Types

Configure which events trigger alerts:
- Monitor DOWN (status change to failed)
- Monitor UP (recovery)
- Monitor DEGRADED (partial failure)
- SSL expiration warnings
- Performance regressions

## Managing Channels

### Edit Channel

1. Go to **Settings → Notification Channels**
2. Click channel to edit
3. Update configuration
4. Click **Test** to verify
5. Click **Save**

**Note**: Changes immediately affect all monitors using this channel

### Delete Channel

1. Go to channel list
2. Click delete icon
3. Confirm deletion
4. Monitors no longer receive alerts via this channel

**Tip**: Check which monitors use a channel before deleting (shown in channel details)

### Test Channel

Every channel has a **Test** button:
- Sends sample alert
- Verifies configuration is correct
- Confirms connectivity
- Shows any errors

**Test Often**:
- When creating new channels
- After configuration changes
- Monthly for validation
- Before on-call rotations

## Notification Best Practices

### 1. Layer Your Alerts

Don't send everything to everyone. Use different channels for different purposes:
- Email for records
- Slack for team awareness
- PagerDuty for urgent escalation
- SMS for on-call backup

### 2. Reduce Alert Fatigue

- Don't assign low-priority monitors to critical channels
- Configure confirmation checks (2-3 failures before alert)
- Use multi-region thresholds
- Archive/remove old monitors
- Pause noisy monitors and investigate

### 3. Test Everything

- Test when channels are created
- Test after configuration changes
- Monthly validation tests
- Include tests in on-call rotation changes

### 4. Document Your Setup

Keep a reference of:
- What each channel covers
- Who receives each type of alert
- Escalation procedures
- Current on-call contacts
- Emergency runbooks

### 5. Maintain Your Channels

- Monthly: Review alert volume and usefulness
- Quarterly: Update recipient lists and routing
- Quarterly: Review and optimize thresholds
- Annually: Full audit of all channels

### 6. Redundancy for Critical Services

- Multiple channels for mission-critical monitors
- Don't rely on single notification method
- Have backup contacts for all channels
- Test failover procedures

## Troubleshooting

### Not Receiving Alerts

**Check Channel Configuration**:
- Verify channel is active
- Test the channel
- Confirm credentials (API keys, phone numbers) are valid
- Check if external service (Slack, email) has issues

**Check Monitor Assignment**:
- Confirm monitor has channel assigned
- Verify monitor is active (not paused)
- Check if monitor is actually failing

**Check Event Type**:
- Verify alert type is enabled (DOWN, UP, DEGRADED, etc.)
- Check confirmation thresholds are being met
- Review alert filters if configured

### Duplicate Alerts

**Cause**: Monitor assigned to same channel multiple times

**Fix**: Review monitor settings and remove duplicate channel assignments

### Too Many Alerts

**Solutions**:
- Increase confirmation thresholds (2-3 before alert)
- Require multi-region failures
- Consolidate related monitors
- Create separate lower-priority channel

### Delayed Alerts

**Check**:
- StatusApp status page
- External service status (Slack, email, etc.)
- Network connectivity
- Alert volume and queue

## Channel Types by Plan

| Channel | Starter | Professional | Business | Enterprise |
|---------|---------|--------------|----------|-----------|
| Email | ✓ | ✓ | ✓ | ✓ |
| Slack | Plan-based | ✓ | ✓ | ✓ |
| Discord | Plan-based | ✓ | ✓ | ✓ |
| Telegram | Plan-based | ✓ | ✓ | ✓ |
| SMS | ✗ | ✓ | ✓ | ✓ |
| PagerDuty | ✗ | ✓ | ✓ | ✓ |
| OpsGenie | ✗ | ✓ | ✓ | ✓ |
| Microsoft Teams | Plan-based | ✓ | ✓ | ✓ |
| Webhooks | Plan-based | ✓ | ✓ | ✓ |

**Note**: "Plan-based" means the feature availability depends on your specific plan configuration. Check **Settings > Plan** for your enabled features.

## Next Steps

- **[Email Notifications](/help/knowledge-base/email-notifications)** - Set up email alerts
- **[Slack Integration](/help/knowledge-base/slack-notifications)** - Connect Slack
- **[PagerDuty Integration](/help/knowledge-base/pagerduty-integration)** - On-call management
- **[Incident Management](/help/knowledge-base/incident-lifecycle)** - Handle incidents when they occur
- **[Team Collaboration](/help/knowledge-base/team-collaboration)** - Coordinate with your team
