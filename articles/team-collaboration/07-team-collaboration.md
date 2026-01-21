---
title: Team Collaboration and Permissions
slug: team-collaboration
category: Team Collaboration
excerpt: Learn how to invite team members and set up role-based access control in StatusApp.
order: 7
---

# Team Collaboration and Permissions

## Overview

StatusApp supports role-based access control allowing you to invite team members with specific permissions. Different team members can have different levels of access based on their role.

## User Roles

### Owner
- **Access Level**: Full administrative access
- **Capabilities**:
  - Create and delete monitors
  - Manage team members
  - Create and modify status pages
  - Access billing and settings
  - Delete incidents
  - Configure integrations
  - Manage API keys
- **Count**: Can have multiple owners
- **Best For**: Team leads, senior engineers

### Admin
- **Access Level**: Administrative access
- **Capabilities**:
  - Create and delete monitors
  - Manage public status pages
  - Create and resolve incidents
  - Configure monitors
  - View analytics
  - Manage team members (invite only)
  - View billing information
- **Restrictions**: Cannot change billing or delete account
- **Best For**: Engineering managers, senior staff

### Member
- **Access Level**: Full operational access
- **Capabilities**:
  - Create, update, delete monitors
  - Create and manage incidents
  - View analytics
  - Configure monitors
  - View team members
  - Manage personal API keys
- **Restrictions**: Cannot manage team or settings
- **Best For**: Engineers, operations staff

### Viewer
- **Access Level**: Read-only access
- **Capabilities**:
  - View monitors and status
  - View analytics and reports
  - View incidents (read-only)
  - View status pages
  - View team information
- **Restrictions**: Cannot create or modify anything
- **Best For**: Stakeholders, executives, customers

## Inviting Team Members

### Step 1: Access Team Settings

1. Go to **Settings > Team**
2. Click **Invite Member**

### Step 2: Send Invitation

1. Enter team member's email
2. Select their role
3. Add optional welcome message
4. Click **Send Invite**

### Step 3: Invitation Process

- Invitation sent via email
- Link valid for 7 days
- Team member clicks link
- Creates account or signs in
- Automatically added to team

### Step 4: Welcome

- Team member sees welcome dashboard
- Access based on their role
- Can begin collaborating immediately

## Managing Team Members

### Viewing Team

1. Go to **Settings > Team**
2. See list of all team members
3. Shows:
   - Name and email
   - Role
   - Join date
   - Last active
   - Status (active/invited)

### Changing Roles

1. Click team member
2. Select new role from dropdown
3. Confirm change
4. New permissions take effect immediately

### Removing Team Members

1. Click team member
2. Click **Remove from Team**
3. Confirm removal
4. Access revoked immediately
5. Historical data preserved

### Resending Invitations

For pending invitations:
1. Click invited member
2. Click **Resend Invite**
3. New link sent
4. 7-day expiration

## Permission Details

### Monitor Permissions

| Action | Owner | Admin | Member | Viewer |
|--------|-------|-------|--------|--------|
| View Monitors | ✓ | ✓ | ✓ | ✓ |
| Create Monitor | ✓ | ✓ | ✓ | ✗ |
| Edit Monitor | ✓ | ✓ | ✓ | ✗ |
| Delete Monitor | ✓ | ✓ | ✓ | ✗ |
| View Analytics | ✓ | ✓ | ✓ | ✓ |
| Export Data | ✓ | ✓ | ✓ | ✗ |

### Incident Permissions

| Action | Owner | Admin | Member | Viewer |
|--------|-------|-------|--------|--------|
| View Incidents | ✓ | ✓ | ✓ | ✓ |
| Create Incident | ✓ | ✓ | ✓ | ✗ |
| Update Incident | ✓ | ✓ | ✓ | ✗ |
| Resolve Incident | ✓ | ✓ | ✓ | ✗ |
| Delete Incident | ✓ | ✗ | ✗ | ✗ |

### Status Page Permissions

| Action | Owner | Admin | Member | Viewer |
|--------|-------|-------|--------|--------|
| View Status Pages | ✓ | ✓ | ✓ | ✓ |
| Create Status Page | ✓ | ✓ | ✓ | ✗ |
| Edit Status Page | ✓ | ✓ | ✓ | ✗ |
| Delete Status Page | ✓ | ✓ | ✗ | ✗ |
| Publish Status Page | ✓ | ✓ | ✗ | ✗ |

### Admin Permissions

