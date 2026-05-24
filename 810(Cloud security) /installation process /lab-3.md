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
