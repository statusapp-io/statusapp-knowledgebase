---
title: Creating Status Pages for Customers
slug: creating-status-pages
category: Status Pages
excerpt: Learn how to create and customize public status pages to keep your customers informed about service status.
order: 3
---

# Creating Status Pages for Customers

## What is a Status Page?

A status page is a public website that displays the real-time status of your services. It allows your customers to:
- Check if services are up or down
- View incident history
- Subscribe to status updates
- See estimated resolution times

## Creating Your First Status Page

### Step 1: Access Status Pages
1. Log in to StatusApp
2. Go to **Status Pages** from the main menu
3. Click **Create Status Page**

### Step 2: Basic Information
- **Page Name**: Name visible to customers (e.g., "Acme Corp Status")
- **Domain**: Your custom domain (e.g., status.acmecorp.com)
- **Description**: Brief description of your services
- **Timezone**: Select your company's timezone

### Step 3: Design Customization

#### Branding
- **Logo**: Upload your company logo
- **Color Scheme**: Choose colors matching your brand
- **Custom CSS**: Add custom styling (Premium feature)
- **Favicon**: Custom browser tab icon

#### Layout Options
- **Show Monitors**: Display individual services
- **Show Past Incidents**: Display incident history
- **Show Metrics**: Display uptime percentages
- **Show Team**: Display contact information

### Step 4: Add Components

Components are the services/systems displayed on your status page.

1. Click **Add Component**
2. Select monitors to include
3. Customize component name and description
4. Organize into groups (optional)

### Step 5: Configure Settings

#### Incident Notifications
- **Automatic Updates**: Auto-update status based on monitor status
- **Notification Email**: Email for incident notifications
- **Webhook**: Send incident data to external systems

#### Appearance
- **Page Title**: Browser tab title
- **Language**: Default language for the page
- **Show API Documentation**: Link to API status
- **Show Subscribe Section**: Email subscription for updates

## Publishing Your Status Page

### Enable Public Access
1. Toggle **Public** to make the page visible
2. Choose access level:
   - **Public**: Anyone can view
   - **Private with Link**: Share specific URL
   - **Protected**: Require password

### Custom Domain Setup

#### Using StatusApp Subdomain
- Automatically available (e.g., status.statusapp.io)
- No configuration needed
- Free to use

#### Using Your Custom Domain

1. In StatusApp: Copy the **CNAME Record**
2. In your DNS provider (Namecheap, GoDaddy, Route 53, etc.):
   - Add a CNAME record pointing to StatusApp
   - Example: `status.acmecorp.com` → `cname.statusapp.io`
3. Wait for DNS propagation (usually 15 minutes - 48 hours)
4. StatusApp will automatically configure SSL certificate

#### Verification
- StatusApp will display a green checkmark when domain is active
- Test by visiting your status page URL
- SSL certificate is automatic with Let's Encrypt

## Managing Components

### Adding Services
1. Click **Edit Components**
2. Click **Add Component**
3. Link to existing monitors or create new ones
4. Set component visibility

### Grouping Components

Organize services into logical groups:
- **API Services**: All API-related services
- **Databases**: All database systems
- **Web Infrastructure**: CDN, load balancers
- **Third-Party Services**: External dependencies

### Hiding Components
- Hide sensitive infrastructure components
- Show only customer-facing services
- Use groups to organize visibility

## Understanding Status Indicators

### Component Status
- **Operational (Green)**: Service is running normally
- **Degraded Performance (Yellow)**: Service is responding but slowly
- **Partial Outage (Orange)**: Some users affected
- **Major Outage (Red)**: Service is down

### System Status Summary
Automatically calculated based on component statuses:
- **All Systems Operational**: All components green
- **System Degradation**: Some yellows
- **Partial Service Disruption**: Mix of yellow/orange/red
- **Major Service Disruption**: Multiple red components

## Incident Reporting

### Automatic Incidents
When a monitor goes down, StatusApp automatically:
1. Creates an incident
2. Updates status page
3. Sends notifications
4. Logs incident details

### Manual Incidents

1. Click **Report Incident** on status page
2. Enter incident details:
   - **Title**: Brief description
   - **Affected Components**: Which services are impacted
   - **Status**: Investigating, Identified, Monitoring, Resolved
   - **Description**: Detailed explanation

### Incident Updates

1. Click **Update Incident**
2. Add new information
3. Notify subscribers of updates
4. Update affected components
5. Mark as resolved when complete

## Email Subscriptions

### Enable Subscription Feature
1. Go to Status Page **Settings**
2. Enable **Allow Email Subscriptions**
3. Customize subscription message

### What Subscribers Receive
- Incident notifications
- Incident updates
- Resolution notifications
- Scheduled maintenance alerts

### Managing Subscribers
- View subscriber list in dashboard
- Bulk send announcements
- Export subscriber data
- Manage preferences

## Scheduled Maintenance

### Creating Maintenance Windows

1. Click **Schedule Maintenance**
2. Enter details:
   - **Title**: Maintenance description
   - **Date & Time**: Start and end times
   - **Affected Components**: Which services will be affected
   - **Automatic Resolution**: Auto-resolve when time ends

### Customer Notifications
- Customers are notified of upcoming maintenance
- Reminders sent 24 hours before
- Automatic status update at start time
- Automatic resolution at end time

## Analytics & Metrics

### Uptime Reporting
- **Daily Uptime**: Per-day statistics
- **Monthly Uptime**: Full month statistics
- **Custom Period**: Select any date range

### Response Time Metrics
- **Average Response Time**: Overall average
- **P95/P99 Response Times**: Percentile metrics
- **Incident Impact**: Service degradation costs

### Historical Data
- View past incident reports
- Export incident data
- Analyze incident patterns
- Track MTTR (Mean Time To Resolution)

## Best Practices

### Design
1. **Keep It Simple**: Don't overcomplicate the status page
2. **Use Your Branding**: Make it visually consistent
3. **Clear Status Indicators**: Make status obvious at a glance
4. **Mobile Responsive**: Ensure it works on all devices

### Content
1. **Honest Communication**: Always tell the truth about incidents
2. **Timely Updates**: Update frequently during incidents
3. **Provide Context**: Explain what's happening and why
4. **Transparency**: Publish incident post-mortems

### Monitoring
1. **Monitor Status Pages**: Status pages need monitoring too!
2. **Test Regularly**: Verify status page accuracy
3. **Review Incidents**: Learn from past issues
4. **Gather Feedback**: Ask customers about their experience

## Troubleshooting Status Pages

### Status Page Won't Update
1. Verify monitors are connected
2. Check monitor status updates
3. Clear browser cache
4. Verify automation is enabled

### DNS Issues
1. Verify CNAME record is correct
2. Check DNS propagation (use mxtoolbox.com)
3. Wait longer for DNS to propagate
4. Contact your DNS provider if needed

### SSL Certificate Issues
1. Verify domain is properly configured
2. Wait 24-48 hours for certificate issuance
3. Clear browser cache
4. Try in incognito window

## Advanced Features

### Custom Branding (Premium)
- Custom CSS
- White-label domain
- Subdomain options
- Customizable email templates

### API Integration
- Programmatically create incidents
- Update component status
- Query historical data
- Integrate with external systems

### Webhooks
- Send incident data to external systems
- Trigger automations
- Integrate with chat systems
- Custom integrations

## Next Steps
- Learn about [analytics](/articles/analytics/understanding-analytics)
- Set up [incident management](/articles/incidents/incident-management-basics)
- Configure [alerting](/articles/alerting-notifications/setting-up-notifications)