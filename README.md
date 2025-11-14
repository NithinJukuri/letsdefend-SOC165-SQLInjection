
# 🚨 SQL Injection Attempt Investigation (LetsDefend Case 115)

## 🧾 Raw Alert Log (Important Summary)
This section contains the *exact log data* that triggered the investigation, placed here at the top for clarity.

```
Alert Name: SOC165 - Possible SQL Injection Payload Detected
Event ID: 115
Event Time: Feb 25, 2022, 11:34 AM
Source IP: 167.99.169.17
Destination IP: 172.16.17.18
Hostname: WebServer1001
Request Method: GET
Requested URL: /search/?q=" OR 1 = 1 -- -
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:40.0) Gecko/20100101 Firefox/40.1
Device Action: Allowed
Trigger Reason: Requested URL Contains OR 1 = 1
HTTP Response Status: 500
```

---

## 📝 Overview
This repository documents the full investigation of **EventID: 115**, triggered in LetsDefend due to a SQL Injection payload detected in the HTTP request.

The attacker repeatedly attempted SQL Injection payloads targeting the `/search` endpoint of the internal web server.

This README provides a complete SOC-style analysis covering logs, behavior, evidence, and findings.

---

## 📌 Summary of Findings

| Category | Result |
|---------|--------|
| **Alert Type** | Web Attack – SQL Injection |
| **Source IP** | `167.99.169.17` (Malicious, flagged on VirusTotal) |
| **Destination** | `172.16.17.18` (Company Web Server) |
| **HTTP Status** | 500 for all malicious requests |
| **Attack Result** | **Unsuccessful** |
| **Final Classification** | **True Positive (Attempted Attack)** |
| **Tier 2 Escalation** | Not Required |

---

## 📊 1. Evidence & Log Details

### 🔹 Observed SQL Injection Payloads
```
' 
" OR 1 = 1 -- -
' OR '1
' OR 'x'='x
1' ORDER BY 3 --
```

### 🔹 HTTP Response Comparison
| Payload | Status | Response Size |
|---------|--------|----------------|
| SQLi Attempts | 500 | 948 bytes |
| Normal Request | 200 | 3547 bytes |

Same-size error responses confirm the SQL queries failed.  
No data leakage or unusual behavior.

---

## 🔍 2. Source IP Analysis
**IP:** `167.99.169.17`  
- Hosted on DigitalOcean  
- Malicious reputation  
- VirusTotal: 4 vendors flagged  
- Behavior consistent with automated scanning tools (SQLMap/Havij)

---

## 🖥 3. Endpoint Activity Review
Checked:
- Processes  
- Terminal History  
- Network Actions  
- Browser Activity  

**No suspicious activity detected**, confirming no command execution (like `xp_cmdshell`) or compromise.

---

## 🧠 4. Attack Analysis

### ✔ Attack Type:
SQL Injection (error-based + boolean-based enumeration)

### ✔ Success?
❌ **No — All payloads failed (HTTP 500)**

### ✔ Real Attack?
✔ **Yes — True Positive**

---

## 🛡 MITRE ATT&CK Mapping

| Technique | ID |
|----------|----|
| SQL Injection | T1190 – Exploit Public-Facing Application |
| Input Manipulation | T1059.008 |
| Recon/Scanning | T1595 |

---

## 📁 Artifacts

### IOCs
```
Source IP: 167.99.169.17
Payload: " OR 1 = 1 -- -
Target URL: /search/?q=" OR 1 = 1 -- -
```

---

## ✔ Final Conclusion
This was a **True Positive SQL Injection attempt**, but the attack **did not succeed**.  
Security controls and backend error handling prevented exploitation.

No signs of compromise.

Case closed.

---

## 📎 Author
**Jukuri Nithin Kumar**  
SOC Analyst Trainee – LetsDefend Labs  
