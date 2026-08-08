# Failed Login Detection

Detect repeated authentication failures that may indicate password spraying or brute-force activity.

## Starting Splunk search

```spl
index=botsv1 (EventCode=4625 OR action=failure)
| stats count by user, src, host
| where count >= 5
| sort - count
```
