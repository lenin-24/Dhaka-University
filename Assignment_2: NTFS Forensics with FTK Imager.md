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






