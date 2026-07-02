# Wireshark: Packet Operations - TryHackMe

# Task 2 - Statistics

### Q2.1 Investigate the resolved addresses. What is the IP address of the hostname starting with "bbc"?

**How I Solved It**
- Opened `Exercise.pcapng`.
- Navigated to **Statistics → Resolved Addresses**.
- Searched for `bbc` in the hostname list.

**Answer**
```
199.232.24.81
```

---

### Q2.2 What is the number of IPv4 conversations?

**How I Solved It**
- Opened **Statistics → Conversations**.
- Selected the **IPv4** tab.
- Read the total conversation count.

**Answer**
```
435
```

---

### Q2.3 How many bytes (k) were transferred from the "Micro-St" MAC address?

**How I Solved It**
- Opened **Statistics → Endpoints**.
- Enabled **Name Resolution**.
- Sorted by Address and located **Micro-St**.
- Read the **Bytes (k)** value.

**Answer**
```
7474
```

---

### Q2.4 What is the number of IP addresses linked with "Kansas City"?

**How I Solved It**
- Stayed in **Statistics → Endpoints**.
- Switched to the **IPv4** tab.
- Sorted by **City**.
- Counted the entries for **Kansas City**.

**Answer**
```
4
```

---

### Q2.5 Which IP address is linked with "Blicnet" AS Organisation?

**How I Solved It**
- In **Statistics → Endpoints**, sorted by **Organization**.
- Located **Blicnet d.o.o**.
- Read the associated IP address.

**Answer**
```
188.246.82.7
```

---

# Task 3 - Protocol Statistics

### Q3.1 What is the most used IPv4 destination address?

**How I Solved It**
- Opened **Statistics → Endpoints**.
- Switched to the **IPv4** tab.
- Sorted by **Rx Packets** (or **Rx Bytes**).
- Identified the destination with the highest traffic.

**Answer**
```
10.100.1.33
```

---

### Q3.2 What is the maximum DNS request-response time?

**How I Solved It**
- Opened **Statistics → DNS**.
- Viewed the **Service Stats** section.
- Read the maximum request-response time.

**Answer**
```
0.467897
```

---

### Q3.3 How many HTTP requests were made to `rad.msn.com`?

**How I Solved It**
- Opened **Statistics → HTTP → Load Distribution**.
- Located **rad.msn.com**.
- Read the request count.

**Answer**
```
39
```

---

# Task 5 - Protocol Filters

### Q5.1 What is the number of IP packets?

**How I Solved It**
- Applied the display filter:

```text
ip
```

- Checked the displayed packet count.

**Answer**
```
81420
```

---

### Q5.2 How many packets have a TTL value less than 10?

**How I Solved It**
- Applied the filter:

```text
ip.ttl < 10
```

- Read the packet count from the status bar.

**Answer**
```
66
```

---

### Q5.3 How many packets use TCP port 4444?

**How I Solved It**
- Applied the filter:

```text
tcp.port == 4444
```

- Checked the displayed packet count.

**Answer**
```
632
```

---

### Q5.4 How many HTTP GET requests were sent to port 80?

**How I Solved It**
- Applied the filter:

```text
http.request.method == "GET" && tcp.port == 80
```

- Read the displayed packet count.

**Answer**
```
527
```

---

### Q5.5 How many Type A DNS queries are there?

**How I Solved It**
- Opened **Analyze → Display Filter Expression**.
- Expanded the **DNS** protocol fields.
- Selected the filter for **Type A DNS Queries**.
- Applied the filter and checked the packet count.

**Answer**
```
51
```

---

# Task 6 - Advanced Filtering

### Q6.1 Find all Microsoft IIS servers. How many packets did not originate from port 80?

**How I Solved It**
- Applied the filter:

```text
http.server contains "IIS" && tcp.srcport != 80
```

- Read the displayed packet count.

**Answer**
```
21
```

---

### Q6.2 Find all Microsoft IIS servers running version 7.5.

**How I Solved It**
- Applied the filter:

```text
http.server contains "IIS" && http.server contains "7.5"
```

- Checked the displayed packet count.

**Answer**
```
71
```

---

### Q6.3 What is the total number of packets using ports 3333, 4444, or 9999?

**How I Solved It**
- Applied the filter:

```text
tcp.port == 3333 || tcp.port == 4444 || tcp.port == 9999
```

- Read the displayed packet count.

**Answer**
```
2235
```

---

### Q6.4 How many packets have even TTL values?

**How I Solved It**
- Applied the filter:

```text
string(ip.ttl) matches "[02468]$"
```

- Checked the displayed packet count.

**Answer**
```
77289
```

---

### Q6.5 Change the profile to "Checksum Control". How many Bad TCP Checksum packets are there?

**How I Solved It**
- Switched to the **Checksum Control** profile.
- Opened **Analyze → Expert Information**.
- Read the **Bad TCP Checksum** count.

**Answer**
```
34185
```

---

### Q6.6 Use the existing filtering button. How many packets are displayed?

**How I Solved It**
- Clicked the predefined filter button available in the toolbar.
- Checked the displayed packet count.

**Answer**
```
261
```
