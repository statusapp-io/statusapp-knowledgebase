---
title: Billing Plans & Features
slug: billing-plans-comparison
category: Billing Plans
excerpt: Compare StatusApp pricing plans and their features to find the right fit for your monitoring needs.
order: 8
---

# Billing Plans & Features

## Plan Overview

StatusApp offers flexible pricing tiers designed to scale with your monitoring needs. All plans are configured dynamically through our platform, allowing for customization to meet specific requirements.

## Available Plans

### Starter Plan

**Best For**: Small teams getting started with monitoring

**Core Features**:
- Monitor count based on plan configuration
- Check intervals starting at 60 seconds minimum
- Multi-region monitoring from configured locations
- Data retention based on plan settings
- Team member seats included

**Notification Channels**:
- Email alerts (included on all plans)
- Slack integration (plan-dependent)
- Additional channels based on plan tier

**Key Capabilities**:
- HTTP/HTTPS website monitoring
- Ping (ICMP) monitoring
- DNS record monitoring
- Heartbeat/CRON monitoring
- Domain monitoring
- SSL certificate monitoring
- API access

### Professional Plan

**Best For**: Growing companies with multiple services

**Everything in Starter, plus**:
- More monitors
- Faster check intervals (30 seconds minimum available)
- More monitoring regions
- Extended data retention
- More team members

**Additional Features**:
- SMS notifications
- All notification integrations (Discord, Telegram, etc.)
- PagerDuty integration
- OpsGenie integration
- Custom webhooks
- API monitoring with authentication
- Port/TCP monitoring
- GraphQL monitoring
- Performance metrics
- Custom domains for status pages

### Business Plan

**Best For**: Organizations with SLA requirements

**Everything in Professional, plus**:
- Higher monitor limits
- Faster check intervals (10 seconds minimum)
- All monitoring regions
- Extended data retention (1 year)
- Larger team capacity

**Enterprise Features**:
- Advanced analytics
- Custom dashboards
- Scheduled reports
- Maintenance windows
- Incident management
- Escalation rules
- Root cause analysis
- Performance budgets
- SLA tracking and compliance
- Role-based access control (RBAC)

### Enterprise Plan

**Best For**: Large organizations with complex requirements

**Everything in Business, plus**:
- Unlimited monitors
- Custom check intervals (as low as 5 seconds)
- Unlimited data retention
- Unlimited team members

**Premium Features**:
- SSO/SAML authentication
- Audit logs
- White-labeling
- Private monitoring locations
- Dedicated account manager
- Priority support
- Custom SLA
- Onboarding and training included

### Custom Plan

**Best For**: Organizations needing tailored solutions

Our Custom Plan allows you to configure exactly what you need:

**Configurable Options**:
- Number of monitors
- Team member seats
- Check interval minimums
- Data retention period
- Number of status pages

**Feature Add-ons**:
- SMS alerts
- Slack integration
- Priority support
- Custom domains
- Multi-region monitoring
- API access

---

## Feature Comparison

### Monitoring Capabilities

| Feature | Starter | Professional | Business | Enterprise |
|---------|---------|--------------|----------|------------|
| Website/HTTP Monitoring | Yes | Yes | Yes | Yes |
| Ping Monitoring | Yes | Yes | Yes | Yes |
| DNS Monitoring | Yes | Yes | Yes | Yes |
| Heartbeat/CRON | Yes | Yes | Yes | Yes |
| Domain Monitoring | Yes | Yes | Yes | Yes |
| SSL Certificate | Yes | Yes | Yes | Yes |
| API Monitoring | Plan-based | Yes | Yes | Yes |
| Port/TCP Monitoring | Plan-based | Yes | Yes | Yes |
| GraphQL Monitoring | Plan-based | Yes | Yes | Yes |
| Server Monitoring | No | Plan-based | Yes | Yes |

### Notification Channels

