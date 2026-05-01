# 🔷 TryHackMe - Summit Room Writeup

> **Focus: Detection Engineering & Defensive Security Analysis**

---

## Overview

This writeup documents my approach to detecting and mitigating simulated malware activity in the Summit room on TryHackMe.

The room follows the Pyramid of Pain model, where detection evolves from simple indicators (hashes) to more advanced behavioral detection.

---

## Objectives

* Detect malware using file hashes
* Block malicious IP communication
* Identify and filter malicious domains (DNS)
* Detect command & control (C2) behavior
* Map activity to MITRE ATT&CK

---

## Stage 1: File Hash Detection

### Observed Artifact

* File: `sample1.exe`
* Hash (example):

```
MD5: <your_hash_here>
SHA256: <your_hash_here>
```

### Analysis

* File hashes uniquely identify malware samples
* However, they are **easy to modify**, making them weak indicators

### Detection

* Added hash to blocklist using endpoint detection system

### Insight

* Hashes are at the **bottom of the Pyramid of Pain**
* Attackers can bypass this by modifying the file slightly ([0xb0b][1])

---

## Stage 2: IP-Based Detection

### Observed Activity

* Process: `sample2.exe`
* Connection:

```
154.35.10.113:4444
```

### Analysis

* Outbound connection to suspicious IP
* Port 4444 commonly used for C2 / reverse shells

### Detection

* Blocked **egress traffic** to malicious IP via firewall

### Insight

* IP blocking is stronger than hashes
* But attackers can rotate IPs easily

---

## Stage 3: DNS-Based Detection

### Observed Artifact

* Domain:

```
emudyn.bresonicz.info
```

### Analysis

* Malware switched from raw IP → domain-based communication
* Indicates evolving attacker behavior

### Detection

* Created DNS filter rule:

  * Domain: `emudyn.bresonicz.info`
  * Action: Deny

### Insight

* DNS filtering increases attacker cost
* Still bypassable via domain changes or DGA

---

## Stage 4: Network (C2 Beaconing) Detection

### Observed Activity

* Process: `beacon.bat`
* Method: `POST`
* URL:

```
https://bababab1ola.cn/keep-alive?hostname=WK102
```

### Analysis

* Repeated POST requests → **beaconing behavior**
* Fixed endpoint (`/keep-alive`) indicates periodic check-ins
* Likely Command & Control (C2) communication

### Detection Rule

* Method: POST
* URL Contains: `bababab1ola.cn/keep-alive`

### MITRE ATT&CK

* Tactic: Command & Control (TA0011)
* Technique: T1071.001 — Web Protocols

---

## Stage 5: Registry (Defense Evasion) Detection

### Observed Modification

```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Defender\Real-Time Protection
```

* Value:

```
DisableRealtimeMonitoring = 1
```

### Impact

* Windows Defender real-time protection disabled
* Allows malware to evade detection

### Detection Rule

* Monitor registry changes to Defender settings
* Alert on disabling security controls

### MITRE ATT&CK

* Tactic: Defense Evasion (TA0005)
* Technique: T1562.001 — Impair Defenses

---

## MITRE ATT&CK Summary

| Behavior                   | Tactic            | Technique |
| -------------------------- | ----------------- | --------- |
| Hash Detection             | N/A               | N/A       |
| Malicious IP Communication | Command & Control | T1071     |
| DNS Filtering              | Command & Control | T1071     |
| HTTP Beaconing             | Command & Control | T1071.001 |
| Defender Disabled          | Defense Evasion   | T1562.001 |

---

## Key Takeaways

* Hash-based detection is weakest and easily bypassed
* IP-based blocking improves detection but lacks resilience
* DNS filtering adds stronger control but still bypassable
* Behavioral detection (C2 + registry tampering) is most effective
* Combining multiple layers provides strong defense

---

## Conclusion

This room demonstrates how defenders must evolve detection strategies alongside attackers.
By moving up the Pyramid of Pain—from hashes to behavior—we significantly increase the attacker’s cost and reduce the likelihood of successful compromise.

---

[1]: https://0xb0b.gitbook.io/writeups/tryhackme/2024/summit?utm_source=chatgpt.com "Summit | Writeups"
