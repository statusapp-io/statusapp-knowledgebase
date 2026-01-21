---
title: Incident Management Basics
slug: incident-management-basics
category: Incidents
excerpt: Learn how to manage, track, and resolve service incidents in StatusApp.
order: 6
---

# Incident Management Basics

## What is an Incident?

An incident is any event that disrupts or degrades your service availability or performance. In StatusApp, incidents provide a complete timeline of issues from detection through resolution, helping you communicate with customers and learn from failures.

**Incidents Track**:
- When the issue started and how long it lasted
- Which monitors/services were affected
- All status updates posted during the incident
- Root cause analysis and prevention steps
- Communication sent to subscribers
- Incident severity and impact
- Mean Time To Resolution (MTTR)

## How Incidents Are Created

### Automatic Incident Creation

StatusApp automatically creates incidents when monitors fail:

**Trigger Conditions**:
1. Monitor check fails (returns non-200 status, timeout, or error)
2. Multiple consecutive checks fail (confirmation, not a fluke)
3. Incident created with unique incident number (e.g., `INC-00123`)
4. Status page updated in real-time
5. Subscribers notified via email

**Automatic Incident Data**:
- Incident number (unique identifier)
- Start time (when first failure detected)
- Affected monitor (which service is down)
- Severity (automatically determined based on history)
- Affected regions (which monitoring regions saw failures)
- Check failure details (status codes, errors, response times)

**Grace Period**

Configure monitors to wait before creating incidents:
- Prevents false positives from temporary network blips
- Common values: 1-5 minutes
- Set in monitor configuration
- Balances speed vs accuracy

## Automatic Incident Creation

### Manual Incident Creation

Create incidents manually for issues not automatically detected:

**When to Create Manually**:
- Planned maintenance windows
- Known issues affecting multiple services
- Third-party service outages
- Issues detected by users before monitoring
- Performance degradation not triggering thresholds

**Creating a Manual Incident**:

1. Navigate to **Incidents** page
2. Click **Create Incident**
3. Fill in incident details:
   - **Affected Monitor**: Select the impacted service
   - **Initial Status**: Usually "Investigating"
   - **Severity**: Critical, High, Medium, or Low
   - **Mark as Public**: Show on status page
   - **Initial Message**: What's happening (optional)
4. Click **Create Incident**
5. Incident appears on status page
6. Subscribers notified automatically

## Incident Severity Levels

### Critical

**Definition**: Complete service outage affecting all or most users

**Characteristics**:
- Core functionality completely unavailable
- Revenue-impacting
- Affects majority of customers
- Requires immediate all-hands response
- Page on-call team immediately

**Examples**:
- Website completely down
- API returning 500 errors for all requests
- Database offline
- Authentication system failure

### High

**Definition**: Significant functionality impaired, many users affected

**Characteristics**:
- Major feature unavailable
- Performance severely degraded
- Subset of users cannot access service
- Workarounds may exist but difficult
- Urgent response required

**Examples**:
- Payment processing down
- Search functionality broken
- Slow API response times (> 5s)
- Email delivery failures

### Medium

**Definition**: Some functionality affected, limited user impact

**Characteristics**:
- Minor feature unavailable
- Performance slightly degraded
- Small percentage of users affected
- Workarounds readily available
- Standard response process

**Examples**:
- Image uploads slow
- Report generation delayed
- Non-critical API endpoint down
- Email notifications delayed

### Low

**Definition**: Minor issues with minimal impact

**Characteristics**:
- Cosmetic or UI issues
- Non-critical functionality
- Easy workarounds exist
- Very few users affected
- Can be addressed during business hours

**Examples**:
- Typo in UI text
- Minor CSS styling issue
- Non-critical widget not loading
- Admin panel feature not working

## Incident Status Lifecycle

Incidents progress through these statuses:

### 1. Open (Initial State)

- Incident just detected or created
- Team notified
- Investigation beginning
- First status shown on status page

**Actions**: Review incident details, assign team member, begin investigation

### 2. Investigating

- Team actively investigating the issue
- Root cause not yet identified
- Gathering logs and metrics
- Testing hypotheses

**Actions**: Post investigation updates every 15-30 minutes, communicate findings

### 3. Identified

- Root cause has been identified
- Solution/fix determined
- Working on implementing fix
- ETA may be available

**Actions**: Share root cause, explain fix plan, provide timeline

### 4. Monitoring

- Fix has been deployed
- Monitoring for stability
- Waiting to confirm issue resolved
- Not yet fully confident

**Actions**: Watch metrics closely, update on progress, be ready to roll back

