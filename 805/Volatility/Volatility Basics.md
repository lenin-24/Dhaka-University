dump file link

https://drive.google.com/file/d/1kwFvRTQJ5RapBufebaCTODp1U9P68jG3/view

volatility link

https://drive.google.com/file/d/13sHXEhSPD75VueHXcZZ2HeACtVB8G1m3/view


cmd to check installation
```
vol.exe –h
```

cmd to verify dump file 
```
vol.exe -f wmd.mem windows.info
```
# Useful comands for exam 

If your Command Prompt is already opened in the same directory as the .mem file, you do not need to give the full path.

vol.exe -f wmd.mem windows.info

vol.exe -f wmd.mem windows.sessions

vol.exe -f wmd.mem windows.netscan

vol.exe -f wmd.mem windows.pslist

vol.exe -f wmd.mem windows.pstree

vol.exe -f wmd.mem windows.psscan

vol.exe -f wmd.mem windows.cmdline

vol.exe -f wmd.mem windows.dlllist

vol.exe -f wmd.mem windows.hashdump

vol.exe -f wmd.mem windows.registry.hivelist

vol.exe -f wmd.mem windows.filescan


....................................................................................................................

windows.info – Identifies the operating system, architecture, and boot time to establish the system context.

windows.sessions – Shows which users were logged into the system and their session details.

windows.netscan – Extracts active and closed network connections to identify suspicious communications.

windows.pslist – Lists active processes as seen by the Windows kernel at capture time.

windows.pstree – Displays parent-child relationships between processes to spot abnormal spawning.

windows.psscan – Scans memory to find hidden or terminated processes not shown by pslist.

windows.cmdline – Reveals the exact command-line arguments used to launch each process.

windows.dlllist – Lists DLLs loaded by processes to detect injection or malicious modules.

windows.hashdump – Extracts NTLM password hashes from memory for credential compromise analysis.

windows.registry.hivelist – Identifies registry hives in memory for persistence and configuration analysis.

windows.filescan – Scans memory for file objects, including deleted or in-memory malware artifacts.


## Volatility 3 Plugin Descriptions

| Plugin                 | Purpose                                                       | Notes                                               |
| ---------------------- | ------------------------------------------------------------- | --------------------------------------------------- |
| **windows.psscan**     | Recovers processes by scanning memory for EPROCESS structures | Detects hidden or terminated processes (rootkits)   |
| **windows.pstree**     | Shows processes in parent-child hierarchy                     | Good for timeline/attack chain                      |
| **windows.handles**    | Lists process handles: files, mutexes, registry keys, events  | Malware often creates suspicious mutexes            |
| **windows.dlllist**    | Shows DLLs loaded by each process                             | Used for malware injection detection                |
| **windows.driverscan** | Finds kernel drivers by memory scan                           | Good for rootkit detection                          |
| **windows.sessions**   | Displays logon sessions and logon users                       | Useful to track intruder accounts                   |
| **windows.hashdump**   | Dumps password hashes from SAM/LSA                            | Can be cracked offline                              |
| **windows.lsadump**    | Extracts credentials/tokens from LSASS memory                 | Often reveals plaintext or DPAPI secrets            |
| **windows.filescan**   | Carves FILE_OBJECTs to locate hidden or deleted files         | Very useful for ransomware investigations           |
| **windows.evtx**       | Extracts and parses Windows Event Log records from memory     | Correct plugin name is windows.evtx (not eventevtx) |


## Additional Very Useful Plugins

| Plugin                        | Purpose                                                   |
| ----------------------------- | --------------------------------------------------------- |
| **windows.netscan**           | Shows network connections (TCP/UDP) & malware C2 channels |
| **windows.malfind**           | Detects injected or suspicious code inside processes      |
| **windows.cmdline**           | Shows command-line arguments of processes                 |
| **windows.cmdscan**           | Recovers command history from console buffers             |
| **windows.registry.hivelist** | Lists registry hives recovered from memory                |
| **windows.registry.printkey** | View specific registry keys and malware persistence       |
| **windows.svcscan**           | Lists services including hidden or malicious ones         |
