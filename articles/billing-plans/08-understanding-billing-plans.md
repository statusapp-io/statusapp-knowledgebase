---
title: Understanding Billing Plans
slug: understanding-billing-plans
category: Billing Plans
excerpt: Learn about StatusApp pricing plans, features, and billing management.
order: 8
---

# Understanding Billing Plans

## Available Plans

StatusApp offers five pricing tiers designed to scale with your monitoring needs:

### Free Plan

**Price**: $0/month

**Features**:
- 1 monitor
- 5-minute check interval
- Single region monitoring
- Email notifications only
- 7-day data retention
- 1 team member
- Basic support

**Best For**: Testing the platform, personal projects

### Starter Plan

**Price**: Starting at $19/month

**Features**:
- Up to 10 monitors
- 1-minute check interval
- Multi-region monitoring (3 regions)
- Email & Slack notifications
- 30-day data retention
- 5 team members
- 1 status page
- Standard support

**Best For**: Small teams, getting started with monitoring

### Professional Plan

**Price**: Starting at $79/month

**Features**:
- Up to 50 monitors
- 30-second check interval
- Multi-region monitoring (5 regions)
- All notification types (Email, Slack, SMS, Discord, Telegram, PagerDuty, Opsgenie, MS Teams, Webhooks)
- 90-day data retention
- 25 team members
- 5 status pages
- API access (5,000 requests/hour)
- Custom headers & request bodies
- Response validation & assertions
- Priority support

**Best For**: Growing teams, comprehensive monitoring needs

### Business Plan

**Price**: Starting at $299/month

**Features**:
- Up to 200 monitors
- 15-second check interval
- Multi-region monitoring (all regions)
- All notification types
- 365-day data retention
- Unlimited team members
- Unlimited status pages
- API access (10,000 requests/hour)
- Custom domains for status pages
- Performance regression detection
- Advanced analytics & insights
- Root cause analysis
- Priority support with SLA

**Best For**: Mid to large companies, business-critical monitoring

### Enterprise Plan

**Price**: Custom pricing

**Features**:
- Unlimited monitors
- Custom check intervals (as low as 5 seconds)
- Multi-region monitoring (all regions + custom regions)
- All notification types
- Unlimited data retention
- Unlimited team members
- Unlimited status pages
- API access (100,000 requests/hour)
- Custom domains & white-label branding
- Dedicated infrastructure
- Custom integrations & development
- 99.99% uptime SLA
- 24/7 dedicated support
- Custom security requirements
- SSO & SAML integration

**Best For**: Large enterprises, mission-critical systems

## Feature Comparison

| Feature | Free | Starter | Professional | Business | Enterprise |
|---------|------|---------|--------------|----------|------------|
| **Monitors** | 1 | 10 | 50 | 200 | Unlimited |
| **Check Interval** | 5 min | 1 min | 30 sec | 15 sec | Custom |
| **Checks/Month** | 8,640 | 432K | 4.3M | 17.3M | Unlimited |
| **Team Members** | 1 | 5 | 25 | Unlimited | Unlimited |
| **Status Pages** | 0 | 1 | 5 | Unlimited | Unlimited |
| **Monitoring Regions** | 1 | 3 | 5 | All | All + Custom |
| **Email Alerts** | ✓ | ✓ | ✓ | ✓ | ✓ |
| **SMS Alerts** | ✗ | ✗ | ✓ | ✓ | ✓ |
| **Slack/Discord/Teams** | ✗ | ✓ | ✓ | ✓ | ✓ |
| **PagerDuty/Opsgenie** | ✗ | ✗ | ✓ | ✓ | ✓ |
| **Webhooks** | ✗ | ✗ | ✓ | ✓ | ✓ |
| **Data Retention** | 7 days | 30 days | 90 days | 365 days | Unlimited |
| **API Access** | ✗ | ✗ | ✓ | ✓ | ✓ |
| **API Rate Limit** | - | - | 5K/hr | 10K/hr | 100K/hr |
| **Custom Headers** | ✗ | ✗ | ✓ | ✓ | ✓ |
| **Response Validation** | ✗ | ✗ | ✓ | ✓ | ✓ |
| **GraphQL Monitoring** | ✗ | ✗ | ✓ | ✓ | ✓ |
| **SSL Cert Monitoring** | ✗ | ✓ | ✓ | ✓ | ✓ |
| **DNS Monitoring** | ✗ | ✓ | ✓ | ✓ | ✓ |
| **CRON/Heartbeat** | ✗ | ✓ | ✓ | ✓ | ✓ |
| **Custom Domains** | ✗ | ✗ | ✗ | ✓ | ✓ |
| **White Label** | ✗ | ✗ | ✗ | ✓ | ✓ |
| **Regression Detection** | ✗ | ✗ | ✗ | ✓ | ✓ |
| **Root Cause Analysis** | ✗ | ✗ | ✗ | ✓ | ✓ |
| **Support** | Community | Standard | Priority | Priority + SLA | 24/7 Dedicated |

