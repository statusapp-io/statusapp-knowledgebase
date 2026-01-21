---
title: Opsgenie Integration
slug: opsgenie-integration
category: Alerting & Notifications
excerpt: Connect StatusApp to Opsgenie for advanced on-call management and escalation.
order: 11
---

# Opsgenie Integration

## Overview

Opsgenie integration routes incidents to Opsgenie for team-based on-call management, escalation policies, and incident coordination.

## Setting Up Opsgenie

### Step 1: Get Opsgenie API Key

1. Log into **Opsgenie**
2. Go to **Settings** → **Team Settings** (if on team) or **Account Settings**
3. Click **API key management**
4. Create new API key:
   - Name: `StatusApp Integration`
   - Select team/account
   - Grant: Read + Create (minimum needed)
5. Copy API key

**Keep the key secret** - don't share or expose in logs.

### Step 2: Create Opsgenie Team (Optional)

If using Opsgenie for first time:

1. Create team in Opsgenie
2. Add team members
3. Set up escalation policies
4. Configure on-call schedules

StatusApp will create incidents in this team/account.

### Step 3: Add to StatusApp

1. Go to **Notifications** → **Add Channel**
2. Select **Opsgenie**
3. Enter **API Key**: Your Opsgenie API key
4. Select **Region**: US or EU (match your Opsgenie region)
5. Click **Test** to verify connection
6. Name: `Opsgenie Prod Team` (or your choice)
7. Click **Save**

## What Happens

### Incident Created in StatusApp
```
StatusApp Incident Down:
  ├─ Service: Payment API
  ├─ Time: 2:45 PM
  └─ Message: 503 Service Unavailable

StatusApp sends to Opsgenie:
  ├─ Creates Opsgenie alert
  ├─ Title: Payment API Down
  ├─ Severity: Critical (configurable)
  ├─ Routed to: On-call team
  ├─ Escalates: Per policy if not acknowledged
  └─ Notifications: Team gets alerted
```

### Incident Resolved in StatusApp
```
Service comes back online:
  ├─ StatusApp resolves incident
  ├─ Closes Opsgenie alert automatically
  └─ Team notified of recovery
```

### Incident Acknowledged in Opsgenie
```
Team acknowledges in Opsgenie:
  ├─ Shows as acknowledged in StatusApp
  ├─ Stops escalation in Opsgenie
  ├─ Team can update status in Opsgenie
  └─ StatusApp mirrors status
```

## Configuration

### Alert Severity

StatusApp incidents map to Opsgenie severity:

```
StatusApp → Opsgenie
Critical   → Critical (triggers paging)
High       → High (escalates quickly)
Medium     → Medium (acknowledged within hour)
Low        → Low (tracked but not urgent)
```

**Set severity in StatusApp monitor settings:**
```
Monitor: Payment Processing
Severity: Critical
Notification: Opsgenie
```

### Team & Escalation

Opsgenie handles escalation:

```
Incident Created
  └─ Alert sent to On-Call team
     ├─ 5 minutes: Team notified
     ├─ 15 minutes: Not acknowledged → Escalate
     ├─ 30 minutes: Escalate to manager
     ├─ 60 minutes: Escalate to director
     └─ Page sent based on policy
```

**Define escalation policies in Opsgenie**, not StatusApp.

### Responder Notifications

Opsgenie notifies responders through:
- Push notifications
- SMS
- Phone calls
- Slack
- Email

**Users configure preferences in Opsgenie app.**

## Common Patterns

### Production Incidents Only

Route production incidents to Opsgenie:

```
Monitor: Production API
├─ Severity: Critical
├─ Notifications: 
│  ├─ Opsgenie (automatic escalation)
│  ├─ Slack #production (awareness)
│  └─ Email (formal log)
└─ Response Time: SLA 15 minutes

Monitor: Staging API
├─ Severity: Medium
├─ Notifications:
│  ├─ Slack #engineering (awareness)
│  └─ Email (log only)
└─ No Opsgenie routing
```

### Escalation Policies by Severity

```
P1 (Critical - Revenue Impacting):
├─ Severity: Critical in Opsgenie
├─ Escalates: Immediately to on-call
├─ Response SLA: 5 minutes
└─ Notifications: Opsgenie + SMS + Call

P2 (High - Feature Broken):
├─ Severity: High in Opsgenie
├─ Escalates: After 15 minutes
├─ Response SLA: 30 minutes
└─ Notifications: Opsgenie + Slack

P3 (Medium - Degraded):
├─ Severity: Medium in Opsgenie
├─ Escalates: After 30 minutes
├─ Response SLA: 4 hours
└─ Notifications: Slack only

P4 (Low - Minor):
├─ No Opsgenie routing
├─ Slack only
└─ Log in dashboard
```

### Multi-Team Setup

Different teams get different incidents:

```
Payment Service Down:
└─ Opsgenie Alert
   ├─ Team: Payment On-Call
   ├─ Escalation: Payment team policy
   └─ Notify: Payment team members

API Service Down:
└─ Opsgenie Alert
   ├─ Team: Backend On-Call
   ├─ Escalation: Backend team policy
   └─ Notify: Backend team members
```

**In StatusApp:**
```
Each monitor assigned to:
- Different Opsgenie account/team
- Different notification channel per team
```

### War Room Escalation

For critical incidents:

```
5 minutes → Opsgenie alert to on-call
15 minutes → Not resolved → Page on-call manager
30 minutes → Not resolved → Page VP Engineering
60 minutes → Not resolved → War room initiated

Each stage triggers next escalation level
```

