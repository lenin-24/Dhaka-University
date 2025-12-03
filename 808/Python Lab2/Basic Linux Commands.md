# Basic Linux Commands

## File Operations

| Command | Description |
|--------|-------------|
| `ls` | Lists all files and directories in the present working directory |
| `ls -R` | Lists files in sub-directories recursively |
| `ls -a` | Shows hidden files (those starting with `.`) |
| `ls -al` | Lists files/directories with detailed info (permissions, size, owner, etc.) |
| `cd directoryname` | Changes to the specified directory |
| `cd ..` | Moves one level up in the directory tree |
| `pwd` | Displays the present working directory |
| `cat > filename` | Creates a new file and allows input |
| `cat filename` | Displays the content of a file |
| `cat file1 file2 > file3` | Concatenates `file1` and `file2`, saves output to `file3` |
| `touch filename` | Creates an empty file or updates the timestamp of an existing file |
| `rm filename` | Deletes a file |
| `cp source destination` | Copies a file from source to destination |
| `mv source destination` | Moves (or renames) a file |
| `find / -name filename` | Searches for a file by name starting from root (`/`) |
| `file filename` | Determines the type of a file |
| `less filename` | Views file content page by page (scrollable) |
| `head filename` | Displays the first 10 lines of a file |
| `tail filename` | Displays the last 10 lines of a file |

---

## Directory Operations and Permissions

| Command | Description |
|--------|-------------|
| `mkdir directoryname` | Creates a new directory |
| `rmdir directoryname` | Removes an **empty** directory |
| `cp -r source destination` | Recursively copies directories and their contents |
| `mv olddir newdir` | Renames or moves a directory |
| `find / -type d -name directoryname` | Finds a directory by name starting from root |
| `chmod octal filename` | Changes file permissions (e.g., `chmod 755 file`) |
| `chown ownername filename` | Changes file owner |
| `chgrp groupname filename` | Changes group owner |

> **Permission Octal Reference**:  
> - `0` = no permissions  
> - `1` = execute  
> - `2` = write  
> - `3` = write + execute  
> - `4` = read  
> - `5` = read + execute  
> - `6` = read + write  
> - `7` = read + write + execute  

---

## Process Operations

| Command | Description |
|--------|-------------|
| `ps` | Displays currently active processes |
| `top` | Shows real-time view of all running processes |
| `kill pid` | Terminates the process with the given PID |

---

## Networking

| Command | Description |
|--------|-------------|
| `ping host` | Tests connectivity to a host |
| `whois domain` | Retrieves WHOIS registration info for a domain |
| `dig domain` | Queries DNS records for a domain |
| `netstat -pnltu` | Shows network connections, listening ports, and related processes |
| `ifconfig` | Displays IP addresses and network interface info *(deprecated on some systems; use `ip a` instead)* |
| `ssh user@host` | Securely logs into a remote host |
| `scp file user@host:/path` | Securely copies files between hosts |
| `wget url` | Downloads files from the web |
| `curl url` | Sends HTTP requests and displays response |
| `traceroute domain` | Traces the network path to a destination |
| `mtr domain` | Combines `ping` + `traceroute` for real-time network diagnostics |

---

## Archives and Compression

| Command | Description |
|--------|-------------|
| `tar cf file.tar files` | Creates a tar archive |
| `tar xf file.tar` | Extracts a tar archive |
| `gzip file` | Compresses file → `file.gz` |
| `gzip -d file.gz` | Decompresses a `.gz` file |
| `zip -r file.zip files` | Creates a ZIP archive (recursively) |
| `unzip file.zip` | Extracts a ZIP archive |
| `tar -cvf archive.tar dirname/` | Create uncompressed tar archive |
| `tar -xvf archive.tar` | Extract tar archive |
| `tar -jcvf archive.tar.bz2 dirname/` | Create bzip2-compressed tar |
| `tar -jxvf archive.tar.bz2` | Extract bzip2-compressed tar |

---

## Text Processing

