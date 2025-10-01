This task mainly helps to create a exact copy of the contnets which the imposter had so that the forensics team can analyze 
the data for any signs of malicious activity (e.g., a hacker’s actions) without touching the original USB.
Example- Samin was smuggling company top secret to Nayeem via pendrive but got caught by company and now to prove his misdeeds the company 
brought Lenin a digital Forensics expert to see what types of things he did so without risking of data in usb Lenin made a copy of everything in usb without entering it and 
did analysis to have proof of Samins bad activities.


Step1- download the write blocker 
(((It is used so that third part cant manipulate data from this device by writing anything or deleting)))
https://sourceforge.net/projects/usbwriteblockerforwindows8/


Step2- extract and open the main file than write 1 and press enter. it will start the write blocker.
<img width="1096" height="632" alt="bloker on" src="https://github.com/user-attachments/assets/40654a4e-a278-454a-b525-dc819026d63d" />



Step3- Insert the pendrive now 
(((If you insert the pendrive before activating the write blocker, there’s a risk of unintended write operations (e.g., Windows updating file access timestamps or creating system files).
This could compromise the forensic validity of the data, as even minor changes can affect hash values and evidence admissibility.)))



step4- open FTK Imager. than go to File-> Create Disk Image
<img width="1522" height="982" alt="create disk image" src="https://github.com/user-attachments/assets/6f7d22d6-ed8f-457a-a4be-db8a010a5401" />




Step5- Select Physical as it is a usb
<img width="758" height="516" alt="physical" src="https://github.com/user-attachments/assets/f6156acf-378a-411c-ab30-68bdc4e849f2" />



Step6- select the pendrive section
<img width="685" height="493" alt="select pendrive" src="https://github.com/user-attachments/assets/2988aa42-bb73-439d-94a5-806b449c02ad" />




Step7- select destisnation of file save where select E01 and fill up evidence info 
<img width="561" height="562" alt="select dest" src="https://github.com/user-attachments/assets/97d13372-bfa2-40c8-aa64-af45fb35c8fa" />




Step8- it will take time to process the image.
<img width="482" height="375" alt="process" src="https://github.com/user-attachments/assets/78503f24-6bca-4b15-a50a-cf2d883f0c67" />




Final step- a window of the result will pop up to show required info
<img width="1722" height="577" alt="hash result" src="https://github.com/user-attachments/assets/5e65da68-715b-446e-a726-dccd8ff36877" />

The MD5 hash is a cryptographic checksum used to verify data integrity.
The computed hash (generated during imaging) matches the stored verification hash (saved with the image) and the reported hash.
A "Match" indicates that the forensic image is an exact, unaltered copy of the original USB drive’s data, confirming no corruption or changes occurred during the imaging process.
