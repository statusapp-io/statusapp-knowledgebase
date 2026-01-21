---
title: SMS Notifications
slug: sms-notifications
category: Alerting & Notifications
excerpt: Send critical alerts via SMS text messages to on-call staff and team members.
order: 9
---

# SMS Notifications

## Overview

SMS notifications deliver urgent alerts directly to phones as text messages. Use SMS for critical, time-sensitive issues that require immediate action.

## Setting Up SMS

1. Go to **Settings → Notification Channels**
2. Click **Create Channel**
3. Select **SMS**
4. Enter a phone number with country code (e.g., +1-555-123-4567)
5. Verify the phone number (verification code sent via SMS)
6. Click **Save**

## Configuration

### Phone Numbers
- Format: `+[country-code][phone-number]`
- Examples:
  - US: `+1-555-123-4567`
  - UK: `+44-7700-900000`
  - Canada: `+1-289-123-4567`
  - International: `+[country-code]-[number]`

### Verification
- StatusApp sends a verification code to your number
- Enter the code to confirm ownership
- Required for security

## SMS Content

Messages are concise and include:
- Alert indicator: `[StatusApp]`
- Status: `CRITICAL:`, `WARNING:`, etc.
- Monitor name
- Duration if applicable
- Short dashboard link
- Character count optimized for readability

**Example**:
```
[StatusApp] CRITICAL: Production API is DOWN for 5min. 
Check: https://app.statusapp.io/m/abc123
```

## When to Use SMS

### Perfect For
- Production outages
- Revenue-impacting services
- Mission-critical systems
- On-call escalation
- Time-sensitive issues requiring immediate response

### Not Ideal For
- Non-critical monitors
- DEGRADED status alerts
- Informational messages
- Development environment alerts
- High-frequency changes

## Best Practices

### Use Sparingly
SMS is expensive and intrusive. Use strategically:
- Reserve SMS for critical monitors only
- Don't send SMS for every status change
- Use SMS as last-resort escalation, not primary notification
- Combine with email and Slack for layered alerting

### Configuration
- Set confirmation thresholds (2-3 failures before SMS)
- Use multi-region requirements (require 2+ regions down)
- Implement time-based delivery (e.g., only business hours for non-critical)
- Test thoroughly before production use

### Team Usage
- Use distribution lists or group numbers when available
- Coordinate with on-call team on expected frequency
- Update numbers when team members change
- Document which numbers receive what alerts

## Alert Thresholds

Configure SMS to only trigger after certain conditions:

**Confirmation Checks**:
- Alert after 1 failure (high sensitivity)
- Alert after 2-3 failures (balanced)
- Alert after 5+ failures (low sensitivity, fewer false alerts)

**Multi-Region Requirements**:
- Alert if 1 region fails (catch regional issues)
- Alert if 2+ regions fail (only real outages)
- Alert if all regions fail (definite outage)

**Time-Based Rules**:
- Business hours only
- During on-call shift only
- No weekend alerts for non-critical

## Assigning to Monitors

### Per-Monitor Assignment

1. Create or edit a monitor
2. Go to **Notifications** section
3. Select your SMS channel
4. Can combine with other channels (email, Slack, PagerDuty)
5. Save the monitor

## Cost Management

### Understanding Costs
- Each SMS incurs a per-message charge
- Costs vary by region and carrier
- Check your billing dashboard for usage

### Reducing Costs
- Limit SMS to truly critical monitors only
- Use higher confirmation thresholds
- Set multi-region requirements (don't alert on regional issues)
- Use quiet hours or time-based rules
- Consolidate alerts into fewer messages where possible

### Tracking Usage
- View SMS sent count in **Settings → Notification Channels**
- Check billing dashboard for cost breakdown
- Monitor alert frequency by monitor
- Identify and optimize noisy monitors

## Troubleshooting

### SMS Not Received

**Verify Configuration**:
- Check phone number format is correct
- Confirm verification was completed
- Send a test alert from channel settings
- Try a different phone number

**Carrier/Phone Issues**:
- Confirm phone is receiving text messages
- Check if number is in blocking list
- Verify carrier hasn't flagged messages as spam
- Try from different phone if possible

**StatusApp Issues**:
- Check SMS credits/balance
- Verify monitor is actually failing
- Check if monitor has SMS channel assigned
- Confirm alert threshold is being met

### Messages Arriving Late

**Possible Causes**:
- Carrier delays (usually within 5-10 seconds)
- High alert volume on platform
- Network issues

**Check**:
- Look at timestamps in StatusApp
- Confirm when message actually sent
- Check carrier status page
- Contact carrier support if persistent

### Multiple SMS from Same Alert

**Cause**: Channel configured multiple times on same monitor

**Fix**:
- Go to monitor settings
- Check Notifications section
- Remove duplicate SMS channel assignments

## Integration with Other Alerts

SMS works best combined with:
- **Slack**: Slack for team awareness + SMS for on-call individual
- **Email**: Email for detailed records + SMS for urgent escalation
- **PagerDuty**: PagerDuty for routing + SMS as backup alert

## On-Call Integration

### Setup for On-Call
1. Create one SMS channel per on-call person (or a shared number)
2. Assign critical monitors to on-call SMS
3. Update SMS number when on-call rotates
4. Test SMS during on-call handoff

### Escalation Pattern
- Tier 1: Slack notification (5 min)
- Tier 2: SMS to on-call (if Slack not acknowledged)
- Tier 3: Call to on-call (if SMS not acknowledged, requires PagerDuty)

## Next Steps

- Learn about [PagerDuty integration](/articles/alerting-notifications/pagerduty-integration) for sophisticated on-call escalation
- Explore [Slack notifications](/articles/alerting-notifications/slack-notifications) for team coordination
- Return to [Notification Channels Overview](/articles/alerting-notifications/notification-channels-guide)
