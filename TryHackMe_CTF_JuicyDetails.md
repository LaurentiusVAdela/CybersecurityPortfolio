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

## 📁 File Access Attempts

The attacker discovered the `/ftp` directory and attempted to retrieve:

- `coupons_2013.md.bak`
- `www-data.bak`

**Service used:** `ftp`  
**User:** `anonymous`

✔️ The `vsftpd.log` confirmed successful downloads of these files.

---

## 🔓 Shell Access

The attacker then moved laterally:

- Used **brute-force SSH** login attempts against the `www-data` user.
- Successfully authenticated as:
  ```
  ssh, www-data
  ```

**Confirmed in `auth.log` at:**

```
11/Apr/2021:09:41:19 +0000
```

---

## 🧠 Attacker Movement Timeline

| Time          | Action                                       |
| ------------- | -------------------------------------------- |
| ~09:35        | Anonymous FTP login & file download          |
| ~09:36–09:38  | Brute-force attempts via SSH (`hydra`)       |
| 09:41:19      | SSH access gained as `www-data`              |
| 09:16:31      | Successful web login confirmed (HTTP 200)    |
| Shortly after | SQL injection of user data & password hashes |

---

## 🧠 Analysis and Findings

- The attacker scraped **product review pages** to identify user emails.
- Used brute-force attacks and SQL Injection to access sensitive data.
- Exploited exposed FTP and SSH services with weak credentials and anonymous access.

---

## 🔒 Recommendations

- Disable anonymous FTP access immediately.
- Block brute-force attempts using `fail2ban` or rate-limiting.
- Sanitize input on vulnerable endpoints (`/rest/products/search`).
- Prevent service accounts like `www-data` from SSH access.
- Regularly monitor logs for suspicious access patterns.

---

## 📁 Files Used

- `auth.log`
- `vsftpd.log`
- `access.log`
