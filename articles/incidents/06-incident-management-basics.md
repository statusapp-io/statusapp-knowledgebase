---
title: Incident Management Basics
slug: incident-management-basics
category: Incidents
excerpt: Learn how to manage, track, and resolve service incidents in StatusApp.
order: 6
---

# Incident Management Basics

## What is an Incident?

An incident in StatusApp represents any event that affects service availability or performance. Incidents can be:

- **Automatic**: Created when a monitor detects an issue
- **Manual**: Created by team members for known issues
- **External**: Created via API or webhook

## Automatic Incident Creation

### How It Works

1. Monitor detects a problem (down, timeout, error)
2. Incident automatically created
3. Status page updated
4. Notifications sent
5. Team is alerted

### Configuration

1. Go to monitor settings
2. Enable **Auto-Create Incidents**
3. Set grace period (optional)
4. Choose incident template
5. Save settings

### Grace Period

Delay before creating an incident (prevents false positives):
- **30 seconds**: Quick detection
- **1-2 minutes**: Balanced
- **5 minutes**: For flaky services

## Creating Manual Incidents

### When to Create Manual Incidents

- Network outages affecting multiple services
- Third-party service issues
- Database maintenance
- Scheduled maintenance
- Known issues during investigation

### Creating an Incident

1. Go to **Incidents > Create Incident**
2. Enter incident title
3. Select affected components/monitors
4. Choose initial status:
   - **Investigating**: We're looking into it
   - **Identified**: Root cause found
   - **Monitoring**: Fix in progress
   - **Resolved**: Issue is fixed

5. Add detailed description
6. Set impact level (low, medium, high, critical)
7. Create incident

## Managing Active Incidents

### Incident Details

Each incident shows:
- Title and description
- Creation time and current duration
- Affected services/components
- Current status
- Assigned team members
- Recent updates
- Historical timeline

### Updating Incident Status

1. Click active incident
2. Click **Update**
3. Select new status:
   - **Investigating**: Still determining cause
   - **Identified**: Root cause understood
   - **Monitoring**: Fix deployed, monitoring recovery
   - **Resolved**: Issue fully resolved

4. Add update message
5. Notify subscribers (automatic)
6. Save update

### Adding Updates During Incident

Keep customers informed:

1. Click **Add Update**
2. Provide clear information:
   - What's happening
   - What we're doing about it
   - Expected resolution time
   - Impact on users

3. Notify subscribers
4. Pin important updates
5. Update status page

### Incident Assignment

1. Click on incident
2. Click **Assign To**
3. Select team member
4. Send notification
5. Track who's responsible

## Incident Severity Levels

### Critical
- Complete service outage
- Affects all users
- Requires immediate response
- All hands on deck

### High
- Significant functionality impaired
- Affects many users
- Urgent response needed
- Page on-call team

### Medium
- Some functionality affected
- Affects subset of users
- Should be addressed soon
- Normal alerting

### Low
- Minor issues
- Workarounds available
- Can be addressed in normal flow
- No urgent action required

## Incident Timeline

### Key Metrics

- **MTTD**: Mean Time To Detect (when you first notice)
- **MTTI**: Mean Time To Identify (root cause)
- **MTTR**: Mean Time To Resolve (time to fix)
- **Total Duration**: From start to full resolution

### Timeline View

1. Each incident has timeline
2. Shows all events in chronological order:
   - Status changes
   - Team updates
   - Customer notifications
   - Resolution actions
   - Post-incident notes

## Resolving Incidents

### Before Marking Resolved

1. Verify all affected services recovered
2. Confirm monitoring is normal
3. Run any post-incident tests
4. Brief the team
5. Plan post-mortem if needed

### Marking as Resolved

1. Click incident
2. Select status: **Resolved**
3. Add final update with summary
4. Include resolution details
5. Post-mortem scheduling (optional)
6. Notify all subscribers

### Post-Incident Analysis

1. Schedule post-mortem meeting
2. Invite relevant team members
3. Discuss what happened:
   - Detection time
   - Response time
   - Resolution process
   - Communication effectiveness

4. Identify improvements
5. Create action items
6. Set follow-up meetings

## Incident Communication

### External Communication

For public incidents:
1. Update status page immediately
2. Send email notification to subscribers
3. Post to Slack channels
4. Provide frequent updates (every 15-30 min)
5. Explain impact on customers
6. Give estimated resolution time

### Internal Communication

1. Alert incident commander
2. Page on-call team
3. Post in incident Slack channel
4. Update incident tracking
5. Regular status updates
6. Share findings/progress

### Customer Communication

- Be honest and transparent
- Provide timely updates
- Explain what you're doing
- Give realistic ETAs
- Apologize when appropriate
- Follow up after resolution

## Tracking Incident History

### Incident List

View all incidents:
1. Go to **Incidents**
2. Filter by:
   - Date range
   - Status
   - Severity
   - Affected service

3. Sort by:
   - Most recent
   - Duration
   - Severity

### Incident Search

- Find by title
- Search in descriptions
- Filter by assignee
- Filter by component
- Filter by status

### Incident Details

Each incident page shows:
- Full incident timeline
- All communications
- Resolution notes
- Related monitors
- Customer impact
- Post-mortem link

## Bulk Incident Management

### Bulk Actions

1. Select multiple incidents
2. **Bulk Update**: Change status for all
3. **Bulk Assign**: Assign to same person
4. **Bulk Archive**: Hide old incidents
5. **Bulk Delete**: Remove resolved incidents

### Incident Merging

Merge related incidents:
1. Select primary incident
2. Click **Merge**
3. Select incident to merge
4. Combine timeline and updates
5. Update affected components

## Incident Templates

### Creating Templates

Save time with incident templates:

1. Go to **Settings > Incident Templates**
2. Click **Create Template**
3. Set standard components
4. Add template description
5. Set default status
6. Save template

### Using Templates

1. Create new incident
2. Select template
3. Auto-populate standard fields
4. Customize as needed
5. Save incident

## Integration with Monitoring

### Automatic Linking

- Incidents automatically linked to triggering monitors
- See which monitors caused incident
- Track monitor health
- Historical correlation

### Monitor Status During Incident

- Incidents show monitor status
- Red highlighting for down monitors
- Green highlighting when recovered
- See status over time

## Best Practices

### Incident Response

1. **Respond Quickly**: First update within 5 minutes
2. **Be Transparent**: Share what you know
3. **Over-Communicate**: Update frequently
4. **Be Honest**: Explain limitations/unknowns
5. **Follow Up**: Post-mortem and prevention

### Documentation

1. Document all steps taken
2. Record timeline of events
3. Note root cause
4. List preventive measures
5. Archive for reference

### Learning

1. Hold blameless post-mortems
2. Identify systemic issues
3. Create action items
4. Track follow-ups
5. Share learnings

## Troubleshooting

### Incident Won't Resolve
- Check all affected monitors
- Verify manual recovery
- Review recent changes
- Restart monitors if needed

### Incidents Not Creating
- Verify auto-create is enabled
- Check monitor status
- Review monitor thresholds
- Check grace period

### Communication Not Sending
- Verify subscriber settings
- Check notification channels
- Review quiet hours
- Test manually

## Next Steps

- Learn about [status pages](/articles/status-pages/creating-status-pages)
- Set up [notifications](/articles/alerting-notifications/setting-up-notifications)
- Review [analytics](/articles/analytics/understanding-analytics)
- Explore [API](/articles/api/api-overview)