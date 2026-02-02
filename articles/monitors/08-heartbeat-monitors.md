---
title: Heartbeat & Cron Monitors
slug: heartbeat-monitors
category: Monitors
excerpt: Monitor scheduled jobs and background tasks using reverse heartbeat monitoring.
order: 8
---

# Heartbeat & Cron Monitors

## Overview

Heartbeat monitors work in reverse - your scheduled tasks ping StatusApp when they run. If StatusApp doesn't receive a ping within the expected timeframe (grace period), it triggers an alert. Perfect for monitoring backups, data syncs, cron jobs, and other scheduled tasks.

## How It Works

**Normal Monitoring** (StatusApp pings your service):
1. StatusApp → Checks your endpoint at intervals
2. Your service responds
3. StatusApp alerts if no response

**Heartbeat Monitoring** (Your service pings StatusApp):
1. You create a CRON monitor → receive unique heartbeat URL
2. Your scheduled job runs and pings the URL
3. If StatusApp doesn't receive a ping within the grace period → incident created

---

## Creating a Heartbeat Monitor

### Via Dashboard

1. Go to **Monitors** > **Create Monitor**
2. Select **CRON** as the monitor type
3. Enter a descriptive name (e.g., "Nightly Database Backup")
4. Set the **Grace Period** (60 seconds to 24 hours)
5. Configure notification channels
6. Click **Create Monitor**
7. Copy the generated **heartbeat URL**

### Via API

```bash
curl -X POST https://ops.statusapp.io/api/v1/monitors \
  -H "X-API-Key: your_api_key" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Daily Backup Job",
    "url": "https://placeholder.local",
    "type": "CRON",
    "gracePeriod": 3600,
    "notifications": [
      {
        "type": "EMAIL",
        "enabled": true,
        "config": { "emails": ["alerts@example.com"] }
      }
    ]
  }'
```

The response includes a unique `heartbeatUrl` that you'll use to ping StatusApp.

---

## Configuration

### Grace Period

The grace period defines how long StatusApp waits for a ping before triggering an alert.

| Setting | Range |
|---------|-------|
| Minimum | 60 seconds (1 minute) |
| Maximum | 86400 seconds (24 hours) |
| Default | 300 seconds (5 minutes) |

**Recommended Grace Periods by Job Frequency**:

| Job Frequency | Grace Period |
|---------------|--------------|
| Every minute | 2-5 minutes |
| Every 5 minutes | 10-15 minutes |
| Hourly | 1-2 hours |
| Every 6 hours | 7-8 hours |
| Daily | 2-6 hours |
| Weekly | 24-48 hours |

**Tip**: Set the grace period to **150-200% of your job's expected duration** to account for occasional slow runs.

---

## Heartbeat API Endpoints

All heartbeat endpoints are available at:
```
https://ops.statusapp.io/api/v1/heartbeat/{heartbeatId}
```

The `heartbeatId` is extracted from your unique heartbeat URL.

### Success Signal

Report successful job completion. This is the default ping.

```http
GET  /api/v1/heartbeat/{heartbeatId}
POST /api/v1/heartbeat/{heartbeatId}
HEAD /api/v1/heartbeat/{heartbeatId}
```

All HTTP methods work identically. Choose whichever is most convenient for your environment.

**Optional Message** (POST only, max 10KB):
```bash
curl -X POST https://ops.statusapp.io/api/v1/heartbeat/{id} \
  -d '{"message": "Processed 1,234 records in 45 seconds"}'
```

**Response**:
```json
{
  "message": "OK"
}
```

### Start Signal

Indicate that a long-running job has started. Useful for tracking job duration and distinguishing between "job didn't start" vs "job is still running".

```http
GET  /api/v1/heartbeat/{heartbeatId}/start
POST /api/v1/heartbeat/{heartbeatId}/start
```

### Fail Signal

Explicitly report job failure. This immediately triggers an incident.

```http
GET  /api/v1/heartbeat/{heartbeatId}/fail
POST /api/v1/heartbeat/{heartbeatId}/fail
```

**With Error Message**:
```bash
curl -X POST https://ops.statusapp.io/api/v1/heartbeat/{id}/fail \
  -H "Content-Type: application/json" \
  -d '{"error": "Database connection timeout after 30s"}'
```

### Log Signal

Record a log message without changing monitor status. Useful for tracking progress in long-running jobs.

```http
POST /api/v1/heartbeat/{heartbeatId}/log
Content-Type: application/json
```

**Request Body**:
```json
{
  "message": "Processing batch 5 of 10..."
}
```

---

## Rate Limiting

Heartbeat endpoints have a rate limit of **5 pings per minute** per monitor. This prevents accidental flooding if a script loops incorrectly.

---

## Integration Examples

### Bash/Shell Script

**Simple ping at end of job**:
```bash
#!/bin/bash

# Your job logic here
/usr/local/bin/run-backup.sh

# Ping success (using exit code)
if [ $? -eq 0 ]; then
  curl -s https://ops.statusapp.io/api/v1/heartbeat/abc123
else
  curl -s https://ops.statusapp.io/api/v1/heartbeat/abc123/fail
fi
```

