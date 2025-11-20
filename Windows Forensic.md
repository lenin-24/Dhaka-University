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
2. Right-click the image → **View Details**
3. Check the displayed hash values

**Answer:**  
`e46414fdc2bb1a9012896799435f7dea1ce18143`
