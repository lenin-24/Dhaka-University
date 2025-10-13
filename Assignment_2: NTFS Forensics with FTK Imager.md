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


# c) Identify Resident File
## Step 10: Find a Small File (< 512 bytes)

In FTK Imager, look for small_file.txt or resident.txt
Check file size in Properties - should be < 512 bytes
Note its MFT entry number = 46 (47104)

![c1](https://github.com/user-attachments/assets/177d5c1f-cef8-42ba-9928-2f82925888ea)


## Step 11: Analyze in MFT

Open $MFT in hex editor
Navigate to the file's MFT entry
Look for DATA attribute (0x80):
Search for: 80 00 00 00 (attribute type 0x80)
Check the attribute header:

Byte at offset +0x08 from attribute start
If value is 0x00 = RESIDENT
If value is 0x01 = NON-RESIDENT
For resident files:

Data is stored RIGHT THERE in the MFT
You can see the actual file content in hex
Look for attribute offset and length
As value is 0x00 = RESIDENT

![c2](https://github.com/user-attachments/assets/dbfd2263-0e30-48ca-8573-26a9d256e3ee)


So the bytes 53 6d 61 6c 6c 20 63 6f 6e 74 65 6e 74 20 68 65 72 65 represent:
👉 "Small content here"

![c3](https://github.com/user-attachments/assets/9659fb4e-fa2f-47e6-99b3-32f5d25cdde0)



## Step 12: Verify Content

Note the content offset within the attribute
Read the hex values
Convert to ASCII to see actual content
It should match your file content!
Documentation for (c):



Screenshot showing the file size < 512 bytes in FTK
Screenshot of MFT entry in hex editor
Highlight attribute 0x80 (DATA)
Show resident flag (0x00)
Highlight the actual file content stored in MFT
Convert hex to ASCII and show it matches the file

# d) Data Runs for Non-Resident Files
## Step 13: Select a Large File

In FTK Imager, select large_file.dat or notepad.exe
Check size > 512 bytes= 10240
Note MFT entry number= 43 (44032)
43 × 1024 = 44,032 (0xAC00) 
![d1](https://github.com/user-attachments/assets/30fe3c6b-4211-49df-bb12-b527135fa354)

in 0xad10  DATA attribute value is 01 so non-resident

## Step 14: Find Data Run Information

Open $MFT in hex editor
Navigate to file's MFT entry
Find DATA attribute (0x80)
Check non-resident flag (0x01)
Locate the data run field


![d2](https://github.com/user-attachments/assets/b2766bc7-2140-41e7-bd2e-f27c1c738d67)

$DATA Attribute Start: 0x0000ad10
0x0000ad10 + 0x08 = 0x0000ad18
→ You confirmed it’s 01 → non-resident ✅

Data runs offset = 0x0040 (little-endian of 40 00) → 64 bytes
So, data runs begin at:
0x0000ad10 + 0x40 = 0x0000ad50
it is 21 03 89 15 00 00 00 00 ff ff ff ff 82 79 47 11

Step 4: Calculate Physical Location 

From FTK Imager (or standard NTFS USB format), the cluster size is almost certainly 4096 bytes. 

    Physical Byte Offset = Cluster Offset × Cluster Size
    = 5,513 × 4,096
    = 22,581,248 bytes from the start of the partition. 
     

File Size on Disk: 

    = Run Length × Cluster Size
    = 3 × 4,096
    = 12,288 bytes 
     

(Your file is 10,240 bytes, so it uses 12,288 bytes on disk — normal due to cluster rounding.) 

1. Header Byte: 21 

    High nibble (2) = 2 bytes for cluster offset
    Low nibble (1) = 1 byte for run length
     

2. Run Length: 03 

    Length = 0x03 = 3 clusters
     

3. Cluster Offset: 89 15 

    This is in little-endian format → reverse the bytes: 15 89
    Convert to hex: 0x1589
    Convert to decimal:
    0x1589 = (1 × 4096) + (5 × 256) + (8 × 16) + 9 = 4096 + 1280 + 128 + 9 = **5,513**
     

✅ So the file data starts at cluster 5,513 from the beginning of the partition’s data area. 

Final Answer for Your Report (Section d) 

    File: large_file.dat
    MFT Entry: #43
    $DATA Attribute Start: 0x0000ad10
    Data Runs Offset Field: 40 00 → 64 bytes
    Data Runs Start: 0x0000ad50
    Data Run Bytes: 21 03 89 15   

    Decoding:   

        Header: 21 → 2 bytes for offset, 1 byte for length  
        Length: 03 → 3 clusters  
        Offset: 89 15 → little-endian = 0x1589 = 5,513 clusters  
        Cluster Size: 4,096 bytes  
        Physical Offset = 5,513 × 4,096 = 22,581,248 bytes from partition start  
        File Size on Disk = 3 × 4,096 = 12,288 bytes
         



===============================================================================     