## Monitor Types by Plan

All paid plans include these monitor types:

- **WEBSITE** - HTTP/HTTPS uptime monitoring
- **API** - Advanced API monitoring with authentication, schema validation, assertions
- **GRAPHQL** - GraphQL API monitoring with query validation
- **PING** - ICMP ping monitoring
- **PORT** - TCP/UDP port availability
- **DNS** - DNS record validation
- **SSL_CERT** - SSL certificate expiration tracking
- **CRON** - Heartbeat monitoring for scheduled jobs

Free plan only includes: WEBSITE monitoring

## Plan Limits Summary

| Limit | Free | Starter | Professional | Business | Enterprise |
|-------|------|---------|--------------|----------|------------|
| Monitors | 1 | 10 | 50 | 200 | Unlimited |
| Checks/Month | 8,640 | 432K | 4.3M | 17.3M | Unlimited |
| Check Interval (min) | 300s | 60s | 30s | 15s | Custom |
| Team Members | 1 | 5 | 25 | Unlimited | Unlimited |
| SMS Alerts/Month | 0 | 0 | 100 | 500 | Unlimited |
| Email Alerts/Month | 100 | 1,000 | 10,000 | Unlimited | Unlimited |
| Status Pages | 0 | 1 | 5 | Unlimited | Unlimited |
| Data Retention (days) | 7 | 30 | 90 | 365 | Unlimited |
| API Requests/Hour | 0 | 0 | 5,000 | 10,000 | 100,000 |
| Notification Channels | 1 | 5 | 25 | Unlimited | Unlimited |

## Understanding Check Limits

### What is a "Check"?

A check is one monitoring request to your endpoint. For example:
- Website monitor runs every 5 minutes = 288 checks/day
- API monitor runs every 30 seconds = 2,880 checks/day
- Multi-region checks multiply this (3 regions = 3× checks)

### Calculating Your Monthly Checks

```
Checks per month = (Monitors × Regions × Checks per hour × 24 × 30)

Example - Professional Plan:
10 monitors × 3 regions × 120 checks/hour × 24 × 30 = 2,592,000 checks/month
```

### What Happens When You Hit Limits?

When approaching plan limits:
1. **80% Usage** - Warning email sent
2. **90% Usage** - Additional warning with upgrade prompt
3. **100% Usage** - Checks slow down to stay within limit or upgrade required

You can always upgrade mid-month to increase limits immediately.

## Billing Basics

### Subscription Billing

- Billed monthly or annually
- Automatic renewal on the same date
- Cancel anytime with no penalty
- Annual billing: Save 20% vs monthly

### Monthly Billing

- Charged on the same day each month
- Invoices emailed automatically
- Payment methods: Credit card, wire transfer (Business+)
- Upgrades/downgrades take effect immediately

### Annual Billing

- 20% discount compared to monthly pricing
- Charged once per year
- 30-day cancellation notice for refund
- Prorated refunds available

### How Plan Features Are Enforced

Features are enforced based on your active plan:
- **Monitors**: New monitors disabled if over limit
- **Check Interval**: Automatically adjusted to plan maximum
- **Team Members**: Cannot invite more than limit
- **API Access**: Rate limited based on plan
- **Notifications**: SMS limited by plan quota

## Upgrading Your Plan

### Why Upgrade?

Upgrade when you need:
- More monitors
- Faster check intervals
- Additional team members
- More notification types
- Better API access
- Longer data retention

### How to Upgrade

1. Navigate to **Settings → Billing → Plans**
2. Click **Upgrade** on desired plan
3. Review feature comparison
4. Confirm upgrade
5. Features activate immediately

### Prorated Billing

When you upgrade mid-cycle:
1. Remaining time credit calculated from current plan
2. New plan price prorated for remainder of cycle
3. Charged only the difference
4. Next bill will be full new plan price

**Example**:
- Currently on Starter ($19/month), 15 days remaining
- Upgrade to Professional ($79/month)
- Credit for Starter: $9.50 (15/30 × $19)
- Prorated Professional: $39.50 (15/30 × $79)
- Charge today: $30 ($39.50 - $9.50)

## Downgrading Your Plan

### Before Downgrading

