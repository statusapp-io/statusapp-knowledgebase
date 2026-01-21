---
title: Incident Lifecycle & Management
slug: incident-lifecycle
category: Incidents
excerpt: Understand incident creation, statuses, and lifecycle in StatusApp.
order: 6
---

# Incident Lifecycle & Management

## What is an Incident?

An incident is any event that disrupts or degrades service availability or performance. StatusApp provides a complete timeline from detection through resolution, helping you communicate and learn from failures.

**Incidents Track**:
- When the issue started and duration
- Which monitors/services affected
- All status updates during incident
- Severity and impact
- Mean Time To Resolution (MTTR)
- Customer communications

## How Incidents Are Created

### Automatic Incident Creation

StatusApp automatically creates incidents when monitors fail:

**Trigger Process**:
1. Monitor check fails (non-200 status, timeout, or error)
2. Confirmation checks verify failure (prevent false positives)
3. Incident created with unique ID (INC-00123)
4. Status page updated instantly
5. Subscribers notified via email

**Automatic Incident Data**:
- Incident ID (unique identifier)
- Start time (when first failure detected)
- Affected monitor (which service)
- Severity (auto-determined based on impact)
- Affected regions (which locations affected)
- Check failure details (status codes, errors, times)

**Grace Period**:
- Configure monitors to wait before creating incident
- Prevents false positives from temporary blips
- Common values: 1-5 minutes
- Balances speed vs accuracy

### Manual Incident Creation

Create incidents for issues not automatically detected:

**When to Create Manually**:
- Planned maintenance windows
- Third-party services down (not monitored)
- Internal issues affecting customers
- Customer-reported issues
- Special events/incidents

**How to Create**:

1. Go to **Incidents** page
2. Click **Create Incident**
3. Fill in details:
   - **Title**: Clear, concise description
   - **Affected Monitors**: Select affected services
   - **Status**: Start with "Investigating"
   - **Message**: Detailed explanation
   - **Severity**: Low, Medium, High, or Critical
4. Click **Create & Notify**

**Incident Details**:
- Title: "Payment Processing Down"
- Affected: Payment API, Payment Gateway
- Status: Investigating
- Severity: Critical
- Message: "Our payment processor is experiencing issues..."

Subscribers automatically notified.

## Incident Statuses

Incidents move through statuses as you progress toward resolution:

### Investigating

**Meaning**: Issue reported, team assessing

**When to use**:
- Just discovered the problem
- Trying to understand scope
- Collecting information
- Determining root cause

**Example update**: "We've identified increased error rates in the payment service. Our team is investigating the root cause."

### Identified

**Meaning**: Root cause found, working on fix

**When to use**:
- You know what's causing the problem
- Implementing a solution
- ETA available or being determined
- Working on remediation

**Example update**: "Root cause identified: database connection pool exhausted. We're scaling up connections now."

### Monitoring

**Meaning**: Fix deployed, validating stability

**When to use**:
- Solution has been applied
- Testing or monitoring for stability
- Not yet 100% confident in resolution
- May need rollback

**Example update**: "We've deployed the fix. Monitoring for stability over the next 10 minutes."

### Resolved

**Meaning**: Issue fixed, service fully operational

**When to use**:
- Confirmed service is working normally
- All customers impacted no longer affected
- Back to normal operations
- Ready to close incident

**Example update**: "The incident has been resolved. All services are operating normally."

## Incident Workflow

### Typical Incident Timeline

```
1. Monitor Failure Detected
   ↓
2. Incident Created (Investigating)
   ├─ Update 1: Initial assessment
   ├─ Update 2: Root cause found (Identified)
   ├─ Update 3: Deploying fix
   ├─ Update 4: Monitoring (Monitoring)
   ├─ Update 5: Confirmed stable
   ↓
3. Incident Resolved
   ↓
4. Post-Mortem (optional)
```

### Updating Incidents

Keep customers informed with regular updates:

1. Open incident
2. Click **Post Update**
3. Change status if needed
4. Add update message
5. Click **Post Update**

**Best Practices for Updates**:
- Update every 30-60 minutes during outage
- Be specific about what's happening
- Provide ETAs when possible
- Explain what you're doing
- Update when status changes
- Thank customers for patience

**Example Updates**:

```
10:30 AM - Investigating
"We've detected elevated error rates affecting payment processing. 
Our team is currently investigating the cause."

10:45 AM - Identified
"Root cause identified: database connection limits reached due to 
surge in traffic. We're scaling up connections now."

11:00 AM - Monitoring
"We've increased database capacity. Monitoring for stability 
before marking resolved."

11:15 AM - Resolved
"The incident is now resolved. All payments are processing normally. 
We apologize for the disruption."
```

## Incident Severity

Severity levels indicate impact:

### Critical
**Description**: Major functionality down, revenue impact

**Examples**:
- Core platform unavailable
- Payment processing down
- Authentication broken
- Data loss

**Response**: Immediate escalation, all hands on deck

### High
**Description**: Important feature broken, significant impact

**Examples**:
- API endpoint down
- Reporting feature unavailable
- Performance severely degraded
- Affecting multiple customers

