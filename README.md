# QF.Agent.Service

## Windows service command's

Install windows service

```Commandbox (run as Administrator!)
# install service
sc.exe create QF.Live.Agent start= auto binpath= "[path to]\QF.Agent.Host.exe" displayname= "QF Live connector"
sc.exe failure QF.Live.Agent reset= 86400 actions= restart/1000/restart/1000/restart/3600
```
# add dependency between services

## Dependency on one other service:
sc config ServiceA depend= ServiceB
Above means that ServiceA will not start until ServiceB has started. If you stop ServiceB, ServiceA will stop automatically.
### example
sc config Beltar.Rhodium24.RidderiQ.Integration depend= QF.Live.Agent

## Dependency on multiple other services:
sc config ServiceA depend= ServiceB/ServiceC/ServiceD/"Service Name With Spaces"
Above means that ServiceA will not start until ServiceB, ServiceC, and ServiceD have all started. If you stop any of ServiceB, ServiceC, or ServiceD, ServiceA will stop automatically.

## To remove all dependencies:
sc config ServiceA depend= /

## To list current dependencies:
sc qc ServiceA