### 5. Resolved

- Issue completely fixed
- Service fully operational
- Confidence in stability
- Incident closed

**Actions**: Post resolution summary, conduct post-mortem, update prevention steps

## Managing Active Incidents

### Viewing Incident Details

Click on any incident to see full details:

**Incident Header**:
- Incident number (e.g., `INC-00123`)
- Current status badge
- Severity indicator
- Started time and duration
- Assigned team member (if any)

**Key Metrics**:
- **Duration**: How long incident has been active
- **MTTR**: Mean Time To Resolution (when resolved)
- **Checks Failed**: Number of failed monitoring checks
- **Checks Total**: Total checks during incident
- **Impact**: Percentage of checks that failed

**Affected Information**:
- Monitor name and URL
- Affected regions (which locations saw failures)
- Status updates timeline
- Check logs with detailed error information

**Root Cause Analysis** (when identified):
- Root cause category
- Detailed description
- Prevention steps
- Who identified it and when

### Posting Status Updates

Keep customers informed with regular updates:

**Creating an Update**:

1. Open the incident
2. Click **Post Update**
3. Select update type:
   - **Investigating**: We're looking into the issue
   - **Identified**: We've found the cause
   - **Monitoring**: Fix deployed, watching for stability
   - **Update**: General progress update
   - **Resolved**: Issue is fixed
4. Add title (brief summary)
5. Write detailed message (supports markdown)
6. Mark as **Public** to show on status page
7. Click **Post Update**

**Update automatically**:
- Appears in incident timeline
- Updates status page in real-time
- Notifies all subscribers via email
- Updates incident status (if Investigating/Resolved type)
- Timestamps with your identity

**Best Practices for Updates**:
- Update every 15-30 minutes during active incidents
- Be specific about what you're doing
- Include ETAs when possible ("investigating, update in 15 min")
- Use plain language, avoid jargon
- Admit what you don't know
- Update when status changes

### AI-Suggested Updates

StatusApp can suggest update messages based on context:

1. Click **Post Update**
2. Click **Get Suggestions**
3. Review AI-generated update suggestions
4. Select one or edit to customize
5. Post the update

Suggestions consider:
- Incident duration
- Severity level
- Previous updates
- Monitor type and error details
- Best practices for incident communication

### Identifying Root Cause

Document what caused the incident:

**Adding Root Cause**:

1. Open resolved or active incident
2. Click **Identify Root Cause**
3. Select root cause category:
   - DNS Failure
   - Network Timeout
   - Server Error (5xx)
   - Application Error
   - Database Issue
   - SSL Certificate
   - Configuration Error
   - Deployment Issue
   - Third-Party Service
   - Infrastructure
   - DDoS Attack
   - Maintenance
   - Unknown
4. Write detailed description (what specifically went wrong)
5. Add prevention steps (how to avoid in future)
6. Click **Save Root Cause**

**Root Cause appears**:
- In incident details
- On public incident page (if incident is public)
- In incident analytics
- In post-mortem reports

**AI Root Cause Suggestions**:

Click **Get Suggestions** to see AI-analyzed likely causes based on:
- Error patterns in check logs
- Similar past incidents
- Common failure modes for monitor type
- Status codes and error messages

### Resolving Incidents

**Automatic Resolution**:

Incidents auto-resolve when:
- Monitor returns to operational state
- Multiple consecutive successful checks
- Confirms issue is truly fixed

**Manual Resolution**:

1. Post a **Resolved** status update
2. Incident automatically marked as resolved
3. Resolution time recorded
4. MTTR calculated
5. Subscribers notified
6. Status page updated

**Before Marking Resolved**:
- Verify all symptoms have cleared
- Confirm monitoring shows green
- Check affected regions all recovered
- Test critical user flows
- Give it a few minutes to be confident

**Resolution Summary Should Include**:
- What was fixed
- Why it happened
- How long users were affected
- Any remaining follow-up needed

### Incident Assignment

Assign incidents to team members for ownership:

1. Open incident
2. Look for **Assigned To** field
3. Click to assign
4. Select team member
5. They receive notification
6. Shows who's responsible on incident page

Benefits:
- Clear ownership
- Avoid duplicate work
- Track response accountability
- Show customers someone's on it

## Incident Timeline

Each incident has a detailed timeline showing everything that happened:

**Timeline Includes**:

**Status Updates**:
- All posted updates with timestamps
- Who posted each update
- Public vs internal updates
- Full message content

**Check Logs**:
- All monitoring checks during incident
- Success vs failure status
- Status codes and error messages
- Response times per check
- Which regions failed
- Timestamps for each check

