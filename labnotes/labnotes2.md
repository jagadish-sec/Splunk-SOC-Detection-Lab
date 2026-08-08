## User Enumeration

### Query

```spl
index=botsv1 sourcetype=wineventlog:security
| stats count by Account_Name
| sort -count
```

### Findings

- Human user account identified: **bob.smith**
- Machine accounts:
  - WE8105DESK$
  - WE9041SRV$
  - WE1149SRV$
- Administrative account:
  - Administrator
- Windows service accounts:
  - NT AUTHORITY\SYSTEM
  - LOCAL SERVICE
  - NETWORK SERVICE
- IIS accounts:
  - IUSR
  - DefaultAppPool
- Web application account:
  - joomla
- Anonymous logon events observed (194).

### Analysis

The environment includes Active Directory computer accounts, administrative accounts, Windows service accounts, and an IIS web server. The presence of a Joomla account suggests a hosted Joomla application, which may become relevant during later threat hunting.

### SOC Relevance

Understanding legitimate user, service, and computer accounts establishes a baseline for detecting unusual authentication activity and privilege misuse.
