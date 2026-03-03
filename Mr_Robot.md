



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
## Vulnerability
```bash
A wordpress login page was found after gobuster scan , found the credentials through hydra and fsocity.dic file found in robots.txt.
Username: Elliot
Password: ER28-0652 (Elliot's employee number)

after the login found an editor option which can be exploited for revers shell.
a netcat lister was setup on the attack machine with port 53 to avoid firewall blockage
from pentestmonkey got the php-reverse shell.
```

## Keys
```
1.073403c8a58a1f80d943455fb30724b9
2. 822c73956184f694993bede3eb39f959
3.04787ddef27c3dee1ee161b21670b4e4
```
