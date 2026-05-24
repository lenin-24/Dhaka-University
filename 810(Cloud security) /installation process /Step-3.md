# Configuring Lab Network (VMnet8/NAT)


##  Step-by-Step Network Configuration

> 💡 **Purpose**: Proper NAT network configuration ensures your Ubuntu VM can communicate with the host, access the internet, and connect to SDN controllers running on the host machine.

---

### Step 1: Verify VMnet8 (NAT) Network Settings

63. In **VMware Workstation**, go to:
    ```
    Edit > Virtual Network Editor
    ```
64. If prompted, click **Change Settings** to run as Administrator ✅
65. Select `VMnet8` from the list:
    | Property | Expected Value |
    |----------|---------------|
    | Type | `NAT` |
    | Subnet IP | `192.168.66.0` |
    | Subnet Mask | `255.255.255.0` |
66. Click **NAT Settings...** button
67. Note the **Gateway IP** (should be `192.168.66.2`)
    > ⚠️ **Critical**: This gateway IP will be used as the default route inside your Ubuntu VM. Save it for later configuration.
68. Click **OK** to close NAT Settings
69. Click **OK** to close Virtual Network Editor

---

### 🔍 Verification Checklist

| Check | Expected Result |
|-------|----------------|
| VMnet8 Type | `NAT` |
| Subnet | `192.168.66.0/24` |
| Gateway | `192.168.66.2` |
| DHCP Enabled | ✅ Yes (default) |

---

### 📋 Network Topology Overview

```
┌─────────────────────────────────────┐
│ Host Machine (Windows)              │
│  • VMware VMnet8: 192.168.66.1      │
│  • NAT Gateway:   192.168.66.2      │
│  • Floodlight:    localhost:8080    │
│  • OpenDaylight:  localhost:8181    │
└────────────┬────────────────────────┘
             │ NAT
             ▼
┌─────────────────────────────────────┐
│ Ubuntu VM ('openstack')             │
│  • ens33: 192.168.66.x/24 (DHCP)    │
│  • ens34: 192.168.66.y/24 (DHCP)    │
│  • Gateway: 192.168.66.2            │
│  • Controller: 192.168.66.1:6653    │
└─────────────────────────────────────┘
```

> 🔄 **Next Step**: After verifying VMnet8 settings, proceed to configure static/dual-NIC networking inside the Ubuntu VM (Section 4).

---

### 💡 Pro Tips

- **DHCP Reservation**: For consistent IP assignment, consider reserving IPs in VMware's DHCP settings (`Virtual Network Editor > VMnet8 > DHCP Settings`)
- **Firewall**: Ensure Windows Firewall allows inbound connections on ports `6633` and `6653` (OpenFlow) for controller communication
- **Ping Test**: After VM boot, verify connectivity:
  ```bash
  # From Ubuntu VM:
  ping 192.168.66.1    # Should reach host
  ping 8.8.8.8         # Should reach internet via NAT
  ```



### Step 2: Network Architecture Understanding

> 💡 **Purpose**: Understanding the network layout is critical for OpenStack deployment. This architecture ensures proper isolation between management, provider, and external traffic.

---

#### 🌐 Network Component Overview

| Component | IP Address / Range | Purpose |
|-----------|-------------------|---------|
| **VMnet8 Subnet** | `192.168.66.0/24` | Virtual network for all VMware VMs |
| **NAT Gateway (Host)** | `192.168.66.2` | Default route for VM internet access |
| **DHCP Range** | `192.168.66.128–254` | Automatic IP assignment for dynamic interfaces |
| **OpenStack VM (ens33)** | `192.168.66.3` *(static)* | ✅ Management network interface (API, SSH, Kolla) |
| **OpenStack VM (ens37)** | *No IP assigned* | 🔗 OpenStack Neutron external/provider network (bridge mode) |
| **OpenStack VIP** | `192.168.66.4` | 🎯 Kolla-Ansible internal Virtual IP for HA services |

