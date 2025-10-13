# PHASE 1: Preparation
## Step 1: Prepare the Pen Drive

Format a USB drive with NTFS:

Right-click on drive → Format → NTFS
Create test structure:

## Create folders
mkdir E:\Documents
mkdir E:\Images
mkdir E:\Data

![make file](https://github.com/user-attachments/assets/668681c4-6f78-458f-9471-bbe335ea1c85)


## Create a small file (< 512 bytes) - RESIDENT FILE
echo This is a small file with less than 512 bytes of content > E:\small_file.txt

## Create a larger file - NON-RESIDENT FILE
fsutil file createnew E:\large_file.dat 10240

## Add some regular files
copy C:\Windows\System32\notepad.exe E:\notepad.exe
echo Document content > E:\Documents\document.txt

Create a file specifically < 512 bytes (for resident analysis):
echo Small content here > E:\resident.txt 


# PHASE 2: Create Forensic Image
## Step 2: Download and Install FTK Imager

* Download from: AccessData/Exterro website (free)
* Install FTK Imager
## Step 3: Create Disk Image

* Open FTK Imager
* Click File → Create Disk Image
* Select source:
* Choose "Physical Drive"
* Select your USB pen drive
* Click Finish
* Add destination:
* Click Add...
* Select image type: Raw (dd) or E01 (Expert Witness)
* Browse to save location: [desired place]


![ftk evidence](https://github.com/user-attachments/assets/820e79ec-cc97-4396-9de9-983287e8c300)
* Give it a name: PenDrive_Evidence
* Click Start
* Wait for imaging to complete
* Verify the image (check hash values)

![image proof](https://github.com/user-attachments/assets/bdd791a8-be7c-4448-aa46-89a8383ae026)


# PHASE 3: Add Evidence and Analysis
## Step 4: Add Evidence to FTK Imager

Click File → Add Evidence Item
Select Image File
Browse to your image: C:\Forensics\USB_Image.001 or .dd
Click Finish
The drive will now appear in the Evidence Tree panel.
![evidence path](https://github.com/user-attachments/assets/3c487f32-c266-48b3-a90c-52b5b3f3eec5)


# PHASE 4: Assignment Questions
a) MFT Entry Analysis (1024 bytes per entry)
## Step 5: Locate and Export MFT

In FTK Imager Evidence Tree:

Expand your evidence item
Expand the partition (e.g., NONAME [NTFS])
Navigate to [root]
Look for $MFT file
Export the MFT:

Right-click on $MFT
Select Export Files
Save to: C:\Forensics\MFT_Export\

![a2](https://github.com/user-attachments/assets/08118095-69f7-4d7f-b91b-82556369c79d)

## Step 6: Analyze MFT with Hex Editor

Download a hex editor (HxD - free)
Open the exported $MFT file
Observe the structure:
Offset 0x000: 46 49 4C 45 (FILE signature)
- This marks the start of an MFT entry
- Each entry is exactly 1024 bytes (0x400)

Offset 0x400: Next FILE signature
Offset 0x800: Next FILE signature
...and so on
![a3 1](https://github.com/user-attachments/assets/e82c15de-828c-4912-b48b-1303ef0e3e58)
![a3](https://github.com/user-attachments/assets/a7448e48-3e78-4327-be54-2ae6db9209bd)


## Documentation for (a):

Take screenshot of hex editor showing:
FILE signature at offset 0x000
Next FILE signature at offset 0x400 (1024 bytes later)
Highlight the 1024-byte span

# b) Find MAC Times from MFT Metadata
## Step 7: Select a Specific File

In FTK Imager, select any file (e.g., small_file.txt)
Note its MFT entry number from the Properties panel(My is 42)
Look at the "Properties" tab in FTK Imager - it shows timestamps

![mft record no](https://github.com/user-attachments/assets/9f01d521-3742-4832-ac02-20a35f744d3b)


## Step 8: Manual MFT Analysis

Open $MFT in hex editor

Calculate entry location:

MFT Entry Offset = Entry Number × 1024
offset=42*1024=43008, HEX= 0xA800


Navigate to that offset

Find Standard Information attribute (0xA800):


![standard time](https://github.com/user-attachments/assets/1282fda1-9481-4129-ab0e-afe19b3ae823)



Look for: 46 49 4C 45 .. (attribute type 0xA800)
You look for 46 49 4C 45 because it’s the magic signature that marks the start of every valid MFT entry in NTFS.

After attribute header, you'll find timestamps:
Offset from attribute start:
+0x00 to +0x07: Created Time (8 bytes)
+0x08 to +0x0F: Modified Time (8 bytes)
+0x10 to +0x17: MFT Modified Time (8 bytes)
+0x18 to +0x1F: Accessed Time (8 bytes)
Timestamps are in Windows FILETIME format:

64-bit value
Number of 100-nanosecond intervals since Jan 1, 1601
## Step 9: Convert Timestamp 

Use online converter or PowerShell:

![time](https://github.com/user-attachments/assets/ea1e17ea-89d2-4cb0-8658-7ceaaa86a1ba)

1️⃣ Created
[DateTime]::FromFileTime(0x01DC3B985F301B9F)

✅ Result: Monday, October 13, 2025 21:50:14 UTC
(≈ 10:50:14 PM UTC+6 → Bangladesh time)

2️⃣ Modified
[DateTime]::FromFileTime(0x01DC3B985F3031B5)


✅ Result: Monday, October 13, 2025 21:50:15 UTC
(≈ 10:50:15 PM UTC+6 → Bangladesh time)

# Convert FILETIME to readable date
[DateTime]::FromFileTime(0x01D7F5B5C6A8B000)

## Documentation for (b):

Screenshot of MFT entry showing attribute 0x10
Highlight the 4 timestamp fields (Created, Modified, MFT Modified, Accessed)
Note the hex values
Show converted human-readable dates
Compare with FTK Imager Properties panel




