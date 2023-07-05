## Use cases covered by IOP

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

The Commands module enables remote commands to be sent to a Location/EVSE.
The following commands are supported : 
- `START_SESSION`
- `STOP_SESSION`

In case of connection to GIREVE, here is the list of use cases that a CPO can implement :

## 1. Remote authorisation

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| **Start** Session | FromIOP | IOP, triggered by a remote start received from an eMSP, requests a CPO to start remotely the charging-session. |
| **Stop** Session | FromIOP | IOP, triggered by a remote stop received from an eMSP, requests a CPO to stop remotely the charging-session. This use case can happen even for charging-session beginning by an authorisation per RFID badge. |

## 2. Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE :

### 2.1 Always required

| Use case |  Why ? | 
| ----------- | ----------- |
| Push EVSE status/ToIOP | A CPO connected to GIREVE must transfer “in realtime” EVSE status change of its EVSEs. | 
| Pull Locations/FromIOP | GIREVE wants to be able to refresh EVCI data when needed. |
