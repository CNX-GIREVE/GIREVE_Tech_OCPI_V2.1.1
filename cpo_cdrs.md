## Use cases covered by IOP

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

In case of connection to GIREVE, here is the list of use cases that a CPO can implement :

## 1. Technical Use cases

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Push CDR | ToIOP | A CPO sends to IOP, a CDR related to a terminated charging-session. |
| Pull CDR | FromIOP | IOP requests a CPO to retrieve CDRs that belongs to it. |
| Check CDR | ToIOP | A CPO requests IOP to get CDRs it has previously sent. |

## 2. Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE :

### 2.1 If the CPO implements the “Roaming” feature

| Use case | Usage |
| ----------- | ----------- |
| Push CDR/ToIOP | The CPO must send the CDR directly after the end of the charging-session. |
| Pull CDR/FromIOP | GIREVE must be able to retrieve CDRs from CPO platform when needed. |

