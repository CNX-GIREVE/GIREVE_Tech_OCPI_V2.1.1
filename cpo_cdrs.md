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

## 3 CDRs module specifications

IOP follows the OCPI standard for Sessions sent by a CPO. [See OCPI specifications](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_cdrs.md).

### 3.1 CDR sending frequency

**GIREVE requires CDRs to be sent directly at the end of the charging-session.**
eMSPs will so be able to display the charging-session price directly to their end customers.

### 3.2 Usage of “authorization_id”

When sending a CDR, the CPO defines the Authorization it refers to on providing the “authorization_id” property. Please refer to [New attribute « authorization_id »](checkup_edits.md).

### 3.3 CDR content

The “total_time” value is the total duration of this session (including the duration of charging and not charging).
It doesn’t include the duration during which the EVSE is out of order so cannot supply the service. 
The out of order duration should be free of charge for eMSPs.

### 3.4 Store and forward – POST CDRs

Similarly to a PUT sessions, Store and Forward mechanism must be implemented to ensure that no CDR may be lost, in case of a connection loss. 
Any POST Cdrs that didn’t get a correct response from the GIREVE platform IOP (timeout, http code 500) must be stored on CPO side and a retry process must be active. After the connection recovery, the Cdr messages must be resent in a FIFO manner.

### 3.5 Send the signed data (Calibration Law / Eichrecht)

GIREVE has extended OCPI 2.1.1 for CPOs to send signed data in CDRs, aiming to be compliant with the German calibration law (Eichrecht).
These signed data are included in CDRs following the [OCPI 2.2 specifications](https://github.com/ocpi/ocpi/blob/master/mod_cdrs.asciidoc#mod_cdrs_signed_data_class).

*To activate this feature, the CPO must validate its implementation through the standard GIREVE certification process before the deployment and activation on PRODUCTION. (Please contact connection@gireve.com).*
