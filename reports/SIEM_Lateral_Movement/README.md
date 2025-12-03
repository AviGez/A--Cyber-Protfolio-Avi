# 🛡️ SIEM Incident Report – Lateral Movement  
**Analyst:** Avi Gaz  
**Tools Used:** Elastic SIEM + Microsoft Defender XDR (AI-Assisted)  
**Date:** ___________

---

## 1. Executive Summary  
A lateral movement attempt was identified inside the organization.  
An attacker with access to **INTERNAL-1** attempted to move to **INTERNAL-2** using compromised credentials (**user-admin**).  
Multiple failed logons, an encoded PowerShell execution, and a later successful network logon indicated credential compromise.  
The attacker was contained before achieving higher privileges.

---

## 2. Why INTERNAL-2 Was Targeted  
The attacker targeted **INTERNAL-2 because it holds higher privileges** and grants access to sensitive systems.

---

## 3. Indicators of Compromise (IOCs)

### Hostnames  
- Source: **INTERNAL-1**  
- Target: **INTERNAL-2**

### User Activity  
- Account: **user-admin**  
- Pattern: Failed logons → Encoded PowerShell → Successful login

### Network Indicators  
- Repeated WinRM attempts on port **5985**  
- Burst of authentication failures  

### Behavioral Indicators  
- Base64-encoded PowerShell execution  
- Possible credential harvesting behavior  
- Remote command execution via WinRM  

---

## 4. Timeline of Events  

| Time  | Event | Description |
|-------|--------|-------------|
| 13:02 | Failed Logons | 6 failed WinRM attempts from INTERNAL-1 → INTERNAL-2 |
| 13:04 | Suspicious Execution | Encoded PowerShell command detected |
| 13:05 | Behavioral Alert | Possible LSASS credential extraction pattern |
| 13:07 | Successful Logon | user-admin authenticates to INTERNAL-2 (Event 4624 - LogonType 3) |
| 13:08 | Remote Command | PowerShell executed via WinRM |
| 13:10 | Mitigation Applied | Locked account + disabled WinRM |

---

## 5. Log Evidence  

### Failed Logons (EventID 4625)
```
EventID: 4625  
Status: 0xC000006A  
User: user-admin  
Source: INTERNAL-1  
Target: INTERNAL-2  
```

### Encoded PowerShell  
```
powershell.exe -enc SQBtAHAAbwByAHQAIABTAHkAcwB0AGUAbQA=
```

### Successful Logon (EventID 4624)
```
LogonType: 3  
User: user-admin  
```

### Remote Command Execution  
```
winrm invoke "powershell {Get-LocalUser}"
```

---

## 6. Root Cause  
Weak or reused credentials allowed compromise of **user-admin**, enabling lateral movement attempts.

---

## 7. Mitigations  
- Locked compromised account  
- Disabled WinRM temporarily  
- Reset passwords and authentication keys  
- Added SIEM rule to detect encoded PowerShell  
- Enforced MFA  
- Strengthened segmentation  

---

## 8. Final Assessment  
Severity: **Medium–High**  
The attacker achieved initial access but did not escalate to higher privileged systems.

---

## 9. Lessons Learned  
- Strengthen password policies  
- Monitor encoded PowerShell activity  
- Improve segmentation  
- Enforce least privilege  
