## Part 1: Setup and Create Test File

# Step 1: Create a working directory

mkdir C:\ADS_Lab
cd C:\ADS_Lab
Step 2: Create a normal file

echo This is the main content > testfile.txt

## Part 2: ADD Alternate Data Streams

# Step 1: Add text to ADS

echo This is hidden data > testfile.txt:hidden.txt
echo Secret information > testfile.txt:secret.txt
echo More hidden content > testfile.txt:stream3.txt

## Part 3: DISPLAY/DETECT Alternate Data Streams
Step : Using DIR command
dir /r
 This shows files with their ADS names and sizes

## download stream to delete directly in terminal
https://learn.microsoft.com/en-us/sysinternals/downloads/streams

## Part 4: Using Streams utility

Remove all ADS from a file
C:\streams.exe -d testfile.txt

This tells Windows:

Use the tool from C:\streams.exe
Target the file at D:\DU\ADS\ADS_Lab\testfile.txt




 
