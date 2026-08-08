# Day 02 - Environment Profiling

## Objective

Understand the enterprise environment by identifying systems, users, and authentication relationships before investigating suspicious activity.

---

# Host Enumeration

## Query

```spl
index=botsv1
| stats count by host
| sort -count
```

## Findings

| Host | Events | Assessment |
|------|-------:|------------|
| splunk-02 | 293,579 | Splunk Server |
| we8105desk | 244,009 | User Workstation |
| suricata-ids.waynecorpinc.local | 125,584 | IDS Sensor |
| we1149srv | 121,348 | Windows Server |
| we9041srv | 90,300 | Windows Server |
| 192.168.250.1 | 80,922 | FortiGate Firewall |
| 192.168.2.50 | 65 | Auxiliary Host |

---

## Analysis

The environment consists of a centralized Splunk server, multiple Windows systems, a Suricata IDS sensor, and a FortiGate firewall.

The infrastructure appears consistent with a small enterprise network.

---

# User Enumeration

## Query

```spl
index=botsv1 sourcetype=wineventlog:security
| stats count by Account_Name
| sort -count
```

## Findings

### Human User

- bob.smith

### Administrative Accounts

- Administrator

### Machine Accounts

- WE8105DESK$
- WE9041SRV$
- WE1149SRV$

### Windows Service Accounts

- NT AUTHORITY\SYSTEM
- LOCAL SERVICE
- NETWORK SERVICE

### IIS Accounts

- IUSR
- DefaultAppPool

### Application Accounts

- joomla

---

# User-to-Host Mapping

## Query

```spl
index=botsv1 sourcetype=wineventlog:security
| stats count by Account_Name, host
| sort -count
```

## Key Findings

### bob.smith

| Host | Events |
|------|-------:|
| we9041srv | 16,969 |
| we8105desk | 565 |

Primary activity is associated with **we9041srv**, with limited activity on the workstation.

This observation will be revisited during authentication analysis.

---

### Administrator

Authenticated across multiple hosts, consistent with administrative responsibilities.

---

### Web Server Indicators

Both:

- IUSR
- joomla

appear only on:

- we1149srv

This strongly suggests that **we1149srv** hosts an IIS web server running Joomla.

---

# Current Environment Map

Internet

↓

FortiGate Firewall (192.168.250.1)

↓

we8105desk (User Workstation)

↓

we9041srv (Windows Server)

↓

we1149srv (Windows Server / IIS / Joomla)

↓

Suricata IDS

---

# Evidence Summary

High confidence:

- Splunk server identified.
- User workstation identified.
- IDS identified.
- Firewall identified.
- IIS server identified.
- Joomla application identified.

Requires further investigation:

- Why bob.smith primarily authenticates to we9041srv.
- Exact role of we9041srv.
- Authentication patterns across the environment.

---

# SOC Relevance

Environment profiling establishes the baseline required for effective threat hunting.

Without knowing the assets, users, and authentication relationships, it is difficult to determine whether later events represent normal business activity or malicious behavior.

---

## Next Objective

Investigate authentication activity by identifying failed logons, successful logons, and suspicious authentication patterns that may indicate credential attacks or lateral movement.
