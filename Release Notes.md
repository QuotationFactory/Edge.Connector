# QF.Agent.Service release notes

## Update 07-05-2024
* Agent Build version is send to backend to make it visible in the agent settings.


## Update 23-04-2024
* updates versioned datacontract to version 0.3.51
* Updates nuget packages to latest versions


## Update 22-04-2024
* Implements KeepAlive and Waiting for response loop.
    When response not received in max 5 minutes, connector will restart and try to re-connect.
* updates versioned datacontract to version 0.3.45

## Update 05-04-2024
NEEDS update of appsettings.json
Implemented improved logging implementation (Application Insights)

## Update 27-03-2024
Improved re-connection to the cloud infrastructure
Upgrade from .net6 to .net8 incl. dependency packages
