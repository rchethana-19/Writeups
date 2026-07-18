# TryHackMe – Linux Threat Detection 1

This room focused on investigating a compromised Linux system using authentication logs, Nginx access logs, and Linux audit logs. The objective was to identify brute-force attacks, SSH compromise, web exploitation, and the attacker's post-exploitation activities.

---

# Investigation

## 1. When did the `ubuntu` user log in via SSH for the first time?

### How I solved it

I searched the authentication logs for successful SSH login events.

```bash
cat /var/log/auth.log | grep "Accepted"
```

The earliest successful login for the **ubuntu** user occurred on:

**Answer:**

```
2024-10-22
```

---

## 2. Did the ubuntu user authenticate using SSH keys?

### How I solved it

From the same authentication logs, I looked at the authentication method. Since the log entry mentioned **Accepted publickey**, it indicated SSH key-based authentication rather than a password.

**Answer:**

```
Yes
```

---

## 3. When did the SSH password brute-force attack begin?

### How I solved it

I searched for failed login attempts.

```bash
cat /var/log/auth.log | grep "Failed"
```

The logs showed a large number of failed authentication attempts within a short period, indicating the start of a brute-force attack.

**Answer:**

```
2025-08-21
```

---

## 4. Which four users did the botnet attempt to compromise?

### How I solved it

To list the usernames targeted during the failed login attempts, I extracted the username field from the authentication logs.

```bash
cat /var/log/auth.log | grep "Failed" | awk '{print $9}' | sort -u
```

This returned the unique usernames targeted by the attacker.

**Answer:**

- root
- roy
- sol
- user

---

## 5. Which IP successfully compromised the root account?

### How I solved it

After reviewing the failed attempts, I looked for the successful login event for the **root** account. Unlike the earlier SSH key authentication, this login succeeded using a password.

**Answer:**

```
91.224.92.79
```

---

## 6. What is the path of the Python file the attacker accessed?

### How I solved it

I examined the Nginx access logs to understand the attacker's activity after gaining access.

```bash
cat /var/log/nginx/access.log
```

The logs showed the attacker executing commands like:

- `whoami`
- `ls`
- `cat`

Eventually, they viewed the application's Python source code.

**Answer:**

```
/opt/trypingme/main.py
```

---

## 7. What flag is inside the Python file?

### How I solved it

After locating the file from the previous task, I opened it to inspect its contents.

**Flag:**

```
THM{i_am_vulnerable!}
```

---

## 8. What is the PPID of the suspicious `whoami` command?

### How I solved it

I used the Linux Audit logs to trace the execution of the `whoami` command.

```bash
ausearch -i -x whoami
```

The audit logs showed:

- PID: 1020
- PPID: 1018

**Answer:**

```
1018
```

---

## 9. What is the PID of the TryPingMe application?

### How I solved it

Since I already knew the parent process ID, I traced one level higher in the process tree.

```bash
ausearch -i --pid 1018
```

This led me to the TryPingMe application process.

**Answer:**

```
577
```

---

## 10. Which program did the attacker use to establish a reverse shell?

### How I solved it

I continued tracing the process tree starting from the TryPingMe application.

```bash
ausearch -i --pid 577
```

The child process responsible for launching the reverse shell was clearly visible.

**Answer:**

```
python3
```

---

# What I Learned

This room gave me hands-on experience investigating Linux systems after compromise using authentication logs, web server logs, and audit logs.

Some of my key takeaways were:

- Investigating SSH authentication using `/var/log/auth.log`.
- Identifying brute-force attacks by analyzing repeated failed logins.
- Distinguishing between password-based and SSH key authentication.
- Following an attacker's actions through Nginx access logs.
- Using Linux Audit (`ausearch`) to reconstruct the process tree.
- Tracing commands back to their parent processes to identify the source of malicious activity.
- Understanding how attackers leverage Python to establish reverse shells after compromising a Linux server.
