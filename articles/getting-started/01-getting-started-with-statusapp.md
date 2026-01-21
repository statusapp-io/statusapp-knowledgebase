---
title: Getting Started with StatusApp
slug: getting-started-with-statusapp
category: Getting Started
excerpt: Learn how to set up your first monitor and start tracking your services in minutes.
order: 1
---

# Getting Started with StatusApp

## Welcome!

StatusApp is an enterprise-grade monitoring platform that helps you track the health and performance of your websites, APIs, servers, and infrastructure. Whether you're monitoring a single website or managing hundreds of services across multiple regions, StatusApp provides real-time insights into uptime, performance, and incidents.

## Setting Up Your Account

### 1. Creating Your Account
- Visit StatusApp and create an account using your email address
- You'll receive a verification email - click the link to confirm your email address
- All new accounts start on the **Starter plan** with 10 monitors included
- You can upgrade to a paid plan anytime to access more features and monitors

### 2. Understanding the Dashboard

After signing in, you'll see your main dashboard with several key sections:

- **Dashboard**: Real-time overview of all your monitors, incidents, and system health
- **Monitors**: Create and manage all your monitoring checks (HTTP, API, DNS, Port, SSL, Heartbeat)
- **Incidents**: Track detected incidents and their resolution status
- **Status Pages**: Create public status pages to keep customers informed about service health
- **Analytics**: View detailed performance metrics, response times, and uptime statistics
- **Notification Channels**: Configure how and where you receive alerts (Email, Slack, Discord, etc.)
- **Settings**: Configure your account, billing, team members, and integrations

## Creating Your First Monitor

### Choose Your Monitor Type

StatusApp supports multiple monitor types to suit different monitoring needs:

1. **WEBSITE (HTTP/HTTPS)** - Monitor website availability and response codes
2. **API** - Monitor REST API endpoints with authentication, schema validation, and assertions
3. **GraphQL** - Monitor GraphQL endpoints with query validation
4. **PING (ICMP)** - Monitor server connectivity and network latency
5. **PORT** - Monitor TCP/UDP ports (databases, SSH, mail servers, etc.)
6. **DNS** - Monitor DNS resolution and record validation  
7. **SSL_CERT** - Monitor SSL certificate expiration dates
8. **CRON (Heartbeat)** - Monitor scheduled jobs and background tasks

### Basic Monitor Setup

1. Click **Create Monitor** from the dashboard
2. Choose your monitor type (e.g., WEBSITE for a basic website check)
3. Enter a **name** for your monitor (e.g., "My Website - Production")
4. Enter the **URL** to monitor (e.g., `https://example.com`)
5. Set the **check interval** (how often to check):
   - **Starter Plan**: 5 minutes minimum
   - **Professional Plan**: 1 minute minimum
   - **Business Plan**: 30 seconds minimum

### Advanced Monitor Configuration

#### Multi-Region Monitoring
Monitor from multiple geographic locations to detect regional outages:
- **North America**: United States (multiple regions), Canada
- **Europe**: UK, Germany, France, Ireland
- **Asia Pacific**: Singapore, Tokyo, Sydney
- **South America**: Brazil
- **Africa**: South Africa

#### Timeout Settings
Set how long to wait before marking a check as failed (default: 30 seconds)

#### Response Validation
- **Status Code Checking**: Verify specific HTTP status codes (200, 301, etc.)
- **Response Body Validation**: Check for specific text or patterns in responses
- **Schema Validation**: Validate JSON/GraphQL responses against schemas
- **Assertions**: Create custom validation rules for API responses

#### Authentication Options (For API/HTTP monitors)
- **Bearer Token**: JWT or API tokens
- **Basic Authentication**: Username and password
- **API Key**: Custom API key headers
- **OAuth 2.0**: OAuth token authentication
- **Custom Headers**: Add any custom authentication headers

## Understanding Monitor Status

Your monitors display real-time status indicators:

- **✅ UP**: Service is responding normally
- **⚠️ DEGRADED**: Service is responding but with performance issues or partial failures
- **❌ DOWN**: Service is not responding or returning errors
- **⏸️ PAUSED**: Monitor is temporarily paused (no checks being performed)
- **🔄 CHECKING**: Monitor check is currently in progress

## Setting Up Notification Channels

StatusApp uses a flexible notification channel system that lets you configure where alerts are sent.

### Creating Notification Channels

1. Go to **Settings → Notification Channels**
2. Click **Create Channel**
3. Choose your channel type and configure it:

#### Email Notifications
- Add one or more email addresses (comma-separated)
- Supports team distribution lists
- HTML-formatted incident reports

#### Slack Integration
- Connect your Slack workspace
- Choose specific channels for alerts
- Rich message formatting with status colors

#### Discord Integration
- Create a Discord webhook URL
- Configure rich embeds with color-coded alerts
- Direct channel notifications

