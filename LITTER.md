sherlock scenarion
```
Khalid has just logged onto a host that he and his team use as a testing host for many different purposes, it’s off their corporate network but has access to lots of resources in network.
The host is used as a dumping ground for a lot of people at the company but it’s very useful, so no one has raised any issues.Little does Khalid know; the machine has been compromised and company information that should not have been on there has now been stolen –
it’s up to you to figure out what has happened and what data has been taken.
```

its a pcap file inside the zip file( Task 3-8 can be answered after decoding the hex value using any tool from the pcap file
```
┌──(alienx㉿alienX)-[~/Desktop/PPP/SHERLOCK/LITTLE]
└─$ ls
litter.zip  suspicious_traffic.pcap

answ(1): DNS
answ(2):192.168.157.145

ip.addr == 192.168.157.145&&dns

5e36011ccd4160752877686f616d690a6465736b746f702d756d6e636265<375c746573740d0a0d0a433a5c55736572735c746573745c446f776e6c6f.6164733e

after decrpt

^6ÍA`u(whoami
desktop-umncbe7\test

C:\Users\test\Downloads>


answer(TASK 3) : command:whoami
answ(Task 4): dnscat-v0.07(tool used to enumerate the domain name)

N/B: ren( a command in windows that can be used to rename the a file or directory)

answer(Task 5):win_installer.exe ( here the attacker tried to rename the dnscat2-v0.07-client-win32.exe  to win_installer.exe


answer(Task 6): 0 ( 0 records were found from the cloud storage)



answer(Task 7):C:\Users\test\Documents\client data optimisation\user details.csv


answer(8):721 (the number of records were stolen)

```
