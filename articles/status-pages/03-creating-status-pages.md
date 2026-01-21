---
title: Creating Status Pages
slug: creating-status-pages
category: Status Pages
excerpt: Build a public status page to communicate service status with customers.
order: 3
---

# Creating Status Pages

## What is a Status Page?

A status page is a public-facing website that displays real-time status information about your services. It provides transparency to your customers by showing:

- **Service Status**: Current operational state of all monitored services
- **Incidents**: Active issues with real-time updates
- **History**: Past incidents and resolution timelines
- **Uptime Metrics**: Historical availability and reliability
- **Subscriptions**: Allow customers to receive notifications

## Why Use Status Pages?

**Proactive Communication**
- Customers see status before support tickets arrive
- Reduces support burden during outages
- Builds trust through transparency

**Incident Management**
- Centralized incident communication
- Automatic notifications to subscribers
- Professional incident history

**Professional Image**
- Demonstrates operational maturity
- Industry standard for SaaS/hosted services
- Shows reliability commitment

## Creating Your First Status Page

### Step 1: Navigate to Status Pages

1. Log in to StatusApp dashboard
2. Click **Settings** in sidebar
3. Select **Status Pages**
4. Click **Create New Status Page**

### Step 2: Basic Information

Enter basic page details:

**Name**: `Customer Status` or your brand name
- Shows as browser tab title
- Used in email notifications
- Keep short and memorable

**Slug**: `status` or `statuspage`
- Used in URL: `yourcompany.com/status`
- Must be unique
- Only letters, numbers, hyphens
- Can't be changed later (plan carefully)

**Description**: Optional tagline
- "Uptime and incident history for [Company] services"
- Displays on status page
- 100 characters max

**Company Name**: Your organization name
- Shows in header/branding
- Used in notifications
- Keep consistent with branding

**Timezone**: Select primary operating timezone
- All incident times in this timezone
- Customers see in their local timezone

### Step 3: Design & Branding

Customize appearance with your logo, brand color, and theme. See [Custom Domains](/help/knowledge-base/custom-domains) for professional branding with your own domain.

### Step 4: Add Monitors

1. Click **Add Monitor**
2. Select monitors from list  
3. Organize into groups
4. Drag to reorder
5. Save page

**Include**: Customer-facing production services only
**Exclude**: Internal databases, staging environments, sensitive infrastructure

### Step 5: Publish

1. Review all settings
2. Click **Save**
3. Status page is live at `statusapp.io/{slug}`
4. Share URL with customers

## Monitor Groups

Organize monitors into logical groups:

```
Web Platform
├─ Website
├─ API
└─ Dashboard

Payment Services
├─ Payment Processing
├─ Billing
└─ Invoicing
```

Groups help customers quickly find relevant services.

## Status Page Content
- **Public**: Anyone can view the status page
- **Private**: Only accessible via direct link (not indexed)

Choose public for customer-facing pages, private for internal use.

### Step 4: Add Monitors

**Select Monitors to Display**

After setting up basic information, select which monitors appear on your status page:

1. Scroll to **Monitors** section
2. Check the monitors you want to display
3. Selected monitors will appear on your public status page
4. Monitor status updates automatically

**Best Practices for Monitor Selection**:
- Only show customer-facing services
- Exclude internal infrastructure monitors
- Group related services logically
- Use clear, customer-friendly names

### Step 5: Organize with Monitor Groups

**Monitor Groups Tab**

Organize your monitors into logical groups for better presentation:

**Creating Groups**

1. Switch to **Monitor Groups** tab
2. Click **Create Group**
3. Enter group details:
   - **Name**: Group name (e.g., "API Services", "Web Infrastructure")
   - **Description**: Optional description of what's in this group
   - **Order**: Display order on status page (lower numbers first)
   - **Collapsed by Default**: Start with group collapsed or expanded

**Assigning Monitors to Groups**

1. Click on a group to expand it
2. Click **Assign Monitors**
3. Select monitors to add to this group
4. Drag to reorder monitors within groups

