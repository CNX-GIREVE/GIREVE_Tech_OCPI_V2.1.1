### [<- Back to module selection](emsp_edits.md)

# Contents 

* [4.1 Use Cases Covered By IOP](#41-use-cases-covered-by-iop)
  - 4.1.3 Roaming
* [4.2 Use Cases Required By GIREVE](#42-use-cases-required-by-gireve)
  - 4.2.4 If the eMSP implements the “Sessions” feature
* [4.7 Sessions module specifications](#47-sessions-module-specifications)
  - 4.7.1 Usage of “authorization_id”

***


## 4.1 Use cases covered by IOP 

OCPI features are composed by several use cases that an eMSP can choose to implement or not when connecting to an operator. In case of connection to GIREVE, here is the list of use cases that a CPO can implement :

### 4.1.3 Roaming

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Push Sessions | FromIOP | IOP requests an eMSP to initialise or update an OCPI Session object following an authorised Authorisation. |
| Pull Sessions | ToIOP | An eMSP requests IOP to get Sessions that belong to it. |


## 4.2 Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE :

### 4.2.4 If the eMSP implements the “Sessions” feature

| Use case |  Why ? | 
| ----------- | ----------- |
| Push Session/FromIOP OR Pull Session/ToIOP | Required to be alerted about charging-sessions status changes. | 

## 4.7 Sessions module specifications

IOP follows the OCPI standard for Sessions sent by a CPO. [*See OCPI specifications*](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_sessions.md)*.*

### 4.7.1 Usage of “authorization_id”

If an “authorization_id” has been provided by the eMSP during the Authorisation, this information will be defined in all Sessions PUT/PATCH requests sent to the eMSP. It allows eMSPs to link the Session with its previous Authorisation.

Please refer to paragraph *2.4.2 New attribute « authorization_id »*.
