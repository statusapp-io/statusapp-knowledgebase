---
title: Slack Notifications
slug: slack-notifications
category: Alerting & Notifications
excerpt: Integrate StatusApp with Slack for real-time alert threads and team coordination.
order: 6
---

# Slack Notifications

## Overview

Slack integration enables real-time monitoring alerts in your Slack workspace. Alerts appear as rich, color-coded messages with full incident context, all organized in threads for easy tracking.

## Setting Up Slack Integration

1. Go to **Settings → Notification Channels**
2. Click **Create Channel**
3. Select **Slack**
4. Click **Connect to Slack**
5. Authorize StatusApp in your Slack workspace
6. Select the Slack channel where alerts should be posted
7. Click **Save**

## Message Features

### Color Coding
- 🔴 **Red** - Monitor DOWN or critical issue
- 🟢 **Green** - Monitor recovered (UP)
- 🟠 **Orange** - Monitor DEGRADED
- 🟣 **Purple** - Performance regression
- 🔵 **Blue** - Regression resolved

### Message Content
- Monitor name and status
- Response time metrics
- Incident duration
- Error messages
- Direct links to dashboard
- Action buttons

### Threading
- Initial alert creates a thread
- All subsequent updates posted as replies
- Keeps channel clean and organized
- Easy to follow incident progression from start to resolution

## Channel Setup

### Create Dedicated Channels

Rather than mixing alerts with team chat, create dedicated monitoring channels:

**Examples**:
- `#monitoring` - General monitor status
- `#production-incidents` - Production outages only
- `#staging-alerts` - Staging environment
- `#performance` - Performance degradation alerts
- `#ssl-certificates` - Certificate expiration warnings

### Channel Routing Strategy

**By Environment**:
- Production monitors → `#production-incidents`
- Staging monitors → `#staging-monitoring`
- Development monitors → `#dev-monitoring`

**By Severity**:
- Critical services → `#critical-alerts` (with @oncall mentions)
- Standard services → `#alerts` (no mentions)
- Informational → `#monitoring-info` (no urgent messages)

**By Team**:
- Frontend monitors → `#frontend-alerts`
- Backend monitors → `#backend-alerts`
- Database monitors → `#database-alerts`

## Mentions and Escalation

### Configuring Mentions

When creating or editing a Slack channel, you can configure automatic mentions:

- **@channel** - Mentions all channel members
- **@here** - Mentions only active/online members
- **@oncall** - Custom group or user name
- **Specific users** - Direct mentions (if enabled)

### When to Use Mentions

**Critical/Production Issues**:
- DOWN status
- Security-related issues
- Revenue-impacting services
- Use mentions sparingly

**Non-Urgent Alerts**:
- DEGRADED status
- Non-critical services
- Performance regressions
- No mentions needed

## Assigning to Monitors

### Per-Monitor Assignment

1. Create or edit a monitor
2. Go to **Notifications** section
3. Select your Slack channel
4. Can choose multiple channels
5. Save the monitor

### Bulk Assignment

1. Go to **Monitors** list
2. Select multiple monitors
3. Click **Bulk Actions → Assign Notification Channels**
4. Choose your Slack channel
5. Apply changes

## Best Practices

### Organization
- Create dedicated channels for monitoring (don't mix with team chat)
- Name channels clearly (`#prod-alerts`, not `#random`)
- Use channel topics/descriptions to document what they monitor
- Pin a message explaining alert meanings

### Handling Alerts
- Use threads for incident discussion
- Don't clutter main channel with side conversations
- Resolve thread when incident resolves
- React with emoji (✓, ⚠️) instead of commenting "got it"

### Testing
- Test the integration when first setting up
- Test after configuration changes
- Periodically verify the Slack app hasn't been removed
- Include Slack in on-call rotation tests

### Scalability
- Start with one alert channel, split as volume grows
- Use filters/rules to reduce noise
- Archive old channels when services sunset
- Document what each channel monitors

## Troubleshooting

### Not Receiving Alerts

**Check Slack Authorization**:
- Verify Slack workspace is authorized
- Check if StatusApp app was removed from workspace
- Reconnect the integration
- Send a test alert

**Check Channel Permissions**:
- Confirm Slack channel still exists
- Verify StatusApp bot has access to channel
- Check if channel is archived
- Try assigning to different channel

**Check Monitor Assignment**:
- Verify monitor has Slack channel assigned
- Confirm monitor is active
- Check if monitor is actually failing

### Alerts Not Appearing in Threads

**Causes**:
- Thread might have expired (Slack keeps recent messages)
- Initial message was deleted
- Channel settings preventing threads

**Solutions**:
- New incidents will create new threads
- Don't delete initial alert messages
- Check Slack workspace settings

### Duplicate Alerts

**Causes**:
- Monitor assigned to same channel multiple times
- Multiple monitors for same service

**Fix**:
- Review monitor channel assignments
- Remove duplicate assignments
- Consolidate similar monitors

### Slow/Delayed Messages

**Causes**:
- Slack workspace outage
- High alert volume
- Network issues

**Solutions**:
- Check Slack status page
- Check StatusApp status page
- Reduce alert frequency if overwhelming
- Contact support if persistent

## Integration with Other Notifications

Slack works well combined with:
- **Email**: Slack for urgency + email for detailed records
- **SMS**: Slack for awareness + SMS for on-call critical alerts
- **PagerDuty**: Slack for team notifications + PagerDuty for escalation

## Next Steps

- Learn about [Discord notifications](/help/knowledge-base/discord-notifications)
- Set up [SMS alerts](/help/knowledge-base/sms-notifications) for critical escalation
- Explore [PagerDuty integration](/help/knowledge-base/pagerduty-integration) for on-call
- Return to [Notification Channels Overview](/help/knowledge-base/notification-channels-overview)
