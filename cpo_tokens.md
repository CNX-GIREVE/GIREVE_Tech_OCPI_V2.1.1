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

## 2. Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE :

### 2.1 If the CPO implements the “Roaming” feature



