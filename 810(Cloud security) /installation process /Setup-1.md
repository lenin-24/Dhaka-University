# Installing VMware Workstation Pro


##  Step-by-Step Installation Instructions

### Step 1: Register and Download VMware Workstation Pro

1. Open a browser and navigate to: [https://profile.broadcom.com/web/registration](https://profile.broadcom.com/web/registration)
2. Create a free Broadcom account using your email address
3. After email verification, log in to the **Broadcom Support Portal**
4. Search for `VMware Workstation Pro` in the downloads section
5. Select version **17.6.1** (or latest) for **Windows x64** and click **Download**
6. Save the installer file (approximately 450 MB, e.g., `VMware-workstation-full-17.6.1-24319023.exe`)

### Step 2: Run the Installer

7. Navigate to the downloaded file in Windows Explorer
8. Right-click the installer and select **Run as administrator**
9. The **VMware Workstation Pro Setup Wizard** will open — click **Next**
10. Read the End-User License Agreement, tick ✅ *I accept the terms in the License Agreement*, click **Next**
11. On the **Custom Setup** screen:
    - Keep default install path: `C:\Program Files (x86)\VMware\VMware Workstation\`
    - Ensure ✅ *Add VMware Workstation console tools into system PATH* is checked
    - Click **Next**
12. On **User Experience Settings**:
    - ❌ **UNTICK** *Join the VMware Customer Experience Improvement Program* (privacy best practice)
    - Click **Next**
13. On **Shortcuts**: keep *Desktop* and *Start Menu* checked, click **Next**
14. Click **Install** — the installation takes approximately **3–5 minutes**
15. When complete, click **Finish**
    > ⚠️ **Do NOT enter a license key** — VMware Workstation Pro is **free for personal use**
16. Reboot the system if prompted

---

## 11. Verification Steps

17. Search `VMware` in the Windows search bar — **VMware Workstation Pro** should appear
18. Double-click to launch — the home screen should display:
    - ✅ *Create a New Virtual Machine*
    - ✅ *Open a Virtual Machine*
    - ✅ *Connect to a Remote Server*
19. Verify installation settings:
    - Go to **Edit > Preferences**
    - Confirm the installation path and default settings are correctly configured

### ✅ Quick Verification Checklist

| Check | Expected Result |
|-------|----------------|
| Application Launch | VMware Workstation Pro opens without errors |
| Home Screen | Shows "Create/Open/Connect" options |
| Preferences Path | Points to `C:\Program Files (x86)\VMware\VMware Workstation\` |
| License Status | Shows "Personal Use" (no key required) |

> 💡 **Tip**: After installation, you can immediately start creating VMs. For optimal performance, ensure hardware virtualization (VT-x/AMD-V) is enabled in your BIOS/UEFI settings.
