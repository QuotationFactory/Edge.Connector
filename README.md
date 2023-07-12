# QF.Agent.Service

## Windows service command's

Install windows service

```Commandbox (run as Administrator!)
# install service
sc.exe create QF.Live.Agent start= auto binpath= "[path to]\QF.Agent.exe" displayname= "QF Agent"
```