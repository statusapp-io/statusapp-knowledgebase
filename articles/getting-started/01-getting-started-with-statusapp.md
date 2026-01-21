---
title: Getting Started with StatusApp
slug: getting-started-with-statusapp
category: Getting Started
excerpt: Learn how to set up your first monitor and start tracking your services in minutes.
order: 1
---

# Getting Started with StatusApp

## Welcome!

StatusApp is an enterprise-grade monitoring platform that helps you track the health and performance of your websites, APIs, and infrastructure. Whether you're monitoring a single website or managing hundreds of services, StatusApp provides real-time insights into uptime, performance, and incidents.

## Setting Up Your Account

### 1. Signing Up
- Visit StatusApp and create an account using your email
- You'll receive a verification email - click the link to confirm your email address
- Choose your initial plan (Free to start, upgrade anytime)

### 2. Initial Configuration

After signing up, you'll see the dashboard. Here's what you need to know:

- **Dashboard**: Overview of all your monitors and their current status
- **Monitors**: Create and manage all your monitoring checks
- **Incidents**: Track and manage detected incidents
- **Status Pages**: Create public status pages to keep customers informed
- **Analytics**: View detailed performance metrics
- **Settings**: Configure integrations, notifications, and team access

## Creating Your First Monitor

### Basic Setup

1. Click **Create Monitor** from the dashboard
2. Enter your website URL (e.g., `https://example.com`)
3. Choose your monitor type:
   - **HTTP/HTTPS**: Monitor websites and APIs
   - **TCP/UDP**: Monitor server ports
   - **Ping (ICMP)**: Monitor server connectivity
   - **DNS**: Monitor DNS records

### Choosing Check Interval

Your monitoring plan determines available intervals:
- **Free Plan**: 5 minutes
- **Starter Plan**: 1 minute minimum
- **Professional Plan**: 30 seconds minimum
- **Enterprise Plan**: Custom intervals

### Advanced Options

- **Regions**: Monitor from multiple geographic locations
- **Custom Headers**: Add authentication or custom headers to requests
- **Response Validation**: Check for specific response codes, text, or schemas
- **Timeout**: Set how long to wait before considering a check failed

## Understanding Monitor Status

- **✅ Up**: Service is responding normally
- **⚠️ Degraded**: Service is responding but with issues
- **❌ Down**: Service is not responding or returning errors
- **⏱️ Checking**: Monitor is currently performing a check

## Setting Up Notifications

### Email Alerts
1. Go to **Settings > Notifications**
2. Enable **Email Alerts**
3. Enter email addresses to receive alerts
4. Choose which events trigger notifications

### Slack Integration
1. Go to **Settings > Integrations**
2. Click **Connect Slack**
3. Authorize StatusApp to post to your workspace
4. Select channels for different alert types

### Other Integrations
StatusApp supports SMS (via Twilio), Webhooks, and custom API integrations.

## Best Practices

1. **Start with Critical Services**: Monitor your most important services first
2. **Use Multiple Regions**: Get a better picture of global performance
3. **Set Up Teams**: Invite team members with appropriate permissions
4. **Create Status Pages**: Keep customers informed about incidents
5. **Review Analytics**: Understand trends and performance patterns

## Next Steps

- Read about [creating advanced monitors](/articles/monitors/advanced-monitor-configuration)
- Learn about [setting up status pages](/articles/status-pages/creating-your-first-status-page)
- Explore [team collaboration features](/articles/team-collaboration/inviting-team-members)
- Check out [API documentation](/articles/api/api-overview)

## Need Help?

If you need assistance, visit our [Support Center](/help) or email support@statusapp.io.