| Channel | Starter | Professional | Business | Enterprise |
|---------|---------|--------------|----------|------------|
| Email | Yes | Yes | Yes | Yes |
| Slack | Plan-based | Yes | Yes | Yes |
| Discord | Plan-based | Yes | Yes | Yes |
| Telegram | Plan-based | Yes | Yes | Yes |
| SMS | No | Yes | Yes | Yes |
| PagerDuty | No | Yes | Yes | Yes |
| OpsGenie | No | Yes | Yes | Yes |
| Microsoft Teams | Plan-based | Yes | Yes | Yes |
| Custom Webhooks | Plan-based | Yes | Yes | Yes |

### Analytics & Reporting

| Feature | Starter | Professional | Business | Enterprise |
|---------|---------|--------------|----------|------------|
| Basic Analytics | Yes | Yes | Yes | Yes |
| Performance Metrics | No | Yes | Yes | Yes |
| Advanced Analytics | No | No | Yes | Yes |
| Custom Dashboards | No | No | Yes | Yes |
| Scheduled Reports | No | No | Yes | Yes |
| Filter Templates | No | No | Yes | Yes |

### Operations

| Feature | Starter | Professional | Business | Enterprise |
|---------|---------|--------------|----------|------------|
| Public Status Pages | Yes | Yes | Yes | Yes |
| Custom Domains | No | Plan-based | Yes | Yes |
| Maintenance Windows | No | No | Yes | Yes |
| Incident Management | No | No | Yes | Yes |
| Escalation Rules | No | No | Yes | Yes |
| Root Cause Analysis | No | No | Yes | Yes |
| SLA Management | No | No | Yes | Yes |

### Team & Security

| Feature | Starter | Professional | Business | Enterprise |
|---------|---------|--------------|----------|------------|
| Team Members | Limited | More | Many | Unlimited |
| Role-Based Access | Basic | Basic | Full RBAC | Full RBAC |
| API Access | Yes | Yes | Yes | Yes |
| SSO/SAML | No | No | No | Yes |
| Audit Logs | No | No | No | Yes |
| White-Labeling | No | No | No | Yes |

### Support

| Feature | Starter | Professional | Business | Enterprise |
|---------|---------|--------------|----------|------------|
| Support Level | Standard | Standard | Priority | Dedicated |
| Dedicated Account Manager | No | No | No | Yes |
| Onboarding | No | No | No | Yes |
| Training | No | No | No | Yes |

---

## Rate Limits by Plan

API rate limits are enforced per hour:

| Plan | API Requests/Hour |
|------|-------------------|
| Starter | 1,000 |
| Professional | 5,000 |
| Business | 10,000 |
| Enterprise | 100,000 |

---

## Monitor Types by Plan

### Available on All Plans

- **WEBSITE** - HTTP/HTTPS website availability
- **PING** - ICMP connectivity checks
- **DNS** - DNS record monitoring
- **CRON/Heartbeat** - Scheduled job monitoring
- **DOMAIN** - Domain expiry and health
- **SSL_CERT** - SSL certificate expiration

### Professional and Above

- **API** - REST API monitoring with authentication
- **PORT** - TCP/UDP port monitoring
- **GRAPHQL** - GraphQL endpoint monitoring

### Business and Above

- **SERVER** - Server metrics and health monitoring

---

## Check Intervals

Minimum check intervals vary by plan:

| Plan | Minimum Interval |
|------|------------------|
| Starter | 60 seconds |
| Professional | 30 seconds |
| Business | 10 seconds |
| Enterprise | 5 seconds (or custom) |

**Recommended Intervals**:
- **Critical Production Services**: 30-60 seconds
- **Standard Production**: 2-5 minutes
- **Non-Critical Services**: 5-15 minutes
- **Heartbeat/CRON Jobs**: Match your job schedule

---

## Data Retention

How long historical data is kept:

| Plan | Retention Period |
|------|------------------|
| Starter | 30 days |
| Professional | 90 days |
| Business | 1 year |
| Enterprise | Unlimited |

**What's Retained**:
- Check results and response times
- Incident history and logs
- Performance metrics
- Analytics data
- Audit logs (Enterprise)

