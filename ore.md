Description
```
One of our technical partners are currently managing our AWS infrastructure. We requested the deployment of some technology into the cloud. The solution proposed was an EC2 instance hosting the Grafana application. Not too long after the EC2 was deployed the CPU usage ended up sitting at a continuous 98%+ for a process named "xmrig". Important Information Our organisation's office public facing IP is 86.5.206.121, upon the deployment of the application we carried out some basic vulnerability testing and maintenance.
```

qn1
```
from the version

just a simple google:https://github.com/pedrohavay/exploit-grafana-CVE-2021-43798
answer:CVE-2021-43798
```

qn2
```
86.5.206.121,95.181.232.32,195.80.150.137,
89.247.167.247
86.5.206.121 (t=2022-11-22T10:25:53+0000 lvl=info msg="Request Completed" logger=context userId=0 orgId=0 uname= method=GET path=/public/plugins/alertlist/../../../../../../../../etc/passwd status=200 remote_addr=86.5.206.121 time_ms=0 size=1609 refere
)



answer: 44.204.18.94,95.181.232.32,195.80.150.137
answer:/opt/automation/updater.sh
answer:wget
answer:/opt/automation/
```
