# Hunt 05 – Threat Hunting Summary

## Hunt Objective

Summarize the findings from the threat hunting activities conducted throughout Phase 3 and assess whether the available evidence indicates a successful compromise of the monitored environment.

---

# Investigation Scope

The investigation focused on suspicious web activity targeting a Joomla web server identified through Suricata IDS alerts. Multiple data sources were correlated to determine whether the observed network activity resulted in authentication events, host compromise, or malicious post-exploitation activity.

---

# Hunts Performed

| Hunt | Status |
|------|--------|
| Hunt 01 – Web Attack Pivot | ✅ Completed |
| Hunt 02 – Joomla Administrator Access | ✅ Completed |
| Hunt 03 – Authentication Correlation | ✅ Completed |
| Hunt 04 – Web Server Investigation | ✅ Completed |

---

# Hunt 01 Summary

### Objective

Identify systems targeted by Suricata IDS alerts and determine the initial attack surface.

### Key Findings

- High-volume IDS alerts were generated against internal web servers.
- The primary target identified was **192.168.250.70**.
- Multiple HTTP-related signatures indicated suspicious web activity.

### Assessment

Network telemetry suggested active reconnaissance or web application probing.

---

# Hunt 02 Summary

### Objective

Investigate activity targeting the Joomla administrator interface.

### Key Findings

- Administrator portal:
  - `/joomla/administrator/index.php`
- Most requests originated from:

```
23.22.63.114
```

- Both GET and POST requests were observed.
- Administrator page was repeatedly accessed.

### Assessment

The administrator interface was a primary target of the observed activity.

---

# Hunt 03 Summary

### Objective

Determine whether suspicious web activity resulted in Windows authentication events.

### Key Findings

- Windows Event ID 4624 events were present.
- Machine account authentication was observed.
- No interactive user logons were identified.
- Event ID 4625 returned no failed authentication attempts.
- IIS logs showed successful page delivery without authenticated users.

### Assessment

No evidence was found linking the web activity to successful Windows authentication or host compromise.

---

# Hunt 04 Summary

### Objective

Investigate HTTP activity from the perspective of the web server.

### Key Findings

- Joomla resources accounted for the majority of HTTP requests.
- The administrator interface received sustained attention.
- POST requests exceeded GET requests.
- HTTP response codes included:
  - 200
  - 303
  - 404
  - 500
- HTTP activity aligned with previously observed IDS alerts.

### Assessment

The web server experienced sustained interaction consistent with reconnaissance or repeated application access. Available telemetry does not confirm successful exploitation.

---

# Overall Correlation Timeline

```
Suricata IDS Alert
        │
        ▼
HTTP Requests
        │
        ▼
Joomla Administrator Access
        │
        ▼
Windows Authentication Review
        │
        ▼
Web Server Investigation
        │
        ▼
No Confirmed Host Compromise
```

---

# Evidence Collected

## Network

- Suricata IDS alerts
- Source IP analysis
- Destination IP analysis

## Web

- HTTP requests
- HTTP methods
- HTTP response codes
- Joomla administrator activity

## Windows

- Successful authentication events (4624)
- Failed authentication events (4625)

## Server

- IIS web logs
- Timeline correlation

---

# MITRE ATT&CK Coverage

| Tactic | Technique | ATT&CK ID |
|---------|-----------|-----------|
| Reconnaissance | Active Scanning | T1595 |
| Initial Access | Exploit Public-Facing Application (Potential) | T1190 |
| Credential Access | Brute Force (Investigated) | T1110 |
| Persistence | Valid Accounts (Investigated) | T1078 |
| Command and Control | Application Layer Protocol | T1071.001 |

---

# Overall Assessment

The investigation identified sustained web activity targeting a Joomla web application. Suricata IDS alerts, HTTP telemetry, and IIS logs consistently showed repeated interaction with the Joomla administrator interface.

Windows Security logs were reviewed to determine whether the observed web activity resulted in successful authentication or evidence of host compromise. Although machine account authentication events were present, no interactive user logons or failed authentication attempts were observed during the investigation period.

Based on the available telemetry, the activity is assessed as suspicious web reconnaissance or repeated application interaction. The investigation did not identify sufficient evidence to confirm successful exploitation or compromise of the monitored Windows hosts.

---

# Lessons Learned

- Correlating multiple log sources provides stronger investigative context than analyzing a single telemetry source.
- IDS alerts alone are insufficient to confirm compromise and must be validated against endpoint and authentication logs.
- Web server telemetry is valuable for identifying attack patterns and targeted application resources.
- Authentication analysis helps distinguish between reconnaissance and confirmed host compromise.

---

# Recommendations

- Continue monitoring repeated access to administrative web interfaces.
- Enable additional endpoint telemetry where available to improve visibility into post-exploitation activity.
- Monitor repeated POST requests targeting authentication endpoints.
- Develop correlation searches that combine IDS, HTTP, IIS, and Windows Security events to accelerate future investigations.

---

# Phase 3 Outcome

| Category | Status |
|----------|--------|
| IDS Investigation | ✅ |
| Web Application Investigation | ✅ |
| Authentication Correlation | ✅ |
| Web Server Investigation | ✅ |
| Threat Hunting Summary | ✅ |

---

# Conclusion

Phase 3 successfully demonstrated a structured threat hunting workflow by correlating IDS alerts, HTTP traffic, IIS web logs, and Windows Security events. While suspicious activity targeting the Joomla administrator interface was confirmed, no evidence was found to indicate successful authentication or host compromise. The investigation highlights the importance of multi-source correlation when validating potential security incidents and reflects a practical SOC threat hunting methodology.