**Group Examples**:
- **Core Services**: API, Authentication, Database
- **Website & Apps**: Website, Mobile App, Customer Portal
- **Third-Party**: Payment Processing, Email Delivery
- **Infrastructure**: CDN, Load Balancers, DNS

**Ungrouped Monitors**

Monitors not assigned to a group appear in an "Other Services" section at the bottom of your status page.

### Step 6: Save and Publish

1. Review all settings on both tabs
2. Click **Create Status Page**
3. Your status page is now live!
4. Share the URL with your customers

## Customizing Email Templates

StatusApp sends email notifications to subscribers when incidents occur. You can customize these email templates to match your brand.

**Accessing Email Templates**

1. Go to your status page in **Settings → Status Pages**
2. Click the status page name
3. Click **Email Templates** tab
4. You'll see three template types

**Template Types**

**1. Incident Verification**
- Sent when someone subscribes to your status page
- Contains verification link
- Confirms email address

**2. Incident Update**
- Sent when new incident created or updated
- Contains incident details and current status
- Includes link to status page

**3. Incident Resolved**
- Sent when incident is marked as resolved
- Contains resolution details and incident duration
- Includes link to incident timeline

**Customizing Templates**

1. Click **New Template** to create custom version
2. Select template type
3. Customize:
   - **Subject Line**: Email subject with variables
   - **HTML Content**: Full email body with styling
4. Use template variables: `{{status_page_name}}`, `{{monitor_name}}`, `{{update_message}}`, etc.
5. Preview your template with sample data
6. Mark as **Active** to use it

**Template Variables Available**:
- `{{status_page_name}}` - Your status page name
- `{{brand_color}}` - Your brand color
- `{{logo_section}}` - Your logo HTML
- `{{monitor_name}}` - Affected monitor name
- `{{incident_number}}` - Unique incident ID
- `{{verification_url}}` - Subscription verification link
- `{{incident_url}}` - Link to incident page
- `{{update_message}}` - Incident update text
- `{{update_label}}` - Status label (Investigating, Identified, etc.)

**Default Templates**

StatusApp provides default templates that work out of the box. Custom templates override defaults for your status page only.

### Custom Domain Setup


**Default StatusApp Domain**

Every status page gets a free subdomain:
- Format: `statusapp.io/{your-slug}`
- Example: `statusapp.io/acme-status`
- No setup required
- SSL included
- Works immediately

#### Using Your Custom Domain

(Business & Enterprise Plans)

**Step 1: Choose Your Domain**

Decide on your status page domain:
- Common: `status.yourdomain.com`
- Alternative: `statuspage.yourdomain.com`
- Alternative: `health.yourdomain.com`

**Step 2: Get CNAME Details from StatusApp**

1. In StatusApp: Copy the **CNAME Record**
2. You'll see the target: typically `cname.statusapp.io`

**Step 3: Configure DNS**

In your DNS provider (examples: Cloudflare, Route 53, Namecheap, GoDaddy):

1. Log in to your DNS provider
2. Find DNS management for your domain
3. Create a new CNAME record:
   - **Type**: CNAME
   - **Name**: `status` (or your chosen subdomain)
   - **Target**: `cname.statusapp.io` (value from StatusApp)
   - **TTL**: 3600 or Auto
4. Save the DNS record

**Example DNS Configuration**:
```
Type:   CNAME
Name:   status
Value:  cname.statusapp.io
TTL:    3600
```

**Step 4: Wait for DNS Propagation**

- DNS changes take 15 minutes to 48 hours to propagate
- Most changes are live within 1-2 hours
- Check status in StatusApp settings
- Green checkmark appears when active

**Step 5: SSL Certificate**

- SSL certificate is automatically provisioned
- Uses Let's Encrypt for free SSL
- Renews automatically every 90 days
- Usually issues within 15 minutes of DNS propagation

**Step 6: Update Status Page Settings**

1. Go back to status page settings in StatusApp
2. Enter your custom domain: `status.yourdomain.com`
3. Click **Verify Domain**
4. StatusApp checks DNS and provisions SSL
5. Status shows "Active" when ready