**With start signal for long jobs**:
```bash
#!/bin/bash
HEARTBEAT_URL="https://ops.statusapp.io/api/v1/heartbeat/abc123"

# Signal job start
curl -s "${HEARTBEAT_URL}/start"

# Run the job
if /usr/local/bin/backup.sh 2>/tmp/backup_error.log; then
  # Success
  curl -s "${HEARTBEAT_URL}"
else
  # Failure with error details
  ERROR=$(cat /tmp/backup_error.log | head -c 1000)
  curl -s -X POST "${HEARTBEAT_URL}/fail" \
    -H "Content-Type: application/json" \
    -d "{\"error\": \"$ERROR\"}"
  exit 1
fi
```

### Crontab Entry

```cron
# Simple: backup job runs daily at 2 AM
0 2 * * * /path/to/backup.sh && curl -s https://ops.statusapp.io/api/v1/heartbeat/abc123

# With error handling
0 2 * * * /path/to/backup.sh && curl -s https://ops.statusapp.io/api/v1/heartbeat/abc123 || curl -s https://ops.statusapp.io/api/v1/heartbeat/abc123/fail
```

### Node.js

```javascript
const HEARTBEAT_URL = 'https://ops.statusapp.io/api/v1/heartbeat/abc123';

async function runScheduledJob() {
  // Signal start
  await fetch(`${HEARTBEAT_URL}/start`);

  try {
    // Your job logic
    const result = await performBackup();

    // Signal success with details
    await fetch(HEARTBEAT_URL, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ message: `Backed up ${result.fileCount} files` })
    });

  } catch (error) {
    // Signal failure with error
    await fetch(`${HEARTBEAT_URL}/fail`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ error: error.message })
    });

    throw error;
  }
}
```

### Python

```python
import requests
import sys

HEARTBEAT_URL = "https://ops.statusapp.io/api/v1/heartbeat/abc123"

def run_scheduled_job():
    # Signal start
    requests.get(f"{HEARTBEAT_URL}/start", timeout=10)

    try:
        # Your job logic
        result = perform_backup()

        # Signal success with message
        requests.post(HEARTBEAT_URL,
            json={"message": f"Backed up {result['size']} bytes"},
            timeout=10
        )

    except Exception as e:
        # Signal failure with error
        requests.post(f"{HEARTBEAT_URL}/fail",
            json={"error": str(e)},
            timeout=10
        )
        raise

if __name__ == "__main__":
    run_scheduled_job()
```

### PHP

```php
<?php
$heartbeatUrl = 'https://ops.statusapp.io/api/v1/heartbeat/abc123';

function pingHeartbeat($endpoint = '', $data = null) {
    global $heartbeatUrl;
    $url = $heartbeatUrl . $endpoint;

    $ch = curl_init($url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_TIMEOUT, 10);

    if ($data) {
        curl_setopt($ch, CURLOPT_POST, true);
        curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($data));
        curl_setopt($ch, CURLOPT_HTTPHEADER, ['Content-Type: application/json']);
    }

    $response = curl_exec($ch);
    curl_close($ch);
    return $response;
}

// Signal start
pingHeartbeat('/start');

try {
    // Your job logic
    runBackup();

    // Signal success
    pingHeartbeat('', ['message' => 'Backup completed']);

} catch (Exception $e) {
    // Signal failure
    pingHeartbeat('/fail', ['error' => $e->getMessage()]);
    throw $e;
}
```

### Go

```go
package main

import (
    "bytes"
    "encoding/json"
    "net/http"
    "time"
)

const heartbeatURL = "https://ops.statusapp.io/api/v1/heartbeat/abc123"

var client = &http.Client{Timeout: 10 * time.Second}

func pingHeartbeat(endpoint string, data map[string]string) error {
    url := heartbeatURL + endpoint

    if data != nil {
        body, _ := json.Marshal(data)
        _, err := client.Post(url, "application/json", bytes.NewBuffer(body))
        return err
    }

    _, err := client.Get(url)
    return err
}

func runScheduledJob() error {
    // Signal start
    pingHeartbeat("/start", nil)

    err := performBackup()
    if err != nil {
        // Signal failure
        pingHeartbeat("/fail", map[string]string{"error": err.Error()})
        return err
    }

    // Signal success
    pingHeartbeat("", map[string]string{"message": "Backup completed"})
    return nil
}
```

### Docker/Container

```bash
#!/bin/sh
HEARTBEAT_URL="https://ops.statusapp.io/api/v1/heartbeat/abc123"

# Signal start
curl -s --max-time 10 "${HEARTBEAT_URL}/start" || true

# Run your job
if your_job_command; then
    curl -s --max-time 10 "${HEARTBEAT_URL}" || true
else
    curl -s --max-time 10 -X POST "${HEARTBEAT_URL}/fail" \
        -H "Content-Type: application/json" \
        -d '{"error": "Job failed"}' || true
    exit 1
fi
```

