---
title: Understanding Analytics and Metrics
slug: understanding-analytics
category: Analytics
excerpt: Learn how to interpret uptime metrics, response time analytics, and performance trends in StatusApp.
order: 5
---

# Understanding Analytics and Metrics

## Overview

StatusApp includes built-in analytics to track reliability, performance, and incident quality. Dashboards focus on what matters most: uptime, response time percentiles, incident frequency, MTTR, and root causes. This guide explains each metric, where to find it, and how to act on it.

## Key Metrics

### Uptime Percentage

```
Uptime % = (Successful Checks ÷ Total Checks) × 100
```

- Calculated per monitor, per region, and overall
- Includes DOWN and DEGRADED time
- Shown for 24h, 7d, 30d, and 90d periods

**Industry benchmarks**
- 99.9% ("three nines") ≈ 43.2 min downtime/month
- 99.99% ("four nines") ≈ 4.3 min downtime/month
- 99.999% ("five nines") ≈ 26 sec downtime/month

### Response Time Percentiles

How fast your endpoint responds, shown as:
- **P50 (Median)**: Typical user experience
- **P90/P95**: Slow end of normal traffic
- **P99**: Outliers; great for spotting regressions
- **Average**: Helpful but can hide spikes

Interpreting:
- Rising P95/P99 = emerging performance issue
- Spiky graph = inconsistent performance
- Flat, low percentiles = healthy service

### MTTR (Mean Time To Resolution)

```
MTTR = (Sum of incident durations) ÷ (Number of resolved incidents)
```
- Measures how quickly you recover
- Tracked per monitor and overall
- Shown on Incident Analytics

### Time to First Update
- Measures responsiveness in communication
- Tracks minutes from incident start to first public update
- Aim for < 15 minutes

### Incident Frequency
- Count of incidents over time (daily/weekly/monthly)
- Highlights noisy services
- Correlate with deployments or traffic peaks

### Top Root Causes
- Breakdown of root cause categories across incidents
- Identify systemic issues (e.g., DATABASE_ISSUE, DEPLOYMENT_ISSUE)

## Dashboards

### Overview Dashboard (Incidents)
Shows reliability and communication quality at a glance:
- Active vs Resolved incidents
- Average MTTR
- Time to first update
- Incident frequency over time
- Severity breakdown
- Top root causes
- Top monitors by incident count

### Monitor Analytics
For each monitor, view:
- Current status (UP/DOWN/DEGRADED)
- Uptime % for 24h/7d/30d/90d
- Response time percentiles (P50/P95/P99)
- Region-level performance (where available)
- Incident history and duration
- Recent checks with status codes/errors

### Status Page Reliability
When a monitor is on a status page, its uptime and incidents roll into public metrics:
- 90-day uptime chart (public)
- Daily uptime bars with color coding
- Public incident history

## Time Ranges

- Quick ranges: 24h, 7d, 30d, 90d
- Custom range: choose start/end dates
- All charts and tables respect the selected range

## Charts & Visuals

### Uptime Timeline
- Green = operational
- Yellow = degraded
- Red = down
- Hover for exact timestamps and duration

### Response Time Chart
- Line or area chart of response times
- P50/P95/P99 overlays for quick regression spotting
- Spikes highlight potential performance incidents

### Incident Frequency & Severity
- Bar/line chart of incidents over time
- Severity breakdown (Critical/High/Medium/Low)
- Click through to the incident details

### Root Cause Breakdown
- Pie/bar chart of root cause categories
- Track whether fixes reduce recurring causes

## How to Use Analytics

### Detect Regressions Early
- Watch P95/P99 for rising trends
- Compare 24h vs 7d to see fresh problems
- Correlate spikes with deployments

### Improve Reliability
- Track MTTR over the last 30/90 days
- Find top failing monitors and prioritize fixes
- Reduce time to first update during incidents

### Capacity & Performance Planning
- Monitor response time trends before traffic peaks
- Identify slow regions and add redundancy
- Use historical baselines for SLO/SLA targets

### Communication Quality
- Keep "time to first update" under 15 minutes
- Ensure regular updates during incidents
- Measure resolution quality with root cause tracking

## Exports

- Export incident lists and metrics (CSV) for reporting
- Export check logs for deep analysis (per monitor)
- Copy uptime numbers for status reports

## Plan Notes

- Analytics available on all paid plans (Starter+)
- Response time percentiles and incident analytics on Pro+
- Exports and longer retention on Business/Enterprise

## Quick Reference

**If uptime drops:**
1) Check incident list for active issues
2) Open monitor analytics to see failing regions
3) Review recent deployments or config changes

