# Kolla-Ansible OpenStack Deployment


##  Step-by-Step Kolla-Ansible Installation

> 💡 **Purpose**: Kolla-Ansible automates OpenStack deployment using Docker containers. This guide installs and configures Kolla-Ansible for an **all-in-one** OpenStack deployment on your Ubuntu VM.

> ⚠️ **Prerequisites**:
> - ✅ Ubuntu Server 24.04.1 LTS with static IP `192.168.66.3`
> - ✅ Docker Engine installed and running
> - ✅ Dual-NIC configuration: `ens33` (management), `ens37` (external/provider)
> - ✅ At least 10 GB RAM, 50 GB disk, 2 CPU cores

---

### Step 1: Switch to Root User

```bash
sudo su
```
🔁 **Prompt changes**:
```
user@openstack:~$  →  root@openstack:/home/user#
```

---

### Step 2: Install System Dependencies

```bash
# Update package index
apt update

# Install core build dependencies
apt install -y git python3-dev libffi-dev gcc libssl-dev python3-docker

# Install DBus dependencies for Ansible modules
apt install -y libdbus-1-dev libdbus-glib-1-dev

# Install Python venv module
apt install -y python3-venv
```

---

### Step 3: Create Working Directory and Python Virtual Environment

```bash
# Create dedicated OpenStack working directory
mkdir /home/user/openstack
cd /home/user/openstack

# Create and activate Python virtual environment
python3 -m venv .
source bin/activate
```
✅ **Prompt should now show**:
```
(openstack) root@openstack:/home/user/openstack#
```

> 💡 **Why a virtual environment?** Isolates Kolla-Ansible dependencies from system Python to avoid conflicts.

---

### Step 4: Upgrade pip and Install Required Python Packages

```bash
# Upgrade pip to latest version
pip install -U pip

# Install Ansible Core (compatible version range)
pip install 'ansible-core>=2.16,<2.18'

# Install Docker and DBus Python bindings
pip install docker dbus-python
```

📦 **Key Packages Installed**:
| Package | Purpose |
|---------|---------|
| `ansible-core` | Ansible engine for orchestration |
| `docker` | Python library to manage Docker containers |
| `dbus-python` | Required for systemd/service management modules |

---

### Step 5: Install Kolla-Ansible from OpenDev Repository

```bash
# Install Kolla-Ansible from the stable 2024.2 (Dalmatian) branch
pip install git+https://opendev.org/openstack/kolla-ansible@stable/2024.2
```
⏱️ **Expected**: Takes 3–5 minutes depending on internet speed  
✅ **Success indicator**: No error messages; `kolla-ansible` command becomes available

---

### Step 6: Create Configuration Directory and Copy Files

```bash
# Create Kolla configuration directory
mkdir -p /etc/kolla/
chown $USER:$USER /etc/kolla

# Copy example configuration files
cp -r share/kolla-ansible/etc_examples/kolla/* /etc/kolla/

# Copy the all-in-one inventory file to working directory
cp share/kolla-ansible/ansible/inventory/all-in-one .
```

📁 **Resulting Structure**:
```
/home/user/openstack/
├── bin/ activate          # Virtual environment
├── all-in-one            # Ansible inventory for single-node deployment
└── /etc/kolla/
    ├── globals.yml       # Main configuration file (edit this!)
    ├── passwords.yml     # Auto-generated credentials (protect!)
    └── ... other config files
```

---

### Step 7: Install Ansible Galaxy Dependencies

```bash
# Install required Ansible collections from Galaxy
kolla-ansible install-deps
```
✅ **Expected output**:
```
openstack.kolla:1.0.0 was installed successfully
```

---

### Step 8: Generate Passwords

```bash
# Generate secure random passwords for all OpenStack services
kolla-genpwd
```
🔐 **What this does**:
- Populates `/etc/kolla/passwords.yml` with auto-generated credentials
- Includes passwords for: Keystone, Nova, Neutron, Glance, Horizon, etc.

> ⚠️ **WARNING**: This file contains **admin credentials**. Restrict access:
> ```bash
> chmod 600 /etc/kolla/passwords.yml
> ```

---