| Command | Description |
|--------|-------------|
| `grep pattern files` | Searches for a pattern in files |
| `grep -r pattern dir` | Recursively searches for pattern in a directory |
| `command \| grep pattern` | Pipes output of a command to `grep` |
| `echo 'text'` | Prints text to terminal |
| `sed 's/string1/string2/g' filename` | Replaces all occurrences of `string1` with `string2` in file |
| `diff file1 file2` | Shows differences between two files |
| `wc filename` | Counts lines, words, and characters |

---

## Disk Usage

| Command | Description |
|--------|-------------|
| `df` | Shows disk space usage for mounted filesystems |
| `df -h` | Human-readable disk usage (e.g., in GB/MB) |
| `du` | Shows disk usage of directories/files |
| `du -sh` | Summarizes total disk usage of current directory in human-readable format |
| `free` | Shows memory and swap usage |
| `free -m` | Shows memory usage in megabytes |
| `whereis app` | Locates binary, source, and manual pages for an application |

---

## System Info

| Command | Description |
|--------|-------------|
| `date` | Shows current date and time |
| `cal` | Displays this month’s calendar |
| `uptime` | Shows system uptime and load averages |
| `w` | Displays who is logged in and what they’re doing |
| `whoami` | Shows current logged-in user |
| `uname -a` | Displays kernel and system information |

---

## Package Management (APT - Debian/Ubuntu)

| Command | Description |
|--------|-------------|
| `sudo apt-get update` | Updates package index |
| `sudo apt-get upgrade` | Upgrades installed packages |
| `sudo apt-get install pkgname` | Installs a package |
| `sudo apt-get remove pkgname` | Removes a package (keeps config files) |

---

## Search and Find

| Command | Description |
|--------|-------------|
| `locate filename` | Quickly finds files by name (uses a database; run `sudo updatedb` to refresh) |
| `whereis programname` | Locates binary, source, and manual pages |
| `which commandname` | Shows full path of a command executable |

---

# Useful Tools for Cybersecurity

## 🔒 Network Security Tools
- **Nmap**: Network discovery and security auditing
- **Wireshark**: Graphical network protocol analyzer
- **Tcpdump**: Command-line packet capture and analysis
- **NetworkMiner**: Network forensic analysis tool (NFAT)
- **Snort**: Open-source intrusion detection system (IDS)
- **Aircrack-ng**: Suite for assessing Wi-Fi network security
- **Ettercap**: Man-in-the-Middle (MitM) attack and sniffing tool

## 🛡️ Vulnerability Assessment & Penetration Testing
- **Nessus**: Commercial vulnerability scanner
- **OWASP ZAP**: Open-source web app security scanner
- **Burp Suite**: Web vulnerability scanner and proxy
- **Acunetix**: Automated web vulnerability scanner
- **Metasploit**: Penetration testing framework
- **OpenVAS**: Open-source vulnerability scanning platform
- **SQLMap**: Automatic SQL injection and database takeover tool
- **DirBuster**: Directory/file brute-forcing tool
- **Nikto**: Web server scanner for outdated software and misconfigurations

## 🔐 Encryption & Cryptography Tools
- **OpenSSL**: Toolkit for TLS/SSL and general-purpose cryptography
- **VeraCrypt**: Disk encryption software (successor to TrueCrypt)
- **GnuPG (GPG)**: OpenPGP-compliant encryption/signing tool
- **PuTTY**: SSH/Telnet client for Windows
- **AES Crypt**: File encryption using AES algorithm
- **PGP**: Protocol for secure communication and data encryption

## 🔑 Password Recovery & Cracking Tools
- **John the Ripper**: Fast password cracker (supports many hash types)
- **Hashcat**: Advanced, GPU-accelerated password recovery tool
- **Hydra**: Parallelized network logon cracker (supports 50+ protocols)
- **Medusa**: Modular, high-speed brute-forcer
- **Ophcrack**: Windows password cracker using rainbow tables

## 🖥️ Endpoint Monitoring & Troubleshooting
- **Sysinternals Suite**: Advanced Windows system utilities (by Microsoft) for monitoring, troubleshooting, and security analysis