**Key Events**:
- Incident created
- Status changes
- Assignment changes
- Root cause identified
- Incident resolved

**Timeline Filters**:

Filter timeline to focus on specific events:
- **All Events**: Everything chronologically
- **Status Updates Only**: Just team communications
- **Failed Checks Only**: Only failures
- **Successful Checks Only**: Only passes

**Timeline Sorting**:
- Newest first (default) - most recent at top
- Oldest first - chronological from start

**Pagination**:
- 50 events per page
- Load more to see older events
- Quick navigation to any page

## Incident Metrics & Analytics

### Key Performance Indicators

**MTTR (Mean Time To Resolution)**:
- Average time from incident start to resolution
- Calculated across all resolved incidents
- Lower is better
- Track improvements over time

**Incident Frequency**:
- Number of incidents per day/week/month
- Trend analysis
- Identify problem patterns

**Severity Distribution**:
- How many critical vs low severity incidents
- Are critical incidents decreasing?

**Top Failing Monitors**:
- Which services have most incidents
- Focus improvement efforts

**Common Root Causes**:
- What causes incidents most frequently
- Systemic issues to address

### Viewing Analytics

1. Navigate to **Incidents** page
2. View dashboard showing:
   - Total incidents
   - Active incidents
   - Average MTTR
   - Incident frequency chart
   - Severity breakdown
   - Top root causes
   - Most affected monitors
3. Filter by date range
4. Export data for further analysis

## Public Incident Pages

Make incidents visible to customers:

### Enabling Public Visibility

**For Individual Incidents**:
1. Open incident
2. Check "Mark as Public" when creating
3. Or edit existing incident to make public
4. Incident appears on status page

**Public incidents show**:
- Incident number and start time
- Current status
- Affected service name
- All public status updates
- Resolution summary (when resolved)
- Root cause (if shared publicly)

**Status page integration**:
- Active incidents section shows public incidents
- Real-time updates as you post them
- Incident history for transparency
- Shareable incident URLs

### Incident Communication

**Email Notifications**:

Subscribers automatically receive emails when:
- New incident created
- Status update posted (public updates only)
- Incident resolved

Email includes:
- What's affected
- Current status
- Latest update message
- Link to full incident page
- Unsubscribe option

**Status Page Updates**:

Status page automatically reflects:
- Monitor status (UP/DOWN/DEGRADED)
- Active incidents section
- Latest update message
- Time since last update
- Resolution when fixed

## Incident History

### Viewing Past Incidents

1. Go to **Incidents** page
2. Switch to **Resolved** tab
3. Browse chronologically
4. Filter by:
   - Date range
   - Severity
   - Monitor
   - Root cause category
5. Search by incident number or text

### Learning from Incidents

**Post-Mortem Process**:

After major incidents (High or Critical severity):

1. **Document timeline**: What happened when
2. **Identify root cause**: What went wrong and why
3. **Document impact**: How many users affected, for how long
4. **List detection gaps**: How could we detect faster
5. **Prevention steps**: How to prevent recurrence
6. **Action items**: Concrete steps to improve
7. **Share learnings**: Team review, post to engineering blog

**Blameless Culture**:
- Focus on systems, not people
- Incidents are learning opportunities
- Encourage transparency
- Share mistakes openly
- Turn failures into improvements

### Incident Patterns

Look for patterns in incident history:

**Time-based patterns**:
- Do incidents cluster at certain times?
- Day of week trends
- Before/after deployments

**Service patterns**:
- Which monitors fail most often
- Recurring issues
- Correlated failures

**Cause patterns**:
- Most common root causes
- Preventable vs unpreventable
- External dependencies

## Best Practices

### Incident Response

**1. Respond Quickly**
- Post first update within 5-15 minutes
- Even if just "We're investigating"
- Speed builds trust

**2. Communicate Frequently**
- Update every 15-30 minutes during active incidents
- Even if no progress: "Still investigating, will update in 15 min"
- Never go silent

**3. Be Transparent**
- Share what you know and what you don't
- Admit mistakes honestly
- Explain impact clearly
- Give realistic ETAs

**4. Use Clear Language**
- Avoid technical jargon
- Explain in customer terms
- Be specific but understandable
- "Payment processing is down" not "Stripe webhook handler crashed"

**5. Own the Incident**
- Take responsibility
- Apologize when appropriate
- Focus on fixing, not excuses

### Documentation

**During Incident**:
- Document everything you try
- Record times of key events
- Save error messages and logs
- Note who did what