#### Telegram Integration
- Connect your Telegram bot
- Get chat ID for your group or channel
- Markdown-formatted messages

#### SMS Alerts (Paid Plans)
- Add your phone number with country code
- Receive critical alerts via SMS
- Available on Starter plans and above

#### Advanced Integrations
- **PagerDuty**: Create incidents and trigger on-call escalations
- **Opsgenie**: Alert management with priority routing
- **Microsoft Teams**: Incoming webhooks to Teams channels
- **Custom Webhooks**: Send alerts to any HTTP endpoint

### Assigning Channels to Monitors

After creating notification channels:
1. Edit any monitor
2. Go to the **Notifications** tab
3. Select which channels should receive alerts for this monitor
4. Save your changes

You can assign different notification channels to different monitors, allowing flexible alert routing.

## Understanding Incidents

When a monitor detects a problem, StatusApp automatically creates an incident:

- **Automatic Detection**: Incidents are created when monitors fail based on your configuration
- **Severity Levels**: Incidents are categorized as LOW, MEDIUM, HIGH, or CRITICAL
- **Status Tracking**: Track incidents from detection through resolution
- **MTTR Metrics**: View Mean Time To Recovery for performance analysis
- **Root Cause Analysis**: Document root causes to prevent future incidents
- **Status Updates**: Post updates to keep stakeholders informed

Incidents can be managed from the **Incidents** dashboard where you can:
- View all active and resolved incidents
- Add comments and status updates
- Assign incidents to team members
- Document root causes and preventive measures
- Export incident reports

## Best Practices for New Users

### 1. Start with Critical Services
Begin by monitoring your most important services first:
- Production websites and APIs
- Customer-facing applications
- Critical infrastructure components
- Database servers and core services

### 2. Use Multi-Region Monitoring
For critical services, enable monitoring from multiple regions to:
- Detect region-specific outages
- Understand global performance
- Avoid false positives from single-region issues
- Provide better insights for CDN performance

### 3. Configure Appropriate Check Intervals
- **Critical Services**: 1-2 minute intervals
- **Standard Services**: 5 minute intervals
- **Background Services**: 10-15 minute intervals
- **Heartbeat Jobs**: Match your job schedule (daily, hourly, etc.)

### 4. Set Up Multiple Notification Channels
Create redundant alerting to ensure you never miss critical issues:
- **Primary**: Email and Slack for team awareness
- **Critical**: SMS for urgent alerts
- **Escalation**: PagerDuty or Opsgenie for on-call rotations

### 5. Create Status Pages
Keep customers informed during incidents:
- Create public status pages for customer-facing services
- Subscribe customers to incident updates
- Maintain transparency during outages
- Build trust through proactive communication

### 6. Review Analytics Regularly
Use the Analytics dashboard to:
- Identify performance trends
- Spot degradation before it becomes critical
- Understand uptime patterns
- Calculate SLA compliance
- Optimize check intervals based on actual needs

### 7. Invite Team Members
Collaborate effectively by inviting team members:
- Assign appropriate roles (Owner, Admin, Member, Viewer)
- Distribute monitoring responsibilities
- Enable team members to respond to incidents
- Share knowledge base access

## Quick Start Checklist

- [ ] Create your first monitor for a critical service
- [ ] Enable multi-region monitoring for production services
- [ ] Set up at least 2 notification channels (e.g., Email + Slack)
- [ ] Create a public status page for customer communication
- [ ] Invite team members if applicable
- [ ] Configure monitor timeout and validation settings
- [ ] Test notifications to ensure they're working
- [ ] Review the Analytics dashboard to understand baseline performance

## Next Steps

Now that you've set up your first monitor, explore these guides to get the most out of StatusApp:

- **[Understanding Monitor Types](/help/knowledge-base/monitor-types-overview)** - Learn about all available monitor types and when to use each one
- **[Creating Status Pages](/help/knowledge-base/creating-status-pages)** - Set up public status pages for your services
- **[Incident Management Basics](/help/knowledge-base/incident-lifecycle)** - Learn how to manage and resolve incidents effectively
- **[Setting Up Notifications](/help/knowledge-base/notification-channels-overview)** - Configure advanced notification channels and alert routing
- **[Understanding Analytics](/help/knowledge-base/understanding-analytics)** - Interpret uptime metrics and performance trends
- **[API Reference Guide](/help/knowledge-base/api-reference-guide)** - Access StatusApp programmatically via our REST API
- **[Understanding Billing Plans](/help/knowledge-base/understanding-billing-plans)** - Learn about plans, features, and pricing

## Need Help?

If you have questions or need assistance:

- **Knowledge Base**: Browse our comprehensive articles for detailed guides
- **Support**: Email support@statusapp.io for direct assistance  
- **Status Page**: Check status.statusapp.io for platform status
- **Documentation**: Visit our docs for technical references

Welcome to StatusApp! We're excited to help you monitor your services and maintain high availability.