**If P95/P99 spike:**
1) Correlate with traffic/load
2) Check slow regions
3) Look for database or third-party latency

**If MTTR is high:**
1) Improve alerting + on-call response
2) Standardize runbooks
3) Post updates faster to keep teams aligned

**If incidents repeat:**
1) Review root cause breakdown
2) Fix systemic issues (config, DB, deployments)
3) Add automated checks for the specific failure mode

- **Pie Chart**: Percentage of each status
- **Success Rate**: HTTP 2xx responses
- **Redirects**: HTTP 3xx responses
- **Client Errors**: HTTP 4xx responses
- **Server Errors**: HTTP 5xx responses

## Advanced Analytics

### Performance Regression Detection

StatusApp automatically detects when performance degrades significantly:

1. Establishes baseline from historical data
2. Alerts when response times exceed baseline + threshold
3. Helps identify issues early
4. Configurable sensitivity levels

### Anomaly Detection

- Identifies unusual patterns
- Detects unexpected downtime
- Flags performance spikes
- Learns from historical patterns

### Prediction Models

- Forecast future performance
- Identify reliability trends
- Predict potential issues
- Plan capacity upgrades

## Creating Reports

### Manual Reports

1. Go to **Analytics > Reports**
2. Click **Create Report**
3. Select monitors to include
4. Choose time period
5. Select metrics to display
6. Generate PDF or email

### Report Contents

- Executive summary
- Detailed metrics
- Charts and graphs
- Incident summary
- Trend analysis
- Recommendations

### Scheduled Reports

1. Create report
2. Enable **Auto-Generate**
3. Choose frequency (weekly, monthly)
4. Set recipients
5. EmailAutomatically sends at specified times

### Report Distribution

- Email to team
- Email to stakeholders
- Email to customers
- Post to status page
- Export as PDF

## Exporting Data

### CSV Export

1. Select time period
2. Choose metrics
3. Click **Export**
4. Opens in Excel/spreadsheet
5. Analyze with external tools

### API Access

Programmatically retrieve analytics:

```bash
curl https://api.statusapp.io/monitors/{id}/analytics \
  -H "Authorization: Bearer sk_live_abc123" \
  -d '{"period": "30d"}'
```

## SLA Management

### Defining SLAs

1. Go to **Settings > SLAs**
2. Click **Create SLA**
3. Define uptime target (e.g., 99.9%)
4. Set measurement period
5. Assign monitors

### SLA Tracking

- Real-time status vs. target
- Monthly SLA reports
- SLA breach alerts
- Historical compliance data
- Credit calculation

### SLA Reporting

- Automatic reports generation
- Email to customers
- Post to status page
- API access
- Audit trail

## Insights and Recommendations

### StatusApp AI Insights

- Automatic analysis of metrics
- Performance recommendations
- Reliability improvements
- Cost optimization suggestions

### When to Investigate

Look for these patterns:

#### Response Time Degradation
- Upward trend in response times
- Sudden spikes
- Increased variability
- Compare with infrastructure changes

#### Increasing Incident Frequency
- More incidents than usual
- Shorter MTBF
- Different types of failures
- Common time patterns

#### Availability Issues
- Declining uptime trend
- Repeated failures
- Similar failure times
- Failed monitor checks

## Best Practices

### Regular Review

1. **Weekly**: Check critical service metrics
2. **Monthly**: Generate full reports
3. **Quarterly**: Analyze trends
4. **Annually**: Review SLA compliance

### Benchmarking

1. Compare with industry standards
2. Track improvement over time
3. Identify weak areas
4. Set realistic targets
5. Celebrate improvements

### Team Communication

1. Share metrics with team
2. Discuss performance trends
3. Plan improvements
4. Celebrate reliability wins
5. Post-mortem on incidents

### Action Items

1. **Identify Issues**: Use analytics to find problems
2. **Root Cause Analysis**: Understand why issues occur
3. **Implement Solutions**: Fix underlying problems
4. **Monitor Results**: Track improvement
5. **Iterate**: Continuously improve

## Troubleshooting Analytics

### Data Not Showing Up
- Verify monitor is running
- Check time period selection
- Ensure monitor has historical data
- Verify permissions

### Unexpected Downtime
- Review incident details
- Check monitor configuration
- Verify false positives
- Review logs

### Inconsistent Metrics
- Check regional differences
- Verify time zone settings
- Review calculation methods
- Compare manual vs. automated

## Next Steps

- Learn about [incident management](/articles/incidents/incident-management-basics)
- Set up [alerts](/articles/alerting-notifications/notification-channels-guide)
- Create [status pages](/articles/status-pages/creating-status-pages)
- Explore [API](/articles/api/api-overview)