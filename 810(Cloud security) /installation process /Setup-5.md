# Docker Installation on Ubuntu Server

## 3. Step-by-Step Docker Installation

> 💡 **Purpose**: Docker is required for Kolla-Ansible OpenStack deployment. This guide installs Docker Engine from the official repository with proper GPG verification and service configuration.

---

### Step 1: Check if Docker is Already Installed

```bash
sudo apt list --installed 2>/dev/null | grep -i docker
```
✅ **Expected**: No output (if Docker is not yet installed)

> ⚠️ If Docker is already installed, proceed to **Step 2** to remove unofficial packages before reinstalling from the official source.

---

### Step 2: Remove Any Unofficial Docker Packages

```bash
# Remove conflicting packages that may cause dependency issues
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do
  sudo apt-get remove $pkg -y
done
```

---

### Step 3: Install Required Dependencies

```bash
# Update package index
sudo apt-get update

# Install prerequisites for HTTPS repositories
sudo apt-get install -y ca-certificates curl

# Create keyring directory with proper permissions
sudo install -m 0755 -d /etc/apt/keyrings
```

---

### Step 4: Add Docker's Official GPG Key

```bash
# Download Docker's official GPG key
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

# Set readable permissions for the key
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Verify the key was downloaded correctly
cat /etc/apt/keyrings/docker.asc | head -1
# ✅ Expected: -----BEGIN PGP PUBLIC KEY BLOCK-----
```

---

### Step 5: Add Docker APT Repository

```bash
# Add the Docker repository dynamically based on your Ubuntu codename
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

> 🔍 **What this does**:
> - `$(dpkg --print-architecture)`: Detects system architecture (amd64/arm64)
> - `$(. /etc/os-release && echo ...)`: Dynamically fetches Ubuntu codename (e.g., `focal`, `jammy`, `noble`)
> - `signed-by=...`: Ensures packages are verified against Docker's GPG key

---

### Step 6: Install Docker Engine

```bash
# Update package index with new Docker repository
sudo apt-get update

# Install Docker Engine, CLI, containerd, and plugins
sudo apt-get install -y \
  docker-ce \
  docker-ce-cli \
  containerd.io \
  docker-buildx-plugin \
  docker-compose-plugin
```

📦 **Installed Components**:
| Package | Purpose |
|---------|---------|
| `docker-ce` | Docker Engine (Community Edition) |
| `docker-ce-cli` | Command-line interface for Docker |
| `containerd.io` | Container runtime daemon |
| `docker-buildx-plugin` | Extended build capabilities (multi-arch) |
| `docker-compose-plugin` | Native `docker compose` command (v2) |

---

### Step 7: Start and Enable Docker Service

```bash
# Start Docker service immediately
sudo systemctl start docker

# Enable Docker to start on boot
sudo systemctl enable docker

# Verify service status
sudo systemctl status docker
```
✅ **Expected output**:
```
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled)
     Active: active (running) since ...
```

---

### Step 8: Verify Docker Installation

```bash
# Check Docker version
sudo docker --version
# ✅ Expected: Docker version 27.4.1, build b9d17ea

# Run the official hello-world test container
sudo docker run hello-world
```
✅ **Expected output**:
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

---

### ✅ Post-Installation Verification Checklist

| Check | Command | Expected Result |
|-------|---------|----------------|
| Docker CLI works | `docker --version` | Shows version ≥ 27.x |
| Docker daemon running | `systemctl is-active docker` | `active` |
| Docker enabled at boot | `systemctl is-enabled docker` | `enabled` |
| Hello-world test | `docker run hello-world` | Success message |
| User permissions *(optional)* | `docker info` without `sudo` | Works if user added to `docker` group |

---

### 🔐 Optional: Run Docker Without `sudo` (Recommended for Development)

> ⚠️ **Security Note**: Adding a user to the `docker` group grants root-equivalent privileges. Only do this on trusted systems.

```bash
# Add your user to the docker group
sudo usermod -aG docker $USER

# Apply group changes (log out/in or run):
newgrp docker

# Verify: Run docker command without sudo
docker run hello-world
# ✅ Should work without 'sudo' prefix
```

---

### 📋 Docker Installation Summary

| Component | Status |
|-----------|--------|
| ✅ Docker Engine | Installed from official repo |
| ✅ GPG Verification | Enabled via `/etc/apt/keyrings/docker.asc` |
| ✅ APT Repository | Added to `/etc/apt/sources.list.d/docker.list` |
| ✅ Service | Active and enabled at boot |
| ✅ CLI Plugins | `buildx`, `compose` included |
| ✅ Test Container | `hello-world` runs successfully |

---

### 🚀 What's Next?

| Section | Purpose |
|---------|---------|
| 🔹 Section 4 | Configure Docker for Kolla-Ansible (registry, storage driver) |
| 🔹 Section 5 | Install Kolla-Ansible and prepare OpenStack deployment |
| 🔹 Section 6 | Deploy OpenStack services with `kolla-ansible deploy` |

> ⏭️ **Proceed to Docker configuration for Kolla-Ansible**:
> ```bash
> # Configure Docker daemon for OpenStack workloads
> sudo nano /etc/docker/daemon.json
> ```

---

### 🛠️ Troubleshooting Tips

```bash
# If docker command not found after install:
source ~/.bashrc  # or log out/in

# If service fails to start:
sudo journalctl -u docker.service --no-pager -n 50

# If repository addition fails:
cat /etc/apt/sources.list.d/docker.list  # Verify content
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 9DC858229FC7DD38854AE2D88D81803C0EBFCD88  # Legacy fallback (not recommended)
```

> 💡 **Pro Tip**: Keep Docker updated regularly:
> ```bash
> sudo apt-get update && sudo apt-get install -y docker-ce docker-ce-cli containerd.io
> ```
