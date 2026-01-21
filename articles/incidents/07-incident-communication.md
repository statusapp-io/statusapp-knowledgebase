---
title: Incident Communication & Best Practices
slug: incident-communication
category: Incidents
excerpt: Communicate effectively during incidents and conduct post-mortems.
order: 7
---

# Incident Communication & Best Practices

## The Importance of Communication

During incidents, communication is as important as fixing the problem.

**Why Communication Matters**:
- Reduces customer support tickets
- Builds trust through transparency
- Shows you're in control
- Prevents rumor/misinformation
- Demonstrates professionalism
- Reduces customer anxiety

**Poor Communication**:
- Silence during outage = customers panic
- Vague updates = customer frustration
- Delayed updates = loss of trust
- No transparency = reputation damage

**Effective Communication**:
- Frequent updates (every 30-60 min)
- Transparent about what you know
- Clear about what you're doing
- Realistic timelines
- Apologetic and professional

## Real-Time Update Strategy

### Initial Update (Within 15 Minutes)

Post first update immediately when issue detected:

**What to Include**:
- Acknowledge the problem
- State what's affected
- Brief description of issue
- What you're doing right now

**Example**:
```
"We're aware of intermittent errors affecting our payment service. 
Our team is investigating. We'll have an update in 10 minutes."
```

**What to Avoid**:
- Speculation about root cause
- Blame on other teams/services
- Over-promises on ETA
- Technical jargon customers don't understand

### Status Updates (Every 30-60 Minutes)

Continue updating customers throughout incident:

**What to Include**:
- Current status
- Progress since last update
- Updated timeline if applicable
- Any new discoveries

**Example**:
```
"Update: We've identified the issue is in our message queue service. 
The team has implemented a fix and is currently validating it. 
We expect full resolution within 30 minutes."
```

**Keep Updates**:
- Concise (2-3 sentences typical)
- Specific (not just "we're working on it")
- Honest (share what you know)
- Professional (no frustration)

### Resolution Update (When Fixed)

Post final update when issue is fully resolved:

**What to Include**:
- Confirmation service is fully operational
- Root cause (if known)
- What prevented recurrence
- Appreciation for patience

**Example**:
```
"The incident is now fully resolved. Root cause was a memory leak 
in our message queue service that accumulated over 3 days. We've 
deployed a fix and will be monitoring closely. We apologize for 
the disruption and appreciate your patience."
```

**Always**:
- Apologize for the inconvenience
- Thank customers for patience
- Explain prevention measures

## Customer Communication Channels

### Status Page

Primary channel for incident communication:

**What Appears**:
- Incident created when monitor fails
- Real-time status updates
- Affected services clearly listed
- Uptime impact calculated
- Accessible 24/7

**Best For**: All customers, maximum visibility

### Email Notifications

Automated emails to subscribers:

**Triggers**:
- New incident created
- Incident status changed
- Incident resolved
- Maintenance scheduled

**Best For**: Detailed communication, record-keeping

### Slack/Discord/Teams

Team channels for internal coordination:

**What to Post**:
- Initial discovery
- Status updates (shorter than public)
- Key decisions
- Resolution confirmation

**Best For**: Team coordination, rapid response

### Twitter/Social Media

Optional for large outages:

**When to Use**:
- Outage affecting major portion of user base
- Incident lasting > 2 hours
- High customer visibility needed

**What to Post**:
- Acknowledge issue
- Share status page link
- Update on progress
- Resolution confirmation

**Example**:
```
"We're currently experiencing issues with our payment service. 
Our team is investigating. Real-time updates: status.company.com"
```

## What NOT to Say During Incidents

### Don't Blame External Services

```
Bad: "Our provider's issue, not ours"
Good: "We're working with our provider to resolve this"

Customers don't care who's responsible - they just want it fixed
```

### Don't Make Promises You Can't Keep

```
Bad: "We'll have this fixed in 5 minutes"
Good: "We expect resolution within 30 minutes"

Broken promises destroy trust more than waiting
```

### Don't Use Technical Jargon

```
Bad: "Database connection pool exhausted, scaling MySQL replicas"
Good: "Database running out of connections, adding capacity"

Customers don't understand technical details
```

### Don't Disappear

```
Bad: No updates for 2 hours during active incident
Good: Update every 30-60 minutes minimum

Silence breeds panic and rumors
```

### Don't Minimize the Issue

```
Bad: "Just a minor glitch affecting a few users"
Good: "We're experiencing service degradation affecting payments"

Be honest about severity
```

## Post-Mortem Process

After incident is resolved, conduct post-mortem:

### Step 1: Schedule Meeting (Within 48 Hours)

Don't wait too long:
- Team memory still fresh
- Details accurate
- Momentum to implement fixes

### Step 2: Gather Information

Collect during meeting:

**What to Document**:
- Timeline of events (minute by minute)
- Root cause (why did it happen?)
- Impact (how many customers affected?)
- Detection time (how long before discovered?)
- Resolution time (how long to fix?)
- Contributing factors

**Use Data**:
- Incident timeline from StatusApp
- Error logs from systems
- Customer impact data
- Team observations

**Example Timeline**:
```
2:00 PM - Issue begins (database connections exhausted)
2:05 PM - First customer report
2:10 PM - Alert fires in monitoring
2:15 PM - Incident created
2:20 PM - Root cause identified
2:30 PM - Fix deployed
2:45 PM - Incident resolved (45 min total MTTR)
```

### Step 3: Identify Root Causes

Dig deeper than immediate cause:

**5 Why's Technique**:
1. Why did database connections exhaust?
   → Traffic spike exceeded capacity

