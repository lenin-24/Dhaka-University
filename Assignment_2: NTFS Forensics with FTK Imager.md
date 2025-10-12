# PHASE 1: Preparation
## Step 1: Prepare the Pen Drive

Format a USB drive with NTFS:

Right-click on drive → Format → NTFS
Create test structure:

## Create folders
mkdir E:\Documents
mkdir E:\Images
mkdir E:\Data

## Create a small file (< 512 bytes) - RESIDENT FILE
echo This is a small file with less than 512 bytes of content > E:\small_file.txt

## Create a larger file - NON-RESIDENT FILE
fsutil file createnew E:\large_file.dat 10240

## Add some regular files
copy C:\Windows\System32\notepad.exe E:\notepad.exe
echo Document content > E:\Documents\document.txt
Create a file specifically < 512 bytes (for resident analysis):

echo Small content here > E:\resident.txt 
