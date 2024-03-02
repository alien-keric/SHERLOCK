step 1: 
```
┌──(alienx㉿alienX)-[~/…/PPP/SHERLOCK/ROGUE/volatility3]
└─$ python3 vol.py -f ../20230810.mem  windows.info.Info      
Volatility 3 Framework 2.6.1
Progress:  100.00               PDB scanning finished                                                                                              
Variable        Value

Kernel Base     0xf80178400000
DTB     0x16a000
Symbols file:///home/alienx/Desktop/PPP/SHERLOCK/ROGUE/volatility3/volatility3/symbols/windows/ntkrnlmp.pdb/3789767E34B7A48A3FC80CE12DE18E65-1.json.xz
Is64Bit True
IsPAE   False
layer_name      0 WindowsIntel32e
memory_layer    1 FileLayer
KdVersionBlock  0xf8017900f398
Major/Minor     15.19041
MachineType     34404
KeNumberProcessors      8
SystemTime      2023-08-10 11:32:00
NtSystemRoot    C:\WINDOWS
NtProductType   NtProductWinNt
NtMajorVersion  10
NtMinorVersion  0
PE MajorOperatingSystemVersion  10
PE MinorOperatingSystemVersion  0
PE Machine      34404
PE TimeDateStamp        Mon Nov 24 23:45:00 2070

```
After running image info we now know what were dealing with here

qn1:
```
┌──(alienx㉿alienX)-[~/…/PPP/SHERLOCK/ROGUE/volatility3]                                                                                                                                                                                      
└─$ python3 vol.py -f ../20230810.mem  windows.netscan.NetScan                                                                                                                                                                                
Volatility 3 Framework 2.6.1                                                                                                                                                                                                                  
Progress:  100.00               PDB scanning finished                                                                                                                                                                                         
Offset  Proto   LocalAddr       LocalPort       ForeignAddr     ForeignPort     State   PID     Owner   Created                                                                                                                               
                                                                                                                                                                                                                                              
0x9d80001baab0  TCPv4   172.17.79.131   64239   192.229.221.95  80      CLOSE_WAIT      8224    SearchApp.exe   2023-08-10 11:28:48.000000                                                                                                    
0x9e8b876769f0  TCPv4   0.0.0.0 49671   0.0.0.0 0       LISTENING       788     services.exe    2023-08-10 11:13:44.000000                                                                                                                    
0x9e8b87676cb0  TCPv4   0.0.0.0 49671   0.0.0.0 0       LISTENING       788     services.exe    2023-08-10 11:13:44.000000                                                                                                                    
0x9e8b87676cb0  TCPv6   ::      49671   ::      0       LISTENING       788     services.exe    2023-08-10 11:13:44.000000                                                                                                                    
0x9e8b8853ae90  TCPv4   0.0.0.0 49665   0.0.0.0 0       LISTENING       656     wininit.exe     2023-08-10 11:13:43.000000                                                                                                                    
0x9e8b8853ae90  TCPv6   ::      49665   ::      0       LISTENING       656     wininit.exe     2023-08-10 11:13:43.000000   
0x9e8b8cb58010  TCPv4   172.17.79.131   64254   13.127.155.166  8888    ESTABLISHED     6812    svchost.exe     2023-08-10 11:30:03.000000 

answer: pid:6812
```
qn 2:
```
in order to check for child process lets change the plugin here
┌──(alienx㉿alienX)-[~/…/PPP/SHERLOCK/ROGUE/volatility3]                                                                                                                                                                                      
└─$ python3 vol.py -f ../20230810.mem  windows.pstree.PsTree                                                                                                                                                                                  
Volatility 3 Framework 2.6.1                                                                                                                                                                                                                  
Progress:  100.00               PDB scanning finished                                                                                                                                                                                         
PID     PPID    ImageFileName   Offset(V)       Threads Handles SessionId       Wow64   CreateTime      ExitTime        Audit   Cmd     Path    

*** 2776        7436    RamCapture64.e  0x9e8b8aa66080  5       -       1       False   2023-08-10 11:31:52.000000      N/A     \Device\HarddiskVolume3\Users\simon.stark\Desktop\BelkaSoft Live RAM Capturer\BelkaSoft Live RAM Capturer\RamCapture64.exe    "C:\Users\simon.stark\Desktop\BelkaSoft Live RAM Capturer\BelkaSoft Live RAM Capturer\RamCapture64.exe"         C:\Users\simon.stark\Desktop\BelkaSoft Live RAM Capturer\BelkaSoft Live RAM Capturer\RamCapture64.exe
**** 9816       2776    conhost.exe     0x9e8b91cda080  6       -       1       False   2023-08-10 11:31:52.000000      N/A     \Device\HarddiskVolume3\Windows\System32\conhost.exe    \??\C:\WINDOWS\system32\conhost.exe 0x4 C:\WINDOWS\system32\conhost.exe
*** 6812        7436    svchost.exe     0x9e8b87762080  3       -       1       False   2023-08-10 11:30:03.000000      N/A     \Device\HarddiskVolume3\Users\simon.stark\Downloads\svchost.exe "C:\Users\simon.stark\Downloads\svchost.exe" C:\Users\simon.stark\Downloads\svchost.exe
**** 4364       6812    cmd.exe 0x9e8b8b6ef080  1       -       1       False   2023-08-10 11:30:57.000000      N/A     \Device\HarddiskVolume3\Windows\System32\cmd.exe        C:\WINDOWS\system32\cmd.exe     C:\WINDOWS\system32\cmd.exe
***** 9204      4364    conhost.exe     0x9e8b89ec7080  3       -       1       False   2023-08-10 11:30:57.000000      N/A     \Device\HarddiskVolume3\Windows\System32\conhost.exe    \??\C:\WINDOWS\system32\conhost.exe 0x4 C:\WINDOWS\system32\conhost.exe
* 956   744     fontdrvhost.ex  0x9e8b89530180  5       -       1       False   2023-08-10 11:13:43.000000      N/A     \Device\HarddiskVolume3\Windows\System32\fontdrvhost.exe        "fontdrvhost.exe"       C:\WINDOWS\system32\fontdrvhost.exe
10044   9952    OneDrive.exe    0x9e8b90507080  0       -       1       True    2023-08-10 11:15:31.000000      2023-08-10 11:15:37.000000      \Device\HarddiskVolume3\Users\simon.stark\AppData\Local\Microsoft\OneDrive\OneDrive.exe -   


NB: as seen above, we have a PID that spawned the malicous process was triggered by the PID = 4364
```

