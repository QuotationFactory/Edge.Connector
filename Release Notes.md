# QF.Agent.Service release notes

## Update 22-04-2024
Implements KeepAlive and Waiting for response loop.
When response not received in max 5 minutes, connector will restart and try to re-connect.

## Update 05-04-2024
NEEDS update of appsettings.json
Implemented improved logging implementation (Application Insights)

## Update 27-03-2024
Improved re-connection to the cloud infrastructure
Upgrade from .net6 to .net8 incl. dependency packages
