## Use cases covered by IOP

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

In case of connection to GIREVE, here is the list of use cases that a CPO can implement :

## 1. Roaming

### 1.1 Tokens

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Push Tokens | ToIOP | A CPO requests IOP to get Tokens of eMSPs, it is allowed to get (ie, depending on roaming agreements). |
| Pull Tokens (of a single eMSP) | ToIOP | A CPO requests IOP to get Tokens of a single given eMSP which is identified by “OCPI-TO” headers. |
| Pull Tokens by uid | ToIOP | A CPO requests IOP to get single Token which is identified by the user UID. |

### 1.2 Local authorization

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Realtime Authorisation | ToIOP | A CPO requests IOP to get Authorisation triggered by a local authentication (swiping of an RFID badge by an EV driver on one of its EVSE, for example). |

## 2. Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE :

### 2.1 If the CPO implements the “Roaming” feature

| Use case |  Why ? | 
| ----------- | ----------- |
| Realtime Authorisation/ToIOP | A CPO should be able to request eMSP through IOP when a driver uses his RFID badge to charge. | 
| Pull Tokens/ToIOP | Pull Tokens is the only way open for a CPO to download Tokens of eMSP it is in contract with. This feature is “global” (CPO downloads all the tokens of all eMSP it is in contract with), or “unitary” (CPO downloads all the tokens of a single given eMSP). |