#### Verification

Test your custom domain:
1. Visit `https://status.yourdomain.com` in browser
2. Verify SSL shows secure (padlock icon)
3. Check that branding displays correctly
4. Test on mobile devices

**Troubleshooting DNS Issues**:
- Use DNS checker: `whatsmydns.net`
- Verify CNAME points to correct target
- Check for conflicting A records
- Contact support if issues persist after 48 hours

## Email Subscriptions


Allow customers to subscribe to incident notifications via email.

**Enabling Subscriptions**

Subscriptions are automatically enabled on all public status pages. Customers can:
1. Visit your status page
2. Click "Subscribe to Updates"
3. Enter their email address
4. Select notification preferences
5. Verify email address
6. Receive incident notifications

**Subscription Preferences**

Subscribers can choose to receive notifications for:
- **All Incidents** - Every incident on any monitor
- **Specific Monitors** - Only incidents affecting selected monitors
- **Critical Only** - Only major outages

**Managing Subscribers**

View and manage subscribers:
1. Go to status page settings
2. Click **Subscribers** tab
3. View list of all subscribers
4. See subscription preferences
5. Manually add/remove subscribers
6. Export subscriber list

**Subscriber Notifications**

Subscribers receive emails for:
- New incidents created
- Incident status updates
- Incident resolutions
- Scheduled maintenance (if subscribed)

## Incidents on Status Pages

### Automatic Incident Creation

When a monitor goes down:
1. StatusApp automatically detects the failure
2. Incident is created and linked to monitor
3. Status page updated in real-time
4. Subscribers notified via email
5. Incident timeline starts tracking

### Manual Incident Creation

Create incidents manually for planned maintenance or issues not detected by monitors:

1. Go to **Incidents** page
2. Click **Create Incident**
3. Fill in incident details:
   - **Title**: Clear, concise description
   - **Affected Monitors**: Select impacted services
   - **Status**: Investigating, Identified, Monitoring, or Resolved
   - **Message**: Detailed explanation of the issue
   - **Severity**: Low, Medium, High, or Critical
4. Click **Create & Notify**

**Incident automatically**:
- Appears on status page
- Triggers email to subscribers
- Updates monitor status indicators
- Creates incident timeline

### Incident Status Lifecycle

**1. Investigating**
- Issue reported, team is looking into it
- Cause not yet identified
- Typical first status

**2. Identified**
- Root cause identified
- Working on fix
- ETA may be available

**3. Monitoring**
- Fix deployed
- Monitoring for stability
- Not yet fully confident

**4. Resolved**
- Issue completely fixed
- Service fully operational
- Incident closed

### Updating Incidents

Keep customers informed with regular updates:

1. Open the incident
2. Click **Post Update**
3. Change status if needed
4. Add update message
5. Click **Post Update**
6. Subscribers automatically notified

**Best Practices for Updates**:
- Update every 30-60 minutes during active incidents
- Be specific about what's happening
- Provide ETAs when possible
- Explain what you're doing to fix it
- Update when status changes

### Incident History

Past incidents appear in the **Incident History** section of your status page:
- Shows last 90 days by default
- Includes resolution time
- Links to full incident timeline
- Demonstrates transparency

## Monitor Status Indicators

### Status States

Monitors on your status page display one of these states:

**Operational (Green ✓)**
- Monitor passing all checks
- Service functioning normally
- No issues detected

**Degraded (Yellow ⚠)**
- Monitor responding but slower than normal
- Performance regression detected
- Service usable but impaired

**Down (Red ✗)**
- Monitor failing checks
- Service unavailable
- Active incident

**Maintenance (Blue 🔧)**
- Scheduled maintenance in progress
- Expected downtime
- Not counted as incident

### Overall Status

The status page header shows overall system status:

**All Systems Operational**
- All monitors green
- No active incidents
- No degraded performance

**Degraded Performance**
- One or more monitors yellow
- Service impacted but functional

**Partial Outage**
- One or more monitors down
- Some services unavailable
- Active incident

**Major Outage**
- Multiple monitors down
- Significant service disruption
- Critical incident

