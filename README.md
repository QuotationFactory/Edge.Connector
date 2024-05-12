# QF.Agent.Service

## Windows service command's

Install windows service

STEP 1
```Commandbox (run as Administrator!)
# install service
sc.exe create QF.Live.Agent start= auto binpath= "[path to]\QF.Agent.Host.exe" displayname= "QF Live connector"
```
STEP 2
```Commandbox (run as Administrator!)
# configure service
sc.exe failure QF.Live.Agent reset= 86400 actions= restart/1000/restart/1000/restart/3600
```
# add dependency between services

## Dependency on one other service:
```Commandbox (run as Administrator!)
sc.exe config ServiceA depend= ServiceB
```
Above means that ServiceA will not start until ServiceB has started. If you stop ServiceB, ServiceA will stop automatically.
### example
```Commandbox (run as Administrator!)
sc.exe config Beltar.Rhodium24.RidderiQ.Integration depend= QF.Live.Agent
```
## Dependency on multiple other services:
```Commandbox (run as Administrator!)
sc.exe config ServiceA depend= ServiceB/ServiceC/ServiceD/"Service Name With Spaces"
```
Above means that ServiceA will not start until ServiceB, ServiceC, and ServiceD have all started. If you stop any of ServiceB, ServiceC, or ServiceD, ServiceA will stop automatically.

## To remove all dependencies:
```Commandbox (run as Administrator!)
sc.exe config ServiceA depend= /
```
## To list current dependencies:
```Commandbox (run as Administrator!)
sc.exe qc ServiceA
```
# Quotation Factory Agent (QF Agent)

## Overview

The Quotation Factory Agent, or QF Agent, is an integral component of the Quotation Factory ecosystem, designed for the metalworking industry. This Windows-based .NET Core application serves as a connector, facilitating robust and secure communication between local network and on-premise systems of metalworking companies and the Quotation Factory cloud platform through Azure IoT technology.

## Key Features and Capabilities

### Integration and Configuration

- **Setup**: Metalworking companies can easily add the QF Agent through the Quotation Factory cloud platform's settings page under 'Integrations => QF Agent'.
- **Agent ID**: Each added QF Agent is assigned a unique Agent ID (an Azure IoT device ID) which is crucial for its configuration and operation within the network.

### Dual Role as Azure IoT Client

- **From Cloud to Agent**: Receives commands and files, facilitating updates and instructions from the cloud.
- **From Agent to Cloud**: Sends commands and files back to the cloud to keep it updated with the latest operational statuses.

### Integration with ERP and CAM Systems

- **ERP System Integration**:
  - Automates and synchronizes tasks like creation and updating of Bill of Materials and Production Routes.
  - Manages estimating metal usage, production times, and synchronizes details when RfQs, Quotations, and Sales Orders are created.
  - Sends real-time production status back to the cloud for updates on internal KANBAN boards and customer self-service portals.

- **CAM System Integration**:
  - Automates the workflow of sending CAD files to CAM systems.
  - Handles automatic setting of quantities and material specifications.
  - Performs manufacturability checks and sends back CNC programs or manufacturability issues in various formats.

### MQTT Protocol Integration

- **Advanced Communication**: The beta version includes a built-in MQTT host/server and acts as an MQTT client, enhancing communication flexibility with standardized events and topics.

## Example Scenarios

### Scenario 1: Creating a Bill of Materials

When a new Request for Quotation (RfQ) is initiated, the QF Agent automatically generates a detailed Bill of Materials in the linked ERP system based on the data received from the cloud platform.

### Scenario 2: Syncing with Sales Orders

Upon creation of sales orders on the cloud platform, the QF Agent syncs this information with the ERP system, updating crucial details like customer and article information.

### Scenario 3: Updating Production Status

As production progresses, the QF Agent sends real-time updates back to the cloud platform, ensuring that the KANBAN board within the metalworking company and customer self-service portal are updated in real-time.

## Conclusion

The QF Agent is a vital bridge between on-premise systems and cloud operations, optimizing production processes and enhancing customer interactions through real-time data flows and integrated communication with ERP and CAM systems.

