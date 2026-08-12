# Hunt 02 – Joomla Administrator Access

## Objective

Investigate access to the Joomla administrator interface to determine which systems interacted with the administrative portal and whether the observed activity appears suspicious.

---

## Hunt Hypothesis

Repeated requests to the Joomla administrator interface may indicate legitimate administrative activity, automated scanning, or an attempted compromise.

---

## Investigation 1 – Identify Source IPs

### Query

```spl
index=botsv1 sourcetype=stream:http uri="/joomla/administrator/index.php"
| top src_ip
```

### Purpose

Identify which clients generated requests to the Joomla administrator portal.

### Findings

| Source IP | Requests | Percentage |
|-----------|---------:|-----------:|
| 23.22.63.114 | 1,235 | 98.96% |
| 40.80.148.42 | 13 | 1.04% |

### Analyst Notes

Nearly all administrator requests originated from a single external IP address (**23.22.63.114**). Such concentration warrants additional investigation because repeated requests from one source can indicate automated activity.

---

## Investigation 2 – HTTP Methods

### Query

```spl
index=botsv1 sourcetype=stream:http uri="/joomla/administrator/index.php"
| top http_method
```

### Purpose

Determine how the administrator interface is being accessed.

### Findings

| HTTP Method | Count |
|-------------|------:|
| GET | 823 |
| POST | 425 |

### Analyst Notes

The administrator portal received both GET and POST requests.

- GET requests indicate page retrieval.
- POST requests typically represent authentication attempts or form submissions.

The presence of numerous POST requests suggests interaction beyond simply viewing the login page.

---

## Investigation 3 – Request Timeline

### Query

```spl
index=botsv1 sourcetype=stream:http uri="/joomla/administrator/index.php"
| timechart span=5m count
```

### Findings

The majority of requests occurred during a short period beginning around **2016-08-11 03:15**.

Observed counts:

| Time | Requests |
|------|---------:|
| 03:15 | 1,236 |
| 03:20 | 4 |
| Remaining intervals | 0 |

### Analyst Notes

The traffic occurred as a concentrated burst rather than normal user activity, which may indicate scripted or automated access.

---

## Investigation 4 – Request Details

### Query

```spl
index=botsv1 sourcetype=stream:http uri="/joomla/administrator/index.php"
| table _time src_ip dest_ip http_method user_agent
```

### Findings

- Destination host: **192.168.250.70**
- Primary source IP: **23.22.63.114**
- Both GET and POST requests were observed.
- User-Agent information was not populated in the available telemetry.

### Analyst Notes

Although User-Agent values were unavailable, the request pattern strongly indicates repeated interaction with the administrator interface from a single external host.

---

# Evidence Summary

## Observations

- A single external IP (**23.22.63.114**) generated approximately **99%** of administrator requests.
- Both GET and POST methods were used.
- Requests occurred within a short burst of activity.
- The target was the Joomla administrator portal hosted on **192.168.250.70**.

---

# Analyst Assessment

This hunt identified a concentrated series of requests targeting the Joomla administrative interface.

While the dataset does not confirm successful authentication or exploitation, the combination of:

- repeated requests from one external IP,
- significant POST activity,
- and a tightly grouped timeline

makes this activity worthy of further investigation in a real SOC environment.

Additional authentication logs or web server responses would be required to determine whether any login attempts succeeded.

---

## MITRE ATT&CK Mapping

| Technique | ID |
|-----------|----|
| Active Scanning | T1595 |
| Exploit Public-Facing Application (possible) | T1190 |
| Valid Accounts (possible if authentication succeeded) | T1078 |

---

## Next Hunt

**Hunt 03 – Authentication Correlation**

Objective:

- Correlate Joomla administrator activity with Windows authentication events.
- Determine whether administrator portal access was followed by successful logons or other host activity.