### Step 9: Configure `globals.yml` ⭐ Most Critical Step

```bash
# Open the main configuration file
nano /etc/kolla/globals.yml
```

## 📝 All Configuration Changes for `globals.yml`

> 💡 **Tip**: In `nano`, press `Ctrl+_` (Control + Underscore) to jump directly to a line number. Enter the line number when prompted.

---

### 🔧 9 Critical Changes (with Line Numbers)

| # | Line | Find (Original) | Change To (New Value) | Purpose |
|---|------|-----------------|----------------------|---------|
| 1 | ~53 | `#kolla_base_distro: "rocky"` | `kolla_base_distro: "ubuntu"` | ✅ Use Ubuntu as base OS for containers |
| 2 | ~63 | `#kolla_internal_vip_address: "10.10.10.254"` | `kolla_internal_vip_address: "192.168.66.4"` | 🎯 Set VIP to unused IP in your subnet |
| 3 | ~133 | `#network_interface: "eth0"` | `network_interface: "ens33"` | 🔗 Management network interface (static IP) |
| 4 | ~162 | `#neutron_external_interface: "eth1"` | `neutron_external_interface: "ens34"` | 🌐 External/provider network (no IP) |
| 5 | ~313 | `#enable_haproxy: true` | `enable_haproxy: "no"` | ⚙️ Disable HAProxy for all-in-one setup |
| 6 | ~314 | `#enable_keepalived: ...` | `enable_keepalived: "{{ enable_haproxy \| bool }}"` | 🔁 Keepalived follows HAProxy setting |
| 7 | ~396 | `enable_neutron_provider_networks: false` | `enable_neutron_provider_networks: "yes"` | 🚀 Enable floating IPs via provider networks |
| 8 | ~417 | `#enable_proxysql: ...` | `enable_proxysql: "no"` | 🗄️ Disable ProxySQL to simplify deployment |
| 9 | ~609 | `#nova_compute_virt_type: "kvm"` | `nova_compute_virt_type: "qemu"` | 🖥️ Use QEMU for nested virtualization in VMware |

> ⚠️ **Line numbers are approximate** — actual lines may vary slightly depending on Kolla-Ansible version. Use `Ctrl+_` and search for the key name if needed.

---

### 🛠️ Quick Edit Commands (Using `nano`)

```bash
# Open the configuration file
nano /etc/kolla/globals.yml

# Jump to line (example: line 53):
# Press Ctrl+_ → type 53 → Enter

# After editing, save and exit:
# Ctrl+O → Enter (write out)
# Ctrl+X (exit)
```

---

💾 **Save and exit**: `Ctrl+O` → `Enter` → `Ctrl+X`

> 🎯 **Why these settings?**
> - `kolla_internal_vip_address`: Used by OpenStack services for internal API communication
> - `neutron_external_interface`: Allows VMs to get floating IPs and external network access
> - `nova_compute_virt_type: qemu`: Enables VM nesting inside VMware (KVM requires hardware passthrough)

---

### Step 10: Bootstrap, Precheck, and Deploy

```bash
# 1️⃣ Bootstrap: Prepare the server (install packages, configure users, etc.)
kolla-ansible bootstrap-servers -i ./all-in-one

# 2️⃣ Prechecks: Validate configuration before deployment (fix errors here!)
kolla-ansible prechecks -i ./all-in-one

# 3️⃣ Deploy: Pull Docker containers and start all OpenStack services (~40 minutes)
kolla-ansible deploy -i ./all-in-one

# 4️⃣ Post-deploy: Generate admin credential files
kolla-ansible post-deploy -i ./all-in-one
```

⏱️ **Deployment Timeline**:
| Stage | Duration | Notes |
|-------|----------|-------|
| `bootstrap-servers` | 2–5 min | Installs system packages, configures users |
| `prechecks` | 1–3 min | Validates config; **fix errors before proceeding** |
| `deploy` | 30–50 min | Pulls ~30 Docker images; starts 50+ services |
| `post-deploy` | <1 min | Generates `clouds.yaml` and `admin-openrc` |

✅ **Success indicator**: Final output shows `PLAY RECAP` with `ok=... failed=0`

---

