# Snort - TryHackMe

## Snort Overview

**Snort** is an open-source, rule-based Network Intrusion Detection and Prevention System (NIDS/NIPS).

### Modes

- Sniffer Mode
- Packet Logger Mode
- NIDS Mode
- NIPS Mode

---

# Task 2 - Basic Detection

### Q1. What is the IP address that attempted an SSH connection?

### How I Solved It

Opened the PCAP using Snort:

```bash
sudo snort -q -l /var/log/snort -r Intro_to_IDS.pcap -A alert_fast -c /etc/snort/snort.lua
```

Checked the generated alerts and identified the source IP.

**Answer**

```text
10.11.90.211
```

---

### Q2. What is the SID of the rule that detects SSH?

### How I Solved It

Used the same alert output and located the **SID** associated with the SSH detection rule.

**Answer**

```text
1000002
```

---

### Q3. What other rule message was detected?

### How I Solved It

Reviewed the generated alerts.

**Answer**

```text
PING
```

---

# Task 5 - Packet Logger Mode

### Q1. What source port connected to destination port 53?

### How I Solved It

Started packet logging:

```bash
sudo snort -dev -l .
```

Executed the traffic generator:

```bash
./traffic_generator.sh
```

Navigated to the generated directory (`145.254.160.237`) and inspected the packet logs.

**Answer**

```text
3009
```

---

# Task 6 - Reading Log Files

### Q1. What is the IP ID of the 10th packet?

### How I Solved It

Read the first 10 packets:

```bash
snort -r snort.log.1640048004 -n 10
```

Checked the IP ID of packet 10.

**Answer**

```text
49313
```

---

### Q2. What is the Referer of the 4th packet?

### How I Solved It

Displayed packet contents in ASCII:

```bash
snort -r snort.log.1640048004 -n 10 -X
```

Read the HTTP Referer header.

**Answer**

```text
http://www.ethereal.com/development.html
```

---

### Q3. What is the ACK number of the 8th packet?

### How I Solved It

Used the same command and inspected packet 8.

**Answer**

```text
0x38AFFFF3
```

---

### Q4. How many TCP port 80 packets are there?

### How I Solved It

Applied a packet filter:

```bash
snort -r snort.log.1640048004 "tcp and port 80"
```

Counted the displayed packets.

**Answer**

```text
41
```

---

# Task 7 - Traffic Generator

### Q1. How many HTTP GET methods were detected?

### How I Solved It

Executed the traffic generator and selected **TASK-7 Exercise**.

Reviewed the generated alerts.

**Answer**

```text
2
```

---

# Task 8 - Configuration Files

### Q1. How many alerts are generated using the default configuration?

### How I Solved It

```bash
sudo snort -c /etc/snort/snort.conf -A full -l . -r mx-1.pcap
```

Read the summary output.

**Answer**

```text
170
```

---

### Q2. How many TCP Segments are Queued?

### How I Solved It

Continued reading the summary output.

**Answer**

```text
8
```

---

### Q3. How many HTTP Response Headers were extracted?

### How I Solved It

Used the same output summary.

**Answer**

```text
3
```

---

### Q4. How many alerts are generated using the second configuration?

### How I Solved It

```bash
sudo snort -c /etc/snort/snortv2.conf -A full -l . -r mx-1.pcap
```

Read the summary.

**Answer**

```text
68
```

---

### Q5. How many alerts are generated for `mx-2.pcap`?

### How I Solved It

```bash
sudo snort -c /etc/snort/snort.conf -A full -l . -r mx-2.pcap
```

Reviewed the output summary.

**Answer**

```text
340
```

---

### Q6. How many TCP packets were detected?

### How I Solved It

Continued reading the Snort summary.

**Answer**

```text
82
```

---

# Task 9 - Writing Rules

### Q1. Filter packets with IP ID `35369`. What is the request name?

### How I Solved It

Edited `local.rules` and added:

```snort
alert ip any any <> any any (msg:"test"; id:35369; sid:1000000001; rev:1;)
```

Ran Snort:

```bash
snort -c local.rules -A full -l . -r task9.pcap
```

Read the generated alert.

**Answer**

```text
TIMESTAMP REQUEST
```

---

### Q2. Filter UDP packets with the same source and destination IP.

### How I Solved It

Removed previous alerts:

```bash
sudo rm alert
```

Commented out previous rules in `local.rules`.

Added the new rule:

```snort
alert udp any any <> any any (msg:"Same Source and Destination IP"; sameip; sid:1000001; rev:1;)
```

Executed:

```bash
sudo snort -c /etc/snort/rules/local.rules -A full -l . -r task9.pcap
```

Read the total number of matching packets.

**Answer**

```text
7
```
