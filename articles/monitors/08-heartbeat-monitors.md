---
title: Heartbeat & Cron Monitors
slug: heartbeat-monitors
category: Monitors
excerpt: Monitor scheduled jobs and background tasks using reverse heartbeat monitoring.
order: 8
---

# Heartbeat & Cron Monitors

## Overview

Heartbeat monitors work in reverse - your scheduled tasks ping StatusApp when they run. If StatusApp doesn't receive a ping within the expected timeframe, it alerts you. Perfect for monitoring backups, data syncs, and other scheduled jobs.

## How It Works

**Normal Monitoring** (StatusApp pings your service):
1. StatusApp → Ping your API endpoint
2. API responds
3. StatusApp sends alert if no response

**Heartbeat Monitoring** (Your service pings StatusApp):
1. Your scheduled job runs
2. Your job → Pings StatusApp heartbeat URL
3. If StatusApp doesn't hear from you on schedule, it alerts

## Creating a Heartbeat Monitor

1. Create a new monitor
2. Select **Heartbeat/Cron** type
3. Enter job name
4. Set expected interval
5. Copy heartbeat URL
6. Add URL to your job
7. Save monitor

## Configuration

### Monitor Name
Descriptive name for your job:
- "Nightly Backup"
- "Data Sync Job"
- "Report Generation"
- "Cache Cleanup"

### Expected Interval
How often the job should run:
- **Hourly**: Every 1 hour
- **Every 6 Hours**: Twice daily
- **Daily**: Once per day (usually with time)
- **Weekly**: Once per week

### Grace Period
Extra time allowed after expected run time:
- Default: 5 minutes
- Recommended: 5-15 minutes
- Buffer for normal job variation
- Prevents false alerts from minor delays

**Example**:
- Expected interval: Daily at 2:00 AM
- Grace period: 15 minutes
- Job runs between 1:50 AM - 2:15 AM → OK
- Job hasn't run by 2:15 AM → Alert

## Setting Up Your Job

### Step 1: Copy Heartbeat URL
After creating monitor, StatusApp generates unique URL:
```
https://statusapp.io/api/v1/heartbeat/abc123xyz789
```

**Keep this URL secret** - treat it like a password

### Step 2: Add to Your Job

**Bash Script**:
```bash
#!/bin/bash

# Your scheduled task
echo "Starting backup..."
/usr/local/bin/backup.sh

# Check if successful
if [ $? -eq 0 ]; then
  echo "Backup completed successfully"
  # Ping StatusApp on success
  curl -X POST https://statusapp.io/api/v1/heartbeat/abc123xyz789
else
  echo "Backup failed"
  # Optionally ping failure endpoint
  curl -X POST https://statusapp.io/api/v1/heartbeat/abc123xyz789/fail
  exit 1
fi
```

**Python Script**:
```python
import requests
import sys
from datetime import datetime

print(f"[{datetime.now()}] Starting backup job...")

try:
    # Your scheduled task
    result = run_backup()
    
    if result['success']:
        print("Backup completed successfully")
        # Ping StatusApp on success
        response = requests.post(
            'https://statusapp.io/api/v1/heartbeat/abc123xyz789',
            timeout=5
        )
        print(f"Heartbeat sent: {response.status_code}")
    else:
        print("Backup failed")
        # Ping failure endpoint
        requests.post(
            'https://statusapp.io/api/v1/heartbeat/abc123xyz789/fail',
            timeout=5
        )
        sys.exit(1)
        
except Exception as e:
    print(f"Error during backup: {e}")
    # Ping failure endpoint on exception
    requests.post(
        'https://statusapp.io/api/v1/heartbeat/abc123xyz789/fail',
        timeout=5
    )
    sys.exit(1)
```

**Docker/Container**:
```dockerfile
# In your container's startup script
your-job.sh && curl -X POST https://statusapp.io/api/v1/heartbeat/abc123xyz789
```

**Node.js**:
```javascript
const axios = require('axios');

async function runJob() {
  try {
    console.log('Starting job...');
    
    // Your scheduled task
    const result = await performTask();
    
    if (result.success) {
      // Ping on success
      await axios.post('https://statusapp.io/api/v1/heartbeat/abc123xyz789');
      console.log('Heartbeat sent');
    } else {
      // Ping on failure
      await axios.post('https://statusapp.io/api/v1/heartbeat/abc123xyz789/fail');
      process.exit(1);
    }
  } catch (error) {
    console.error('Job failed:', error);
    // Ping on error
    await axios.post('https://statusapp.io/api/v1/heartbeat/abc123xyz789/fail');
    process.exit(1);
  }
}

runJob();
```

### Step 3: Test the Job
Before deploying to production:
```bash
# Run job manually to verify heartbeat works
./backup.sh

# Check StatusApp shows it was received
# Monitor should show "Last Ping: just now"
```

## Common Use Cases

### Nightly Backup
Monitor database backup runs daily:

```
Name: Database Backup
Interval: Daily (2:00 AM)
Grace Period: 15 minutes
Notifications: Email + Slack
```

### Data Sync Job
Monitor data syncs between systems:

```
Name: Customer Data Sync
Interval: Every 6 hours
Grace Period: 10 minutes
Notifications: Email + PagerDuty
```

