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

# File that you need to open in autopsy


[Uploading L<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<AutopsyCase>
  <SchemaVersion>6.0</SchemaVersion>
  <CreatedDate>2025/11/18 01:06:10 (BDT)</CreatedDate>
  <ModifiedDate>2025/11/18 01:06:14 (BDT)</ModifiedDate>
  <CreatedByAutopsyVersion>4.22.1</CreatedByAutopsyVersion>
  <SavedByAutopsyVersion>4.22.1</SavedByAutopsyVersion>
  <Case>
    <Name>lab_image_investigation_20251118_010610</Name>
    <DisplayName>Lab_Image_Investigation</DisplayName>
    <Number>001</Number>
    <Examiner>Lenin</Examiner>
    <ExaminerPhone/>
    <ExaminerEmail/>
    <CaseNotes/>
    <CaseType>Single-user case</CaseType>
    <Database/>
    <CaseDatabase>autopsy.db</CaseDatabase>
    <TextIndex/>
  </Case>
  <ContentProvider>
    <Name/>
  </ContentProvider>
  <OriginalCase/>
</AutopsyCase>
ab_Image_Investigation.aut…]()

# Images

## 1
![1](https://github.com/user-attachments/assets/e918bcda-26c3-41c5-b157-d4093d715255)

## 2
![2](https://github.com/user-attachments/assets/92756863-bf5e-4bda-8206-8e93d863bcbd)


## 3
![3](https://github.com/user-attachments/assets/fa2b1989-3a6e-46a8-af06-3c7951cdfe20)

## 4
![4](https://github.com/user-attachments/assets/7edf7ffb-e50f-41f1-baed-ef21b3382852)

## 5
![5](https://github.com/user-attachments/assets/aafd18e7-d8b4-4f1b-9da6-637b4ab0fdae)

## 6
![6](https://github.com/user-attachments/assets/7623f077-8ca6-4b66-8b8c-8eb3f1df8adf)

## 7
![7](https://github.com/user-attachments/assets/c180b436-51f0-4cfb-97fe-ccdcdd8848c0)

## 8
![8](https://github.com/user-attachments/assets/a5ed6ea0-d40a-4659-a2f5-fc8b31deff02)

## 9
![9](https://github.com/user-attachments/assets/246df15c-fc32-4b2d-8794-47bb658f1fda)

## 10

![10](https://github.com/user-attachments/assets/c3bb3c50-0c07-4611-8fe6-7baa9176ed17)