qn 3
```
This qn need the md5 hash, and in inorder to get the hash we need to dump the the pid and once we get the file we can easy calculate the hash from it

┌──(alienx㉿alienX)-[~/Desktop/PPP/SHERLOCK/ROGUE]                                                                                                                                                                                            
└─$ vol.py -f 20230810.mem windows.dumpfiles.DumpFiles --pid 6812 

now lets run the command:
┌──(alienx㉿alienX)-[~/Desktop/PPP/SHERLOCK/ROGUE]
└─$ md5sum file.0x9e8b91ec0140.0x9e8b90819750.DataSectionObject.svchost.exe.dat 
d41d8cd98f00b204e9800998ecf8427e  file.0x9e8b91ec0140.0x9e8b90819750.DataSectionObject.svchost.exe.dat
                                                                                                                                                                                                                                              
┌──(alienx㉿alienX)-[~/Desktop/PPP/SHERLOCK/ROGUE]
└─$ md5sum file.0x9e8b91ec0140.0x9e8b957f24c0.ImageSectionObject.svchost.exe.img
5bd547c6f5bfc4858fe62c8867acfbb5  file.0x9e8b91ec0140.0x9e8b957f24c0.ImageSectionObject.svchost.exe.img

answer:5bd547c6f5bfc4858fe62c8867acfbb5

```

qn 4
```
question 4 need us to get the connection was established to which machine, if we recal qn1, the connection was established from this source ip and port number

answer:13.127.155.166:8888
```

qn 5:
```
This question was a chain question of qn1 and qn4 so in order to get the timestamp the connection were established we just need to check check qn1 the or qn4

answer:10/08/2023 11:30:03
```

qn 6
```
I thought it would be easy and a direct question scan the disire PID 6812

┌──(alienx㉿alienX)-[~/Desktop/PPP/SHERLOCK/ROGUE]
└─$ vol.py -f 20230810.mem windows.psscan.PsScan --pid 6812
Volatility 3 Framework 2.5.2
Progress:  100.00               PDB scanning finished                        
PID     PPID    ImageFileName   Offset(V)       Threads Handles SessionId       Wow64   CreateTime      ExitTime        File output

6812    7436    svchost.exe     0x9e8b87762080  3       -       1       False   2023-08-10 11:30:03.000000      N/A     Disabled


And from here we can get the offset we want
answer:0x9e8b87762080
```


qn 7
```
It is very clear here that we need to upload our file to the virus total and if we view details we get

answer:10/08/2023 11:58:10
```

```
NEW IDEA: NOP

CHALLENGES: The big challenge here was my standalone volatility was not working so decide to check for volatility-master(V-python2) the got the same problem so decide to check for the last update from volatility3(V-python3) which worked

REFERENCE:
https://www.virustotal.com/gui/file/eaf09578d6eca82501aa2b3fcef473c3795ea365a9b33a252e5dc712c62981ea/details
https://app.hackthebox.com/sherlocks/RogueOne
```


