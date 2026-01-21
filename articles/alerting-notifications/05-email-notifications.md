---
title: Email Notifications
slug: email-notifications
category: Alerting & Notifications
excerpt: Configure email alerts for monitor status changes, incidents, and performance regressions.
order: 5
---

# Email Notifications

## Overview

Email notifications are the foundation of alerting in StatusApp. Emails are HTML-formatted and include all relevant context about monitor status changes and incidents.

## Setting Up Email Notifications

1. Go to **Settings → Notification Channels**
2. Click **Create Channel**
3. Select **Email**
4. Enter a channel name (e.g., "Ops Team Email", "Critical Alerts")
5. Enter recipient email addresses (comma-separated)
6. Click **Send Test** to verify
7. Click **Save**

## Configuration

### Channel Name
- Use descriptive names for easy identification
- Examples: "Production Alerts", "Team Email", "Critical Only"

### Recipient Emails
- Enter multiple addresses separated by commas
- Example: `ops@company.com, alerts@company.com, oncall@company.com`
- Supports distribution lists and group emails
- No limit on number of recipients

## Email Content

Each email includes:
- **Subject**: Clear indication of issue and monitor name
- **Status**: Current monitor status with color coding
- **Monitor Details**: Name, URL, type, and region
- **Timing**: When the issue started and current duration
- **Metrics**: Response time, error details
- **Action Links**: Direct links to the dashboard and monitor
- **Professional HTML Format**: Easy to read with styling

## What Triggers Emails

- Monitor status changes to DOWN
- Monitor status changes to UP (recovery)
- Monitor status changes to DEGRADED
- Performance regressions detected
- SSL certificate expiration warnings (15, 7, and 1 day before)
- Multi-region outages
- New incidents created
- Incident resolution

## Best Practices

### Organization

- **Create separate channels for severity levels**
  - "Critical Alerts" → Production monitors only
  - "Standard Alerts" → All production monitors
  - "Development" → Non-production environments

- **Use distribution lists**
  - `oncall@company.com` instead of individual emails
  - Easier to update without changing channel configuration
  - Scales as team grows

- **Document your setup**
  - Keep a list of what each email channel covers
  - Share with team in runbooks
  - Update as systems change

### Testing

- Always test when creating a new channel
- Re-test after configuration changes
- Include email tests in on-call rotation changes
- Check spam/junk folders if emails aren't arriving

### Reducing Noise

- Assign only truly important monitors to each email channel
- Use multiple channels for different severity levels
- Configure multi-region thresholds (alert only if 2+ regions fail)
- Set confirmation checks (2-3 failures before alerting)

## Assigning to Monitors

### Per-Monitor Assignment

1. Create or edit a monitor
2. Go to **Notifications** section
3. Select your email channel
4. Can choose multiple channels
5. Save the monitor

### Bulk Assignment

1. Go to **Monitors** list
2. Select multiple monitors with checkboxes
3. Click **Bulk Actions → Assign Notification Channels**
4. Choose your email channel
5. Apply changes

## Troubleshooting

### Not Receiving Emails

**Check Channel Configuration**:
- Verify recipient email addresses are correct
- Test the channel (button in channel settings)
- Confirm channel is assigned to the monitor

**Check Email Delivery**:
- Look in spam/junk folder
- Allowlist emails from `mail.statusapp.io`
- Check email for any bounce/delivery failure notices
- Ask a colleague if they received it

**Check Monitor Status**:
- Confirm the monitor is actually failing
- Verify the monitor is active (not paused)
- Check if it meets confirmation check thresholds

### Too Many Emails

**Reduce Frequency**:
- Increase confirmation check threshold (2-3 failures before alert)
- Consolidate related monitors into single channel
- Use separate channel for non-critical monitors
- Set multi-region threshold (require 2+ regions down)

### Emails Delayed

**Causes**:
- High alert volume
- Email provider delays
- Network issues

**Solutions**:
- Check StatusApp status page
- Contact your email provider
- Check if other alerts are arriving

## Integration with Other Alerts

Email works well combined with:
- **Slack**: Quick notifications in chat + detailed emails for records
- **SMS**: Email for full context + SMS for on-call urgency
- **PagerDuty**: Email for team awareness + PagerDuty for escalation

## Next Steps

- Learn about [Slack notifications](/articles/alerting-notifications/slack-notifications)
- Set up [SMS alerts](/articles/alerting-notifications/sms-notifications) for critical monitors
- Explore [PagerDuty integration](/articles/alerting-notifications/pagerduty-integration) for on-call management
- Return to [Notification Channels Overview](/articles/alerting-notifications/notification-channels-guide)
