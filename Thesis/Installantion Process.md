```markdown
# SDN Project Installation Guide 🛠️

> Complete step-by-step installation commands for all tools needed for your Software-Defined Networking (SDN) project.  
> **Recommended OS:** Ubuntu 20.04/22.04 (via VirtualBox)

---

## 🔹 1. System Update (First Step)
```bash
sudo apt update && sudo apt upgrade -y
```

---

## 🔹 2. Install Basic Dependencies
```bash
sudo apt install -y git curl wget python3 python3-pip net-tools \
build-essential openssh-server
```

---

## 🔹 3. Install Mininet (Network Emulator)
```bash
git clone https://github.com/mininet/mininet
cd mininet
sudo ./util/install.sh -a
```

### ✅ Verify Installation
```bash
sudo mn --test pingall
```

---

## 🔹 4. Install Open vSwitch (if not installed)
```bash
sudo apt install -y openvswitch-switch
```

### ✅ Check Installation
```bash
sudo ovs-vsctl show
```

---

## 🔹 5. Install Ryu Controller (Recommended Main Controller)
```bash
pip3 install ryu
```

### ✅ Test Ryu
```bash
ryu-manager ryu.app.simple_switch
```

---

## 🔹 6. Install POX Controller
```bash
git clone https://github.com/noxrepo/pox
cd pox
```

### ✅ Run POX
```bash
./pox.py forwarding.l2_learning
```

---

## 🔹 7. Install Floodlight Controller
```bash
sudo apt install -y default-jdk
git clone https://github.com/floodlight/floodlight.git
cd floodlight
ant
```

### ✅ Run Floodlight
```bash
java -jar target/floodlight.jar
```

---

## 🔹 8. Install OpenDaylight (Advanced Controller)
```bash
sudo apt install -y unzip
wget https://nexus.opendaylight.org/content/repositories/public/org/opendaylight/integration/distribution-karaf/0.12.3/distribution-karaf-0.12.3.tar.gz

tar -xvzf distribution-karaf-0.12.3.tar.gz
cd distribution-karaf-0.12.3
```

### ✅ Run OpenDaylight
```bash
./bin/karaf
```

### ✅ Install Features (inside Karaf console)
```
feature:install odl-restconf odl-l2switch-switch odl-mdsal-apidocs
```

---

## 🔹 9. Install Wireshark (Packet Analyzer)
```bash
sudo apt install -y wireshark
```

### ✅ Allow Non-Root Capture
```bash
sudo dpkg-reconfigure wireshark-common
sudo usermod -aG wireshark $USER
```
> 💡 **Note:** Restart your system after this step for group changes to take effect.

---

## 🔹 10. Install Traffic Tools (IMPORTANT for Testing)
```bash
sudo apt install -y iperf iperf3 hping3
```

---

## 🔹 11. Install Visualization Tools (Optional for Graphs)
```bash
pip3 install matplotlib pandas
```

---

## 🔹 12. Basic Test Setup (VERY IMPORTANT)

### Step 1: Run Controller (Terminal 1)
```bash
ryu-manager ryu.app.simple_switch
```

### Step 2: Run Mininet (Terminal 2)
```bash
sudo mn --topo single,3 --controller remote
```

### Step 3: Test Network
```bash
pingall
```

---

## 🔹 13. Capture OpenFlow Traffic in Wireshark
1. Open **Wireshark**
2. Select interface (e.g., `lo` or `eth0`)
3. Apply display filter:
   ```
   openflow
   ```

---

## 🔹 14. Common Problems & Fixes

| Issue | Solution |
|-------|----------|
| ❌ Permission error | `sudo usermod -aG sudo $USER` |
| ❌ Mininet cleanup | `sudo mn -c` |
| ❌ Port already in use (6653) | `sudo fuser -k 6653/tcp` |

---

> ⚠️ **Pro Tips:**
> - Always run Mininet commands with `sudo`
> - Use separate terminal tabs for controller, Mininet, and Wireshark
> - Backup your VM before major installations
> - Document your topology and test results for reproducibility

---

*Last Updated: $(date)*  
*Compatible with: Ubuntu 20.04 LTS / 22.04 LTS*
```

---

### 📋 How to Use:
1. Copy the entire code block above
2. Paste into a new file named `README.md` or `INSTALL.md`
3. Commit to your GitHub repo:
   ```bash
   git add INSTALL.md
   git commit -m "Add SDN installation guide"
   git push
   ```
4. GitHub will automatically render the Markdown with proper formatting, syntax highlighting, and emoji support 🎉

Let me know if you'd like me to add a table of contents, badges, or a license section!