**Response**: Urgent response, prioritize fix

### Medium
**Description**: Feature degraded, some users impacted

**Examples**:
- Slow response times (but working)
- One feature broken (others work)
- Affecting specific user segments
- Non-critical functionality

**Response**: Important, address within hours

### Low
**Description**: Minor issue, minimal impact

**Examples**:
- UI glitch on rarely-used page
- Email notification delays
- Minor performance degradation
- One user impacted

**Response**: Address when convenient

## Incident Resolution

### Auto-Resolution for Monitored Services

When a failed monitor recovers:
- StatusApp detects recovery
- Incident automatically resolves
- Final status change posted
- Subscribers notified

**Automatic Detection**:
- Monitor returns to "Up" status
- 1-2 successful check cycles confirm
- Incident marked resolved
- Recovery time recorded

### Manual Resolution

For manually-created incidents:
- Update status to "Resolved"
- Post final update message
- Incident is closed
- Customers notified

**Final Update Example**:
```
"The issue has been fully resolved. Payment processing is operating 
at normal capacity. We apologize for the inconvenience and thank you 
for your patience."
```

## Incident Analytics

### Mean Time To Resolution (MTTR)

Time from incident start to resolution:

```
MTTR = Incident End Time - Incident Start Time
```

**Tracking MTTR**:
- View in incident details
- Historical MTTR trends
- Compare across incident types
- Identify patterns

**Example**: Incident from 10:30 AM to 11:15 AM = 45 minute MTTR

### Incident Frequency

Number of incidents per time period:

**Tracking**:
- Dashboard shows recent incidents
- Analytics show trends
- Identify reliability patterns
- Plan improvements

**Example**: 3 incidents in last week
- Monday: 1 incident
- Wednesday: 2 incidents
- Other days: None

### Mean Time Between Failures (MTBF)

Time between consecutive incidents:

```
MTBF = Time Between Incident End and Next Incident Start
```

**Improvement**: Longer MTBF = more stable service

## Incident History

View all incidents on your status page:

**Historical Record**:
- Last 90 days by default
- Includes resolution time
- Links to full incident details
- Shows customer impact

**Benefits**:
- Track reliability trends
- Identify patterns
- Demonstrate transparency
- Learn from failures

**Example Incident History**:
```
INC-00125: Database Timeout - Resolved (15 min)
INC-00124: API Rate Limiting - Resolved (45 min)
INC-00123: SSL Certificate Update - Resolved (5 min)
```

## Best Practices

### 1. Create Incidents Promptly

Don't delay creating incidents:

```
Good: Create immediately when issue detected
Bad: Wait until multiple customers complain
Bad: Wait until team has full root cause
```

### 2. Update Every 30-60 Minutes

Keep customers informed:

```
During Active Incident:
├─ 0 min: Create incident
├─ 15 min: Post initial update
├─ 30 min: Post status update
├─ 45 min: Post significant update
└─ Resolution: Mark resolved
```

### 3. Use Consistent Severity

Be consistent with severity classification:

```
Good: Critical reserved for major outages
Bad: Calling everything Critical
Bad: Never using Critical even for major issues

Customers learn to ignore if overused
```

### 4. Be Transparent

Share what you know:

```
Good: "We're still investigating but database team is engaged"
Bad: "We have no idea what's happening"
Bad: Silence during active incident

Transparency builds trust
```

### 5. Include Root Cause in Resolved Update

Help customers understand:

```
Good: "Root cause was X. We've deployed Y to prevent recurrence."
Bad: "It's fixed now."

Explains what happened and prevents future incidents
```

## Common Incident Scenarios

### Scenario 1: Database Connection Pool Exhausted

```
10:00 - Incident Created: API Timeouts Detected
       Status: Investigating
       Severity: Critical
       
10:05 - First Update: "We've identified elevated database connection 
        usage. Investigating cause."
       Status: Identified
       
10:10 - Second Update: "Root cause: connection pool limit exceeded 
        due to surge in traffic. Scaling connections now."
       Status: Monitoring
       
10:15 - Third Update: "We've increased connection pool size. 
        Monitoring for stability."
       
10:20 - Resolved: "Database connection pool increased from 50 to 200 
        connections. Service operating normally."
```

### Scenario 2: Third-Party Service Down

```
12:00 - Incident Created: Payment Processor Integration Down
       Status: Investigating
       Severity: Critical
       Message: "Our payment processor is experiencing issues"
       
12:05 - Update: "Contacted payment processor support. They confirm 
        service degradation in US region."
       Status: Identified
       
12:15 - Update: "Payment processor reports 90% of service restored. 
        Monitoring their status page for full recovery."
       
12:30 - Resolved: "Payment processor has fully recovered. All 
        payment processing operating normally."
```

## Next Steps

- **[Incident Communication](/help/knowledge-base/incident-communication)** - Best practices for customer communication
- **[Status Pages](/help/knowledge-base/creating-status-pages)** - Display incidents publicly
- **[Notifications](/help/knowledge-base/notification-channels-overview)** - Alert your team
