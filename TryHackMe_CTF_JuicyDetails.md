# 🕵️‍♂️ TryHackMe: Juicy Details – Incident Analysis

**Room:** [Juicy Details (Blue Team)](https://tryhackme.com/room/juicydetails)  
**Difficulty:** Easy  
**Type:** Log Analysis / Blue Team  
**Focus:** Recon, Brute Force, SQL Injection, FTP Abuse, SSH Access  
**Date Completed:** July 2025

---

## 📖 Scenario

You are a SOC Analyst for one of the world’s most popular juice shops. An attacker has breached your web server, and it’s your job to investigate how it happened by analyzing the logs provided by the IT team.

---

## 🎯 Objectives

1. Identify the tools and techniques used by the attacker
2. Discover which endpoints were vulnerable
3. Analyze what sensitive data was accessed
4. Trace how the attacker escalated access

---

## 🧪 Reconnaissance

The attacker targeted the web server using various tools, identified through the `access.log` file by inspecting their **User-Agent** headers and request patterns.

### 🧰 Tools used by attacker (in order):

1. `nmap` – Port/service scanning
2. `hydra` – Brute-force attack on `/rest/user/login`
3. `sqlmap` – SQL Injection against `/rest/products/search`
4. `curl` – Manual SQLi via HTTP requests
5. `feroxbuster` – Directory brute-forcing

---

## 🔍 Endpoint Analysis

| Endpoint                | Vulnerability      | Tool Used        |
| ----------------------- | ------------------ | ---------------- |
| `/rest/user/login`      | Brute-force login  | `hydra`          |
| `/rest/products/search` | SQL Injection      | `sqlmap`, `curl` |
| `/ftp`                  | Exposed file store | `feroxbuster`    |

- The **`q` parameter** in `/rest/products/search` was exploited using SQL Injection.
- The `/ftp` directory contained backup files:
  - `coupons_2013.md.bak`
  - `www-data.bak`

---

## 📥 Sensitive Data Accessed

### 🎯 Data extracted via SQL Injection:

- `email`
- `password`  
  (from the `Users` table using a UNION-based SQLi)

---
