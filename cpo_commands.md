# [<- Back to module selection](cpo_edits.md)

# Contents

* [3.3 Use Cases Covered By IOP](#33-use-cases-covered-by-iop)
  - 3.3.3 Roaming                                                            
* [3.4 Use Cases Required By GIREVE](#34-use-cases-required-by-gireve)
  - 3.4.2 If the CPO implements the "Roaming" feature 
* [3.8 Commands module specifications](#38-commands-module-specifications)
  - 3.8.1 StartSession request: new attribute « authorization_id »
  - 3.8.2 ReserveNow command
  - 3.8.3 UnlockConnector command

***

## 3.3 Use cases covered by IOP

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

The Commands module enables remote commands to be sent to a Location/EVSE.
The following commands are supported : 
- `START_SESSION`
- `STOP_SESSION`

In case of connection to GIREVE, here is the list of use cases that a CPO can implement :

### 3.3.3 Remote authorisation

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| **Start** Session | FromIOP | IOP, triggered by a remote start received from an eMSP, requests a CPO to start remotely the charging-session. |
| **Stop** Session | FromIOP | IOP, triggered by a remote stop received from an eMSP, requests a CPO to stop remotely the charging-session. This use case can happen even for charging-session beginning by an authorisation per RFID badge. |

## 3.4 Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE :

### 3.4.2 If the CPO implements the “Roaming” feature

| Use case |  Why ? | 
| ----------- | ----------- |
| Start Session/FromIOP | Remote authorisation and start features on CPO infrastructure are required by GIREVE. | 
| Stop Session/FromIOP | Remote stop features on CPO infrastructure are required by GIREVE. |.

## 3.8 Commands module specifications

IOP follows the OCPI standard for Commands received by a CPO. [See OCPI specifications](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_commands.md).

### 3.8.1 StartSession request: new attribute « authorization_id »

IOP sends a StartSession command to a CPO with a new attribute, the “authorization_id”. Please refer to paragraph 5. [New attribute « authorization_id »](checkup_edits.md).

The CPO must store this information to send it in Sessions and CDRs related to this Authorization.

### 3.8.2 ReserveNow command

The ReserveNow command is not yet implemented by IOP.

### 3.8.3 UnlockConnector command

The UnlockConnector is not yet implemented by IOP.