### Step 11: Install OpenStack CLI and Get Admin Password

```bash
# Install OpenStack CLI client with version constraints
pip install python-openstackclient -c https://releases.openstack.org/constraints/upper/master

# View generated credentials file
cat /etc/kolla/clouds.yaml
```

🔑 **Locate your admin password**:
```yaml
# Inside clouds.yaml, find:
auth:
  auth_url: http://192.168.66.4:5000/v3
  username: admin
  password: "Xy9#mK2$pL5nQ8wR"  # ← Copy this password
  project_name: admin
  user_domain_name: Default
  project_domain_name: Default
```

---

### Step 12: Access the Horizon Dashboard

🌐 **On your Windows host browser**, navigate to:
```
http://192.168.66.4/auth/login/
```

🔐 **Login credentials**:
| Field | Value |
|-------|-------|
| Username | `admin` |
| Password | *(from `cat /etc/kolla/clouds.yaml`)* |
| Domain | `Default` |

✅ **Expected**: OpenStack Horizon dashboard loads with project overview.

---

### ✅ Post-Deployment Verification Checklist

```bash
# Source admin credentials for CLI access
. /etc/kolla/admin-openrc.sh

# List OpenStack services (all should be 'up')
openstack compute service list

# List available images
openstack image list

# List available flavors (VM sizes)
openstack flavor list

# List networks
openstack network list

# Test creating a test VM (optional)
openstack server create --image cirros --flavor m1.tiny --network private test-vm
```

| Check | Command | Expected Result |
|-------|---------|----------------|
| Keystone auth | `openstack token issue` | Returns valid token |
| Nova services | `openstack compute service list` | All services `up` |
| Neutron agents | `openstack network agent list` | Agents `alive` |
| Horizon UI | Browser → `http://192.168.66.4` | Login successful |
| VM launch | `openstack server create ...` | Instance spawns in ~2 min |

---

### 📋 Configuration Summary

| Setting | Value | Purpose |
|---------|-------|---------|
| `kolla_base_distro` | `ubuntu` | Base OS for containers |
| `kolla_internal_vip_address` | `192.168.66.4` | Internal API endpoint (VIP) |
| `network_interface` | `ens33` | Management/API traffic |
| `neutron_external_interface` | `ens37` | External/floating IP traffic |
| `nova_compute_virt_type` | `qemu` | Enables nested virtualization |
| `enable_haproxy` | `no` | Simplified all-in-one setup |
| Inventory | `./all-in-one` | Single-node deployment |

---

### 🛠️ Troubleshooting Tips

```bash
# If deployment fails, check logs:
tail -f /var/log/kolla/*/*.log

# Verify Docker containers are running:
docker ps | grep kolla

# Restart a specific service (e.g., Nova):
kolla-ansible restart --limit nova -i ./all-in-one

# Re-run prechecks after fixing config:
kolla-ansible prechecks -i ./all-in-one --diff

# Reset deployment (last resort):
kolla-ansible destroy -i ./all-in-one --yes-i-really-really-mean-it
```

> 💡 **Pro Tip**: Keep your `globals.yml` and `passwords.yml` backed up:
> ```bash
> cp -r /etc/kolla /home/user/kolla-backup-$(date +%F)
> ```

---



### 📚 Quick Reference: Kolla-Ansible Commands

| Task | Command |
|------|---------|
| Bootstrap servers | `kolla-ansible bootstrap-servers -i ./all-in-one` |
| Run prechecks | `kolla-ansible prechecks -i ./all-in-one` |
| Deploy OpenStack | `kolla-ansible deploy -i ./all-in-one` |
| Post-deploy setup | `kolla-ansible post-deploy -i ./all-in-one` |
| Restart service | `kolla-ansible restart --limit <service> -i ./all-in-one` |
| View logs | `docker logs kolla_<service>_<hostname>` |
| Source admin creds | `. /etc/kolla/admin-openrc.sh` |

## Destroy and Redeploy (If Needed)
# WARNING: This deletes ALL OpenStack containers and data
kolla-ansible destroy --yes-i-really-really-mean-it -i ./all-in-one
 
# After destroy, start from Step 10 (bootstrap) to redeploy