Consider these impacts:
- Monitor limits may require disabling monitors
- Team members may need removal
- Data access may be limited
- Feature access will be restricted

### How to Downgrade

1. Go to **Settings → Billing → Plans**
2. Click **Change Plan** on lower tier
3. Review feature changes and limitations
4. Select which monitors to keep (if over limit)
5. Select which team members to keep (if over limit)
6. Confirm downgrade
7. Changes take effect at next billing cycle

### What Happens to Your Data?

- All monitoring data is retained
- Data older than new retention limit becomes inaccessible
- Data is not deleted - available again if you upgrade
- Disabled monitors are paused, not deleted

### Example Downgrade Scenario

**Professional → Starter:**
- Currently: 30 monitors → New limit: 10 monitors
- Action: Choose 10 monitors to keep active, others paused
- Currently: 15 team members → New limit: 5 members
- Action: Choose 5 members to keep, others removed
- Currently: 90-day retention → New: 30-day retention
- Data beyond 30 days becomes inaccessible (not deleted)

## Free Trial

### Trial Period Details

- **Duration**: 14-day free trial
- **Plan**: Professional plan features
- **Credit Card**: Not required to start
- **Limitations**: Full access to all Professional features
- **Conversion**: Add payment method before trial ends to continue

### Starting Your Free Trial

1. Sign up for a new StatusApp account
2. Trial starts automatically with Professional features
3. Create monitors, set up notifications, invite team
4. Receive reminder emails at 7, 3, and 1 days remaining

### During the Trial

You get full access to:
- Up to 50 monitors
- All monitor types
- Multi-region monitoring
- All notification channels
- API access
- Team collaboration
- Status pages

### After Trial Ends

**Option 1: Upgrade to Paid Plan**
- Add payment method
- Select plan (Starter, Professional, Business, or Enterprise)
- Continue uninterrupted service
- Keep all configurations

**Option 2: Downgrade to Free Plan**
- No payment method needed
- Account limited to Free plan features
- Choose which monitor to keep (Free = 1 monitor)
- Data preserved for 30 days

**Option 3: Cancel Account**
- Account disabled
- Data retained for 30 days
- Can reactivate within 30 days
- After 30 days, data permanently deleted

## Payment Methods

### Accepted Payment Types

- **Credit Cards**: Visa, Mastercard, American Express, Discover
- **Debit Cards**: All major debit cards
- **Wire Transfer**: Available for Business and Enterprise plans
- **Purchase Orders**: Enterprise plans only
- **ACH**: Available for Enterprise plans

### Managing Payment Methods

#### Adding a Payment Method

1. Navigate to **Settings → Billing → Payment Methods**
2. Click **Add Payment Method**
3. Enter card number, expiration, CVV
4. Enter billing address
5. Click **Save**
6. Optionally set as default payment method

#### Updating Your Card

1. Go to **Settings → Billing → Payment Methods**
2. Click on the card you want to update
3. Click **Update Card**
4. Enter new card details
5. Click **Save**

#### Removing a Payment Method

1. Go to **Payment Methods**
2. Click the card to remove
3. Click **Remove Card**
4. Confirm removal (must have another payment method first)

### Payment Security

- All payments processed via Stripe
- PCI DSS Level 1 compliant
- Card details never stored on our servers
- End-to-end encryption
- 3D Secure authentication supported

## Invoices and Receipts

### Viewing Invoices

1. Go to **Settings → Billing → Invoices**
2. See all past invoices
3. View upcoming charges
4. Download PDFs
5. Email invoices to accounting

### Invoice Information

Each invoice includes:
- Invoice number and date
- Billing period (from/to dates)
- Itemized charges by feature
- Plan fees
- Usage fees (if applicable)
- Discounts and credits
- Subtotal, tax, and total
- Payment method used
- Payment status (paid, pending, failed)

### Downloading Invoices

- **PDF Format**: Download individual invoices
- **CSV Export**: Export all invoices for accounting
- **Email**: Forward invoices to accounting team
- **Print**: Print invoices directly

### Failed Payment Recovery

If a payment fails:
1. Email notification sent immediately
2. Automatic retry after 3 days
3. Second retry after 7 days
4. Final notice at 10 days
5. Account suspended at 14 days if still unpaid

To resolve:
- Update payment method
- Contact support if you believe there's an error
- Payment will automatically retry once updated

## Tax Information

### Adding Tax ID

1. Go to **Settings → Billing → Tax Information**
2. Click **Add Tax ID**
3. Select country and tax ID type:
   - VAT (Value Added Tax) - EU
   - EIN (Employer Identification Number) - USA
   - GST (Goods and Services Tax) - India, Australia, Canada
   - ABN (Australian Business Number) - Australia
   - Other country-specific IDs
