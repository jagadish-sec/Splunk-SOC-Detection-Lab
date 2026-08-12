# Hunt 04 – Web Server Investigation

## Hunt Objective

Investigate HTTP activity observed on the web server to identify request patterns, client behavior, and potential indicators of malicious web activity.

---

# Hunt Hypothesis

If the Joomla web server is being actively targeted, HTTP telemetry should reveal suspicious request patterns, repeated access to sensitive resources, and concentrated activity from specific client systems.

---

# Investigation 1 – Most Requested Web Resources

### SPL Query

```spl
index=botsv1 sourcetype=stream:http
| top uri
```

### Purpose

Identify the most frequently requested resources hosted on the web server.

### Findings

| URI | Requests |
|------|---------:|
| /joomla/index.php/component/search/ | 11,928 |
| /joomla/administrator/index.php | 1,248 |
| / | 897 |
| /joomla/index.php | 787 |
| /joomla/agent.php | 194 |

### Analyst Notes

The majority of HTTP traffic targeted Joomla resources.

The administrator portal ranked as the second most requested resource, confirming observations made during previous hunts.

---

# Investigation 2 – HTTP Response Codes

### SPL Query

```spl
index=botsv1 sourcetype=stream:http
| top status
```

### Purpose

Determine how the web server responded to incoming requests.

### Findings

| HTTP Status | Requests |
|-------------|---------:|
| 303 | 11,365 |
| 200 | 4,365 |
| 404 | 2,416 |
| 500 | 1,515 |
| 400 | 62 |

### Analyst Notes

Most responses were HTTP **303 Redirect**, indicating clients were frequently redirected.

A significant number of **404** and **500** responses were also observed, suggesting clients attempted to access resources that either did not exist or generated server-side errors.

---

# Investigation 3 – HTTP Methods

### SPL Query

```spl
index=botsv1 sourcetype=stream:http
| top http_method
```

### Purpose

Determine how clients interacted with the web application.

### Findings

| Method | Requests |
|---------|---------:|
| POST | 14,248 |
| GET | 6,639 |
| HEAD | 31 |
| OPTIONS | 5 |
| TRACE | 1 |

### Analyst Notes

POST requests account for the majority of HTTP traffic.

POST requests commonly indicate:

- Authentication attempts
- Form submissions
- Administrative actions

The presence of TRACE is uncommon but occurred only once and is insufficient to indicate malicious activity on its own.

---

# Investigation 4 – Top Client Systems

### SPL Query

```spl
index=botsv1 sourcetype=stream:http
| top src_ip
```

### Purpose

Identify the clients generating the largest volume of HTTP requests.

### Findings

| Source IP | Requests |
|-----------|---------:|
| 40.80.148.42 | 17,547 |
| 23.22.63.114 | 1,429 |
| 192.168.2.50 | 818 |
| 192.168.250.100 | 93 |
| 192.168.250.70 | 7 |

### Analyst Notes

The majority of HTTP requests originated from **40.80.148.42**.

The previously identified host **23.22.63.114** remained a significant source of administrator page requests and continued to warrant investigation.

---

# Investigation 5 – User-Agent Analysis

### SPL Query

```spl
index=botsv1 sourcetype=stream:http
| top user_agent
```

### Findings

No results were returned.

### Analyst Notes

The available dataset does not expose a populated HTTP User-Agent field within the Stream HTTP logs.

Because of this limitation, browser or client application identification could not be performed.

---

# Investigation 6 – Joomla Administrator Activity

### SPL Query

```spl
index=botsv1 sourcetype=stream:http uri="/joomla/administrator/index.php"
| stats count by src_ip http_method
| sort -count
```

### Findings

| Source IP | Method | Requests |
|-----------|--------|---------:|
| 23.22.63.114 | GET | 823 |
| 23.22.63.114 | POST | 412 |
| 40.80.148.42 | POST | 13 |

### Analyst Notes

Nearly all administrator portal activity originated from a single external IP address.

The combination of repeated GET and POST requests suggests active interaction with the administrator interface rather than simple page browsing.

---

# Investigation 7 – HTTP Activity Timeline

### SPL Query

```spl
index=botsv1 sourcetype=stream:http
| timechart span=5m count
```

### Purpose

Visualize request volume over time.

### Findings

A sustained burst of HTTP activity was observed beginning around **03:05** and continuing through the investigation period.

### Analyst Notes

The timeline aligns closely with Suricata IDS alerts and administrator page requests observed during previous hunts.

---

# Evidence Summary

## Confirmed

- Heavy traffic targeting Joomla resources.
- Significant administrator page activity.
- POST requests dominated HTTP traffic.
- HTTP activity originated primarily from a small number of client systems.
- Sustained request activity occurred during the same period as IDS alerts.

## Not Observed

- User-Agent information.
- Direct evidence of successful exploitation.
- Evidence confirming authenticated administrator access.

---

# Analyst Assessment

Analysis of the web server telemetry identified concentrated activity against a Joomla-based web application. The administrator interface received repeated GET and POST requests, primarily from a single external source IP. HTTP response codes indicate a mixture of successful responses, redirects, missing resources, and server errors.

Although these observations demonstrate suspicious interaction with the web application, the available telemetry does not confirm successful exploitation or compromise. The findings are consistent with reconnaissance or repeated application interaction and should be correlated with additional endpoint telemetry where available.

---

# MITRE ATT&CK Mapping

| Technique | ATT&CK ID |
|------------|-----------|
| Active Scanning | T1595 |
| Exploit Public-Facing Application (Potential) | T1190 |
| Application Layer Protocol | T1071.001 |

---

# Conclusion

The web server investigation successfully characterized HTTP activity associated with the targeted Joomla application. Combined with previous hunts, the evidence indicates sustained interaction with administrative resources but does not provide confirmation of successful compromise.

---

## Next Hunt

**Hunt 05 – Threat Hunting Summary**

Produce a consolidated report summarizing the complete hunting process, evidence collected, conclusions reached, and recommendations for future investigation.
