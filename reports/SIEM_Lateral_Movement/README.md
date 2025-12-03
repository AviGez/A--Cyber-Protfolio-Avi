# SIEM Lateral Movement Report
# 🛡️ SIEM Incident Report – Lateral Movement  
**Analyst:** Avi Gaz  
**Tools Used:** Elastic SIEM + Microsoft Defender XDR (AI-Assisted)  
**Date:** ___________

---

## 1. Executive Summary  
A lateral movement attempt was identified inside the organization.  
An attacker with access to **INTERNAL-1** attempted to move to **INTERNAL-2** using compromised credentials (**user-admin**).  
Multiple failed logins, encoded PowerShell execution, and a later successful remote login indicated a credential compromise.  
The attack was contained before escalation.

---

## 2. Why INTERNAL-2 Was Targeted  
The attacker targeted **INTERNAL-2 because it holds higher privileges** and grants access to more sensitive systems.

---

## 3. Indicators of Compromise (IOCs)

### Hostnames  
- Source: **INTERNAL-1**  
- Target: **INTERNAL-2**

### User Activity  
- Account: **user-admin**  
- Pattern: Failed logons → Suspicious PowerShell → Successful login

### Network Indicators  
- Repeated WinRM attempts on port **5985**  
- Burst of authentication attempts  

### Behavioral Indicators  
- Base64 PowerShell execution  
- Possible credential harvesting (LSASS-like behavior)  
- Remote command execution after login  

---

## 4. Timeline of Events (AI-Assisted)

| Time  | Event | Description |
|-------|--------|-------------|
| 13:02 | Failed Logons | 6 failed WinRM attempts from INTERNAL-1 → INTERNAL-2 |
| 13:04 | Suspicious Execution | Encoded PowerShell command detected |
| 13:05 | Behavioral Alert | Possible LSASS credential extraction |
| 13:07 | Successful Login | user-admin logs into INTERNAL-2 |
| 13:08 | Remote Command | POWERHELL executed via WinRM |
| 13:10 | Mitigation | Account locked, WinRM disabled, rules updated |

---

## 5. Log Evidence

### Failed Logons (Event 4625)
