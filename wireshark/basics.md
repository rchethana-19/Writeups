# Wireshark: The Basics - TryHackMe

## Task 2

### Q2.1 Read the capture file comments. What is the flag?

**How I Solved It**
- Opened `Exercise.pcapng`.
- Navigated to **Statistics → Capture File Properties**.
- Scrolled to the **Comments** section.

**Answer**
```
TryHackMe_Wireshark_Demo
```

---

### Q2.2 What is the total number of packets?

**How I Solved It**
- Loaded the PCAP file.
- Checked the **Status Bar** at the bottom-right of Wireshark.

**Answer**
```
58620
```

---

### Q2.3 What is the SHA256 hash of the capture file?

**How I Solved It**
- Opened **Statistics → Capture File Properties**.
- Copied the SHA256 value shown under the **File** section.

**Answer**
```
f446de335565fb0b0ee5e5a3266703c778b2f3dfad7efeaeccb2da5641a6d6eb
```

---

# Task 3

### Q3.1 View packet 38. Which markup language is used under the HTTP protocol?

**How I Solved It**
- Pressed **Ctrl + G**.
- Entered packet **38**.
- Expanded the **HTTP** section in Packet Details.

**Answer**
```
eXtensible Markup Language
```

---

### Q3.2 What is the arrival date of the packet?

**How I Solved It**
- Expanded the **Frame** section.
- Located the **Arrival Time** field.

**Answer**
```
05/13/2004
```

---

### Q3.3 What is the TTL value?

**How I Solved It**
- Expanded the **Internet Protocol Version 4** section.
- Located the **Time To Live (TTL)** field.

**Answer**
```
47
```

---

### Q3.4 What is the TCP payload size?

**How I Solved It**
- Expanded the **Transmission Control Protocol** section.
- Read the **TCP payload** value.

**Answer**
```
424
```

---

### Q3.5 What is the ETag value?

**How I Solved It**
- Expanded the **Hypertext Transfer Protocol** section.
- Located the **ETag** header.

**Answer**
```
9a01a-4696-7e354b00
```

---

# Task 4

### Q4.1 Search the string "r4w" in Packet Details. What is the name of artist 1?

**How I Solved It**
- Pressed **Ctrl + F**.
- Selected **String** search.
- Searched for `r4w` in **Packet Details**.

**Answer**
```
r4w8173
```

---

### Q4.2 Go to packet 12 and read the comments.

**How I Solved It**
- Pressed **Ctrl + G**.
- Entered packet **12**.
- Right-clicked the packet and selected **Packet Comment**.
- Followed the instructions provided in the comment.

**Answer**
> *(Insert your THM flag here if you want to avoid spoilers.)*

---

### Q4.3 There is a .txt file inside the capture. What is the alien's name?

**How I Solved It**
- Navigated to **File → Export Objects → HTTP**.
- Exported the `.txt` file.
- Opened the exported file.

**Answer**
```
PACKETMASTER
```

---

### Q4.4 What is the number of warnings?

**How I Solved It**
- Opened **Analyze → Expert Information**.
- Checked the **Warnings** count.

**Answer**
```
1636
```

---

# Task 5

### Q5.1 Apply Hypertext Transfer Protocol as a filter. What is the filter query?

**How I Solved It**
- Went to packet **4**.
- Right-clicked **Hypertext Transfer Protocol**.
- Selected **Apply as Filter → Selected**.

**Answer**
```
http
```

---

### Q5.2 What is the number of displayed packets?

**How I Solved It**
- Applied the HTTP display filter.
- Checked the packet count shown in the status bar.

**Answer**
```
1089
```

---

### Q5.3 Go to packet 33790 and follow the stream. What is the total number of artists?

**How I Solved It**
- Pressed **Ctrl + G**.
- Entered packet **33790**.
- Right-clicked the packet.
- Selected **Follow → HTTP Stream**.
- Searched for `artist`.

**Answer**
```
3
```

---

### Q5.4 What is the name of the second artist?

**How I Solved It**
- Continued in the HTTP stream.
- Searched for `artist=2`.

**Answer**
```
Blad3
```
