# Configuring Ubuntu Server (Static IP, SSH, Updates)


## 3. Step-by-Step: Configure Static IP Address

> 💡 **Purpose**: Assigning a static IP to `ens33` ensures your OpenStack VM is always reachable at `192.168.66.3` for SSH, API access, and Kolla-Ansible deployment.

---

### Step 1: Check Current IP Address

```bash
user@openstack:~$ ip address
```
🔍 **Look for**:
- `ens33`: Should show a DHCP-assigned IP (e.g., `192.168.66.128/24`)
- `ens37`: Should exist but have **no IP address** (raw interface for Neutron)

---

### Step 2: Edit Netplan Configuration File

> 📝 **Netplan** is Ubuntu's network configuration tool. The file `50-cloud-init.yaml` controls network interfaces.

```bash
user@openstack:~$ sudo nano /etc/netplan/50-cloud-init.yaml
```

🗑️ **Replace ALL content** with the following configuration:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    ens33:
      addresses:
        - 192.168.66.3/24
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
      routes:
        - to: default
          via: 192.168.66.2
    ens37:
      dhcp4: false
```

💾 **Save the file**:
- Press `Ctrl+O` → `Enter` (write out)
- Press `Ctrl+X` (exit nano)

> ⚠️ **YAML is indentation-sensitive**: Use **spaces only** (no tabs), and maintain consistent 2-space indentation.

---

### Step 3: Disable Cloud-Init Network Management

> 🔄 **Why?** `cloud-init` resets network configuration on every reboot. We must disable this to preserve our static IP.

```bash
user@openstack:~$ sudo nano /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
```

➕ **Add the following single line**:
```yaml
network: {config: disabled}
```

💾 **Save and exit**: `Ctrl+O` → `Enter` → `Ctrl+X`

---

### Step 4: Apply Network Configuration

```bash
user@openstack:~$ sudo netplan apply
```
✅ **No output = success**. Errors will be shown if YAML syntax is wrong.

> 🔧 **Troubleshooting**: If you get an error, run `sudo netplan --debug apply` to see detailed diagnostics.

---

### Step 5: Verify Static IP and Internet Connectivity

```bash
# Verify ens33 has the static IP
user@openstack:~$ ip address show ens33
# ✅ Expected: inet 192.168.66.3/24

# Verify internet connectivity via NAT gateway
user@openstack:~$ ping -c 4 google.com
# ✅ Expected: 4 packets transmitted, 4 received, 0% packet loss

# Verify reachability of VMware NAT gateway
user@openstack:~$ ping -c 2 192.168.66.2
# ✅ Expected: Replies from 192.168.66.2
```

---

### ✅ Verification Checklist

| Check | Command | Expected Result |
|-------|---------|----------------|
| Static IP on ens33 | `ip addr show ens33` | `inet 192.168.66.3/24` |
| No IP on ens37 | `ip addr show ens37` | No `inet` line |
| Default route | `ip route \| grep default` | `default via 192.168.66.2 dev ens33` |
| DNS resolution | `ping -c 2 google.com` | Successful replies |
| Gateway reachability | `ping -c 2 192.168.66.2` | Successful replies |

---

## 4. Step-by-Step: SSH Access via Solar-PuTTY

> 💡 **Purpose**: Connect to your Ubuntu VM from Windows using a persistent, tabbed SSH client.

---

### Setup Instructions

70. Download **Solar-PuTTY** from: [https://www.solarwinds.com/free-tools/solar-putty](https://www.solarwinds.com/free-tools/solar-putty)
71. Extract the downloaded ZIP file to a folder (e.g., `C:\Tools\Solar-PuTTY`)
72. Run `Solar-PuTTY.exe` *(no installation required — portable app)*
73. Click **Create new session**
74. Fill in session details:
    | Field | Value |
    |-------|-------|
    | Session name | `openstack` |
    | IP or hostname | `192.168.66.3` |
    | Port | `22` |
    | Type | `SSHv2` |
75. Under **CREDENTIALS**:
    - Username: `user`
    - Password: *(enter your VM password)*
76. Click **Create**
77. Click on the `openstack` session tile to connect
78. Accept the security alert *(SSH host key fingerprint — click **Accept**)*
79. ✅ You are now logged in via SSH!

---

### 🔐 SSH Connection Summary

```
┌─────────────────────────────────┐
│ Windows Host (Solar-PuTTY)      │
│  • Session: openstack           │
│  • Target: 192.168.66.3:22      │
│  • Auth: Password (user)        │
└────────────┬────────────────────┘
             │ SSH (encrypted)
             ▼
┌─────────────────────────────────┐
│ Ubuntu VM ('openstack')         │
│  • User: user                   │
│  • Shell: /bin/bash             │
│  • Network: ens33=192.168.66.3  │
└─────────────────────────────────┘
```

> 💡 **Pro Tip**: Save credentials securely in Solar-PuTTY's credential manager, or switch to SSH key authentication for passwordless, more secure access.

---

## 5. Step-by-Step: Update System Packages

> 🔄 **Best Practice**: Always update packages before installing new software to ensure security patches and dependency compatibility.

```bash
# Update package index and upgrade installed packages
user@openstack:~$ sudo apt update && sudo apt upgrade -y

# Remove unused/orphaned packages to free space
user@openstack:~$ sudo apt autoremove -y

# Verify no packages are held back or pending upgrade
user@openstack:~$ sudo apt list --upgradable 2>/dev/null
# ✅ Expected: No output (or only non-critical packages)
```

---

### 📋 Post-Update Verification

```bash
# Check system uptime and kernel version
user@openstack:~$ uname -r
user@openstack:~$ uptime

# Verify network is still functional after updates
user@openstack:~$ ping -c 2 192.168.66.2
user@openstack:~$ curl -I https://ubuntu.com | head -n 1
# ✅ Expected: HTTP/2 200
```

---

### 🚀 What's Next?

| Section | Purpose |
|---------|---------|
| 🔹 Section 6 | Install Floodlight & OpenDaylight SDN controllers |
| 🔹 Section 7 | Configure Mininet for topology testing |
| 🔹 Section 8 | Deploy OpenStack with Kolla-Ansible |

> ⏭️ **Proceed to [Section 6: SDN Controller Installation](#6-installing-floodlight-sdn-controller)** to set up your OpenFlow controllers.
