# MITM (Man-in-the-Middle) Analysis

## Task 1: Detecting ARP Spoofing

### Q1. How many ARP packets from the gateway MAC address were observed?

**How I solved it**

I first identified the legitimate gateway details from the ARP traffic:

- Gateway IP: `192.168.10.1`
- Gateway MAC: `02:aa:bb:cc:00:01`

Then I filtered only the ARP packets coming from that gateway.

```wireshark
arp && arp.src.proto_ipv4 == 192.168.10.1 && eth.src == 02:aa:bb:cc:00:01
```

I counted the displayed packets.

**Answer:** `10`

---

### Q2. What MAC address was used by the attacker to impersonate the gateway?

**How I solved it**

Since ARP spoofing causes duplicate IP-to-MAC mappings, I searched for packets where Wireshark detected duplicate ARP addresses.

```wireshark
arp.duplicate-address-detected || arp.duplicate-address-frame
```

The duplicate entry showed the attacker's MAC address pretending to be the gateway.

**Answer:** `02:fe:fe:fe:55:55`

---

### Q3. How many Gratuitous ARP replies were observed for 192.168.10.1?

**How I solved it**

I filtered for Gratuitous ARP packets and counted the entries related to the gateway.

```wireshark
arp.isgratuitous
```

**Answer:** `2`

---

### Q4. How many ARP spoofing packets were observed from the attacker?

**How I solved it**

After identifying the attacker's MAC address, I used the ARP poisoning filter provided in the lab and counted all spoofed ARP packets sent by the attacker.

**Answer:** `14`

---

# Task 2: DNS Spoofing

### Q1. How many DNS responses were observed for the domain `corp-login.acme-corp.local`?

**How I solved it**

I filtered only DNS response packets for the target domain.

```wireshark
dns.flags.response == 1 && dns.qry.name == "corp-login.acme-corp.local"
```

I then counted the returned packets.

---

### Q2. How many DNS responses were observed from IPs other than 8.8.8.8?

**How I solved it**

Since the legitimate DNS server was Google's `8.8.8.8`, I excluded that IP and checked for responses coming from any other source.

```wireshark
dns.flags.response == 1 && ip.src != 8.8.8.8
```

This helped identify any suspicious DNS replies.

---

### Q3. What IP did the attacker's forged DNS response return?

**How I solved it**

Most DNS responses came from `8.8.8.8`, but one response originated from a different IP. That forged response redirected the victim to:

**Answer:** `192.168.10.55`

---

# Task 3: SSL Stripping

### Q1. How many POST requests were observed for `corp-login.acme-corp.local`?

**How I solved it**

I filtered the HTTP traffic between the victim (`192.168.10.10`) and the suspected malicious server (`192.168.10.55`).

```wireshark
http && ip.src == 192.168.10.10 && ip.dst == 192.168.10.55
```

There was only one HTTP POST request.

**Answer:** `1`

---

### Q2. What was the victim's password after the SSL stripping attack?

**How I solved it**

Using the same HTTP filter, I followed the POST request and inspected its contents. Since HTTPS had been stripped to HTTP, the credentials were visible in plaintext.

```wireshark
http && ip.src == 192.168.10.10 && ip.dst == 192.168.10.55
```

**Answer:**

```text
Secret123!
```
