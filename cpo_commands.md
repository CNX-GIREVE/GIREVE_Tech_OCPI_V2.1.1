### [<- Back to module selection](cpo_edits.md)


# Contents

* [Use Cases Covered By IOP](#use-cases-covered-by-iop)
  - Roaming                                                            
* [Use Cases Required By Gireve](#use-cases-required-by-gireve)
  - If the CPO implements the "Roaming" feature 
* [Commands module specifications](#commands-module-specifications)
  - StartSession request: new attribute « authorization_id »
  - ReserveNow command
  - UnlockConnector command

***

## `Use cases covered by IOP`

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

The Commands module enables remote commands to be sent to a Location/EVSE.
The following commands are supported : 
- `START_SESSION`
- `STOP_SESSION`

In case of connection to Gireve, here is the list of use cases that a CPO can implement :

### Remote authorisation

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| **Start Session** | FromIOP | IOP, triggered by a remote start received from an eMSP, requests a CPO to start remotely the charging-session. |
| **Stop Session** | FromIOP | IOP, triggered by a remote stop received from an eMSP, requests a CPO to stop remotely the charging-session. This use case can happen even for charging-session beginning by an authorisation per RFID badge. |

## `Use cases required by Gireve`

Some of these use cases are required when connecting to Gireve :

### If the CPO implements the “Roaming” feature

| Use case |  Why ? | 
| ----------- | ----------- |
| **Start Session/FromIOP** | Remote authorisation and start features on CPO infrastructure are required by Gireve. | 
| **Stop Session/FromIOP** | Remote stop features on CPO infrastructure are required by Gireve. |.

## `Commands module specifications`

IOP follows the OCPI standard for Commands received by a CPO. [See OCPI specifications](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_commands.md).

### StartSession request: new attribute « authorization_id »

> :warning: **<ins>IOP sends a StartSession command to a CPO with a new attribute, the “authorization_id”.</ins>** Please refer to paragraph 5. [New attribute « authorization_id »](checkup_edits.md).

> :memo: **<ins>The CPO must store this information to send it in Sessions and CDRs related to this Authorization.</ins>**

### ReserveNow command

The ReserveNow command is not yet implemented by IOP.

### UnlockConnector command

The UnlockConnector is not yet implemented by IOP.
