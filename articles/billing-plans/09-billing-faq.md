---
title: Billing FAQ & Upgrades
slug: billing-faq
category: Billing Plans
excerpt: Common questions about StatusApp billing, upgrades, and usage.
order: 9
---

# Billing FAQ & Upgrades

## Billing Questions

### When am I billed?

**First Bill**: When you enter a payment method
- If upgrading from Free: Billed immediately for the new plan
- Prorated based on remaining days in current cycle
- Receipt sent to billing email

**Recurring Charges**:
- Monthly plans: Every 30 days from billing date
- Annual plans: Every 365 days from subscription date
- Automatic renewal unless cancelled

### How do I update my payment method?

1. Go to **Settings → Billing**
2. Click **Payment Method**
3. Update card information
4. Click **Save**

Changes take effect on next billing cycle.

### Can I change my billing email?

Yes:

1. Go to **Settings → Billing**
2. Click **Billing Email**
3. Enter new email address
4. Verify email address
5. Click **Confirm**

Receipts and invoices sent to new email.

### Do you offer invoices?

Yes:

1. Go to **Settings → Billing**
2. Click **Invoices**
3. View all invoices and receipts
4. Download as PDF
5. Print for your records

Invoices available for all paid plans.

## Upgrades

### How do I upgrade my plan?

1. Go to **Settings → Billing**
2. Click **Change Plan**
3. Select new plan
4. Review pricing and features
5. Click **Upgrade**
6. Enter/confirm payment method
7. Upgrade takes effect immediately

### Prorating During Upgrades

When you upgrade mid-cycle, you're charged the difference:

**Example**:
```
Current: Starter ($19/month) - 15 days used
New: Professional ($79/month)

Prorated charge: ($79 - $19) ÷ 30 days × remaining 15 days
= $60 ÷ 30 × 15 = $30 (charged immediately)

Next billing cycle: Full $79/month
```

Prorated credit if downgrading.

### Annual Billing

Save 20% with annual billing:

**How to Enable**:
1. Click **Change Billing Period**
2. Select **Annual**
3. Pay upfront for full year
4. Automatic renewal after 12 months

**Calculating Savings**:
- Monthly Starter: $19 × 12 = $228
- Annual Starter: $182.40 (saves $45.60)
- Monthly Professional: $79 × 12 = $948
- Annual Professional: $758.40 (saves $189.60)

Switch to annual at any time. Unused monthly time credited.

## Downgrades

### How do I downgrade my plan?

1. Go to **Settings → Billing**
2. Click **Change Plan**
3. Select lower-tier plan
4. Review warning about feature loss
5. Click **Downgrade**
6. Confirm downgrade

**Important**: Downgrading removes access to high-tier features:
- Extra monitors removed
- Status pages removed
- Notification channels removed
- API access revoked

Downgrade takes effect at end of current billing cycle.

### What happens to my data?

When downgrading:
- Monitors in excess of limit are paused
- Status pages kept but not displayed
- All historical data retained
- Can upgrade to re-enable

**Example**: Downgrading from 50 to 10 monitors
- First 10 monitors stay active
- Monitors 11-50 paused
- Data preserved for all
- Upgrade to re-activate them

## Usage & Costs

### How are additional seats charged?

Additional team member seats: $10/seat/month

**Example**:
- Professional plan: 25 included seats ($79/month)
- Add 5 more seats: +$50/month
- Total: $129/month

Billed with main plan invoice. Cancel seats anytime.

### How are API calls charged?

API limits included in plan:

- Professional: 5,000 requests/hour
- Business: 50,000 requests/hour
- Enterprise: Unlimited

**Overage Charges**: $0.01 per 100 extra requests
- Only charged if you exceed limit
- Prorated hourly
- Usage shown in dashboard

**Monitoring Usage**:
1. Go to **Settings → API**
2. View current hour usage
3. View daily totals
4. View monthly totals

### Monitor Usage Billing

Monitors are based on what you've created:

**Example**:
- Professional plan: Up to 50 monitors
- Create 45 monitors: $79/month
- Create 51st monitor: Violates plan limit

**Solution**: Upgrade to Business (150 monitors) or delete unused monitors.

No overage charges for monitors - you just can't create more than your limit allows.

