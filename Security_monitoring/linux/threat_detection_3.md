# TryHackMe – Linux Threat Detection 3

This room focused on investigating a compromised Linux system using **auditd**, authentication logs, and system persistence mechanisms. The investigation covered privilege escalation, reverse shells, services, cron jobs, user creation, and SSH persistence.

---

# Investigation

## 1. Which command was used to search for the keyword "pass" in files?

### How I solved it

I searched the audit logs for commands containing the keyword **pass**.

```bash
ausearch -i -if /home/ubuntu/scenario/audit.log | grep "pass"
```

The process title revealed the exact command executed by the attacker.

**Answer:**

```bash
grep -iR pass
```

---

## 2. Which command was used to escalate privileges to root?

### How I solved it

I searched the audit logs for references to **root**.

```bash
ausearch -i -if /home/ubuntu/scenario/audit.log | grep "root"
```

The logs showed the attacker switching to the root account.

**Answer:**

```bash
su root
```

---

## 3. What flag is returned after spawning a reverse shell?

### How I solved it

The TryPingMe web application wasn't accessible in my environment, so instead I inspected the application's source code.

I navigated to:

```bash
/opt/trypingme/main.py
```

Reading the source revealed the expected response.

**Flag:**

```
THM{revshells_practitioner!}
```

---

## 4. What flag is obtained from the malware persisting as a service?

### How I solved it

I searched for modifications to system service files.

```bash
ausearch -i -f /etc/system
```

The logs showed that a service file had been edited using **nano**.

I inspected the service:

```bash
cat /etc/system/system/tux.service
```

The service executed a suspicious binary, so I searched it for the flag.

```bash
grep -a "THM" /var/lib/misc/tux
```

**Flag:**

```
THM{hidden_penguin!}
```

---

## 5. What flag is hidden in the cron persistence?

### How I solved it

I first searched for cron-related activity.

```bash
ausearch -i -x crontab
```

This pointed me to:

```bash
/var/spool/cron
```

After reviewing the root crontab, I noticed an unusual reboot entry:

```text
@reboot /usr/sbin/phoenix
```

Since the executable looked suspicious, I searched it for the flag.

```bash
grep -a "THM" /usr/sbin/phoenix
```

**Flag:**

```
THM{ressurect_on_reboot!}
```

---

## 6. Which user was created and added to the sudo group?

### How I solved it

I searched the authentication logs for account creation and group modification events.

```bash
cat /var/log/auth.log | grep -E "useradd|usermod"
```

The logs showed a newly created user being added to the sudo group.

**Answer:**

```
koichi
```

---

## 7. Which file was modified to maintain SSH persistence?

### How I solved it

The hint suggested monitoring changes to SSH authorized keys, so I searched the audit logs for modifications to that file.

```bash
ausearch -i -f ~/.ssh/authorized_keys
```

The attacker modified the SSH authorized keys file to maintain persistent access.

**Answer:**

```
~/.ssh/authorized_keys
```

---

## Achievement

🏅 Successfully completed all three **Linux Threat Detection** rooms on TryHackMe and earned the completion badge.

---

# What I Learned

This room helped me understand how attackers maintain persistence on Linux systems after gaining initial access.

Some of my key takeaways were:

- Using **auditd** to reconstruct attacker activity.
- Detecting privilege escalation through audit logs.
- Investigating reverse shell attempts.
- Identifying persistence using **systemd services**.
- Detecting malicious cron jobs.
- Tracking unauthorized user creation and privilege assignment.
- Monitoring changes to **authorized_keys** for SSH persistence.
- Following an attacker's actions from initial compromise to long-term persistence.