---

#### 🗺️ Network Topology Diagram

```
┌─────────────────────────────────────────────────────┐
│ Host Machine (Windows)                              │
│  • VMnet8: 192.168.66.1                             │
│  • NAT Gateway: 192.168.66.2 ← Internet via NAT     │
│  • DHCP Pool: 192.168.66.128–254                    │
└────────────┬────────────────────────────────────────┘
             │ VMnet8 (NAT)
             ▼
┌─────────────────────────────────────────────────────┐
│ OpenStack VM ('openstack')                          │
│                                                     │
│  ┌─────────────────┐   ┌─────────────────┐         │
│  │ ens33           │   │ ens37           │         │
│  │ 192.168.66.3/24 │   │ No IP (raw)     │         │
│  │ Management Net  │   │ Provider/Ext Net│         │
│  └────────┬────────┘   └────────┬────────┘         │
│           │                     │                   │
│           ▼                     ▼                   │
│  • SSH/API Access      • Neutron External Bridge    │
│  • Kolla-Ansible       • Floating IPs               │
│  • Internal Services   • Tenant Network Routing     │
│                                                     │
│  Virtual IP (VIP): 192.168.66.4                     │
│  → Used by Kolla for HA API endpoints               │
└─────────────────────────────────────────────────────┘
```

---

#### ⚙️ Key Configuration Notes

| Interface | Configuration Type | Notes |
|-----------|-------------------|-------|
| `ens33` | ✅ Static IP | Set via Netplan: `192.168.66.3/24`, gateway `192.168.66.2` |
| `ens37` | 🔗 Raw/Bridge Mode | **No IP assigned** — used by OVS/Docker for external networking |
| `lo:0` (VIP) | 🎯 Virtual IP | Assigned by Kolla-Ansible: `192.168.66.4/32` on `ens33` |

---

#### 🔍 Verification Commands (Inside Ubuntu VM)

```bash
# Verify ens33 has static IP
ip addr show ens33
# Expected: inet 192.168.66.3/24

# Verify ens37 exists but has NO IP
ip addr show ens37
# Expected: NO "inet" line — only link/ether

# Verify default route points to NAT gateway
ip route | grep default
# Expected: default via 192.168.66.2 dev ens33

# Test internet connectivity
ping -c 3 8.8.8.8
ping -c 3 google.com

# Verify VIP is not yet assigned (pre-Kolla)
ip addr show | grep 192.168.66.4
# Expected: No output (VIP assigned later by Kolla)
```

---

#### ⚠️ Critical Reminders

- ❗ **ens37 must NOT have an IP address** — OpenStack Neutron will bind directly to the interface
- ❗ **Static IP on ens33** must be outside the DHCP range (`192.168.66.128–254`) to avoid conflicts
- ❗ **VIP (192.168.66.4)** must be in the same subnet but unused by any other device
- ❗ **Firewall**: Ensure host allows traffic on OpenStack ports (`35357`, `5000`, `8774`, etc.) if accessing from outside the VM

---

#### 📋 Quick Reference: IP Allocation Plan

| Purpose | IP Address | Assigned To | Notes |
|---------|-----------|-------------|-------|
| Host (VMnet8) | `192.168.66.1` | VMware | Default VMware host interface |
| NAT Gateway | `192.168.66.2` | VMware NAT Service | Default route for VMs |
| OpenStack Mgmt | `192.168.66.3` | Ubuntu VM (ens33) | Static — set via Netplan |
| OpenStack VIP | `192.168.66.4` | Kolla-Ansible | Virtual IP for HA services |
| DHCP Pool Start | `192.168.66.128` | VMware DHCP | Avoid assigning statically below this |
| DHCP Pool End | `192.168.66.254` | VMware DHCP | Upper bound for dynamic assignment |

> 🔄 **Next Step**: Proceed to configure static networking on `ens33` and prepare `ens37` for Neutron bridging (Section 4: Ubuntu Network Configuration).