## Failed Payments

### What if my payment fails?

**Notification**:
- Email sent about failed payment
- 7-day retry grace period
- Service continues during grace period

**Retry Process**:
1. StatusApp attempts payment daily for 7 days
2. Updated payment method triggers immediate retry
3. Successful payment ends grace period
4. Receipt sent when payment succeeds

### Grace Period Behavior

During 7-day grace period:
- Full access to StatusApp
- Monitors continue running
- Notifications still send
- All features work normally

**After 7 days** (if still unpaid):
- Account suspended
- Monitors stop checking
- No alerts sent
- Data retained for 30 days

### How do I update payment if it fails?

1. Go to **Settings → Billing**
2. Click **Payment Method**
3. Update card information
4. Click **Save**
5. Payment retried immediately

If retry succeeds, service immediately restored and grace period ends.

## Cancellation

### How do I cancel my subscription?

1. Go to **Settings → Billing**
2. Click **Cancel Subscription**
3. Select reason (optional)
4. Confirm cancellation
5. Done

**Effective Date**:
- Immediate cancellation for monthly plans (no refund)
- End of annual cycle for annual plans (no partial refund)

### What happens after cancellation?

**Immediately**:
- Billing stops
- Monitoring pauses
- Notifications stop

**After 30 days**:
- All data deleted
- Monitors removed
- Status pages removed

### Can I reactivate after cancellation?

Yes:

1. Go to **Settings → Billing**
2. Click **Reactivate**
3. Confirm reactivation
4. Resume monitoring at same plan level

Data may be partially recovered if within 30-day window. Otherwise, you'll need to recreate monitors.

## Tax & Compliance

### Do you collect sales tax?

Yes, where applicable:

**US Sales Tax**:
- Collected for US customers
- Based on shipping address
- Varies by state (0-10%)

**VAT/GST** (International):
- Collected for EU, Canada, Australia, etc.
- Based on customer location
- Varies by country

**Tax-Exempt Organizations**:
- Exempt certificate may apply
- Contact sales@statusapp.io
- Provide proof of exempt status

### Do you offer refunds?

**Monthly Plans**: No refunds
- Service continues through end of month
- Cancel anytime for next billing date

**Annual Plans**: Pro-rated refund for early cancellation
- Refund = amount × (unused days / 365)
- Processed within 5-7 business days
- Contact support@statusapp.io to request

### Can I get an invoice for accounting?

Yes, all invoices available:

1. Go to **Settings → Billing → Invoices**
2. View all invoices
3. Download as PDF
4. Print for records

Invoices show:
- Itemized charges
- Tax amounts
- Payment method
- Invoice number and date

## Common Issues

### I'm charged but can't access my plan features

**Check your plan**:
1. Go to **Settings → Billing**
2. Click **Current Plan**
3. Verify shows correct plan

**Troubleshoot**:
- Hard refresh your browser
- Log out and log back in
- Clear browser cache
- Try different browser

**Contact support** if issue persists.

### I'm seeing charges I don't recognize

**Review recent charges**:
1. Go to **Settings → Billing → Invoices**
2. Review all recent invoices
3. Check charge dates and amounts

**Possible explanations**:
- Additional seats added
- Plan upgrade
- Overage charges
- Refund processed (appears as negative charge)

**Dispute a charge**:
- Contact support@statusapp.io within 30 days
- Include invoice number
- Explain the issue
- We'll review and resolve

### How do I get a receipt?

**Manual download**:
1. Go to **Settings → Billing → Invoices**
2. Find the invoice/receipt
3. Click Download
4. Save PDF

**Email receipt**:
- Sent to billing email automatically
- Check spam folder if not received
- Contact support to resend

## Getting Help

For billing questions or issues:

- **Email**: billing@statusapp.io
- **In-app chat**: Available in dashboard (Business+ plans)
- **Community**: community.statusapp.io
- **Business support**: support@statusapp.io

## Next Steps

- **[Billing Plans](/articles/billing-plans/billing-plans-comparison)** - Compare plans and features
- **[Getting Started](/articles/getting-started/getting-started-with-statusapp)** - Start monitoring
- **[Team Management](/articles/team-collaboration/team-collaboration)** - Add team members
