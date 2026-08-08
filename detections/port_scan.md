# Port Scan Detection

Detect hosts making connections to an unusually high number of destination ports in a short time window.

## Starting Splunk search

```spl
index=botsv1 sourcetype=suricata
| bin _time span=5m
| stats dc(dest_port) as unique_ports values(dest_port) as ports by _time, src_ip, dest_ip
| where unique_ports >= 20
| sort - unique_ports
```