**After Resolution**:
- Complete root cause analysis
- Document prevention steps
- Create action items
- Update runbooks
- Share post-mortem

### Prevention

**Monitor Your Monitors**:
- Ensure monitors catch issues before users
- Test monitors regularly
- Adjust thresholds based on incidents

**Fix Root Causes**:
- Don't just patch symptoms
- Address underlying issues
- Track action item completion

**Learn and Improve**:
- Review incidents quarterly
- Identify systemic issues
- Invest in reliability
- Celebrate MTTR improvements

## Troubleshooting

### Incident Won't Resolve

**Problem**: Incident showing as active but service is actually up

**Solutions**:
1. Check if monitor is still failing
2. Hard refresh incident page
3. Manually post "Resolved" update
4. Verify monitor configuration is correct
5. Check if incident marked as false positive

### Notifications Not Sending

**Problem**: Subscribers not receiving incident emails

**Solutions**:
1. Verify subscribers verified their email addresses
2. Check status page has email subscriptions enabled
3. Verify update marked as "Public"
4. Check spam/junk folders
5. Test with personal email address first
6. Review email template configuration

### Timeline Not Loading

**Problem**: Incident timeline shows "Loading..." forever

**Solutions**:
1. Hard refresh page (Ctrl+F5 or Cmd+Shift+R)
2. Clear browser cache
3. Try different browser
4. Check browser console for errors
5. Contact support if persists

### Cannot Post Updates

**Problem**: "Post Update" button not working

**Solutions**:
1. Verify you're logged in
2. Check you have permissions (team member)
3. Fill in required fields (title and message)
4. Try refreshing page
5. Check browser console for errors

## Advanced Features

### Incident API Access

(Professional, Business, Enterprise plans)

Programmatically create and manage incidents:

```bash
# Get active incidents
GET /api/v1/incidents?status=active

# Get specific incident
GET /api/v1/incidents/{incidentId}

# Create incident (for integrations)
POST /api/v1/incidents
```

See [API Reference](/articles/api/api-reference-guide) for full documentation.

### Webhooks for Incidents

(Business & Enterprise plans)

Receive real-time incident notifications:

**Webhook Events**:
- `incident.created` - New incident detected
- `incident.updated` - Status update posted
- `incident.resolved` - Incident fixed
- `incident.reopened` - Incident recurred

Setup in **Settings → Notification Channels → Webhooks**.

## Related Documentation

- **[Creating Status Pages](/articles/status-pages/creating-status-pages)** - Share incidents with customers
- **[Notification Channels](/articles/alerting-notifications/notification-channels-guide)** - Alert your team
- **[Understanding Monitor Types](/articles/monitors/understanding-monitor-types)** - Configure monitors
- **[Analytics Dashboard](/articles/analytics/understanding-analytics)** - Track incident metrics
- **[API Reference](/articles/api/api-reference-guide)** - Programmatic incident management

## Getting Help

Need help with incident management?

- **Documentation**: Browse this knowledge base
- **Support**: support@statusapp.io
- **Live Chat**: Available in dashboard (Business+ plans)
- **Community**: community.statusapp.io

## Quick Reference

### Incident Lifecycle Checklist

- [ ] Incident detected (automatic or manual)
- [ ] First update posted within 15 minutes
- [ ] Status set to "Investigating"
- [ ] Updates every 15-30 minutes
- [ ] Root cause identified
- [ ] Status updated to "Identified"
- [ ] Fix deployed
- [ ] Status updated to "Monitoring"
- [ ] Verify stability
- [ ] Post "Resolved" update
- [ ] Document root cause
- [ ] Add prevention steps
- [ ] Conduct post-mortem (if High/Critical)
- [ ] Create action items
- [ ] Share learnings

### Status Update Frequency

- **First update**: Within 5-15 minutes
- **During investigation**: Every 15-30 minutes
- **After identified**: Every 30-60 minutes
- **During monitoring**: Every hour
- **Final update**: Resolution summary

### Communication Tone

✅ **Good**: "We're experiencing issues with payment processing. Our team is investigating and will provide an update in 15 minutes."

✅ **Good**: "We've identified the issue as a database connection pool exhaustion. Implementing fix now, expecting resolution within 30 minutes."

✅ **Good**: "Issue resolved. Payment processing is back to normal. The root cause was a configuration error introduced in this morning's deployment."

❌ **Bad**: "Website down. Checking..."

❌ **Bad**: "Database crashed because intern pushed bad code lol"

❌ **Bad**: "Still working on it" (after 60 minutes of silence)