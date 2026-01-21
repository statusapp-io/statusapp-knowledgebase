---
title: Understanding Analytics and Metrics
slug: understanding-analytics
category: Analytics
excerpt: Learn how to interpret uptime metrics, response time analytics, and performance trends in StatusApp.
order: 5
---

# Understanding Analytics and Metrics

## Overview

StatusApp provides comprehensive analytics to help you understand your service performance and identify trends. This guide explains key metrics and how to use them effectively.

## Key Metrics

### Uptime Percentage

Uptime is calculated as:
```
Uptime % = (Total Check Time - Down Time) / Total Check Time × 100
```

#### What It Means
- **99.9% (Three Nines)**: 43.2 minutes of downtime per month
- **99.99% (Four Nines)**: 4.3 minutes of downtime per month
- **99.999% (Five Nines)**: 26 seconds of downtime per month

#### Factors Affecting Uptime
- Actual service outages
- Network connectivity issues
- Monitoring point issues
- False positives from misconfiguration

### Response Time

How quickly your services respond to requests.

#### Response Time Metrics
- **Average**: Mean response time across all checks
- **Median (P50)**: Middle value - half faster, half slower
- **P95**: 95% of responses are faster than this
- **P99**: 99% of responses are faster than this
- **Max**: Slowest response time recorded
- **Min**: Fastest response time recorded

#### Interpreting Response Times
- Improving trend: Faster responses, better user experience
- Degrading trend: Slower responses, potential issues
- Spikes: Temporary slowdowns, often during high load
- Consistency: Lower variation is better

### Mean Time To Recovery (MTTR)

How long it takes to recover from an outage:
```
MTTR = Total Recovery Time / Number of Incidents
```

#### Using MTTR
- Track improvement over time
- Compare with industry standards
- Identify problem areas
- Justify investments in reliability

### Incident Statistics

- **Total Incidents**: Number of detected outages
- **Average Duration**: How long incidents typically last
- **Incident Frequency**: How often outages occur
- **Time Between Incidents**: MTBF (Mean Time Between Failures)

## Analytics Dashboards

### Per-Monitor Analytics

1. Click on a monitor from the dashboard
2. Go to **Analytics** tab
3. Select time period (24h, 7d, 30d, 90d)

#### What's Displayed
- Real-time status
- Current uptime percentage
- Average response time
- Response time graph
- Incident history
- Recent checks log

### Custom Time Periods

1. Select **Custom Range**
2. Choose start and end dates
3. View analytics for specific period
4. Export data if needed

### Comparison Mode

- Compare same monitor across different periods
- Compare different monitors
- Identify seasonal trends
- Track improvements

## Understanding Charts

### Response Time Graph

- **X-Axis**: Time
- **Y-Axis**: Response time in milliseconds
- **Green Area**: Normal response times
- **Red Area**: Degraded or timeout areas
- **Points Above Line**: Individual slow requests

### Uptime Timeline

- **Green Segments**: Uptime (100%)
- **Red Segments**: Downtime (0%)
- **Yellow Segments**: Degraded (partial)
- **Hover for Details**: See exact times and durations

### Status Distribution

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
- Set up [alerts](/articles/alerting-notifications/setting-up-notifications)
- Create [status pages](/articles/status-pages/creating-status-pages)
- Explore [API](/articles/api/api-overview)