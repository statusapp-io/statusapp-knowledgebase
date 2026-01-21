---
title: PagerDuty Integration
slug: pagerduty-integration
category: Alerting & Notifications
excerpt: Integrate StatusApp with PagerDuty for sophisticated on-call management and incident escalation.
order: 10
---

# PagerDuty Integration

## Overview

PagerDuty integration enables StatusApp to create incidents in PagerDuty, automatically triggering on-call schedules and escalation policies. Perfect for teams that need sophisticated incident management and on-call coordination.

## Setup

### Prerequisites
- PagerDuty account (Professional plan or higher)
- StatusApp Professional plan or above

### Configuration Steps

1. In PagerDuty, go to your **Service**
2. Navigate to **Integrations** tab
3. Click **Add Integration**
4. Select **Events API V2**
5. Give it a name (e.g., "StatusApp")
6. Copy the **Integration Key**
7. In StatusApp, go to **Settings → Notification Channels**
8. Click **Create Channel**
9. Select **PagerDuty**
10. Paste the Integration Key
11. Click **Test** to verify
12. Click **Save**

## How It Works

1. **Monitor Fails**: StatusApp detects a problem
2. **Incident Created**: StatusApp automatically creates an incident in PagerDuty
3. **On-Call Notified**: PagerDuty notifies the on-call person based on your schedule
4. **Escalation**: If not acknowledged, PagerDuty escalates per your policy
5. **Resolution**: When monitor recovers, incident auto-resolves in PagerDuty

## Incident Details

Each PagerDuty incident includes:
- Monitor name and URL
- Current status
- Response time metrics
- Duration of outage
- Region where issue detected
- Links back to StatusApp dashboard
- Error messages and context

## PagerDuty Features

### Automatic Incident Creation
- Incidents created immediately when monitor fails
- Full context about the issue
- Accessible to entire on-call team

### Auto-Resolution
- Incidents automatically resolve when monitor recovers
- No manual cleanup needed
- Complete incident timeline tracked
- MTTR automatically calculated

### Escalation Policies
- Define escalation rules in PagerDuty
- 1st level: on-call primary
- 2nd level: on-call backup (after N minutes)
- 3rd level: manager (if still unacknowledged)
- Custom policies per service

### Mobile App Alerts
- Push notifications to PagerDuty mobile app
- Alerts even if phone asleep
- One-touch acknowledgment
- Resolve from app

### Incident Analytics
- Track MTTA (Mean Time To Acknowledge)
- Track MTTR (Mean Time To Resolve)
- Identify frequently escalated services
- Measure team response effectiveness

## On-Call Management

### Setting Up On-Call Schedules

In PagerDuty:
1. Create **On-Call Schedule**
2. Add team members with availability
3. Set rotation frequency (weekly, daily, etc.)
4. Define timezone for schedule
5. Assign schedule to escalation policy

### Escalation Policy

Create escalation rules:
1. **Level 1** (0 min): Notify on-call primary
2. **Level 2** (15 min): Escalate to backup if not acknowledged
3. **Level 3** (30 min): Escalate to manager if still not acknowledged

### Integrating with StatusApp

1. Create PagerDuty integration in StatusApp
2. Assign to critical/production monitors
3. When issue occurs, on-call is automatically notified
4. Incident resolved automatically when monitor recovers

## Assigning to Monitors

### Per-Monitor Assignment

1. Create or edit a monitor
2. Go to **Notifications** section
3. Select your PagerDuty channel
4. Can combine with Slack, email, etc.
5. Save the monitor

### Recommended Setup

**Tier 1 - Critical Production**:
- PagerDuty (immediate escalation)
- Slack with @oncall mention (awareness)
- SMS to primary on-call (backup alert)

**Tier 2 - Standard Production**:
- Slack with team mention (no escalation)
- Email to ops

**Tier 3 - Development/Staging**:
- Email only

## Best Practices

### Configure Wisely
- Assign PagerDuty only to truly critical monitors
- Use appropriate escalation timelines (15-30 min between levels)
- Don't create incidents for every alert (noisy services reduce effectiveness)
- Use PagerDuty thresholds and confirmation checks

### On-Call Rotation
- Regular handoff before end of shift
- Clear documentation about each service
- Runbook links in incident description
- Training for new on-call members

### Incident Response
- Acknowledge incidents quickly
- Post updates in PagerDuty to keep team informed
- Link related incidents
- Resolve in PagerDuty when truly fixed

### Review and Improve
- Weekly: Review PagerDuty incidents
- Monthly: Calculate MTTA and MTTR
- Quarterly: Review escalation policies
- Identify patterns in recurring incidents

## Troubleshooting

### Incidents Not Creating

**Check Configuration**:
- Verify Integration Key is correct
- Test the channel (button in channel settings)
- Confirm PagerDuty service exists

**Check Monitor**:
- Verify monitor has PagerDuty channel assigned
- Confirm monitor is actually failing
- Check alert confirmation thresholds

**Check PagerDuty Service**:
- Ensure service hasn't been deleted
- Confirm team has access to service
- Try adding integration to different service

### Incidents Not Resolving

**Possible Causes**:
- Monitor still in failed state
- Manual hold on incident in PagerDuty
- Auto-resolution disabled

**Solutions**:
- Verify monitor actually recovered
- Check if incident manually held
- Manually resolve in PagerDuty if needed

### Not Receiving Notifications

**Check PagerDuty**:
- Verify you're on-call in the schedule
- Check notification rules are enabled
- Confirm phone/email is correct
- Test PagerDuty notifications directly

**Check StatusApp**:
- Verify channel is assigned to monitor
- Test StatusApp channel
- Confirm monitor is in failed state

## Integration with Other Channels

PagerDuty + other channels for complete coverage:

**Slack + PagerDuty**:
- Slack notifies team for awareness
- PagerDuty alerts on-call engineer
- Slack thread for coordination

**Email + PagerDuty**:
- Email for detailed records
- PagerDuty for real-time escalation
- Good for audit trail

**SMS + PagerDuty**:
- PagerDuty primary alert
- SMS as backup if PagerDuty app not available

## Advanced Scenarios

### Multiple On-Call Teams

**Setup**:
1. Create separate PagerDuty services per team
2. Create separate StatusApp channels per service
3. Assign monitors to appropriate channel
4. Each team has own on-call schedule

### Service Dependencies

**Scenario**: One service depends on another

**Solution**:
- Monitor both services
- Same PagerDuty service for both
- Escalation handles dependency automatically

### After-Hours vs Business Hours

**Setup**:
- Primary on-call during hours
- Backup on-call after hours
- PagerDuty schedule handles both
- Automatic escalation per policy

## Next Steps

- Learn about [SMS notifications](/help/knowledge-base/sms-notifications) for backup alerts
- Explore [Slack notifications](/help/knowledge-base/slack-notifications) for team coordination
- Return to [Notification Channels Overview](/help/knowledge-base/notification-channels-overview)