### Kubernetes CronJob

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: backup-job
spec:
  schedule: "0 2 * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: backup
            image: your-backup-image
            env:
            - name: HEARTBEAT_URL
              valueFrom:
                secretKeyRef:
                  name: monitoring-secrets
                  key: heartbeat-url
            command:
            - /bin/sh
            - -c
            - |
              curl -s "${HEARTBEAT_URL}/start" || true
              if /app/backup.sh; then
                curl -s "${HEARTBEAT_URL}"
              else
                curl -s -X POST "${HEARTBEAT_URL}/fail" \
                  -d '{"error": "Backup failed"}'
                exit 1
              fi
          restartPolicy: OnFailure
```

### AWS Lambda

```python
import urllib.request
import json

HEARTBEAT_URL = 'https://ops.statusapp.io/api/v1/heartbeat/abc123'

def ping_heartbeat(endpoint='', data=None):
    url = f"{HEARTBEAT_URL}{endpoint}"
    if data:
        req = urllib.request.Request(
            url,
            data=json.dumps(data).encode(),
            headers={'Content-Type': 'application/json'}
        )
    else:
        req = urllib.request.Request(url)
    urllib.request.urlopen(req, timeout=10)

def lambda_handler(event, context):
    ping_heartbeat('/start')

    try:
        # Your Lambda logic
        result = process_data(event)

        ping_heartbeat('', {'message': f'Processed {len(result)} items'})
        return {'statusCode': 200, 'body': 'Success'}

    except Exception as e:
        ping_heartbeat('/fail', {'error': str(e)})
        raise
```

---

## Best Practices

### 1. Use Start Signals for Long Jobs

For jobs that take more than a few seconds:
```bash
curl -s "${HEARTBEAT_URL}/start"
# ... long running job ...
curl -s "${HEARTBEAT_URL}"
```

This helps distinguish:
- Job didn't start (scheduler issue)
- Job started but is still running
- Job started but crashed mid-execution

### 2. Include Error Details

When signaling failure, include context:
```bash
curl -s -X POST "${HEARTBEAT_URL}/fail" \
  -H "Content-Type: application/json" \
  -d "{\"error\": \"Exit code $?. Log: $(tail -c 500 /var/log/job.log)\"}"
```

### 3. Set Appropriate Grace Periods

Calculate: `grace_period = (expected_duration * 1.5) + start_delay_buffer`

- Too short → false positives from slow runs
- Too long → delayed detection of missed jobs

### 4. Handle Network Failures Gracefully

Don't let heartbeat failures crash your job:
```bash
# Ping but don't fail if ping fails
curl -s --max-time 10 "${HEARTBEAT_URL}" || true
```

### 5. Secure Your Heartbeat URL

- Store in environment variables, not code
- Don't log it to user-facing outputs
- Don't commit to version control
- Rotate if exposed

### 6. Test Before Production

1. Create the monitor
2. Run your script manually
3. Verify StatusApp shows the ping
4. Test failure scenario with `/fail` endpoint

---

## Troubleshooting

### No Pings Received

**Check**:
- Heartbeat URL is correct (no typos)
- Job host can reach `ops.statusapp.io` (firewall rules)
- Script is actually running (check cron/scheduler logs)
- curl/wget is installed

**Test connectivity**:
```bash
curl -v https://ops.statusapp.io/api/v1/heartbeat/your_id
```

### Rate Limited

Response: `{"error": "Rate limited"}` (HTTP 200)

Your script is pinging more than 5 times per minute. Check for:
- Loops in your script
- Multiple instances running simultaneously
- Overly aggressive retry logic

### False Positives

Getting alerts when jobs are running successfully:
- Increase the grace period
- Ensure ping happens **after** job completion
- Check for script exits before the ping line
- Add logging around the ping call

### Ping Not Updating Status

Verify:
- Monitor type is `CRON` (not WEBSITE or API)
- Heartbeat ID in URL matches your monitor
- Monitor is not paused

---

## Common Use Cases

### Database Backup
```
Name: Nightly Database Backup
Frequency: Daily at 2 AM
Grace Period: 2 hours (for large databases)
```

### ETL Job
```
Name: Data Warehouse ETL
Frequency: Every 6 hours
Grace Period: 1 hour
```

### Report Generation
```
Name: Daily Sales Report
Frequency: Daily at 6 AM
Grace Period: 30 minutes
```

### Cache Cleanup
```
Name: Redis Cache Cleanup
Frequency: Hourly
Grace Period: 10 minutes
```

### Queue Worker Health
```
Name: Queue Worker Heartbeat
Frequency: Every 5 minutes
Grace Period: 10 minutes
```

---

## Next Steps

- **[Monitor Types Overview](/help/knowledge-base/monitor-types-overview)** - Compare all monitor types
- **[Notification Channels](/help/knowledge-base/notification-channels-overview)** - Configure alerts
- **[API Reference](/help/knowledge-base/api-reference-guide)** - Complete heartbeat API docs
- **[Incidents](/help/knowledge-base/incident-lifecycle)** - Understanding incident handling
