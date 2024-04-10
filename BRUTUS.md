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
```