4. Enter tax ID number
5. Click **Save**

Tax ID is automatically included on all future invoices.

### Tax Calculation

- Tax calculated based on your billing address
- Rates vary by location and applicable laws
- US customers: State sales tax where applicable
- EU customers: VAT (reverse charge if valid VAT ID)
- Other regions: Local tax laws apply

### Tax on Invoices

All invoices show:
- Subtotal (before tax)
- Tax rate applied
- Tax amount
- Total (including tax)

### Tax Exemption

If your organization is tax-exempt:
1. Email your tax exemption certificate to billing@statusapp.io
2. Include account email and company name
3. We'll apply exemption within 2 business days
4. Future invoices will show $0 tax

## Discounts and Promotions

### Annual Billing Discount

Save 20% by paying annually vs monthly:

| Plan | Monthly Price | Annual Price | Savings |
|------|---------------|--------------|---------|
| Starter | $19/month | $182/year ($15.17/month) | $46/year |
| Professional | $79/month | $758/year ($63.17/month) | $190/year |
| Business | $299/month | $2,870/year ($239.17/month) | $718/year |

### Nonprofit Discount

Eligible nonprofit organizations receive 25% off all paid plans:

1. Email nonprofit@statusapp.io
2. Include 501(c)(3) documentation or equivalent
3. Verification within 3-5 business days
4. Discount applied retroactively to current cycle

### Education Discount

Academic institutions and student projects receive 50% off:

1. Email education@statusapp.io with .edu email address
2. Include proof of enrollment/employment
3. Discount valid for duration of enrollment
4. Renewable annually

### Startup Program

Early-stage startups (< 2 years old, < $1M funding) qualify for 6 months free Professional:

1. Apply at statusapp.io/startups
2. Include company info and stage
3. Approval within 5 business days
4. After 6 months, 50% off for additional 6 months

### Applying Promo Codes

Have a promo code?

1. Go to **Settings → Billing → Promo Code**
2. Enter code
3. Click **Apply**
4. Discount shown on next invoice
5. Terms displayed (duration, percentage, etc.)

## Cancellation Policy

### How to Cancel

1. Navigate to **Settings → Billing**
2. Click **Cancel Subscription**
3. Select cancellation reason (helps us improve)
4. Optionally provide feedback
5. Click **Confirm Cancellation**

### What Happens When You Cancel

**Immediate Effects:**
- Cancellation confirmation email sent
- Access continues until end of billing period
- No further charges after current period

**At End of Billing Period:**
- Account downgraded to Free plan (1 monitor)
- Select which monitor to keep active
- All data retained for 30 days
- Team members notified of cancellation

**Data Retention:**
- All data saved for 30 days after cancellation
- Can export data during this period
- Reactivate any time within 30 days
- After 30 days, data permanently deleted

### Reactivating After Cancellation

Within 30 days:
1. Log back into your account
2. Go to **Billing**
3. Click **Reactivate Subscription**
4. Select plan
5. Add payment method
6. All previous data and configurations restored

### Refund Policy

**Standard Policy:**
- No refunds for unused portions of monthly subscriptions
- Annual subscriptions: Prorated refund if cancelled within first 30 days
- After 30 days: No refunds, but service continues until end of annual period

**Exceptions:**
- Billing errors: Full refund processed within 5-10 business days
- Service outages: Prorated credit applied to account
- Contact support@statusapp.io for billing disputes

## Frequently Asked Questions

### Can I change plans at any time?

Yes! Upgrade any time with immediate effect. Downgrades take effect at the end of your current billing cycle.

### What happens if I exceed my plan limits?

You'll receive warnings at 80% and 90% usage. At 100%, monitors will slow down or you'll need to upgrade. You can upgrade mid-cycle.

### Do unused checks roll over?

No, check quotas reset each billing month and don't roll over.

### Can I pause my subscription?

Not directly, but you can pause individual monitors or downgrade to the Free plan to keep your account active without charges.

### How do I get an Enterprise quote?

Contact sales@statusapp.io or fill out the form at statusapp.io/enterprise with your requirements.

### Is there a long-term contract?

No, all plans are month-to-month or annual (with annual discount). Cancel anytime.

### Can I use my own infrastructure?

Enterprise plans can include private deployment options. Contact sales for details.

### What payment methods do you accept?

Credit/debit cards for all plans. Wire transfer and ACH available for Business and Enterprise.

### Do you offer refunds?

