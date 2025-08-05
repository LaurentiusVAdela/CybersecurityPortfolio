# 🕵️ Middle Mayhem Lab (Blue Team Labs Online)

## 📌 Scenario
The SOC team at **MiddleMayhem Inc.** detected unusual network traffic directed at their **admin portal**. No confirmed breaches were initially observed.  

As part of the SOC analysis, I was tasked with investigating **SIEM logs provided in Splunk Enterprise** to determine:  
- How attackers bypassed authentication  
- How remote code execution (RCE) was achieved  
- How lateral movement was performed across the network  

---

## 🔎 Investigation & Findings

### **1. Framework Identification**
- Accessed the target web application in a browser.  
- Identified **Next.js v15.0.0** as the JavaScript framework in use.  

---

### **2. Attacker IP Address**
- Splunk Query:
  ```spl
  host="webapp" sourcetype="network"
  ```
- Observed two high-traffic IPs:  
  - `172.217.164.174` → legitimate traffic  
  - `218.92.0.204` → **attacker IP**  
- Attacker attempted to upload a malicious file: **shell.sh**

---

### **3. Unique URIs Accessed**
- Query:
  ```spl
  host="webapp" sourcetype="network" ip_src="218.92.0.204" | stats count by http_request_uri
  ```
- Results showed the attacker accessed **9,930 unique URIs**.  

---

### **4. Hidden Internal URLs**
- Discovered potential internal exposure via:  
  - `/robots.txt`  
  - Additional restricted directories leaking site structure.  

---

### **5. Vulnerability Exploited**
- Research on Next.js v15.0.0 revealed:  
  - **CVE-2025-29927** – *Authorization Bypass Vulnerability*  

---

### **6. Exploit Evidence in Logs**
- Splunk logs revealed suspicious middleware requests.  
- Header indicating CVE exploitation:  
  - `x-middleware-subrequest`

---

### **7. Post-Exploitation Activity**
- After bypassing authorization, the attacker accessed:  
  - `/api/upload`  

---

### **8. Reverse Shell Attempt**
- Attacker uploaded a reverse shell script.  
- Logs indicated **netcat usage**, attempting to connect back to attacker’s host.  
- Extracted **attacker IP and port** from the payload.  

---

### **9. Lateral Movement**
- Further Splunk searches confirmed **SSH brute force attempts**.  
- Technique: password brute force against internal servers.  

---

### **10. Compromised Account**
- Successful login observed for:  
  - **User:** `dbserve`  
- Indicates lateral movement was successful.  

---

## 🛠️ Tools & Techniques Used
- **Splunk Enterprise** (SIEM log analysis)  
- **SPL queries** for filtering attacker activity  
- **OSINT research** for CVE verification  
- Log correlation & traffic volume analysis  

---

## 📌 Key Indicators
- **Attacker IP:** `218.92.0.204`  
- **Unique URIs Accessed:** `9,930`  
- **Interesting URI:** `/api/upload`  
- **Exploit Header:** `x-middleware-subrequest`  
- **Compromised Account:** `dbserve`  

---

## 🔐 Defensive Recommendations
1. **Patch Next.js vulnerabilities** (apply security updates immediately).  
2. **Harden file upload functionality** – restrict executable file types and implement malware scanning.  
3. **Monitor high-volume traffic anomalies** via SIEM.  
4. **Secure SSH access** – enforce MFA, apply IP allowlisting, and use tools like Fail2ban.  
5. **Enhance detection rules** – alert on suspicious HTTP headers (e.g., `x-middleware-subrequest`).  
6. **Regular vulnerability scanning** to detect exposure early.  

---

## 🏁 Summary
The attacker exploited a **Next.js CVE** to bypass authorization, uploaded a reverse shell, and attempted **lateral movement via SSH brute force**.  

By correlating events in **Splunk SIEM**, I identified:  
- The attacker’s IP  
- The exploit method (CVE-2025-29927)  
- The compromised account (`dbserve`)  

This lab reinforced skills in **log analysis, attack chain reconstruction, and SOC detection engineering**.  

---

✍️ **Author:** Laurentius Valdi Adela  
🔗 **Portfolio Repository:** [Cybersecurity Portfolio](https://github.com/LaurentiusVAdela/CybersecurityPortfolio)  
