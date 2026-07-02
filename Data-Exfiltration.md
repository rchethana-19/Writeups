# Data Exfiltration - TryHackMe

## Data Exfiltration Overview

Data exfiltration is the unauthorized transfer of sensitive data from an organization to an external destination controlled by an attacker. It can occur through insider threats or malware.

### Exfiltration Phases

- Discovery / Collection
- Staging / Compression
- Exfiltration Transport
- Command & Control (C2)

---

# DNS Tunneling

## Indicators

- Numerous DNS queries to a single external domain
- Long DNS query names (>70 characters)
- Base64-like strings (`A-Z`, `a-z`, `0-9`, `-`, `=`)
- Frequent TXT records
- Regular query intervals

### How I Solved It

1. Applied the DNS filter:

```text
dns
```

2. Displayed only DNS queries:

```text
dns.flags.response == 0
```

3. Filtered unusually large packets:

```text
dns && frame.len > 70
```

4. Identified the suspicious domain and filtered it:

```text
dns && dns.qry.name contains "tunnelcorp.net"
```

5. Observed multiple internal hosts sending encoded chunks to the same external domain.

### Answers

| Question | Answer |
|----------|--------|
| Suspicious domain | `tunnelcorp.net` |
| Suspicious DNS requests | **315** |
| Internal host with most requests | **192.168.1.103 (72 requests)** |

---

# FTP Exfiltration

## Indicators

- Cleartext credentials
- `USER` and `PASS` commands
- `STOR` (Upload)
- `RETR` (Download)

### How I Solved It

Checked FTP login attempts:

```text
ftp.request.command == "USER" || ftp.request.command == "PASS"
```

Observed multiple login attempts using the **TEST** username.

Looked for uploaded files:

```text
ftp contains "STOR"
```

Filtered CSV transfers:

```text
ftp contains "csv"
```

Followed the FTP stream to recover the hidden flag.

### Answers

| Question | Answer |
|----------|--------|
| Customer file exfiltrated | `customer_data.xlsx` |
| Guest account connections | **5** |
| Hidden FTP flag | `THM{ftp_exfil_hidden_flag}` |
| Internal IP with largest payload | **192.168.1.105** |

---

# HTTP Exfiltration

### How I Solved It

Filtered large HTTP POST requests:

```text
http.request.method == "POST" && frame.len > 750
```

Identified the compromised internal host.

Recovered the truncated payload using **CyberChef**.

### Answers

| Question | Answer |
|----------|--------|
| Compromised internal host | **192.168.1.103** |
| Hidden flag | `THM{http_raw_3xf1ltr4t10n_succ3ss}` |

---

# ICMP Exfiltration

### How I Solved It

Filtered ICMP Echo Requests:

```text
icmp.type == 8
```

Filtered unusually large ICMP packets:

```text
icmp.type == 8 && frame.len > 100
```

Recovered the truncated payload using **CyberChef**.

### Answer

```text
THM{1cmp_3ch0_3xf1ltr4t10n_succ3ss}
```