Annual plans can be refunded within 30 days. Monthly plans are non-refundable except for billing errors.

### Can I transfer monitors between accounts?

Not automatically, but support can help migrate configurations. Contact support@statusapp.io.

### What happens to my data if I don't pay?

Account suspended after 14 days of failed payment. Data retained for 30 days. After that, permanently deleted.

## Support by Plan

### Free Plan Support

- **Documentation**: Full access to knowledge base and guides
- **Community**: Community forum access
- **Email**: Limited email support (72-hour response)

### Starter & Professional Support

- **Documentation**: Full knowledge base access
- **Email**: Standard email support (24-hour response)
- **Chat**: In-app chat support during business hours
- **Onboarding**: Guided setup resources

### Business Support

- **Documentation**: Full knowledge base + premium guides
- **Email**: Priority email support (2-hour response)
- **Chat**: Priority in-app chat (24/7)
- **Phone**: Business hours phone support
- **Account Manager**: Dedicated account manager
- **SLA**: 99.9% uptime SLA

### Enterprise Support

- **Documentation**: Full access + custom documentation
- **Email**: 24/7 email support (1-hour response)
- **Chat**: 24/7 priority chat support
- **Phone**: 24/7 phone support
- **Dedicated Team**: Dedicated support team
- **Account Manager**: Senior account manager
- **Technical Account Manager**: For technical guidance
- **SLA**: 99.99% uptime SLA with penalties
- **Custom**: On-site support available

## Cost Management Best Practices

### 1. Right-Size Your Plan

- Start with a plan that matches your current needs
- Don't over-provision monitors you won't use
- Upgrade as you grow rather than starting high

### 2. Optimize Check Intervals

- Not everything needs 30-second checks
- Use 5-minute intervals for less critical services
- Save faster checks for business-critical endpoints

### 3. Use Multi-Region Strategically

- Enable all regions only for critical services
- Use 1-2 regions for internal services
- Remember: More regions = more checks = higher cost

### 4. Monitor Your Usage

- Check **Billing → Usage** dashboard monthly
- Set up budget alerts
- Review which monitors generate most checks

### 5. Audit Team Members

- Remove inactive team members
- Use role-based access (Business+) for better control
- Regularly review who needs access

### 6. Consolidate Notification Channels

- Use fewer channels efficiently
- Group related monitors to same channels
- Avoid redundant notifications

### 7. Leverage API for Automation

- Use API to create/delete monitors dynamically
- Pause monitors when services are down for maintenance
- Automate monitor lifecycle with deployments

### 8. Consider Annual Billing

- Save 20% with annual commitment
- Predictable costs for budgeting
- Ideal if you're confident in long-term need

### 9. Take Advantage of Discounts

- Apply for nonprofit/education discounts if eligible
- Use startup program if you qualify
- Watch for seasonal promotions

### 10. Review Quarterly

- Set calendar reminder for quarterly billing review
- Assess if current plan still fits needs
- Adjust as business needs change

## Getting Help with Billing

### Billing Questions

Email: billing@statusapp.io
- Invoice questions
- Payment issues
- Plan changes
- Refund requests
- Tax documentation

### Sales Inquiries

Email: sales@statusapp.io
- Enterprise quotes
- Custom requirements
- Volume discounts
- Partnership opportunities

### Technical Support

Email: support@statusapp.io
- Account technical issues
- Integration help
- Bug reports
- Feature questions

### Response Times

- Free: 72 hours
- Starter/Pro: 24 hours
- Business: 2 hours
- Enterprise: 1 hour (24/7)

## Next Steps

Ready to get started or need to adjust your plan?

- **[Start Free Trial](https://statusapp.io/signup)** - 14 days, no credit card required
- **[View Pricing](https://statusapp.io/pricing)** - Compare all plans in detail
- **[Getting Started Guide](/articles/getting-started/getting-started-with-statusapp)** - Set up your first monitor
- **[Understanding Monitor Types](/articles/monitors/understanding-monitor-types)** - Learn about monitoring options
- **[API Reference](/articles/api/api-reference-guide)** - Programmatic access documentation
- **[Contact Sales](mailto:sales@statusapp.io)** - Enterprise and custom plans

## Related Articles

- **[Getting Started with StatusApp](/articles/getting-started/getting-started-with-statusapp)**
- **[Understanding Monitor Types](/articles/monitors/understanding-monitor-types)**
- **[Setting Up Notification Channels](/articles/alerting-notifications/notification-channels-guide)**
- **[Team Collaboration](/articles/team-collaboration/team-collaboration)**
- **[API Reference Guide](/articles/api/api-reference-guide)**