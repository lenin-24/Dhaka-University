Extra
1. Location of the Event Logs: C:\WINDOWS\system32\winevt\Logs
Purpose in Digital Forensics
• Identify user logon/logoff activity
• Detect failed logon attempts or privilege escalation
• Track software installation or system crashes
• Correlate timestamps across other artifacts
• Detect log clearing or tampering (anti-forensic behavior)
   

# Step-by-Step Digital Forensics Investigation Using Autopsy

## Phase 1: Setting Up the Case in Autopsy

### Step 1: Install and Launch Autopsy
- Download and install **Autopsy** (free, open-source tool)
- Launch Autopsy

### Step 2: Create a New Case
1. Click **"New Case"**
2. Fill in case information:
   - **Case Name:** `Lab_Image_Investigation`
   - **Base Directory:** Choose where to store case files
   - **Case Number:** `001`
   - **Examiner:** *Your Name*
3. Click **"Finish"**

### Step 3: Add Data Source
1. Click **"Add Data Source"**
2. Select **"Disk Image or VM File"** → Click **Next**
3. Browse to your image file:  
   `/mnt/user-data/uploads/Lab_Image.E01`
4. Click **Next**
5. Select ingest modules (recommended: **select all**):
   - Recent Activity  
   - Hash Lookup  
   - File Type Identification  
   - Extension Mismatch Detector  
   - Embedded File Extractor  
   - Email Parser  
   - Encryption Detection  
   - Interesting Files Identifier  
6. Click **Next** and wait for ingest to complete (10–30 minutes)

---

## Phase 2: Answering the Questions

### Question 1: SHA-1 Hash Verification
- Provided in the text file:  
  `e46414fdc2bb1a9012896799435f7dea1ce18143`

**To verify in Autopsy:**
1. Go to **Data Sources**
2. Go to Lab Image.E01 tan click on tab "File Metadata"
3. In there you will see value in SHA1.

**Answer:**  
`e46414fdc2bb1a9012896799435f7dea1ce18143`

## Question 2: File System of Primary Volume

**Steps:**

In Autopsy, expand "Data Sources"  
Click the "Lab_Image.E01"  
In the table, click "Text"  
There you will see it is of NTFS type  
Also, in your current view, you’re seeing the files and folders inside the file system (e.g., $OrphanFiles, Users, Windows, etc.), which strongly suggests it is NTFS.

**Answer:**  
`NTFS`

---

## Question 3: Operating System Name

**Steps:**

Navigate to: Data Sources → Lab_Image.E01  
In the table, click "Data Artifacts"  
There, in Program Name, you will see the answer.  
Or  
Go to Data Artifacts in left panel, then click "Operating System Info"  
In the table, check the "OS Name" or "Program Name" field.

**Answer:**  
`Microsoft Windows 10 Pro`

---

## Question 4: Registered Owner

**Steps:**

Navigate to registry hives  
Go to: Data Sources → Volume → Windows → System32 → config → SOFTWARE  
Use Autopsy's registry viewer or:  
Use "Keyword Search" to search for "RegisteredOwner"  
Or  
Look in SOFTWARE registry hive at:  
`Microsoft\Windows NT\CurrentVersion\RegisteredOwner`  
Alternative method:  
Check "OS Account" in the left panel  
Or search for "RegisteredOwner" in keyword search.

**Answer:**  
`Randy Prakken`

---

## Question 5: Computer Name

**Steps:**

Go to registry analysis of config file of system32  
Navigate to: SYSTEM registry hive  
Look at:  
`SYSTEM\ControlSet001\Control\ComputerName\ComputerName\ComputerName`  
There is the answer.

**Answer:**  
`DESKTOP-8VQ4R1N`

---

## Question 6: Which user has "Good Bye Blue Sky.txt"?

**Steps:**

Use "File Search" feature  
In the search box at top right, type: `Good Bye Blue Sky.txt`  
Look at the file path to determine which user directory it's in  
The path will be: `C:\Users\[USERNAME]\...`

**Answer:**  
`Randy`

---

## Question 8: USB Device Serial Number (Kingston DataTraveler)

**Steps:**

Navigate to: `C:\Windows\System32\config\SYSTEM`  
Open SYSTEM registry hive  
Look for:  
`SYSTEM\CurrentControlSet\Enum\USBSTOR`  
Find Kingston DataTraveler entries  
Check the serial number/Device ID

**Answer:**  
`001CC0EC368FFC415A3432D2`

---

## Question 9: Password Manager Software

**Steps:**

Go to "Data Artifacts" → "Installed Programs"  
Look for password manager in Keyword Search  
You need to find "Installed Program Artifact"  
There you will see the software (KeePass, LastPass, 1Password, Bitwarden, etc)

**Answer:**  
`KeePass Password Safe 2.55`

---

## Question 11: Size of pdf.pdf

**Steps:**

Navigate through user directories manually  
Use "File Types" → "Documents" → "PDF"  
Filter or search for "pdf.pdf"  
Check file size in details pane or right-click → properties

**Answer:**  
`102645 bytes`

---

## Question 12: User Account with letterlegal5.doc

**Steps:**

Use "File Types" → "Documents" → search by file name  
Click on file `letterlegal5.doc`  
In the table below, see in text header there is metadata written:  
`meta:last-author: Randy Prakken`

**Answer:**  
`Randy Prakken`