2. Why did traffic spike?
   → New feature launch with viral marketing

3. Why weren't connections scaled?
   → Didn't anticipate that level of traffic

4. Why no capacity planning?
   → Load testing only simulated 50% actual load

5. Why was load testing insufficient?
   → Outdated assumptions about typical usage

**Root Cause**: Insufficient load testing and capacity planning

### Step 4: Discuss Lessons Learned

What to improve:

**Types of Lessons**:
- What worked well (keep doing this)
- What didn't work (don't do again)
- What to improve
- What to prevent recurrence

**Example Lessons**:
```
What Worked Well:
- Team responded quickly
- Communication was clear
- Root cause identified fast

What to Improve:
- Capacity planning was inadequate
- Load testing didn't simulate real conditions
- No pre-incident escalation procedures

Preventive Measures:
- Implement continuous load testing
- Auto-scale database connections
- Set up traffic surge alerts
- Create escalation procedures
```

### Step 5: Assign Action Items

Concrete steps to prevent recurrence:

**Make Assignments**:
- Who is responsible?
- What exactly needs to be done?
- When is deadline?
- How will you verify it's done?

**Example Action Items**:
```
1. Implement auto-scaling for database connections
   Owner: Database team
   Deadline: 1 week
   Verify: Test auto-scaling response

2. Update load testing scenarios
   Owner: QA team
   Deadline: 2 weeks
   Verify: New tests pass

3. Implement traffic surge alerts
   Owner: DevOps team
   Deadline: 1 week
   Verify: Alert triggers at 80% capacity

4. Document escalation procedures
   Owner: On-call engineer
   Deadline: 3 days
   Verify: All team trained
```

### Step 6: Publish Post-Mortem (Optional)

Share with customers:

**Customer-Facing Post-Mortem**:
- High-level timeline
- What you learned
- What you're doing differently
- Appreciation for patience

**Example**:
```
Post-Mortem: Payment Service Incident

On January 20, we experienced a 45-minute outage affecting our 
payment service. Our team has investigated the root cause and 
implemented preventive measures.

What Happened:
We launched a new feature that generated significantly more traffic 
than anticipated. Our database connection pool reached capacity, 
causing services to fail.

What We Learned:
Our load testing didn't accurately simulate real user behavior. We've 
updated our testing procedures.

What We're Doing:
1. Implemented automatic scaling for database resources
2. Updated our load testing to better simulate production conditions
3. Set up early warning alerts for resource saturation

We apologize for the disruption and appreciate your patience.
```

## Communication Templates

### Incident Start Template

```
"We're aware of [service/feature] issues starting at [time]. 
Our team is investigating. We'll provide an update in [15 min]."
```

### Progress Update Template

```
"Update: We've identified [issue]. The team is [action]. 
We expect [timeframe]."
```

### Resolution Template

```
"This incident has been resolved. Root cause was [brief explanation]. 
We've [prevention measure]. We apologize for the disruption."
```

## Best Practices

### 1. Be Proactive

Post incidents before customers call support:

```
Good: Post incident 2 minutes after detecting
Bad: Wait for first customer complaint
Bad: Wait until you know root cause
```

### 2. Be Transparent

Share what you know, not what you don't:

```
Good: "We're still investigating but here's what we know so far..."
Bad: "We have no idea"
Bad: Silent
```

### 3. Be Consistent

Use same status page for all customers:

```
Good: One status page with updates
Bad: Telling different customers different things
Bad: Different information in email vs status page
```

### 4. Be Professional

Maintain professionalism even under pressure:

```
Good: "We're working to resolve this"
Bad: "Our stupid CDN failed again"
Bad: Frustration evident in tone
```

### 5. Follow Up

Post-mortem shows you care about improvement:

```
Good: Conduct post-mortem, share learnings
Good: Implement preventive measures
Good: Share progress with customers

Bad: Forget about it after incident ends
Bad: No preventive measures taken
```

## Common Mistakes to Avoid

### Waiting Too Long for First Update

**Mistake**: "Let's understand the full issue before updating"

**Problem**: Customers panic, support tickets flood in

**Fix**: Update immediately, even if partial information

### Over-Promising on Timeframe

**Mistake**: "Should be fixed in 15 minutes"

**Problem**: When it takes 45 minutes, customer anger increases

**Fix**: Under-promise, over-deliver (say 30 min, fix in 20)

### Using Too Much Technical Jargon

**Mistake**: "Database connection pool exhausted on MySQL replicas"

**Problem**: Most customers don't understand, feel excluded

**Fix**: "Database running out of connections"

### No Post-Mortem

**Mistake**: Fix issue and move on

**Problem**: Same issue happens again in 3 months

**Fix**: Conduct post-mortem, implement preventive measures

### Disappearing After Resolution

**Mistake**: Post "resolved" update and ghost

**Problem**: No post-mortem, no explanation, no prevention

**Fix**: Follow up with post-mortem and learnings

## Tools for Communication

### StatusApp Status Page

- Real-time incident publishing
- Automatic customer notifications
- Incident timeline/history
- Subscriber management

### Email/SMS

- Targeted notifications
- Offline communication
- Record-keeping

### Slack/Discord/Teams

- Internal team coordination
- Rapid communication
- Context sharing

### Social Media

- Large audience reach
- Quick updates
- External visibility

## Next Steps

- **[Incident Lifecycle](/help/knowledge-base/incident-lifecycle)** - How incidents progress
- **[Status Pages](/help/knowledge-base/creating-status-pages)** - Publish incidents publicly
- **[Notifications](/help/knowledge-base/notification-channels-overview)** - Alert your team
