# ☁️ Cloud Security Incident Report – Public S3 Bucket Exposure  
**Analyst:** Avi Gaz  
**Tools Used:** AWS Console, AWS CLI, CloudTrail, AI-Assisted Analysis  
**Date:** ___________

---

## 1. Executive Summary  
A misconfigured Amazon S3 bucket named **company-finance-data** was found to be publicly accessible on the internet.  
The bucket contained sensitive internal files, including financial spreadsheets and operational logs.  
No evidence of data exfiltration was found, but the exposure presented a significant security risk.

This incident highlights the danger of cloud misconfiguration — the most common cause of cloud breaches.

---

## 2. How the Exposure Was Found  
An automated security scanner alerted that the S3 bucket policy allowed:

```
"Effect": "Allow",
"Principal": "*",
"Action": "s3:GetObject"
```

This means **anyone on the internet** could download the files.

---

## 3. Indicators of Misconfiguration (IOCs)

### Bucket Policy Issues  
- Public read access (`s3:GetObject`)  
- No enforcement of encryption  
- Missing block-public-access settings  

### Access Logs  
CloudTrail showed several anonymous “GetObject” attempts:

```
Source IP: 185.22.xxx.xxx
UserAgent: aws-cli/2.8.7
EventName: GetObject
Bucket: company-finance-data
```

### File Sensitivity  
- finance.xlsx  
- salaries-2023.csv  
- internal-report.pdf  

---

## 4. Timeline of Events  

| Time  | Event | Description |
|-------|--------|-------------|
| 11:42 | Alert Triggered | Scanner detects bucket with public read access |
| 11:45 | Analyst Review | Policy inspected — exposure confirmed |
| 11:47 | Logs Checked | CloudTrail shows external GET attempts |
| 11:50 | Mitigation Applied | Public access removed, bucket locked |
| 11:53 | Investigation Complete | No confirmed data exfiltration |
| 12:00 | Hardening | Encryption + new IAM policy applied |

---

## 5. Evidence  

### Bucket Policy Before Fix  
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::company-finance-data/*"
    }
  ]
}
```

### CloudTrail Log  
```
EventName: GetObject
UserIdentity: Anonymous
SourceIP: 185.22.xxx.xxx
Bucket: company-finance-data
Object: salaries-2023.csv
Status: AccessDenied (after fix)
```

---

## 6. Root Cause  
The S3 bucket was manually created without applying required security baselines.  
A misconfigured IAM policy allowed **public read access**, which exposed sensitive files to the internet.

This is a common cloud vulnerability caused by human error.

---

## 7. Mitigations  
- Applied **Block Public Access** on the bucket  
- Removed public statements from the bucket policy  
- Enabled server-side encryption (SSE-S3)  
- Audited all S3 buckets for similar misconfigurations  
- Created an automatic AI-based rule to detect risky S3 policies  
- Required mandatory peer-review for IAM changes  

---

## 8. Final Assessment  
Severity: **High**  
Although no successful exfiltration was confirmed, the exposure had the potential for major data leakage.

---

## 9. Lessons Learned  
- Always enable **Block Public Access** on all buckets  
- Use automated AI-based cloud posture tools  
- Require IAM policy reviews  
- Implement encryption by default  
- Train staff on cloud misconfiguration risks  

