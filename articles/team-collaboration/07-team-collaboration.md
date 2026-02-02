---
title: Team Collaboration
slug: team-collaboration
category: Team Collaboration
excerpt: Invite teammates, manage access, and assign roles to control permissions in your workspace.
order: 7
---

# Team Collaboration

## Overview

StatusApp supports role-based access control (RBAC), allowing you to invite team members with specific permissions. Different team members can have different levels of access based on their assigned role.

When you invite team members, they gain access to your account's resources (monitors, incidents, status pages) based on their role. All resources are tied to the account owner, and team members work within the owner's subscription and limits.

---

## User Roles

StatusApp provides four team roles with increasing levels of access:

### Owner

- **Access Level**: Full administrative access
- **Capabilities**:
  - Create, edit, and delete monitors
  - Manage all team members (invite, remove, change roles)
  - Create and manage status pages
  - Access billing, subscriptions, and plan settings
  - Manage incidents and post updates
  - Configure all integrations and API keys
  - View all analytics, reports, and dashboards
  - Manage SLA configurations
  - Configure maintenance windows
  - Access audit logs (Enterprise)
- **Best For**: Account owners, team leads, account administrators
- **Note**: There should always be at least one Owner per account

### Admin

- **Access Level**: Full operational access (excluding billing)
- **Capabilities**:
  - Create, edit, and delete monitors
  - Manage status pages
  - Create and resolve incidents
  - Manage notification channels
  - Invite and manage team members (except changing Owner role)
  - View all analytics and reports
  - Configure integrations
  - Cannot access billing or subscription settings
  - Cannot change the Owner role
- **Best For**: Senior engineers, DevOps leads, operations managers

### Member

- **Access Level**: Standard operational access
- **Capabilities**:
  - Create and edit monitors
  - Create, acknowledge, and resolve incidents
  - Post incident updates
  - View status pages and analytics
  - Configure personal notification preferences
  - Cannot delete monitors or manage team
  - Cannot manage status pages
- **Best For**: Engineers, on-call responders, developers

### Viewer

- **Access Level**: Read-only access
- **Capabilities**:
  - View monitors and their status
  - View incidents and updates
  - View status pages
  - View analytics and reports
  - Cannot create, edit, or delete anything
  - Cannot take any actions
- **Best For**: Stakeholders, management, external observers, auditors

---

## Inviting Team Members

### Via Dashboard

1. Go to **Settings > Team**
2. Click **Invite Team Member**
3. Enter their email address
4. Enter their name (optional)
5. Select a role: Owner, Admin, Member, or Viewer
6. Click **Send Invitation**

The team member receives an email with a unique invitation link. The invitation expires after a set period (typically 7 days).

### Via API

```bash
curl -X POST https://ops.statusapp.io/api/v1/team/members \
  -H "X-API-Key: your_api_key" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "newmember@example.com",
    "name": "Jane Developer",
    "role": "MEMBER"
  }'
```

---

## Invitation States

| Status | Description |
|--------|-------------|
| **PENDING** | Invitation sent, awaiting acceptance |
| **ACCEPTED** | Team member has accepted and is active |
| **EXPIRED** | Invitation expired without acceptance |
| **REVOKED** | Invitation cancelled by an admin |

---

## Managing Team Members

### View Team Members

1. Go to **Settings > Team**
2. View the list of all team members
3. See their name, email, role, and status

### Via API

```bash
curl -X GET https://ops.statusapp.io/api/v1/team/members \
  -H "X-API-Key: your_api_key"
```

### Change a Member's Role

1. Go to **Settings > Team**
2. Find the team member
3. Click the role dropdown or edit button
4. Select the new role
5. Confirm the change

**Note**: Only Owners can change roles. You cannot demote yourself if you're the only Owner.

### Remove a Team Member

1. Go to **Settings > Team**
2. Click **Remove** next to the team member's name
3. Confirm removal
4. Their access is revoked immediately

### Resend an Invitation

For pending invitations:
1. Go to **Settings > Team**
2. Find the pending invitation
3. Click **Resend**
4. A new invitation email is sent with a fresh link

### Cancel an Invitation

1. Go to **Settings > Team**
2. Find the pending invitation
3. Click **Revoke** or **Cancel**
4. The invitation link becomes invalid

---

## Permission Matrix

