### 1. System: Computer Name
   
(C:\WINDOWS\system32\config\SYSTEM > ControlSet001 > Control > ComputerName)

### 2. Time Zone:

SYSTEM > ControlSet001 > Control > TimeZoneInformation

### 3. IP Address:

SYSTEM\ControlSet001\Services\Tcpip\Parameters\Interfaces\{GUID}\DHCP IP

### 4. Mounted Devices:

C:\WINDOWS\system32\config\SYSTEM >MountedDevices
 
### 5. Software: Operating System  Version

(C:\WINDOWS\system32\config\, SOFTWARE > Microsoft > Windows NT > Current Version)

### 6. Installed Applications:

If you want to find Installed Applications in Autopsy (from a Windows image), the correct registry path is:

SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall

### 7. NTUSER.DAT: 

Search keyword > NTUSER.DAT; Recent documents (NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs)

### 8. event logs: WINDOWS\system32\winevt\Logs

### 9. SAM: C:\WINDOWS\system32\config\SAM > Domain > Account > Users

### 10. System: C:\Windows\System32\Config\System; SYSTEM\CurrentControlSet001\Enum\USBSTOR


==================================================================================

Soln. Mr. Evil (Can be checked through Date Accessed column)

Q9. List the network cards used by this computer?

Soln. Xircom CardBus Ethernet 100 + Modem 56 (Ethernet Interface)

Compaq WL110 Wireless LAN PC Card

We find answer at C:\WINDOWS\system32\config\software\Microsoft\Windows NT\CurrentVersion\NetworkCards\

Q10. What is the IP address and MAC address of the computer?

Soln. IP=192.168.1.111

MAC=00:10:a4:93:3e:09

We go to C:/Program Files/Look@LAN/irunin.ini

Q12. Which Email client is used by Mr. Evil?

Soln: Outlook Express, Forte Agent, MSN Explorer, MSN (Hotmail) Email

Go to C:/WINDOWS/system32/config/Clients/Mail

Q13. What is the SMTP email address for Mr. Evil?

Soln: whoknowsme@sbcglobal.net

We find the answer at C:\Program Files\Agent\Data\AGENT.INI

Q14. How many executable files are in the recycle bin?

Soln. There are 4 namely, Dc1.exe, Dc2.exe, Dc3.exe, Dc4.exe

We find those at C:/RECYCLER (RECYCLER is the directory for Recycle Bin.)

Q15. Are there any viruses on the computer?

Soln. Yes, a zip bomb(unix_hack.tgz) is present.

For this, in the left side panel, we go to Results > Interesting Items > Possible ZipBomb > Interesting Files

Q16. A popular IRC (Internet Relay Chat) program called MIRC was installed. What are the userid, username, email and nickname used when the user was online in a chat channel?

Printed Text (Right Column)
Soln. user=Mini Me, email=none@of.ya, nick=Mr, anick=mrevilrulez

We can find that at C:\Program Files\mIRC\mirc.ini

Q17. Ethereal, a popular “sniffing” program... What is the name of the file that contains the intercepted data?

Soln. File name is ‘Interception’

As hinted we need to go to through My Documents which in this case would be Documents and Setting/Mr.Evil

Q18. What type of wireless computer was the victim... using?

Soln: Internet Explorer 4 on Windows CE

We find this in Interception file.

Q19. What websites victim was accessing?

Soln. https://www.google.com/search?q=Mobile.msn.com, MSN (Hotmail) Email

Q20. What is the web-based email address for main user?

Soln. mrevilrulez@yahoo.com (Through web history)

To find this, in the left side panel, we go to Results > Extracted Content > Web History...


























