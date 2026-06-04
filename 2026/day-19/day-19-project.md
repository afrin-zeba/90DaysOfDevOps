### Task 1: Log Rotation Script
Create `log_rotate.sh` that:
1. Takes a log directory as an argument (e.g., `/var/log/myapp`)
2. Compresses `.log` files older than 7 days using `gzip`
3. Deletes `.gz` files older than 30 days
4. Prints how many files were compressed and deleted
5. Exits with an error if the directory doesn't exist

#!/bin/bash

set -euo pipefail

LOG_DIR="$1"

# Check if directory exists
if [ ! -d "$LOG_DIR" ]; then
    echo "Error: Directory '$LOG_DIR' does not exist."
    exit 1
fi

# Count files to be compressed
compressed_count=$(find "$LOG_DIR" -type f -name "*.log" -mtime +7 | wc -l)

# Compress .log files older than 7 days
find "$LOG_DIR" -type f -name "*.log" -mtime +7 -exec gzip {} \;

# Count .gz files to be deleted
deleted_count=$(find "$LOG_DIR" -type f -name "*.gz" -mtime +30 | wc -l)

# Delete .gz files older than 30 days
find "$LOG_DIR" -type f -name "*.gz" -mtime +30 -delete

echo "Compressed files: $compressed_count"
echo "Deleted files: $deleted_count"

---

### Task 2: Server Backup Script
Create `backup.sh` that:
1. Takes a source directory and backup destination as arguments
2. Creates a timestamped `.tar.gz` archive (e.g., `backup-2026-02-08.tar.gz`)
3. Verifies the archive was created successfully
4. Prints archive name and size
5. Deletes backups older than 14 days from the destination
6. Handles errors — exit if source doesn't exist

#!/bin/bash

set -euo pipefail

SOURCE_DIR="$1"
BACKUP_DIR="$2"

# Check if source directory exists
if [ ! -d "$SOURCE_DIR" ]; then
    echo "Error: Source directory '$SOURCE_DIR' does not exist."
    exit 1
fi

# Check if backup destination exists
if [ ! -d "$BACKUP_DIR" ]; then
    echo "Error: Backup destination '$BACKUP_DIR' does not exist."
    exit 1
fi

# Generate timestamped archive name
TIMESTAMP=$(date +%F)
ARCHIVE_NAME="backup-${TIMESTAMP}.tar.gz"
ARCHIVE_PATH="${BACKUP_DIR}/${ARCHIVE_NAME}"

# Create archive
tar -czf "$ARCHIVE_PATH" -C "$(dirname "$SOURCE_DIR")" "$(basename "$SOURCE_DIR")"

# Verify archive was created
if [ ! -f "$ARCHIVE_PATH" ]; then
    echo "Error: Backup archive was not created."
    exit 1
fi

# Print archive details
ARCHIVE_SIZE=$(du -h "$ARCHIVE_PATH" | cut -f1)

echo "Backup created successfully"
echo "Archive: $ARCHIVE_NAME"
echo "Size: $ARCHIVE_SIZE"

# Delete backups older than 14 days
find "$BACKUP_DIR" \
    -type f \
    -name "backup-*.tar.gz" \
    -mtime +14 \
    -delete

echo "Old backups (>14 days) cleaned up."

---

### Task 3: Crontab
1. Read: `crontab -l` — what's currently scheduled?
2. Understand cron syntax:
   ```
   * * * * *  command
   │ │ │ │ │
   │ │ │ │ └── Day of week (0-7)
   │ │ │ └──── Month (1-12)
   │ │ └────── Day of month (1-31)
   │ └──────── Hour (0-23)
   └────────── Minute (0-59)
   ```
3. Write cron entries (in your markdown, don't apply if unsure) for:
   - Run `log_rotate.sh` every day at 2 AM
   - Run `backup.sh` every Sunday at 3 AM
   - Run a health check script every 5 minutes
### Run `log_rotate.sh` every day at 2:00 AM

```cron
0 2 * * * /path/to/log_rotate.sh /var/log/myapp
```

### Run `backup.sh` every Sunday at 3:00 AM

```cron
0 3 * * 0 /path/to/backup.sh /source/directory /backup/destination
```

### Run health check script every 5 minutes

```cron
*/5 * * * * /path/to/health_check.sh
```

---

### Task 4: Combine — Scheduled Maintenance Script
Create `maintenance.sh` that:
1. Calls your log rotation function
2. Calls your backup function
3. Logs all output to `/var/log/maintenance.log` with timestamps
4. Write the cron entry to run it daily at 1 AM
#!/bin/bash

set -euo pipefail

LOG_FILE="/var/log/maintenance.log"

log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" >> "$LOG_FILE"
}

log "Maintenance started"

# Run log rotation
log "Running log rotation"
/path/to/log_rotate.sh /var/log/myapp >> "$LOG_FILE" 2>&1

# Run backup
log "Running backup"
/path/to/backup.sh /home/ubuntu/myapp /home/ubuntu/backups >> "$LOG_FILE" 2>&1

log "Maintenance completed successfully"

---

## Hints
- Compress old files: `find /path -name "*.log" -mtime +7 -exec gzip {} \;`
- Timestamp: `date +%Y-%m-%d`
- Tar: `tar -czf backup.tar.gz /source/dir`
- Cron edit: `crontab -e`
- Log with timestamp: `echo "$(date): message" >> logfile`

---


## Reference Video

[![Watch the video](https://img.youtube.com/vi/PZYJ33bMXAw/0.jpg)](https://youtu.be/PZYJ33bMXAw?si=RzEzOSom7-FqnopA)


