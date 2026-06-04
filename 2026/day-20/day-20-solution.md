## Challenge Tasks

### Task 1: Input and Validation
Your script should:
1. Accept the path to a log file as a command-line argument
2. Exit with a clear error message if no argument is provided
3. Exit with a clear error message if the file doesn't exist
#!/bin/bash
LOG_FILE="$1"
if [ $# -eq 0 ]; then 
    echo "no argument provided"
elif [-f $LOG_FILE]; then 
    echo "file path is $LOG_FILE"
else 
    echo "file entered does not exist"
fi
---

### Task 2: Error Count
1. Count the total number of lines containing the keyword `ERROR` or `Failed`
2. Print the total error count to the console

#!/bin/bash

LOG_FILE="$1"

ERROR_COUNT=$(grep -E "ERROR|Failed" "$LOG_FILE" | wc -l)

echo "Total error count: $ERROR_COUNT"
---

### Task 3: Critical Events
1. Search for lines containing the keyword `CRITICAL`
2. Print those lines along with their line number

#!/bin/bash
LOG_FILE="$1"

if [ ! -f "$LOG_FILE" ]; then
    echo "Error: File does not exist."
    exit 1
fi
echo "Critical log entries:"
grep -n "CRITICAL" "$LOG_FILE"

Example output:
```
--- Critical Events ---
Line 84: 2025-07-29 10:15:23 CRITICAL Disk space below threshold
Line 217: 2025-07-29 14:32:01 CRITICAL Database connection lost
```

---

### Task 4: Top Error Messages
1. Extract all lines containing `ERROR`
2. Identify the **top 5 most common** error messages
3. Display them with their occurrence count, sorted in descending order

Example output:
```
--- Top 5 Error Messages ---
45 Connection timed out
32 File not found
28 Permission denied
15 Disk I/O error
9  Out of memory
```
---

#!/bin/bash

LOG_FILE="$1"

echo "--- Top 5 Error Messages ---"

grep "ERROR" "$LOG_FILE" \
| sed 's/.*ERROR[: ]*//' \ #Removes everything before ERROR, leaving only the error message.
| sort \
| uniq -c \ #Counts occurrences.
| sort -nr \ #Sorts by count descending.
| head -5 #Shows the top 5.

---

### Task 5: Summary Report
Generate a summary report to a text file named `log_report_<date>.txt` (e.g., `log_report_2026-02-11.txt`). The report should include:
1. Date of analysis
2. Log file name
3. Total lines processed
4. Total error count
5. Top 5 error messages with their occurrence count
6. List of critical events with line numbers

#!/bin/bash

if [ $# -eq 0 ]; then
    echo "Usage: $0 <log_file>"
    exit 1
fi

LOG_FILE="$1"

if [ ! -f "$LOG_FILE" ]; then
    echo "Error: File '$LOG_FILE' does not exist."
    exit 1
fi

DATE=$(date +%F)
REPORT_FILE="log_report_${DATE}.txt"

TOTAL_LINES=$(wc -l < "$LOG_FILE")
TOTAL_ERRORS=$(grep -E "ERROR|Failed" "$LOG_FILE" | wc -l)

{
    echo "=== Log Analysis Report ==="
    echo "Date of Analysis: $(date)"
    echo "Log File: $LOG_FILE"
    echo "Total Lines Processed: $TOTAL_LINES"
    echo "Total Error Count: $TOTAL_ERRORS"
    echo

    echo "=== Top 5 Error Messages ==="
    grep "ERROR" "$LOG_FILE" \
        | sed 's/.*ERROR[: ]*//' \
        | sort \
        | uniq -c \
        | sort -nr \
        | head -5

    echo
    echo "=== Critical Events ==="

    grep -n "CRITICAL" "$LOG_FILE" \
        | while IFS=: read -r line_number line_content
        do
            echo "Line $line_number: $line_content"
        done

} > "$REPORT_FILE"

echo "Report generated: $REPORT_FILE"
---

### Task 6 (Optional): Archive Processed Logs
Add a feature to:
1. Create an `archive/` directory if it doesn't exist
2. Move the processed log file into `archive/` after analysis
3. Print a confirmation message

# Create archive directory if it doesn't exist
mkdir -p archive

# Move processed log file to archive
mv "$LOG_FILE" archive/

# Confirmation message
echo "Report generated: $REPORT_FILE"
echo "Log file archived to: archive/$(basename "$LOG_FILE")"
---

## Sample Log File

A sample log file is available in this directory: `sample_log.log`

You can also pick real-world log datasets from the [LogHub repository](https://github.com/logpai/loghub) to test your script against production-like logs (e.g., ZooKeeper, HDFS, Apache, Linux syslogs).

---

## Hints
- Count errors: `grep -c "ERROR" logfile.log`
- Print with line numbers: `grep -n "CRITICAL" logfile.log`
- Top occurrences: `grep "ERROR" logfile.log | awk '{$1=$2=$3=""; print}' | sort | uniq -c | sort -rn | head -5`
- Associative arrays: `declare -A error_map`
- Date for filename: `date +%Y-%m-%d`
- Move files: `mv logfile.log archive/`

---