## Uptime Metrics

Status pages display uptime percentage for each monitor:

**Uptime Calculation**
```
Uptime % = (Successful Checks ÷ Total Checks) × 100
```

**Display Periods**:
- **Last 24 Hours**: Rolling 24-hour uptime
- **Last 7 Days**: Weekly uptime percentage
- **Last 30 Days**: Monthly uptime percentage
- **Last 90 Days**: Quarterly uptime percentage

**Uptime Bar**
- Visual bar chart showing daily uptime
- Green bars = 100% uptime
- Yellow bars = 95-99.9% uptime
- Red bars = < 95% uptime
- Click bar to see incidents on that day

## Scheduled Maintenance

Inform customers of planned downtime:

### Creating Maintenance Windows

1. Go to **Incidents** page
2. Click **Schedule Maintenance**
3. Enter maintenance details:
   - **Title**: What maintenance is being performed
   - **Start Time**: When maintenance begins
   - **End Time**: Expected completion time
   - **Affected Monitors**: Services that will be impacted
   - **Description**: Why maintenance is needed
4. Click **Schedule**

### Maintenance Notifications

Subscribers are automatically notified:
- **7 days before**: Initial notification
- **24 hours before**: Reminder
- **At start time**: Maintenance started
- **At completion**: Maintenance completed

### Auto-Resolution

Maintenance incidents automatically resolve:
- At scheduled end time
- Monitor returns to operational status
- Subscribers notified of completion
- No manual intervention needed

## Public Status Page Layout

### Standard Layout

**Header**
- Company logo (if configured)
- Status page name
- Overall status indicator
- "Subscribe to Updates" button

**Monitor Groups** (if configured)
- Collapsible sections by group
- Group name and description
- Monitors within each group
- Status indicator per monitor

**Ungrouped Monitors**
- "Other Services" section
- Monitors not assigned to groups

**Active Incidents**
- Current ongoing incidents
- Latest updates
- Affected monitors
- Incident timeline

**Uptime Metrics**
- 90-day uptime bar chart
- Per-monitor uptime percentages
- Historical reliability data

**Incident History**
- Past 90 days of incidents
- Resolution times
- Links to full incident details

**Footer**
- Powered by StatusApp (can be removed on Enterprise)
- Links and contact info (if configured)

### Mobile Responsive

Status pages are fully responsive:
- Works on phones, tablets, desktops
- Touch-friendly interface
- Optimized typography
- Fast loading times

## Analytics & Insights

Track how customers interact with your status page:

### Visitor Analytics
- Page views per day
- Unique visitors
- Peak traffic times
- Traffic sources

### Subscription Metrics
- Total subscribers
- Subscription growth rate
- Verification rate
- Unsubscribe rate

### Incident Impact
- Subscriber notifications sent
- Email open rates
- Click-through rates
- Engagement during incidents

Access analytics:
1. Click **Edit Components**
2. Click **Add Component**
3. Link to existing monitors or create new ones
4. Set component visibility

## Best Practices

### Communication Best Practices

**1. Be Proactive**
- Post incidents before customers notice
- Update frequently during incidents
- Don't wait for customers to ask

**2. Be Transparent**
- Honest about issues and impact
- Explain root causes in post-mortems
- Share what you're doing to prevent recurrence

**3. Be Clear**
- Use plain language, avoid jargon
- Explain technical issues in customer terms
- State impact clearly
- Give realistic ETAs

**4. Be Timely**
- First update within 15 minutes
- Updates every 30-60 minutes during incidents
- Final update when resolved
- Post-mortem within 48 hours for major incidents

### Design Best Practices

**1. Keep It Simple**
- Clean, minimal design
- Focus on status information
- Avoid clutter
- Easy to scan quickly

**2. Use Your Brand**
- Upload your logo
- Use brand colors
- Match your main site's look
- Custom domain for consistency

**3. Organize Logically**
- Group related monitors
- Most critical services at top
- Clear group names
- Collapse less important groups

**4. Mobile First**
- Test on mobile devices
- Readable font sizes
- Touch-friendly buttons
- Fast loading

