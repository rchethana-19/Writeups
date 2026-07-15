# TryHackMe – Windows Event Log Analysis

This room focused on investigating different attack scenarios using Windows Event Logs. The tasks covered RDP brute-force attacks, phishing, malicious downloads, and malware propagation through USB devices. I mainly relied on Event IDs and Windows Event Viewer to trace the attacker's actions.

---

# A. Risks of Exposed RDP

## 1. Which user seems to be most actively brute-forced by botnets?

### How I solved it

I filtered the Security logs for **Event ID 4625**, which records failed logon attempts. Since RDP brute-force attacks generate numerous failed logins, it was easy to identify the account receiving the highest number of attempts.

**Answer:** `Administrator`

---

## 2. Which IP managed to breach the host via RDP (Logon Type 10)?

### How I solved it

After identifying the failed attempts, I searched for **Event ID 4624**, which records successful logins. I specifically looked for **Logon Type 10**, indicating a successful Remote Desktop login.

**Answer:** `203.205.34.107`

---

## 3. Can you find the real workstation name of the attacker?

### How I solved it

Continuing from the successful login event, I checked the network information associated with the login. Although the victim's hostname appeared as `WIN-F89VT9IER10`, the workstation name recorded for the incoming connection revealed the attacker's machine.

**Answer:** `DESKTOP-QNBC4UU`

---

# B. Current State of Phishing

## 1. Run the www.skype.com file. What flag do you get?

### How I solved it

I executed the file inside the virtual environment and observed its behavior. The flag was displayed after execution.

**Answer:**

```
THM{misleading_extension}
```

---

## 2. From which URL does the malicious LNK download the next-stage malware?

### How I solved it

I inspected the shortcut (**.lnk**) file properties to see what command it executed. The download URL was embedded within the command.

**Answer:**

```
http://wp16.hqywlqpa.thm:8000/cgi-bin/f
```

---

## 3. What is the name of the double-extension file?

### How I solved it

After following the download path, I located the downloaded executable disguised as an image using a double extension.

**Answer:**

```
Best-cat.jpg.exe
```

---

# C. Detecting Malicious Download

## 1. Which file did the user download?

### How I solved it

I searched **Event ID 11**, which records file creation events, and found the browser downloading a ZIP archive.

**Answer:**

```
C:\Users\Administrator\Downloads\top-cats.zip
```

---

## 2. In which folder was the archive extracted?

### How I solved it

I reviewed subsequent file activity and registry-related events to determine where the extracted files appeared.

**Answer:**

```
C:\Users\Administrator\Pictures
```

---

## 3. What is the Process ID of the phishing malware?

### How I solved it

I searched **Event ID 1 (Process Create)** to identify when the extracted malware was executed.

**Answer:**

```
5484
```

---

## 4. Which malicious domain did the malware contact?

### How I solved it

I filtered **Event ID 22**, which logs DNS queries, to identify external connections made by the malware.

**Answer:**

```
rjj.stores
```

---

# D. Initial Access via USB

## 1. Which USB file was launched?

### How I solved it

Using **Event ID 1 (Process Create)**, I identified the executable launched directly from the connected USB device.

**Answer:**

```
E:\Open Sandisk 4GB USB.exe
```

---

## 2. Which suspicious file did the malware drop?

### How I solved it

I examined registry and file creation events generated immediately after execution and located the dropped malware.

**Answer:**

```
C:\Users\Public\Documents\winupdate.exe
```

---

## 3. To which USB device did the malware propagate?

### How I solved it

I continued following the malware activity and observed another executable being created on a second removable drive, indicating USB propagation.

**Answer:**

```
F:\Open Data Traveler 32 GB USB.exe
```

---

# What I Learned

This room gave me practical experience investigating Windows Event Logs and understanding how attackers leave traces during different stages of an attack.

Some key takeaways:

- Using **Event ID 4625** to identify RDP brute-force attempts.
- Using **Event ID 4624** to identify successful logins.
- Tracking attacker workstation names through logon events.
- Investigating phishing attacks by analyzing shortcut (.lnk) files.
- Detecting malware execution through **Process Create (Event ID 1)**.
- Using **DNS Query (Event ID 22)** to identify command-and-control communication.
- Following malware behavior from initial access to persistence and USB propagation.

This room reinforced how valuable Windows Event Logs are for incident response and threat hunting.
