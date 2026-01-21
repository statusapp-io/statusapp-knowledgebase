---
title: Telegram Notifications
slug: telegram-notifications
category: Alerting & Notifications
excerpt: Receive StatusApp alerts via Telegram for mobile-first incident notifications.
order: 8
---

# Telegram Notifications

## Overview

Telegram notifications deliver alerts to your phone, group chat, or channel. Ideal for on-call teams and mobile-first incident response.

## Setting Up Telegram

### Step 1: Create Bot with BotFather

1. Open Telegram
2. Search for **@BotFather**
3. Start chat: `/start`
4. Send: `/newbot`
5. BotFather asks for name: `StatusApp Alerts Bot`
6. BotFather asks for username: `statusapp_alerts_bot` (unique)
7. You receive **API Token**: `123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11`

**Keep token secret** - don't share or commit to GitHub.

### Step 2: Get Your Chat ID

**Option A: Direct Messages**

1. Search for your bot username in Telegram: `statusapp_alerts_bot`
2. Start conversation: `/start`
3. Send a message in the chat
4. Visit: `https://api.telegram.org/bot<YOUR_TOKEN>/getUpdates`
5. Find your message in JSON response
6. Your chat ID is in `"chat":{"id":123456789}`

**Option B: Group Chat**

1. Create Telegram group
2. Add your bot to group
3. Send message mentioning bot: `@statusapp_alerts_bot test`
4. Visit: `https://api.telegram.org/bot<YOUR_TOKEN>/getUpdates`
5. Find group message
6. Chat ID is in `"chat":{"id":-987654321}` (negative number for groups)

### Step 3: Add to StatusApp

1. Go to **Notifications** → **Add Channel**
2. Select **Telegram**
3. Enter **Bot Token**: `123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11`
4. Enter **Chat ID**: `123456789` (or `-987654321` for group)
5. Click **Test** to verify
6. Name: `Telegram On-Call` (or your choice)
7. Click **Save**

## Message Format

### Incident Down Alert
```
🔴 Incident: Website Down

Service: Website
Time Down: 2 minutes ago
Last Response: Connection timeout
Regions Affected: US-East, EU-West

Severity: HIGH
Monitor: HTTPS Health Check
```

### Incident Up Alert
```
🟢 Resolved: Website Up

Service: Website
Duration Down: 15 minutes
Status: 200 OK
Resolution Time: 2 minutes
```

### Incident Acknowledged
```
🟡 Acknowledged by John Doe
Website Down
Status: Being investigated
Estimated time to resolve: 30 minutes
```

## Direct Messages vs Groups

### Direct Messages
```
Bot sends alerts to your private Telegram chat
- Private and personal
- Real-time notifications
- Doesn't disturb others
- Best for: On-call individuals
```

### Group Chats
```
Bot sends alerts to team group
- Visible to entire team
- Creates context and knowledge sharing
- Good for collaboration
- Best for: Team channels, on-call groups
```

### Private Channel
```
Telegram Premium feature
- Create private channel
- Add bot and teammates
- Alerts visible to selected people
- Best for: Specialized teams, security-conscious
```

## Assigning to Monitors

### Single Telegram Direct Message
```
Monitor: API Health Check
Notifications: Telegram (Your Phone)
Incident Created: Alert sent to your Telegram
```

### Multiple Telegram Destinations
```
Monitor: Production Database
Notifications:
  - Telegram (Your Phone) - individual on-call
  - Telegram (On-Call Group) - team visibility
  - Slack (#database-alerts) - async team notification
```

### Escalation Path
```
5 min down → Telegram (@oncall)
15 min down → Telegram + SMS (urgent)
30 min down → Phone call to escalation lead
```

## Best Practices

### 1. Test Before Deployment
```
1. Create test bot
2. Send test message
3. Verify formatting and delivery
4. Update production once confirmed
```

### 2. Use Separate Bots for Environments
```
Production Bot:
  - Token: prod-token-123
  - Chat: Production On-Call Group

Staging Bot:
  - Token: staging-token-456
  - Chat: Engineering Group (informational)

This prevents staging alerts from waking up on-call.
```

### 3. Enable Mobile Notifications
Telegram settings on phone:
1. Settings → Notifications
2. Enable notifications for bot
3. Set to "High Priority" for critical alerts
4. Disable "Silent" so you get sound

### 4. Use Groups for Distributed Teams
If team spans multiple time zones:
```
On-Call Group: People currently on-call
- Everyone sees incidents
- Real-time collaboration

Status Updates: Private channel with full team
- Historical record of incidents
- Post-mortem discussions
```

### 5. Don't Over-Notify
Too many alerts = ignored messages:

```
Good: 1-2 Telegram channels per environment
  - On-Call (urgent)
  - Team (informational)

Bad: 5+ channels per service
  - Creates notification fatigue
  - Incidents get missed

Use Telegram channels strategically.
```

### 6. Rotate Bot Tokens Regularly
Security best practice:

