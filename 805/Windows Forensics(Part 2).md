# Windows Forensic Data Locations

## 1. Computer Name
- Go to: `Data Sources → Volume → Windows → System32 → config → System`
- Navigate to hive: `ControlSet001 → Control → ComputerName`
- The computer name is located here.

## 2. Time Zone Info
- Go to: `Data Sources → Volume → Windows → System32 → config → System`
- Navigate to hive: `ControlSet001 → Control → TimeZoneInformation`
- Time zone details are stored here.

## 3. IP Address
- Go to: `Data Sources → Volume → Windows → System32 → config → System`
- Navigate to hive: `ControlSet001 → Services → Tcpip → Parameters → Interfaces`
- Select the **second folder** (typically the active interface) to find the IP address.

## 4. Find the Mounted Device "King Stone"
- Go to: `Data Sources → Volume → Windows → System32 → config → System`
- Navigate to hive: `MountedDevices`
- Look for the entry `\DosDevices\M:`
- Check the binary data at offsets `0x20` and `0x30` for the identifier "King Stone".

## 5. Current Version of Windows
- Go to: `Data Artifacts → Operating System Info`

## 6. Installation Date in UTC (dd/mm/yyyy)
- Go to: `Data Sources → Volume → Windows → System32 → config → Software`
- Navigate to hive: `Microsoft → Windows NT → CurrentVersion`
- The `InstallDate` value shows the installation timestamp (convert from Unix epoch to UTC).

## 7. BillyBob User Account Creation Date
- Go to: `Data Sources → Volume → Windows → System32 → config → SAM`
- Navigate to hive: `SAM → Domains → Account → Users → Names → BillyBob`

## 8. Who Created the User Account "Joe T. Nameless"?
- Go to: `Data Sources → Volume → Windows → System32 → winevt → logs`
- Open: `Security.evtx`
- Right-click → **Open in External Viewer**
- In the viewer:
  - Filter current log: **Event ID = 4720**
  - Locate the event with `Account Name: Sgt. Joe T. Nameless`
  - Check **Event Properties** (right sidebar) to identify the creator.

## 9. BillyBob Last Login Time
- Go to: `Data Sources → Volume → Windows → System32 → winevt → logs`
- Open: `Security.evtx`
- Right-click → **Open in External Viewer**
- In the viewer:
  - Filter current log: **Event ID = 4624**
  - The **topmost (most recent)** event for BillyBob shows the latest successful login.
  - View **Event Properties** for the exact timestamp.

## 10. Did BillyBob Successfully Reset Another User's Password?
- Go to: `Data Sources → Volume → Windows → System32 → winevt → logs`
- Open: `Security.evtx`
- Right-click → **Open in External Viewer**
- In the viewer:
  - Filter current log: **Event ID = 4724**
  - Review events to see if BillyBob performed a password reset.
  - Check **Event Properties** for confirmation and target user details.
