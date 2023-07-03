## Use cases covered by IOP

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

In case of connection to GIREVE, here is the list of use cases that a CPO can implement :

## 1. Technical Use cases

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Push Session | ToIOP | A CPO requests IOP to initialise or update an OCPI Session object following an positive Authorisation. |
| Check Session | ToIOP | A CPO requests IOP to get Sessions it has previously initialised. |

**If a CPO manages several operations, each operation must go through the OCPI connection process (handshake).**

## 2. Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE :

### 2.1 If the CPO implements the “Roaming” feature

| Use case | Why ? |
| ----------- | ----------- |
| **START** Session FromIOP | Remote authorisation and start features on CPO infrastructure are required by GIREVE. |
| **STOP** Session FromIOP | Remote stop features on CPO infrastructure are required by GIREVE. |
| **PUT** Session FromIOP | A CPO must be able to send information about charging-sessions through Session objects (charge started, …). |

## 3. Sessions module specifications

IOP follows the OCPI standard for Sessions sent by a CPO. [See OCPI specifications](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_sessions.md).

### 3.1 Session Initialisation

The CPO must send, after a remote or local authorization validated by the eMSP, a PUT Session with SessionStatus when the EV plugs to the EVSE.
This flow gives information to eMSP that the charge of its customer has really started.

### 3.2 Usage of “authorization_id”

When sending a Session, the CPO defines the Authorization it refers to on providing the “authorization_id” property. Please refer to paragraph 2.4.2 [New attribute « authorization_id »](checkup_edits.md).

### 3.3 Store and forward – PUT Sessions

A Store and Forward mechanism must be implemented to ensure that no session may be lost, in case of a connection loss. Any PUT session that didn’t get a correct response from the GIREVE platform IOP (timeout, http code 500) must be stored on CPO side and a retry process must be active. After the connection recovery, the session messages must be resent in a FIFO manner.
