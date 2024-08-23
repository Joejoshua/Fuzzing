# TryHackMe: Content Discovery Room
### Task 12: Automated Discovery
- IP target: `http://10.10.156.17/`

### Check connecting to the web server
```bash
ping -c 3 10.10.156.17
```

---

### Network Enumeration
```bash
nmap -sC -sV -oN nmap.txt 10.10.156.17
# Ports open: 22(ssh) and 9999(http)
```
- More details about nmap.txt
```
# Nmap 7.80 scan initiated Wed Aug 21 12:19:53 2024 as: nmap -sC -sV -oN nmap.txt 10.10.156.17
Nmap scan report for 10.10.156.17
Host is up (0.29s latency).
Not shown: 997 closed ports
PORT     STATE    SERVICE VERSION
22/tcp   open     ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
2602/tcp filtered ripd
9999/tcp open     http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Aug 21 12:23:47 2024 -- 1 IP address (1 host up) scanned in 234.60 seconds
```

---

### Download Seclists 
```bash
git clone https://github.com/danielmiessler/SecLists.git
```
- Seclist: Is a wordlist, So if you want to know about wordlist you can view this page [lacture_1](lacture_1.md)

---

### Use ffuf with seclist
```bash
ffuf -w /opt/SecLists/Discovery/Web-Content/common.txt -u http://10.10.156.17/FUZZ -recursion
```

- Use curl command.
```bash
curl http://10.10.156.17/monthly
You discovered the directory

curl http://10.10.156.17/development.log
You discovered the log file
```

---

### Answer the questions
- What is the name of the directory beginning "/mo...." that was discovered?
- Answer: /monthly

- What is the name of the log file that was discovered?
- Answer: /development.log

---