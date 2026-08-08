## Environment Profiling

### Query

```spl
index=botsv1
| stats count by host
| sort -count
```

### Result

| Host | Events |
|------|--------|
| splunk-02 | 293,579 |
| we8105desk | 244,009 |
| suricata-ids.waynecorpinc.local | 125,584 |
| we1149srv | 121,348 |
| we9041srv | 90,300 |
| 192.168.250.1 | 80,922 |
| 192.168.2.50 | 65 |

### Analysis

The environment consists of a Splunk server, a user workstation, two Windows servers, a Suricata IDS sensor, a FortiGate firewall, and one additional low-volume host. Understanding the available assets provides the foundation for subsequent threat hunting and incident investigation.

### SOC Relevance

Asset identification is a fundamental step in incident response. It enables analysts to understand where alerts originate, prioritize investigations, and map attack activity across the environment.
