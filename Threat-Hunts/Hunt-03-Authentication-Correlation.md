# Hunt 03 – Authentication Correlation

## Hunt Objective

Determine whether the suspicious activity observed against the Joomla administrator interface resulted in Windows authentication events or evidence of host compromise.

---

## Hunt Hypothesis

If an attacker successfully interacted with the Joomla administrator interface, corresponding Windows authentication events or related host activity may be observed during the same time period.

---

# Investigation 1 – Windows Successful Logons

### SPL Query

```spl
index=botsv1 sourcetype=wineventlog:security EventCode=4624
earliest="08/11/2016:03:10:00"
latest="08/11/2016:03:20:00"
| table _time host Account_Name LogonType
| sort _time
```

### Purpose

Identify successful Windows authentication events during the period of observed web activity.

### Findings

- Successful authentication events were present.
- Authentication activity primarily involved:
  - `WE9041SRV$`
  - `WE1149SRV$`
- Events originated from server machine accounts rather than interactive user logons.

### Analyst Notes

The observed authentication activity appears consistent with normal Windows machine account communication. No evidence of interactive user authentication was identified during the investigated timeframe.

---

# Investigation 2 – Failed Logons

### SPL Query

```spl
index=botsv1 sourcetype=wineventlog:security EventCode=4625
earliest="08/11/2016:03:10:00"
latest="08/11/2016:03:20:00"
| table _time host Account_Name Failure_Reason
| sort _time
```

### Purpose

Determine whether failed Windows authentication attempts occurred during the suspected attack window.

### Findings

**No events returned.**

### Analyst Notes

No failed Windows authentication attempts were observed. This suggests there is no evidence of Windows password guessing or brute-force activity during the investigated timeframe.

---

# Investigation 3 – IIS Correlation

### SPL Query

```spl
index=botsv1 sourcetype=iis
cs_uri_stem="/joomla/administrator/index.php"
| table _time c_ip cs_method sc_status cs_username
```

### Purpose

Correlate Windows authentication activity with web server access to the Joomla administrator interface.

### Findings

- Source IP: **23.22.63.114**
- HTTP Method: **GET**
- HTTP Status: **200 OK**
- Username field: Empty

### Analyst Notes

The Joomla administrator page was successfully served to the client. However, IIS logs did not record an authenticated username, providing no evidence that a successful Windows-authenticated session was established.

---

# Investigation 4 – Suricata Correlation

### SPL Query

```spl
index=botsv1 sourcetype=suricata event_type=alert dest_ip="192.168.250.70"
| table _time src_ip alert.signature alert.category alert.severity
| sort _time
```

### Purpose

Determine whether IDS alerts align with the observed web and authentication activity.

### Findings

- Multiple Suricata alerts targeted **192.168.250.70**.
- Alerts originated primarily from **23.22.63.114**.
- Alert activity occurred before and during access to the Joomla administrator interface.

### Analyst Notes

The IDS alerts confirm that suspicious network activity was directed at the web server during the same period as the observed administrator page requests.

---

# Correlation Timeline

| Time | Event |
|------|-------|
| ~03:06 | Suricata detected suspicious web activity targeting **192.168.250.70** |
| 03:10–03:20 | Windows machine account authentication events observed |
| ~03:15 | Joomla administrator page requested (HTTP 200) |
| Same period | No failed Windows authentication events detected |
| Investigation conclusion | No evidence of Windows host compromise identified |

---

# Evidence Summary

### Confirmed

- Suricata detected suspicious web activity.
- Joomla administrator interface was accessed.
- IIS successfully served administrator page requests.
- Windows authentication events occurred during the investigation window.

### Not Observed

- Failed Windows logons (Event ID 4625)
- Interactive user authentication
- Evidence of successful Windows account compromise
- Evidence linking web activity directly to Windows host compromise

---

# Analyst Assessment

This investigation correlated network IDS alerts, web server activity, and Windows authentication logs.

Although suspicious requests targeted the Joomla administrator interface, the available telemetry does not indicate successful Windows authentication or compromise. The authentication events observed were limited to normal machine account activity, and no failed logons or interactive user sessions were identified.

Based on the available evidence, the activity is assessed as suspicious web probing without confirmed host compromise.

---

# MITRE ATT&CK Mapping

| Technique | ID |
|-----------|----|
| Active Scanning | T1595 |
| Exploit Public-Facing Application (Potential) | T1190 |
| Valid Accounts (Not Confirmed) | T1078 |

---

# Conclusion

The hunt successfully correlated data across multiple telemetry sources, including Suricata IDS, IIS web logs, and Windows Security logs. While the investigation confirmed repeated access to the Joomla administrator interface, no evidence was found to support successful authentication or compromise of the Windows host. Continued monitoring and additional endpoint telemetry would be required to determine whether subsequent attacker activity occurred beyond the scope of this investigation.

---

## Next Hunt

**Hunt 04 – Web Server Investigation**

Focus areas:

- HTTP status code distribution
- Top requested URIs
- Source IP analysis
- User-Agent analysis
- Web application request patterns
- Identification of anomalous web activity
