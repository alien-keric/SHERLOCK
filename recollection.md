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
After the attacker gained access to the machine, the attacker copied an obfuscated PowerShell command to the clipboard. What was the command?
answer:PS C:\Users\user> (gv '*MDR*').naMe[3,11,2]-joIN'' 
```

```
qn:The attacker copied the obfuscated command to use it as an alias for a PowerShell cmdlet. What is the cmdlet name?

answer: Invoke-Expression

resource used: https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-expression?view=powershell-7.4
```