| Action | Owner | Admin | Member | Viewer |
|--------|-------|-------|--------|--------|
| Invite Users | ✓ | ✓ | ✗ | ✗ |
| Remove Users | ✓ | ✓ | ✗ | ✗ |
| Change Roles | ✓ | ✗ | ✗ | ✗ |
| Access Settings | ✓ | Limited | ✗ | ✗ |
| View Billing | ✓ | ✓ | ✗ | ✗ |
| Change Plan | ✓ | ✗ | ✗ | ✗ |

## Team Organization

### Best Practices for Team Structure

#### Small Teams (1-10 people)
- 1-2 Owners
- Several Members
- Optional Viewers for stakeholders

#### Medium Teams (10-50 people)
- 2-3 Owners
- Multiple Admins
- Many Members
- Viewers for leadership

#### Large Teams (50+ people)
- Multiple Owners
- Multiple Admins
- Engineers as Members
- Department leads as Admins
- C-suite as Viewers

### Organizing by Department

Example structure:
- **DevOps Team**: Owners/Admins
- **Engineering**: Members
- **QA Team**: Members
- **Operations**: Members
- **Management**: Admins
- **Executives**: Viewers

## Activity Audit Trail

### Viewing Team Activity

1. Go to **Settings > Audit Log**
2. See all team activities:
   - Monitor creation/changes
   - Incident creation/updates
   - Team member changes
   - Settings modifications
   - Alert configurations

### Filtering Activity

- Filter by user
- Filter by action type
- Filter by date range
- Search by details
- Export log

### Activity Types Logged

- User invitation
- Role changes
- Monitor creation/deletion
- Incident management
- Setting changes
- Alert configuration
- Access logs

## Notification Preferences

### Configuring Personal Preferences

1. Go to **Settings > Notifications**
2. Choose notification channels:
   - Email
   - Slack
   - SMS

3. Select events that trigger notifications
4. Set quiet hours
5. Configure digest mode

### Team-Wide Notifications

1. Set default for new members
2. Different settings per role
3. Override personal preferences
4. Test notifications

## Collaboration Features

### Real-Time Updates

- Live monitor status updates
- Incident notifications in real-time
- Simultaneous editing (conflicts resolved)
- Recent activity feed

### Commenting on Incidents

1. Go to incident
2. Click **Add Comment**
3. @mention team members
4. Discussions visible to all
5. Email notifications sent

### Incident Assignment

1. Assign incident to team member
2. They receive notification
3. Visual indicator in UI
4. Track assignments
5. Workload visibility

## API Access for Team

### Personal API Keys

Each team member can create personal API keys:

1. Go to **Settings > API Keys**
2. Click **Create API Key**
3. Name the key
4. Select permissions
5. Copy and secure the key

### Team API Keys

Shared keys for services:

1. Go to **Settings > Team API Keys**
2. Create key for specific service
3. Assign permissions
4. Share securely
5. Track usage

### API Key Permissions

- **Read**: View monitors and incidents
- **Write**: Create/update monitors and incidents
- **Admin**: Full administrative access
- **Custom**: Specific permissions

## Best Practices for Team Collaboration

### Security

1. **Use Appropriate Roles**: Don't over-privilege
2. **Regular Audits**: Review team members quarterly
3. **Revoke Access**: Remove departing employees immediately
4. **Strong Passwords**: Require secure passwords
5. **2FA**: Enable two-factor authentication

### Communication

1. **Clear Role Definitions**: Everyone knows their role
2. **Regular Meetings**: Daily standups for ops
3. **Incident Reviews**: Weekly post-mortems
4. **Documentation**: Keep runbooks updated
5. **Escalation Paths**: Define who to contact

### Processes

1. **On-Call Rotation**: Clear schedule
2. **Incident Response**: Documented procedures
3. **Change Management**: Approval process
4. **Runbooks**: For common issues
5. **Training**: Onboard new members well

## Troubleshooting Team Issues

### Member Can't Access Resource

1. Check their role
2. Verify team membership
3. Check resource permissions
4. Try logging out and back in
5. Clear browser cache

### Lost Access to Account

1. Ask another owner to re-invite you
2. Use password reset if email lost
3. Contact support with verification

### Too Many Team Members

- Review inactive members
- Remove unnecessary access
- Consolidate similar roles
- Archive old incidents

## Next Steps

- Learn about [API access](/articles/api/api-overview)
- Set up [monitors](/articles/monitors/understanding-monitor-types)
- Configure [notifications](/articles/alerting-notifications/setting-up-notifications)
- Review [security best practices](/help)