---

## Status Pages

| Plan | Status Pages | Custom Domains |
|------|--------------|----------------|
| Starter | 1 | No |
| Professional | 3-5 | Plan-based |
| Business | Unlimited | Yes |
| Enterprise | Unlimited | Yes |

**Status Page Features**:
- Public or private visibility
- Custom branding (logo, colors)
- Monitor grouping
- Subscriber notifications
- Incident communication
- Uptime history display

---

## Team Members & Roles

### Included Seats

| Plan | Team Members |
|------|--------------|
| Starter | 1-5 |
| Professional | 5-25 |
| Business | 25-100 |
| Enterprise | Unlimited |

### Role Types

| Role | Description |
|------|-------------|
| **OWNER** | Full access, billing control |
| **ADMIN** | Manage team, all resources |
| **MEMBER** | Create and manage monitors |
| **VIEWER** | Read-only access |

---

## Billing Cycles

### Available Options

- **Monthly**: Pay month-to-month
- **Annual**: Save with yearly billing
- **Biennial**: 2-year commitment discount
- **Triennial**: 3-year commitment discount

### Annual Discount

Annual billing typically saves 10-20% compared to monthly billing.

---

## Custom Plans

For organizations that need a tailored solution:

### Configurable Quantities

- Number of monitors (10-1000+)
- Team member seats (3-unlimited)
- Check interval minimum (5-300 seconds)
- Data retention period (30 days - unlimited)
- Status page count (1-unlimited)

### Add-on Features

Each feature can be enabled independently:
- SMS notifications
- Slack integration
- Priority support
- Custom domains
- Multi-region monitoring
- API access

### Pricing

Custom plan pricing is calculated based on:
1. Base quantity selections
2. Enabled features
3. Billing cycle discount

Contact sales for a custom quote.

---

## Choosing the Right Plan

### Starter Plan
- Just getting started with monitoring
- Small team (1-5 people)
- Basic alerting needs
- Limited budget

### Professional Plan
- Growing monitoring needs
- Need advanced integrations
- Multiple notification channels
- API monitoring requirements

### Business Plan
- SLA requirements
- Large team (25+ people)
- Compliance needs
- Advanced analytics
- Incident management

### Enterprise Plan
- Large organization
- Complex security requirements
- SSO/SAML needed
- Dedicated support required
- Custom integrations

---

## Upgrading & Downgrading

### Upgrading

- Instant access to new features
- Prorated billing for remainder of cycle
- No data loss
- Existing configurations preserved

### Downgrading

- Takes effect at next billing cycle
- Features exceeding new plan limits will be disabled
- Data retention adjusts to new plan limits
- Monitors exceeding limits will need to be removed

### Custom Plan Changes

Custom plan configurations can be modified:
- Add monitors or team members
- Enable additional features
- Adjust check intervals
- Extend data retention

---

## Billing & Payment

### Payment Methods

- Credit/Debit Card
- Bank Transfer (Enterprise)
- PayPal
- Stripe

### Invoice Access

Access invoices at **Settings > Billing**:
- View payment history
- Download PDF invoices
- Update billing details

### Billing Address

Required for invoicing:
- Company name (for business accounts)
- Billing address
- Country
- VAT number (if applicable)

---

## Account Status

Your account can have the following statuses:

| Status | Description |
|--------|-------------|
| **ENABLED** | Normal active account |
| **DISABLED** | Account disabled (payment issues) |
| **ON_HOLD** | Temporarily suspended |
| **ARREARS** | Payment overdue |
| **CLOSED** | Account permanently closed |

---

## Next Steps

- **[Billing FAQ](/help/knowledge-base/billing-faq)** - Common billing questions
- **[Getting Started](/help/knowledge-base/getting-started-with-statusapp)** - Set up your first monitor
- **[Team Collaboration](/help/knowledge-base/team-collaboration)** - Manage your team
- **[API Reference](/help/knowledge-base/api-reference-guide)** - Programmatic access
