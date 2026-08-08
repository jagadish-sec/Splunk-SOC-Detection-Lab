# 🚀 Phase 1 — Lab Setup & Environment Baseline

> **Status:** ✅ Completed

---

## 🎯 Objective

Build a fully functional Splunk Enterprise lab using the **BOTS v1 Attack Only** dataset and establish an initial environment baseline before beginning detection engineering and threat hunting.

---

# 🖥️ Lab Environment

| Component | Value |
|-----------|-------|
| **SIEM** | Splunk Enterprise 10.4.2 |
| **Operating System** | Windows 11 |
| **Dataset** | BOTS v1 Attack Only |
| **Index** | `botsv1` |
| **Time Range** | Aug 10, 2016 – Aug 24, 2016 |
| **Events Indexed** | **955,807** |

---

# ✅ Tasks Completed

- Installed Splunk Enterprise
- Configured local Splunk instance
- Created `botsv1` index
- Imported the BOTS v1 Attack Only dataset
- Verified successful ingestion
- Confirmed searchable events
- Established initial environment baseline

---

# 🔍 Environment Profiling

## Log Sources Identified

- Windows Security Logs
- Windows Sysmon
- Suricata IDS
- TCP Streams
- SMB Traffic
- HTTP Traffic
- DNS Traffic
- IIS Logs
- FortiGate Firewall Logs
- Nessus Scan Results

---

## Systems Discovered

| Host |
|------|
| `splunk-02` |
| `we8105desk` |
| `we9041srv` |
| `we1149srv` |
| `suricata-ids.waynecorpinc.local` |

---

## Authentication Baseline

Successful Windows authentication events (**Event ID 4624**) were analyzed to understand normal login behavior.

### Primary Accounts Observed

| Account |
|---------|
| Administrator |
| bob.smith |
| SYSTEM |
| NT AUTHORITY\SYSTEM |
| NETWORK SERVICE |
| LOCAL SERVICE |
| ANONYMOUS LOGON |
| WE8105DESK$ |
| WE9041SRV$ |
| WE1149SRV$ |

---

# 🛠️ Skills Demonstrated

- Splunk Enterprise Deployment
- Data Ingestion
- Index Management
- SPL (Search Processing Language)
- Windows Security Log Analysis
- Environment Profiling
- Authentication
```
