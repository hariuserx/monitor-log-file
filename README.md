# monitor-log-file
Template to monitor log files continuously. When new lines appear, checks if there are any errors


```bash
#!/usr/bin/env bash
LOG_FILE="$1"

process_logs() {
      read -r LINE
      while true
      do
          HTTP_PATH=$(echo "$LINE" | awk '($9 == "500" && $8 ~ /^HTTP/) {print $7}') # Check for HTTP 500 errors
          if [ -n "$HTTP_PATH" ]
          then
            echo "error found while calling $HTTP_PATH"  # Do Something if error is found
          fi

          read -r LINE
      done
}


tail -n0 -f "$LOG_FILE" | process_logs
```
