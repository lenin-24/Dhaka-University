# Installing Ubuntu Server 24.04 LTS on VMware


##  Step-by-Step VM Creation Instructions

> 💡 **Prerequisite**: Ensure you have downloaded the **Ubuntu Server 24.04.1 LTS ISO** from [https://ubuntu.com/download/server](https://ubuntu.com/download/server) before starting.

---

### Phase 1: Create the Virtual Machine

20. Open **VMware Workstation Pro**
21. Click **Create a New Virtual Machine** on the home screen
22. Select **Custom (advanced)** configuration — this gives full control over hardware → Click **Next**
23. **Hardware Compatibility**: Select `Workstation 17.5 or later` → Click **Next**
24. **Guest OS Installation**: 
    - Select ✅ *Installer disc image file (iso)*
    - Click **Browse**
25. Navigate to your downloaded Ubuntu ISO file → Select it → Click **Open**
26. VMware detects `Ubuntu 64-bit Server 24.04.1` → Click **Next**
27. **Virtual Machine Name & Location**:
    | Field | Value |
    |-------|-------|
    | VM Name | `openstack` |
    | Location | Choose a folder with **60+ GB free space** |
    → Click **Next**
28. **Processor Configuration**:
    - Number of processors: `2`
    - Number of cores per processor: `1`
    - **Total cores**: `2`
    → Click **Next**
29. **Memory**: Set to `4096 MB` *(we will increase to 10240 MB after adding the 2nd NIC)* → Click **Next**
30. **Network Type**: Select ✅ *Use network address translation (NAT)* → Click **Next**
31. **I/O Controller**: Keep `LSI Logic (Recommended)` → Click **Next**
32. **Disk Type**: Keep `SCSI (Recommended)` → Click **Next**
33. **Select Disk**: Choose ✅ *Create a new virtual disk* → Click **Next**
34. **Disk Capacity**:
    - Size: `40.0 GB+`
    - Select ✅ *Store virtual disk as a single file*
    → Click **Next**
35. **Disk File**: Keep default `openstack.vmdk` → Click **Next**
36. **Review Summary** → Click **Finish**
    > ⚠️ **Do NOT check** *Power on this virtual machine after creation* yet

---

### Phase 2: Add Second Network Adapter and Adjust RAM

37. In the VMware home screen, with `openstack` VM selected → Click **Edit virtual machine settings**
38. Click **Add...** button at the bottom
39. Select ✅ *Network Adapter* from the hardware type list → Click **Finish**
40. A second network adapter appears → Keep it set to **NAT**
41. Click on **Memory** in the device list → Change to `10240 MB` (10 GB)
42. **Verify Final Settings**:
    | Component | Configuration |
    |-----------|--------------|
    | 💾 Memory | 10240 MB (10 GB) |
    | ⚙️ Processors | 2 cores (2×1) |
    | 💿 Hard Disk | 50 GB (single file) |
    | 🌐 Network Adapter | NAT |
    | 🌐 Network Adapter 2 | NAT |
43. Click **OK** to save

---

### Phase 3: Install Ubuntu Server

44. Click **Power on this virtual machine**
45. **GRUB Menu**: Select `Try or Install Ubuntu Server` → Press **Enter**
46. **Language**: Select `English` → Press **Enter**
47. **Installer Update Prompt**: Select `Continue without updating` *(saves time)*
48. **Keyboard Configuration**: Keep `English (US)` → Navigate to **[Done]** → Press **Enter**
49. **Installation Type**: Select `Ubuntu Server` *(not minimized)* → Navigate to **[Done]** → Press **Enter**
50. **Network Configuration**:
    - Wait for DHCP assignment on `ens33` (you should see `192.168.66.x`)
    - Navigate to **[Done]** → Press **Enter**
51. **Proxy Configuration**: Leave blank → Navigate to **[Done]** → Press **Enter**
52. **Ubuntu Archive Mirror**:
    - Wait for mirror test to complete *(shows `bd.archive.ubuntu.com` for Bangladesh)*
    - Navigate to **[Done]** → Press **Enter**
53. **Guided Storage Configuration**: Keep defaults:
    - ✅ *Use entire disk*
    - ✅ *Set up this disk as an LVM group*
    - Navigate to **[Done]** → Press **Enter**
54. **Storage Configuration Summary**: Review partition layout → Navigate to **[Done]** → Press **Enter**
55. **Confirm Destructive Action**: Select **[Continue]**
56. **Profile Configuration** — Fill in:
    | Field | Example Value |
    |-------|--------------|
    | Your name | `user` |
    | Server name | `openstack` |
    | Username | `user` |
    | Password | *(choose a strong password)* |
    | Confirm password | *(re-enter password)* |
    → Navigate to **[Done]**
57. **Ubuntu Pro Upgrade**: Select `Skip for now` → Navigate to **[Continue]**
58. **SSH Configuration**:
    - ✅ Tick *Install OpenSSH server*
    - Navigate to **[Done]** → Press **Enter**
59. **Featured Server Snaps**: Do **NOT** select anything → Navigate to **[Done]**
60. **Installation Proceeds** *(takes 10–20 minutes depending on internet speed)*
61. When `Installation complete!` appears → Select **[Reboot Now]** → Press **Enter**
62. **After Reboot**: Login prompt appears → Enter:
    ```bash
    openstack login: user
    Password: ********
    ```

---

### ✅ Post-Installation Quick Checks

```bash
# Verify network interfaces (should show ens33 and ens34)
ip a

# Verify SSH is running
sudo systemctl status ssh

# Update package list
sudo apt update && sudo apt upgrade -y

# Install essential tools
sudo apt install -y curl wget git net-tools
```

### 📋 VM Configuration Summary

| Setting | Value |
|---------|-------|
| **VM Name** | `openstack` |
| **OS** | Ubuntu Server 24.04.1 LTS |
| **CPU** | 2 cores |
| **RAM** | 10 GB |
| **Disk** | 50 GB (SCSI, single file) |
| **Network** | 2× NAT adapters (ens33, ens34) |
| **SSH** | Enabled |
| **User** | `user` |

> 🔐 **Security Tip**: After first login, change your password immediately with `passwd` and consider setting up SSH key authentication for passwordless access.

> 🚀 **Next Step**: Proceed with SDN controller installation (Floodlight/OpenDaylight) inside this VM using the guide in Section 10.
