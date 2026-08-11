# Hunt 01 - Web Attack Pivot

## Objective

Investigate a high-volume Suricata IDS target by pivoting into related network telemetry to determine whether additional evidence of suspicious web activity exists.

---

## Hunt Hypothesis

A host receiving repeated IDS alerts may also exhibit suspicious web activity that can be identified through HTTP telemetry.

---

## Initial Observation

Previous investigations identified **192.168.250.70** as one of the most targeted internal systems based on Suricata IDS alerts.

This hunt pivots from that observation to determine what other network activity is associated with the host.

---

## Investigation 1 – Telemetry Correlation

### Query

```spl
index=botsv1 "192.168.250.70"
| stats count by sourcetype
| sort -count
```

### Purpose

Identify all telemetry sources that contain events related to the investigated host.

### Findings

| Sourcetype | Events |
|------------|-------:|
| Suricata | 43,350 |
| HTTP Stream | 22,681 |
| IIS | 22,613 |
| FortiGate UTM | 14,302 |
| TCP Stream | 11,590 |
| IP Stream | 2,002 |
| Sysmon | 1,881 |
| FortiGate Traffic | 1,641 |
| DNS Stream | 51 |
| Windows Security Logs | 29 |

### Analyst Notes

The target host appears across multiple telemetry sources, allowing investigation beyond IDS alerts alone.

---

## Investigation 2 – Most Requested HTTP Resources

### Query

```spl
index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70"
| top uri limit=20
```

### Purpose

Identify the web resources receiving the highest number of requests.

### Findings

| URI | Requests |
|-----|---------:|
| /joomla/index.php/component/search/ | 11,928 |
| /joomla/administrator/index.php | 1,248 |
| /joomla/index.php | 787 |
| / | 675 |
| /joomla/agent.php | 194 |

### Analyst Notes

The HTTP activity is heavily focused on a Joomla web application.

Frequent requests to the Joomla administrator interface suggest that administrative resources were actively accessed during the observed period.

At this stage, the available data does **not** indicate whether this activity represents normal administration, automated scanning, or attempted exploitation.

---

## Investigation 3 – HTTP Method Distribution

### Query

```spl
index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70"
| top http_method
```

### Purpose

Determine how clients are interacting with the web application.

### Findings

| HTTP Method | Count |
|-------------|------:|
| POST | 14,238 |
| GET | 5,976 |
| OPTIONS | 5 |
| TRACE | 1 |
| PROPFIND | 1 |

### Analyst Notes

POST requests account for the majority of observed HTTP traffic.

POST requests are commonly associated with:

- User authentication
- Form submissions
- Administrative actions
- File uploads

The presence of TRACE and PROPFIND methods is uncommon; however, each appeared only once and does not, by itself, indicate malicious activity.

---

## Evidence Summary

### Network Evidence

- High volume of Suricata IDS alerts.
- Significant HTTP activity involving the target host.
- Multiple telemetry sources reference the same destination system.

### Web Evidence

- Joomla is the primary web application.
- Administrative resources receive frequent requests.
- POST is the dominant HTTP method.

---

## Analyst Assessment

This hunt successfully pivoted from IDS detections into web telemetry.

The analysis identified sustained interaction with a Joomla-based web application, including repeated access to administrative resources.

Although the activity is noteworthy, the available evidence does not confirm successful exploitation or compromise.

Further investigation is required to determine:

- Which source IPs generated the administrator requests.
- Whether requests occurred in concentrated time periods.
- Whether repeated POST requests indicate authentication attempts or normal application usage.

---

## Next Hunt

**Hunt 02 – Joomla Administrator Access**

The next hunt will focus specifically on:

- Source IPs accessing the Joomla administrator interface.
- Distribution of GET versus POST requests.
- Request timeline analysis.
- User-Agent analysis.
- Identification of repeated or anomalous client behavior.
