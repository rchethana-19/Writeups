# TryHackMe – Windows Threat Detection 3

This room focused on investigating a Windows compromise using Event Viewer, Sysmon logs, Security logs, and built-in Windows tools. The investigation covered malware execution, persistence, privilege escalation, account creation, and ransomware-related attack techniques.

---

# Investigation

## 1. Which suspicious archive did the user download?

### How I solved it

I searched **Sysmon Event ID 11 (File Create)** to identify recently downloaded files. One ZIP archive stood out because it was created immediately before the malicious activity began.

**Answer:**

```
URGENT!.zip
```

---

## 2. What is the domain of the Command and Control (C2) server?

### How I solved it

I filtered **Sysmon Event ID 22 (DNS Query)** to review outbound DNS requests made by the malware. One domain was repeatedly contacted after execution.

**Answer:**

```
route.m365officesync.workers.dev
```

---

## 3. Where did the attackers hide the C2 malware?

### How I solved it

Following the malware timeline, I inspected the process creation events and looked at the **Image Path** of the suspicious executable.

**Answer:**

```
C:\Users\Administrator\AppData\Roaming\update.exe
```

---

## 4. How many failed logins were made against the Administrator account?

### How I solved it

I filtered the Security logs for **Event ID 4625 (Failed Logon)** and counted the failed authentication attempts targeting the Administrator account.

**Answer:**

```
6
```

---

## 5. Which backdoor user did the attacker create?

### How I solved it

I searched for **Event ID 4720**, which records newly created user accounts. This revealed the account created after the attacker successfully logged in.

**Answer:**

```
support
```

---

## 6. Which privileged group was the backdoor user added to?

### How I solved it

After identifying the newly created account, I checked the subsequent account management events to determine which group it was added to.

**Answer:**

```
Administrators
```

---

## 7. Which Windows service was created for persistence?

### How I solved it

I reviewed the installed services created during the attack timeline. One service was clearly added to ensure the malware survived reboots.

**Answer:**

```
Data protection service
```

---

## 8. Which scheduled task was created by the Troy malware?

### How I solved it

I opened **Task Scheduler** and reviewed recently created scheduled tasks. One suspicious task matched the malware activity.

**Answer:**

```
AmazonSync
```

---

## 9. What flag do you get after running the "Kitten" malware?

### How I solved it

I located **kitten.exe** by reviewing **Sysmon Event ID 13 (Registry Value Set)**. The registry activity referenced a key named **basket**, which helped identify the malware. After executing it, the flag was displayed.

**Flag:**

```
THM{persisting_in_basket!}
```

---

## 10. What is the biggest threat to most corporate Windows networks?

### Answer

```
Ransomware
```

---

## 11. At which stage is it best to detect and stop the attack?

### Answer

```
Initial Access
```

Stopping an attacker during the **Initial Access** stage prevents later stages such as privilege escalation, persistence, credential theft, lateral movement, and ransomware deployment.

---

# What I Learned

This room helped me understand how attackers establish persistence and maintain access after compromising a Windows system.

Some of my key takeaways were:

- Using **Sysmon Event ID 11** to investigate downloaded files.
- Identifying malware communication through **DNS Query events (Event ID 22)**.
- Tracking hidden malware locations using process creation logs.
- Detecting brute-force attempts with **Security Event ID 4625**.
- Monitoring account creation using **Event ID 4720**.
- Investigating persistence through Windows Services and Scheduled Tasks.
- Understanding why detecting attacks during **Initial Access** is critical to preventing larger compromises like ransomware.
