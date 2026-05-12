### [<- Back to module selection](emsp_edits.md)

# Contents 

* [Use Cases Covered By IOP](#use-cases-covered-by-iop)
  - Roaming
* [Use Cases Required By Gireve](#use-cases-required-by-gireve)
  - If the eMSP implements the “Sessions” feature
* [Sessions module specifications](#sessions-module-specifications)
  - Usage of “authorization_id”

***


## `Use cases covered by IOP` 

OCPI features are composed by several use cases that an eMSP can choose to implement or not when connecting to an operator. In case of connection to Gireve, here is the list of use cases that a CPO can implement :

### Roaming

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| **Push Sessions** | FromIOP | IOP requests an eMSP to initialise or update an OCPI Session object following an authorised Authorisation. |
| **Pull Sessions** | ToIOP | An eMSP requests IOP to get Sessions that belong to it. |


## `Use cases required by Gireve`

Some of these use cases are required when connecting to Gireve :

### If the eMSP implements the “Sessions” feature

| Use case |  Why ? | 
| ----------- | ----------- |
| **Push Session/FromIOP OR Pull Session/ToIOP** | Required to be alerted about charging-sessions status changes. | 

## `Sessions module specifications`

IOP follows the OCPI standard for Sessions sent by a CPO. [*See OCPI specifications*](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_sessions.md)*.*

### Usage of “authorization_id”

If an “authorization_id” has been provided by the eMSP during the Authorisation, this information will be defined in all Sessions PUT/PATCH requests sent to the eMSP. It allows eMSPs to link the Session with its previous Authorisation.

Please refer to [New attribute « authorization_id »](integration_guidelines.md).

### PULL Sessions ToIOP: Get List Pagination

If the eMSP wants to retrieve a list of Sessions, it can call the URL: /ocpi/cpo/2.1.1/sessions?date_from= using the paginated properties date_from, date_to, offset and limit.

Parameters « offset » and « limit » are optional but IOP always returns a paginated response (subset of objects list and link, X-Total-Count and X-limit headers).
The eMSP must call the link returned in the headers to get the next pages. 

IOP has its own max limit (20 Sessions) and answers with its if the client limit is upper than IOP one or the client doesn’t set its limit.

The timeframe (‘date_from’ and ‘date_to’) used in the request parameters must be lower than one month.
