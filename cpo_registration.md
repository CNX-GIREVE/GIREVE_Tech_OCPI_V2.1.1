## Use cases covered by IOP

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

In case of connection to GIREVE, here is the list of use cases that a CPO can implement :


## 1. Technical Use cases


| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Register | FromIOP | IOP initialises the connection to a CPO beginning by a GET_Versions, then exchanging endpoints and Authorization-token. |
| Register | ToIOP | A CPO initialises the connection to IOP beginning by a GET_Versions, then exchanging endpoints and Authorization-token. |
| Update CPO credentials | ToIOP | A CPO already registered requests IOP to update its Credentials (endPoints, versions, authorization-token). |
| Unregister | FromIOP | IOP unregisters a CPO by requesting a DELETE Credentials. |
| Unregister | ToIOP | A CPO unregisters IOP by requesting a DELETE Credentials. |

**If a CPO manages several operations, each operation must go through the OCPI connection process (handshake).**


## 2. Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE.

### 2.1 Always required

| Use case |  Why ? | 
| ----------- | ----------- |
| Register/FromIOP OR Register/ToIOP |  These use cases are needed to initialise connection between an operator and GIREVE. | 
| Push EVSE status/ToIOP | A CPO connected to GIREVE must transfer “in realtime” EVSE status change of its EVSEs.|
| Pull Locations/FromIOP | GIREVE wants to be able to refresh EVCI data when needed. |
  
### 2.2 Connection & Register specifications

IOP follows the OCPI standard for Connection & Register process. [See OCPI specifications](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/credentials.md).

### 2.2.1 Update Credentials FromIOP

IOP is not able to send a PUT Credentials to update its Credentials on operator backend. In order to update them, IOP sends a “DELETE Credentials” to delete the OCPI connection then starts a new Connection & Register process beginning by a POST Credentials sent by IOP or by the operator.


