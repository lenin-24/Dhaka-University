## 1. Computer Name=
# Go to: Data Sources → Volume → Windows → System32 → config → System
than go to hive where ControlSet001->Control- ComputerName there it is

## 2. Time Zone info
# Go to: Data Sources → Volume → Windows → System32 → config → System
than go to hive where ControlSet001->Control- TimeZoneInformation there it is

## 3. IP Address
# Go to: Data Sources → Volume → Windows → System32 → config → System
than go to hive where ControlSet001->Services-> Tcpip - parameters-interfaces- than 2nd folder select korle ans dakhabe there it is

## 4. Find out the mounted device "King Stone"? 
# Go to: Data Sources → Volume → Windows → System32 → config → System
than go to hive where Mounted Devices where \DosDevices\M: where mark 0x20 & 0x30

## 5. Current version of the windows?  
# Go to: Data Artifacts → Operating System Info 

## 6.Find out the installation Date in UTC? (dd/mm/yyyy)
# Go to: Data Sources → Volume → Windows → System32 → config → Software
than go to hive where microsoft -> Windows NT -> Current Version click you will see isntall date

## 7.  BillyBob user account creation date
# Go to: Data Sources → Volume → Windows → System32 → config → SAM
than go to hive where Sam - Domains - Account- Users - Names - Billybob 
## 8. Who created "Joe T. Nameless" this user account?
# Go to: Data Sources → Volume → Windows → System32 -> winevt -> logs 
than click in	Security.evtx than right click in " Open in External viewer"
than in that viewer select "Filter current log" than in Event id write 4720 
than find Account Name:		Sgt. Joe T. Nameless than select it than you will see "event properties" in the right side click it and you get answer

## 9. BillyBob last login time?
# Go to: Data Sources → Volume → Windows → System32 -> winevt -> logs 
than click in	Security.evtx than right click in " Open in External viewer"
than in that viewer select "Filter current log" than in Event id write 4624
and top e jeta thakbe oitai latest click it and oita than you will see "event properties" in the right side click it and you get answer

## 10. Did BillyBob successfully reset password of the other user?
# Go to: Data Sources → Volume → Windows → System32 -> winevt -> logs 
than click in	Security.evtx than right click in " Open in External viewer"
than in that viewer select "Filter current log" than in Event id write 4724
and top e jeta thakbe oitai latest click it and oita than you will see "event properties" in the right side click it and you get answer