**Configure in Opsgenie escalation policies**, use same severity in StatusApp.

## Integration Details

### Incident Lifecycle

```
StatusApp                          Opsgenie
├─ Monitor detects failure    →    Creates alert
├─ Down 5 min                 →    Escalates if not acked
├─ Team acknowledges in OpsG  ←    Send ACK to StatusApp
├─ Issue gets fixed           →    StatusApp detects recovery
├─ Service back online        →    Close Opsgenie alert
└─ Report generated           ←    Opsgenie records incident
```

### Opsgenie Features Available

**Use in Opsgenie**:
- Assign to specific responder
- Add notes and context
- Start video conference
- Create follow-up tasks
- Track response time metrics
- Generate post-mortems

**StatusApp provides**:
- Automatic alert creation
- Monitoring data in alert
- Auto-resolution on recovery
- Audit trail integration

## Best Practices

### 1. Map Severity Correctly

```
Critical: Revenue loss, security breach, major outages
High: Important feature broken, degraded performance
Medium: Non-critical feature broken, minor issues
Low: Informational, scheduled maintenance

Wrong mapping = alert fatigue or missed incidents
```

### 2. Escalation Policies
Define in Opsgenie, not StatusApp:

```
✅ Do: Set escalation policy in Opsgenie
      - Clear expectations
      - Auto-escalates if not resolved
      - Team owns policies

❌ Don't: Create multiple notification channels
      - Complex and error-prone
      - Hard to maintain
      - Opsgenie policies are cleaner
```

### 3. Use Opsgenie, Not Just Alerts

Leverage Opsgenie features:

```
✅ Use Opsgenie:
   - Acknowledge incidents
   - Assign to specific person
   - Add notes and context
   - Start bridges/calls
   - Track MTTA/MTTR

❌ Just send alerts:
   - Defeats purpose of Opsgenie
   - Use email if just notifying
   - Opsgenie is for response management
```

### 4. Keep Team Information Current

```
1. Update on-call schedules in Opsgenie
2. Ensure escalation policies reflect current org
3. Remove inactive team members
4. Add new team members with proper rotations
5. Define backup escalation paths
```

### 5. Separate Environments

```
Production Opsgenie Account/Team:
├─ Production incidents only
├─ Critical severity
├─ Real on-call rotations
└─ Full escalation policies

Staging Opsgenie Account (Optional):
├─ Non-emergency testing
├─ Lower severity
└─ Limited escalation

Don't mix environments = don't wake on-call for staging issues
```

### 6. Test the Integration

```
1. Create test monitor in StatusApp
2. Send test failure
3. Verify Opsgenie receives alert
4. Check severity is correct
5. Acknowledge in Opsgenie
6. Verify StatusApp reflects acknowledgment
7. Resolve in StatusApp
8. Verify Opsgenie closes alert
```

## Troubleshooting

### Alert Not Creating in Opsgenie

**Verify API Key**:
```
1. Check API key in StatusApp matches Opsgenie
2. Ensure key has "Create alert" permission
3. Test API key from Opsgenie
4. If wrong, generate new key and update
```

**Check Team/Account**:
```
1. Verify team selected in Opsgenie matches API key
2. If team-level key, ensure targeting correct team
3. If account-level key, verify team exists
```

**Check StatusApp Settings**:
```
1. Verify region matches: US or EU
2. Test notification from StatusApp
3. Check notification channel is enabled
4. Verify monitor is assigned to Opsgenie channel
```

### Alerts Not Auto-Resolving

**Check Opsgenie Alert Close Policy**:
```
1. Go to Opsgenie settings
2. Check auto-close or manual close requirement
3. If manual: Team must close in Opsgenie
4. If auto: StatusApp should close automatically
```

**Verify StatusApp Monitoring**:
```
1. Confirm service is actually back online
2. Check StatusApp shows incident resolved
3. See if there's a delay in status update
4. Monitor logs for recovery detection
```

### Delayed Notifications

**Check Escalation Policy**:
```
Some policies have delays before escalation
Verify policy timing in Opsgenie

Check StatusApp to Opsgenie delay:
Typically < 10 seconds
If longer, check network connectivity
```

### Wrong Severity

**Adjust Monitor Settings**:
```
1. Go to monitor in StatusApp
2. Check severity setting
3. Update if needed
4. Verify new incidents use correct severity
```

## Opsgenie vs PagerDuty

| Feature | Opsgenie | PagerDuty |
|---------|----------|-----------|
| On-call Management | ✅ Yes | ✅ Yes |
| Escalation Policies | ✅ Advanced | ✅ Advanced |
| Incident Management | ✅ Full featured | ✅ Full featured |
| Team Collaboration | ✅ Good | ✅ Good |
| Price | $ | $$ |
| Best For | SMB, startups | Enterprise |

Both integrate well with StatusApp. Choose based on your team size and needs.

## Advanced: Custom Fields

**Coming Soon**: Add custom fields to Opsgenie alerts for:
- Service tier
- Owner team
- Runbook link
- Cost of outage
- Etc.

Currently passes: Service name, message, severity, link to incident.

## Next Steps

- **[PagerDuty](/articles/alerting-notifications/pagerduty-integration)** - PagerDuty alternative
- **[Notification Overview](/articles/alerting-notifications/notification-channels-overview)** - All channels
- **[Setting Up Notifications](/articles/alerting-notifications/notification-channels-guide)** - How to assign channels
