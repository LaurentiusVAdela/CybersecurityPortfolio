# 🐷 BTLO - Piggy Challenge Walkthrough

## 📌 Overview
The **Piggy Lab** from Blue Team Labs Online (BTLO) focuses on analyzing multiple PCAP files to identify malicious activity, perform OSINT, and map findings to the **MITRE ATT&CK framework**.  
This lab strengthens skills in **network forensics, malware attribution, and threat intelligence**.

---

## 🧾 Scenario
The SOC team was tasked with analyzing packet captures to investigate unusual traffic patterns.  
Through careful analysis of SSH, DNS, and other protocols, we uncovered evidence of **malware infections, data exfiltration, and crypto-mining activity**.

---

## 🔍 Questions & Findings

### **Q1. First PCAP — SSH Analysis**  
📂 File: `FirstOne.pcap`  
- **Task:** Identify the IP address involved in SSH activity.  
- **Method:** Filtered by keyword `ssh` in Wireshark.  
- **Answer:** `35.211.33.16`

---

### **Q2. First PCAP — Data Transfer**  
- **Task:** Determine total data transferred.  
- **Method:** Wireshark → **Statistics → Endpoints** → review SSH session size.  
- **Answer:** `1131M`

---

### **Q3. Second PCAP — Malware Identification**  
📂 File: `PCAPTwo.pcap`  
- **Task:** Identify the malware present.  
- **Method:** Traffic pattern analysis revealed signatures.  
- **Answer:** `trickbot`

---

### **Q4. Third PCAP — ASN Analysis**  
📂 File: `PCAPThree.pcap`  
- **Task:** Two IPs communicated over unusual ports (8080, 8000).  
- **Method:** OSINT checks on IPs.  
- **Findings:**  
  - `194.233.171.171` → ASN **63949** (Akamai Cloud Services)  
  - `104.236.57.24` → ASN **14061** (DigitalOcean)

---

### **Q5. Third PCAP — Malware Category**  
- **Task:** Identify malware type associated with the IPs.  
- **Method:** OSINT research.  
- **Answer:** `Miner` (Cryptocurrency Mining Malware)

---

### **Q6. Third PCAP — MITRE ATT&CK Technique**  
- **Task:** Map activity to ATT&CK framework.  
- **Method:** Researched crypto-mining techniques in MITRE ATT&CK.  
- **Answer:** `T1496` — *Resource Hijacking*

---

### **Q7. Fourth PCAP — DNS TXT Query**  
📂 File: `PCAPFour.pcap`  
- **Task:** Identify when the first TXT record query was made.  
- **Method:** Applied filter `dns.qry.type == 16` and converted to seconds since capture start.  
- **Answer:** `8.527712`

---

## 🛠️ Tools & Techniques Used
- **Wireshark** (PCAP analysis, filtering, statistics)  
- **OSINT Lookups** (ASN and IP reputation checks)  
- **MITRE ATT&CK Mapping**  
- **Malware Attribution** research (Trickbot, Crypto-mining indicators)

---

## 🎯 Key Takeaways
- SSH traffic analysis revealed suspicious external connections.  
- OSINT attribution showed infrastructure tied to **Akamai & DigitalOcean** abused for crypto-mining.  
- MITRE ATT&CK framework helped classify **resource hijacking (T1496)** activity.  
- DNS TXT record queries may be leveraged for **command & control (C2)**.  

---

## ✅ Lab Results
- **Q1:** `35.211.33.16`  
- **Q2:** `1131M`  
- **Q3:** `trickbot`  
- **Q4:** `ASN: 63949, 14061`  
- **Q5:** `Miner`  
- **Q6:** `T1496`  
- **Q7:** `8.527712`

---

📌 **This lab reinforced network forensic analysis, malware attribution, and linking findings to adversary tactics and techniques (ATT&CK).**

