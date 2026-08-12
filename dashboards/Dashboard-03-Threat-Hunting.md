# Dashboard 03 — Threat Hunting

## Purpose

This analyst-focused dashboard consolidates the Phase 3 investigations into one view. It supports correlation across IDS alerts, Windows authentication activity, SMB access, and suspicious process execution without duplicating the executive or detection-engineering dashboards.

## Panels

| # | Panel | Source | Investigation value |
| --- | --- | --- | --- |
| 1 | Top Victim Hosts | `Most_Attacked.csv` | Prioritises hosts repeatedly targeted during hunts. |
| 2 | Top Attack Sources | `top_attack_ip.csv` | Identifies recurring source IPs. |
| 3 | Attack Timeline | `attack_timeline.csv` | Presents the Hunt 01–05 investigation sequence. |
| 4 | Authentication Investigation | `Windows_Logons.csv` | Correlates accounts and hosts. |
| 5 | Windows Security Investigation | `Windows_Security_Event_Analysis.csv` | Shows Event ID distribution. |
| 6 | SMB Investigation | `SMB_Activity.csv` | Lists observed SMB communication. |
| 7 | Share Access Investigation | Three share and privilege lookups | Correlates share access, inventory, and privileged accounts. |
| 8 | CMD Process Investigation | `CMD_sus.csv` | Shows Event ID 4688 command execution evidence. |
| 9 | PHP-CGI Investigation | `php-cgi_sus.csv` | Shows PHP-CGI process execution tied to the Joomla context. |

## Timeline narrative

The `attack_timeline.csv` lookup captures the Phase 3 investigation story:

1. Joomla attack observed at 03:10.
2. Authentication events reviewed at 03:12.
3. SMB share access investigated at 03:14.
4. `cmd.exe` execution identified at 03:16.
5. PHP-CGI activity identified at 03:18.

## Install in Splunk

Upload `dashboards/lookups/attack_timeline.csv` alongside the existing Dashboard 02 lookup files. Then create a dashboard from source using `dashboards/threat_hunting_dashboard.xml` and save it as **Threat Hunting**.