### Report Generation
Monitor reports are generated on schedule:

```
Name: Daily Sales Report
Interval: Daily (6:00 AM)
Grace Period: 30 minutes
Notifications: Email to managers
```

### Cache Cleanup
Monitor cleanup job runs:

```
Name: Redis Cache Cleanup
Interval: Hourly
Grace Period: 5 minutes
Notifications: Slack (informational)
```

## Best Practices

### 1. Always Ping on Success
Every successful run should call the heartbeat URL:

```bash
# Good: Always ping
command && curl -X POST $HEARTBEAT_URL
command || curl -X POST $HEARTBEAT_URL/fail

# Bad: Only on success (fails silently if job crashes)
command && curl -X POST $HEARTBEAT_URL
```

### 2. Use Failure Endpoint Too
Report failures separately for better alerting:

```bash
# On success
curl -X POST https://statusapp.io/api/v1/heartbeat/abc123xyz789

# On failure  
curl -X POST https://statusapp.io/api/v1/heartbeat/abc123xyz789/fail
```

### 3. Set Realistic Intervals
Match job schedule exactly:
- Job runs at 2 AM → Set interval to "Daily"
- Job runs every 6 hours → Set to "Every 6 hours"
- Don't guess - document the actual schedule

### 4. Grace Period Buffer
Allow for job variation:
- Short jobs (< 5 min): 5-10 min grace
- Medium jobs (5-30 min): 15-30 min grace
- Long jobs (> 30 min): 30-60 min grace

### 5. Secure the URL
Treat heartbeat URL like a password:
- Keep in environment variables, not code
- Don't log it to user-facing output
- Don't commit to version control
- Rotate/regenerate if exposed

### 6. Add Logging
Log heartbeat status for debugging:

```bash
#!/bin/bash

HEARTBEAT_URL="https://statusapp.io/api/v1/heartbeat/abc123xyz789"
LOG_FILE="/var/log/backup.log"

{
  echo "[$(date)] Starting backup..."
  
  if /usr/local/bin/backup.sh; then
    echo "[$(date)] Backup successful, pinging heartbeat..."
    curl -s -X POST $HEARTBEAT_URL
    echo "[$(date)] Heartbeat sent"
  else
    echo "[$(date)] Backup failed, sending failure ping..."
    curl -s -X POST $HEARTBEAT_URL/fail
    exit 1
  fi
} >> $LOG_FILE 2>&1
```

## Troubleshooting

### Heartbeat Not Being Received

**Verify Job is Running**:
- Check system logs or job scheduler logs
- Confirm job didn't error out early
- Verify cron/scheduler is enabled

**Verify Heartbeat URL is Correct**:
- Copy URL exactly from StatusApp
- Check for typos or extra characters
- Confirm URL includes `/api/v1/heartbeat/`

**Test Heartbeat Manually**:
```bash
# Copy and run this to test
curl -X POST https://statusapp.io/api/v1/heartbeat/YOUR_URL_HERE

# Should see in StatusApp: "Last Ping: just now"
```

**Check Network Connectivity**:
```bash
# From job host, verify connectivity to StatusApp
curl -I https://statusapp.io

# Verify firewall isn't blocking outbound HTTPS
```

### Missing Heartbeats from Otherwise Good Job

**Clock Skew**:
- Job host clock might be off
- Sync system time: `ntpd`, `systemctl set-time`, etc.
- StatusApp expects pings within grace period

**Network Issues**:
- Job runs, but heartbeat URL can't be reached
- Job might timeout trying to reach StatusApp
- Add retry logic with backoff

**Job Output Issues**:
```bash
# Bad: Job fails silently
/usr/local/bin/backup.sh && curl $HEARTBEAT_URL

# Good: Report failure explicitly  
if /usr/local/bin/backup.sh; then
  curl $HEARTBEAT_URL
else
  curl $HEARTBEAT_URL/fail
  echo "Backup failed!" | mail admin@company.com
  exit 1
fi
```

### Too Many False Positives

**Increase Grace Period**:
- Job taking longer than expected?
- Increase grace period in monitor settings
- Add buffer for network latency

**Check Job Logs**:
- Sometimes job runs but heartbeat fails
- Review job logs for errors
- Add explicit logging around heartbeat call

## Monitoring Multiple Jobs

**Separate Monitor for Each Job**:
```
Monitor 1: Database Backup (2 AM)
Monitor 2: Data Sync (Every 6 hours)
Monitor 3: Report Generation (6 AM)
Monitor 4: Cache Cleanup (Hourly)
```

Each job has its own unique heartbeat URL.

## Integration with Other Monitoring

Combine with other monitors:

```
Heartbeat Monitor: Backup runs on schedule
└─ Confirms job executes

Database Health Monitor: Database is accessible
└─ Confirms backup can complete

Email Notification: Backup completion
└─ Confirms results are used

Together: Complete backup monitoring
```

## Next Steps

- **[Notifications](/articles/alerting-notifications/notification-channels-overview)** - Set up alerts when jobs miss
- **[Analytics](/articles/analytics/understanding-analytics)** - Track job execution patterns
- **[HTTP Monitors](/articles/monitors/http-website-monitors)** - Monitor services that jobs interact with
