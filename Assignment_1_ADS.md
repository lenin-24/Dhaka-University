# Part 1: Setup and Create Test File

## Step 1: Create a working directory

mkdir C:\ADS_Lab
cd C:\ADS_Lab

![create text](https://github.com/user-attachments/assets/7ba90f7a-ca14-49af-905a-b0a55cf8879d)

Step 2: Create a normal file

echo This is the main content > testfile.txt

![text add](https://github.com/user-attachments/assets/05a32b8e-b639-4604-82e4-942610268334)


# Part 2: ADD Alternate Data Streams

## Step 1: Add text to ADS

echo This is hidden data > testfile.txt:hidden.txt
echo Secret information > testfile.txt:secret.txt
echo More hidden content > testfile.txt:stream3.txt

![text inside](https://github.com/user-attachments/assets/44b9110f-31a1-4126-9a29-4573353af6af)


# Part 3: DISPLAY/DETECT Alternate Data Streams
## Step : Using DIR command
dir /r
 This shows files with their ADS names and sizes
 
![show ads](https://github.com/user-attachments/assets/f5d60a7f-6997-466e-b0ef-cbdd921a5712)

## download stream to delete directly in terminal
https://learn.microsoft.com/en-us/sysinternals/downloads/streams

# Part 4: Using Streams utility

Remove all ADS from a file
C:\streams.exe -d testfile.txt

This tells Windows:

Use the tool from C:\streams.exe
Target the file at D:\DU\ADS\ADS_Lab\testfile.txt


![delete ads](https://github.com/user-attachments/assets/69d5c6c3-40c2-46d9-a5ab-90ac3084feeb)



 
