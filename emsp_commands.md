### [<- Back to module selection](emsp_edits.md)

# Contents 

* [Use Cases Covered By IOP](#use-cases-covered-by-iop)
  - Roaming
* [Use Cases Required By Gireve](#use-cases-required-by-gireve)
  - If the eMSP implements the “Sessions” feature
* [Commands module specifications](#commands-module-specifications)
  - StartSession request: new attribute « authorization_id »
  -  ReserveNow command	
  - UnlockConnector command
  - “evse_uid” mandatory in StartSession command
* [Examples](#examples)
  - ToIOP_PUT_cpo_tokens_2.1.1
  - ToIOP_PATCH_cpo_tokens_2.1.1  


***


## `Use cases covered by IOP`

OCPI features are composed by several use cases that an eMSP can choose to implement or not when connecting to an operator. In case of connection to Gireve, here is the list of use cases that a CPO can implement :


### Roaming

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| **Start Session** | ToIOP | An eMSP requests IOP to start a charging-session remotely on a CPO’s EVSE. |
| **Stop Session** | ToIOP | An eMSP requests IOP to stop remotely a charging-session.
This use case can happen even if the charging-session beginning with an authorisation per RFID badge. |

## `Use cases required by Gireve`

Some of these use cases are required when connecting to Gireve :

### If the eMSP implements the “remote authorisation” feature

| Use case |  Why ? | 
| ----------- | ----------- |
| **Start Session/toIOP** | Required to start a charging-session remotely. | 
| **StopSession/ToIOP** | Required to stop a charging-session remotely | 

## `Commands module specifications`

IOP follows the OCPI standard for Commands received by a CPO. See OCPI specifications.

### StartSession request: new attribute « authorization_id »

> :bulb: When an eMSP sends a StartSession command to a CPO, **<ins>Gireve highly recommends eMSPs to use the new attribute “authorization_id”.</ins>** Please refer to [New attribute « authorization_id »](integration_guidelines.md).

### ReserveNow command

The ReserveNow command is **<ins>not yet implemented</ins>** by IOP.

### UnlockConnector command

The UnlockConnector is **<ins>not yet implemented</ins>** by IOP. 

### “evse_uid” mandatory in StartSession command.

In OCPI 2.1.1 standard, the “evse_uid” property is optional for StartSession command.
IOP requires it to do the mapping with the eMIP protocol.


## `Examples`

### ToIOP_POST_cpo_commands_2.1.1, FromIOP_POST_cpo_commands_2.1.1

*URL :*

`/ocpi/cpo/2.1.1/commands/START_SESSION`


*Request :*

- **VERB: POST**
- **HEADERS :** {Authorization:Token xxx-xxx-xxx}{Connection:close}{Accept:application/json}{Content-Type:application/json}
- **BODY :**

```json
{
    "response_url": "https://xxx.ggg.com/ocpi/emsp/2.1.1/commands/START_SESSION/111-222",
    "token": {
        "uid": "11111",
        "type": "OTHER",
        "auth_id": "22222",
        "visual_number": "33333",
        "issuer": "issuer",
        "valid": true,
        "last_updated": "2020-01-21T06:43:13Z",
        "whitelist": "ALWAYS"
    },
    "authorization_id": "aaa-vvv",
    "location_id": "12345",
    "evse_uid": "54321"
}
```

*Response :*


- **CODE : 200**
- **HEADERS :** {Content-Type:application/json}
- **BODY :**

```json
{
    "data": {
        "result": "ACCEPTED"
    },
    "status_code": 1000,
    "status_message": "Success",
    "timestamp": "2020-01-21T08:09:31Z"
}
```

## Callback ToIOP_POST_cpo_commands_2.1.1, FromIOP_POST_cpo_commands_2.1.1

*URL:*

`/ocpi/emsp/2.1.1/commands/START_SESSION/aaaa-fffff-ggggg`


*Request :*

- **VERB : POST**
- **HEADERS :**{Authorization:Token xxx-xxx-xxx}{Connection:close}{Accept:application/json}{Content-Type:application/json}
- **BODY :**

```json
{
    "result": "ACCEPTED"
}
```

*Response :*

- **CODE : 200**
- **HEADERS :** {Content-Type:application/json}
- **BODY :**

```json
{
    "data": {},
    "status_code": 1000,
    "status_message": "Success",
    "timestamp": "2020-01-06T14:51:38Z"
}
```