```
Every 3-6 months:
1. Create new bot with BotFather: /newbot
2. Add new token to StatusApp
3. Test works
4. Update monitors to use new bot
5. Delete old bot (ask BotFather: /revoke)
```

## Common Patterns

### Personal On-Call Setup
```
Monitor: Production Services
├─ SMS: Phone (critical, doesn't require app)
├─ Telegram: Your Private Chat (default on-call)
├─ Email: Inbox (less urgent)
└─ Slack: #oncall (team visibility)
```

### Team On-Call Group
```
Monitor: Platform Services
├─ Telegram Group: @OnCallTeam
│  └─ All on-call see incidents immediately
├─ Slack: #production-alerts
│  └─ Team context and discussion
└─ Email: ops-team@company.com
   └─ Formal log and archive
```

### Multi-Severity Routing
```
CRITICAL (P1):
├─ Telegram Direct (your phone)
├─ SMS (urgent)
└─ Phone Call (60 min escalation)

HIGH (P2):
├─ Telegram On-Call Group
├─ Slack #alerts
└─ Email

MEDIUM (P3):
├─ Slack #monitoring
└─ Email

LOW (P4):
└─ Dashboard only (no alert)
```

### Geographic Distribution
```
US Team Incidents:
└─ Telegram @US-OnCall

EU Team Incidents:
└─ Telegram @EU-OnCall

Asia Team Incidents:
└─ Telegram @ASIA-OnCall

Critical Service (All):
└─ Telegram @AllTeams (mentions @US-OnCall, etc.)
```

## Telegram Desktop vs Mobile

### Mobile (Primary)
- Get notifications in real-time
- Can respond from anywhere
- Built-in: calls, messages, files
- Best for: On-call individuals

### Desktop App
- All alerts also appear on desktop
- Helpful for: Night shift monitoring
- Backup to mobile notifications
- Good for: War room situations

## Troubleshooting

### Message Not Arriving

**Verify Bot Token**:
```
1. Check token in StatusApp matches BotFather token
2. Ensure no typos or extra spaces
3. If in doubt, create new bot and get new token
```

**Verify Chat ID**:
```
1. Confirm chat ID is correct for bot
2. Direct message: positive number (123456)
3. Group: negative number (-123456)

If wrong:
1. Get new chat ID from getUpdates
2. Update in StatusApp
3. Test notification
```

**Check Bot Permissions**:
```
1. In Telegram group: click group name
2. Members → Find bot
3. Check bot has permission to post
4. Remove and re-add bot if needed
```

**Telegram Network Issues**:
```
- Check internet connection
- Telegram sometimes has regional delays
- Try test notification multiple times
- Check bot hasn't been rate-limited
```

### Delayed Delivery

Telegram can have slight delays:

```
Typical: Delivered within 1-5 seconds
Delayed: 5-30 seconds (network congestion)
Very Delayed: 30+ seconds (check other channels too)

If consistently delayed:
- Telegram network issue
- Your internet connection slow
- Try different bot or channel
```

### Bot Won't Respond

**Bot Offline**:
```
1. Verify token is correct
2. Check bot status in BotFather
3. Create new bot if needed
```

**Wrong Chat ID**:
```
If sending to wrong chat:
1. Get correct chat ID from getUpdates
2. Update in StatusApp
3. Test notification
```

**Group Removed Bot**:
```
If bot was removed from group:
1. Re-add bot to group
2. Message bot to activate
3. Update chat ID in StatusApp if different
```

### Too Many Notifications

**Reduce Alert Frequency**:
```
1. Increase check interval (reduce false positives)
2. Increase confirmation checks needed
3. Filter out non-critical monitors from Telegram
4. Keep only P1/P2 severity on personal channel
```

**Use Multiple Channels**:
```
- Telegram Personal: Only P1 (critical)
- Telegram Group: P1 + P2 (high)
- Slack: All alerts (reference)
- Email: Formal log
```

## Security Considerations

### Protect Your Bot Token
```
Do NOT:
- Share token in Slack, email, chat
- Commit to GitHub
- Store in plaintext configs
- Log to files

Do:
- Store in environment variables
- Use StatusApp secure storage
- Create separate bots per environment
- Rotate regularly
```

### Protect Your Chat ID
```
Chat ID is semi-public (if bot is public)
But combined with token = full access

Protect the token, not the chat ID
Rotate tokens regularly
```

## Rate Limiting

Telegram has rate limits:

```
Per bot: ~30 messages/second
Per chat: ~1 message/second

StatusApp: Respects these limits automatically
Batches alerts: Groups multiple incidents

You won't hit limits with typical monitoring
```

## Webhook Removal

**To Remove Telegram Alerts**:

1. Delete notification channel from StatusApp
2. Optional: Delete bot from BotFather
3. Remove bot from group/chat
4. Done - no more alerts

Deleted bots stop accepting messages immediately.

## Next Steps

- **[SMS](/articles/alerting-notifications/sms-notifications)** - SMS for critical alerts
- **[Discord](/articles/alerting-notifications/discord-notifications)** - Discord alternative
- **[Notification Overview](/articles/alerting-notifications/notification-channels-overview)** - All channels