| Action | Owner | Admin | Member | Viewer |
|--------|:-----:|:-----:|:------:|:------:|
| **Monitors** |||||
| View monitors | Yes | Yes | Yes | Yes |
| Create monitor | Yes | Yes | Yes | No |
| Edit monitor | Yes | Yes | Yes | No |
| Delete monitor | Yes | Yes | No | No |
| Pause/resume monitor | Yes | Yes | Yes | No |
| **Incidents** |||||
| View incidents | Yes | Yes | Yes | Yes |
| Create incident | Yes | Yes | Yes | No |
| Update incident | Yes | Yes | Yes | No |
| Resolve incident | Yes | Yes | Yes | No |
| Assign incident | Yes | Yes | Yes | No |
| **Status Pages** |||||
| View status pages | Yes | Yes | Yes | Yes |
| Create status page | Yes | Yes | No | No |
| Edit status page | Yes | Yes | No | No |
| Delete status page | Yes | Yes | No | No |
| **Notifications** |||||
| View notification channels | Yes | Yes | Yes | Yes |
| Create/edit channels | Yes | Yes | No | No |
| Delete channels | Yes | Yes | No | No |
| **Team & Settings** |||||
| View team members | Yes | Yes | Yes | Yes |
| Invite users | Yes | Yes | No | No |
| Remove users | Yes | Yes | No | No |
| Change roles | Yes | No | No | No |
| Access billing | Yes | No | No | No |
| Manage API keys | Yes | Yes | No | No |
| **Analytics** |||||
| View analytics | Yes | Yes | Yes | Yes |
| Create dashboards | Yes | Yes | No | No |
| Create reports | Yes | Yes | No | No |

---

## Team Member Limits

Team member seats are determined by your subscription plan:

| Plan | Team Members |
|------|--------------|
| Starter | 1-5 |
| Professional | 5-25 |
| Business | 25-100 |
| Enterprise | Unlimited |

Check your current usage at **Settings > Plan** or **Settings > Billing**.

---

## How Team Access Works

When a team member logs in:

1. They see all resources belonging to the account owner
2. Their actions are subject to their role permissions
3. They work within the owner's plan limits and quotas
4. API keys they create are tied to their own identity but access owner's resources
5. Activity is logged for audit purposes

**Important**: Team members do not have separate accounts. They access the owner's workspace.

---

## Best Practices

### Team Structure

**Small Teams (1-10 people)**:
- 1-2 Owners for redundancy
- Critical engineers as Members
- Stakeholders as Viewers

**Medium Teams (10-50 people)**:
- 2-3 Owners across leadership
- Senior engineers as Admins
- Most engineers as Members
- Management as Viewers

**Large Teams (50+ people)**:
- Multiple Owners for different areas
- Admins for each team or service area
- Engineers as Members
- Leadership and external stakeholders as Viewers

### Security Best Practices

- **Principle of least privilege**: Assign the minimum role needed
- **Regular audits**: Review team members monthly
- **Prompt removal**: Remove departing employees immediately
- **Owner redundancy**: Always have at least two Owners
- **Viewer for external**: Use Viewer role for external stakeholders
- **Monitor API keys**: Review and rotate API keys regularly

### Collaboration Tips

- Use notification groups for on-call distribution
- Document escalation paths in your runbooks
- Align monitor ownership with team responsibilities
- Use incident templates for consistent communication
- Set up Slack/Teams channels for different severity levels

---

## Enterprise Features

Available on Enterprise plans:

### SSO/SAML Authentication

- Single sign-on with your identity provider
- Automatic user provisioning
- SAML 2.0 support

### Audit Logs

- Track all user actions
- Export for compliance
- Retention based on plan

### Advanced RBAC

- Custom role definitions
- Fine-grained permissions
- Permission inheritance

---

## Troubleshooting

### Invitation Not Received

- Check spam/junk folders
- Verify email address is correct
- Add `statusapp.io` to allowed senders
- Resend the invitation
- Check if corporate email filters are blocking

### Can't Change Someone's Role

- Only Owners can change roles
- You cannot demote yourself if you're the only Owner
- Admins cannot change other Admins to Owner

### Member Can't Access a Feature

- Verify their role has permission (see matrix above)
- Ask them to log out and back in after role changes
- Check if the feature requires a higher plan

### Invitation Expired

- Invitations expire after 7 days by default
- Simply resend a new invitation
- The old link becomes invalid

### Too Many Team Members

- Check your plan's seat limit at **Settings > Plan**
- Remove inactive members to free up seats
- Upgrade to a higher plan for more seats

---

## API Reference

### List Team Members

```http
GET /api/v1/team/members
```

### Add Team Member

```http
POST /api/v1/team/members
Content-Type: application/json

{
  "email": "user@example.com",
  "name": "User Name",
  "role": "MEMBER"
}
```

### Update Team Member

```http
PUT /api/v1/team/members/{memberId}
Content-Type: application/json

{
  "role": "ADMIN"
}
```

### Remove Team Member

```http
DELETE /api/v1/team/members/{memberId}
```

### List Pending Invitations

```http
GET /api/v1/team/invitations
```

See the [API Reference Guide](/help/knowledge-base/api-reference-guide) for complete documentation.

---

## Next Steps

- **[Getting Started](/help/knowledge-base/getting-started-with-statusapp)** - Set up your first monitors
- **[Notifications](/help/knowledge-base/notification-channels-overview)** - Configure team alerts
- **[Incidents](/help/knowledge-base/incident-lifecycle)** - Manage incidents as a team
- **[API Reference](/help/knowledge-base/api-reference-guide)** - Programmatic team management
