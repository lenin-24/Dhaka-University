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

