---
title: Team Collaboration
slug: team-collaboration
category: Team Collaboration
excerpt: Invite teammates, manage access, and assign roles to control permissions in your workspace.
order: 7
---

# Team Collaboration

## Overview

StatusApp supports role-based access control, allowing you to invite team members with specific permissions. Different team members can have different levels of access based on their role.

## User Roles

### Owner
- **Access Level**: Full access to all features
- **Capabilities**:
  - Create, edit, and delete monitors
  - Manage team members (invite, remove, change roles)
  - Create and manage status pages
  - Access billing and subscription settings
  - Manage incidents and post updates
  - Configure integrations and API keys
  - View all analytics and reports
- **Best For**: Team leads, account administrators

### Admin
- **Access Level**: Administrative access (excluding billing)
- **Capabilities**:
  - Create, edit, and delete monitors
  - Manage status pages
  - Create and resolve incidents
  - View analytics
  - Manage notification channels
  - Cannot access billing or change subscription
  - Cannot remove Owner or change roles
- **Best For**: Senior engineers, operations leads

### Member
- **Access Level**: Standard operational access
- **Capabilities**:
  - Create and edit monitors
  - Acknowledge and resolve incidents
  - Post incident updates
  - View analytics
  - Cannot delete monitors or manage team
- **Best For**: Engineers, on-call responders

### Viewer
- **Access Level**: Read-only access
- **Capabilities**:
  - View monitors and their status
  - View incidents and updates
  - View analytics and reports
  - Cannot create, edit, or delete anything
- **Best For**: Stakeholders, management, external observers

## Inviting Team Members

1. Go to **Settings → Team**
2. Click **Invite Team Member**
3. Enter their email address
4. Select a role (Owner, Admin, Member, or Viewer)
5. Click **Send Invitation**

The team member receives an email with an invitation link. Once they accept, they'll have access according to their assigned role.

## Managing Team Members

### Change a Member's Role

1. Go to **Settings → Team**
2. Find the team member
3. Click the role dropdown next to their name
4. Select the new role
5. Confirm the change

### Remove a Team Member

1. Go to **Settings → Team**
2. Click **Remove** next to the team member's name
3. Confirm removal
4. Their access is revoked immediately

### Resend an Invitation

For pending invitations:
1. Go to **Settings → Team**
2. Find the pending invitation
3. Click **Resend**
4. A new invitation email is sent

## Permission Matrix

| Action | Owner | Admin | Member | Viewer |
|--------|-------|-------|--------|--------|
| **Monitors** |
| View Monitors | ✓ | ✓ | ✓ | ✓ |
| Create Monitor | ✓ | ✓ | ✓ | ✗ |
| Edit Monitor | ✓ | ✓ | ✓ | ✗ |
| Delete Monitor | ✓ | ✓ | ✗ | ✗ |
| **Incidents** |
| View Incidents | ✓ | ✓ | ✓ | ✓ |
| Create Incident | ✓ | ✓ | ✓ | ✗ |
| Update Incident | ✓ | ✓ | ✓ | ✗ |
| Resolve Incident | ✓ | ✓ | ✓ | ✗ |
| **Status Pages** |
| View Status Pages | ✓ | ✓ | ✓ | ✓ |
| Create Status Page | ✓ | ✓ | ✗ | ✗ |
| Edit Status Page | ✓ | ✓ | ✗ | ✗ |
| Delete Status Page | ✓ | ✓ | ✗ | ✗ |
| **Team & Settings** |
| Invite Users | ✓ | ✓ | ✗ | ✗ |
| Remove Users | ✓ | ✓ | ✗ | ✗ |
| Change Roles | ✓ | ✗ | ✗ | ✗ |
| Access Billing | ✓ | ✗ | ✗ | ✗ |
| Manage API Keys | ✓ | ✓ | ✗ | ✗ |
| View Analytics | ✓ | ✓ | ✓ | ✓ |

## Best Practices

### Team Structure

**Small Teams (1-10 people)**:
- 1-2 Owners
- Remaining as Members
- Optional Viewers for stakeholders

**Medium Teams (10-50 people)**:
- 2-3 Owners
- 3-5 Admins
- Majority as Members
- Viewers for leadership

**Large Teams (50+ people)**:
- Multiple Owners across departments
- Admins for each team/service
- Engineers as Members
- Management as Viewers

### Security

- Assign the minimum role needed for each person's responsibilities
- Regularly review team members and remove inactive accounts
- Use Viewer role for external stakeholders who need visibility
- Keep at least two Owners to prevent lockout
- Remove departing employees immediately

### Collaboration

- Add distribution lists for critical notifications (e.g., oncall@company.com)
- Use incident templates to keep updates consistent
- Align monitor ownership with team structure
- Document escalation paths clearly

## Plan Notes

- Team invitations are available on all paid plans
- Seat limits vary by plan; check **Settings → Billing** for your limit
- SSO/SCIM available on Enterprise plans

## Troubleshooting

### Invitation Not Received
- Check spam/junk folder
- Verify email address is correct
- Allowlist emails from statusapp.io
- Resend the invitation

### Can't Change Someone's Role
- Only Owners can change roles
- You cannot change your own role if you're the only Owner

### Member Can't Access Feature
- Verify their role has permission for that action
- Check the permission matrix above
- They may need to log out and back in after role changes