### Monitor Selection Best Practices

**1. Customer-Facing Only**
- Show services customers use
- Hide internal infrastructure
- Exclude development/staging environments
- Focus on production services

**2. Clear Naming**
- Use customer-friendly names
- "Payment Processing" not "Stripe Integration"
- "Website" not "Nginx Load Balancer"
- "API" not "api-prod-cluster-us-east-1"

**3. Appropriate Granularity**
- Not too many monitors (overwhelming)
- Not too few (not helpful)
- 5-15 monitors is ideal
- Group microservices under single monitor

**4. Test Everything**
- Verify monitors update correctly
- Test email notifications
- Check custom domain
- Test on multiple devices

## Troubleshooting

### Status Page Not Updating

**Problem**: Monitor status changes but status page doesn't update

**Solutions**:
1. Hard refresh browser: `Ctrl+F5` (Windows) or `Cmd+Shift+R` (Mac)
2. Clear browser cache and cookies
3. Try incognito/private window
4. Check if monitor is added to status page
5. Verify status page is set to "Public"

### Custom Domain Not Working

**Problem**: Custom domain shows error or doesn't load

**Solutions**:
1. Verify CNAME record in DNS provider
2. Check CNAME points to `cname.statusapp.io`
3. Wait for DNS propagation (up to 48 hours)
4. Use DNS checker: `whatsmydns.net`
5. Remove any conflicting A records
6. Contact support if issue persists

### SSL Certificate Issues

**Problem**: Browser shows "Not Secure" or SSL error

**Solutions**:
1. Wait 15-30 minutes for SSL provisioning
2. Verify DNS is fully propagated
3. Clear browser SSL cache
4. Try different browser
5. Check StatusApp shows domain as "Active"
6. Contact support for manual SSL reissue

### Emails Not Sending

**Problem**: Subscribers not receiving incident notifications

**Solutions**:
1. Check subscriber verified their email
2. Check spam/junk folders
3. Verify email template is active
4. Test with your own email address
5. Check StatusApp email delivery logs
6. Verify email service status

### Monitors Not Grouping

**Problem**: Created groups but monitors not appearing grouped

**Solutions**:
1. Verify you assigned monitors to the group
2. Check group is not set to hidden
3. Hard refresh the status page
4. Edit status page and re-save
5. Verify monitors are selected for status page

## Plan Limits

Status page features vary by plan:

| Feature | Free | Starter | Pro | Business | Enterprise |
|---------|------|---------|-----|----------|------------|
| Status Pages | 0 | 1 | 5 | Unlimited | Unlimited |
| Monitors per Page | - | 10 | 25 | 50 | Unlimited |
| Custom Domain | ✗ | ✗ | ✗ | ✓ | ✓ |
| Email Templates | ✗ | ✗ | ✗ | ✓ | ✓ |
| Monitor Groups | ✗ | ✓ | ✓ | ✓ | ✓ |
| Subscriber Limit | - | 100 | 1,000 | 10,000 | Unlimited |
| White Label | ✗ | ✗ | ✗ | ✗ | ✓ |
| API Access | ✗ | ✗ | ✓ | ✓ | ✓ |

Upgrade your plan at **Settings → Billing → Plans**.

## Next Steps

- **[Getting Started](/help/knowledge-base/getting-started-with-statusapp)** - New to StatusApp? Start here
- **[Understanding Monitor Types](/help/knowledge-base/monitor-types-overview)** - Configure monitors for your status page
- **[Incident Management](/help/knowledge-base/incident-lifecycle)** - Learn incident management best practices
- **[Notification Channels](/help/knowledge-base/notification-channels-overview)** - Set up alerting for your team
- **[Understanding Billing](/help/knowledge-base/billing-plans-comparison)** - Plan features and pricing

## Getting Help

Need assistance with status pages?

- **Documentation**: Browse more articles in this knowledge base
- **Support Email**: support@statusapp.io
- **Live Chat**: Available in dashboard (Business+ plans)
- **Community**: community.statusapp.io