### [<- Back to module selection](emsp_edits.md)

# Contents 

* [Use Cases Covered By IOP](#use-cases-covered-by-iop)
  - Roaming
* [Use Cases Required By Gireve](#use-cases-required-by-gireve)
  - If the eMSP implements the “Sessions” feature
* Tokens module specifications](#tokens-module-specifications)
  - Share Tokens liste
  - PULL Tokens FromIOP : Pagination and Periodicity 
  - POST Authorize request: new attribute « authorization_id »
* [Examples](#examples)
  - ToIOP_PUT_cpo_tokens_2.1.1
  - ToIOP_PATCH_cpo_tokens_2.1.1  


***


## `Use cases covered by IOP` 

OCPI features are composed by several use cases that an eMSP can choose to implement or not when connecting to an operator. In case of connection to Gireve, here is the list of use cases that a CPO can implement :


### Roaming

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| **Push Tokens** | ToIOP | An eMSP updates its Tokens stored in IOP. |
| **Pull Tokens** |  FromIOP | IOP requests an eMSP to retrieve eMSP tokens. |
| **Check Tokens** | ToIOP | An eMSP requests IOP to get its tokens stored in IOP.|
| **Realtime Authorisation** | FromIOP | IOP, triggered by a get authorisation request coming from a CPO, requests an eMSP to get the authorisation for a charging session |

## `Use cases required by Gireve`

Some of these use cases are required when connecting to Gireve :

### If the eMSP implements the “Roaming” feature

| Use case |  Why ? | 
| ----------- | ----------- |
| **Pull Tokens/FromIOP** | An eMSP has no possibility to send a single request to update a list of Tokens. The Pull Tokens feature allows IOP to retrieve all Tokens in a few numbers of requests instead of receiving many single updates. | 

## `Tokens module specifications`

IOP follows the OCPI standard for Tokens module. See OCPI specifications.

### Share Tokens list

OCPI requires eMSPs to transfer their tokens list to CPOs. The eMSP can choose between pulling and pushing its tokens, IOP supports both.

### PULL Tokens FromIOP: Pagination and periodicity

IOP is able to PULL Tokens requesting eMSP backend. In this case, IOP uses the pagination and eMSP must respond with a paginated response. If the response is not paginated, it will be ignored by IOP.
The default periodicity is every day.

### POST Authorize request: new attribute « authorization_id »

When an eMSP answers to a POST Token Authorize request, **<ins>Gireve highly recommends eMSPs to use the new attribute of AuthorizationInfo object, the “authorization_id”.</ins>**
Please refer to [New attribute « authorization_id »](integration_guidelines.md).
## `Examples`

### ToIOP_PUT_cpo_tokens_2.1.1

*URL*:

`/ocpi/cpo/2.1.1/tokens/FR/EMP/AAAAAAAA`
*(FR and EMP refer to the country_code and party_id of the eMSP which owns the Token cf. 2.1.3 Client owned object push)*

*Request*:

- **VERB: PUT**
- **HEADERS**: `{Authorization:Token xxx-xxx-xxx}{Connection:close}{Accept:application/json}{Content-Type:application/json}`
- **BODY**:
```json
{
	"uid": "AAAAAAAA",
	"type": "RFID",
	"auth_id": "111111",
	"visual_number": "22222",
	"issuer": " FR*AAA ",
	"valid": true,
	"whitelist": "ALWAYS",
	"language": "FR",
	"last_updated": "2020-01-21T10:42:30Z"
}
```

*Response*:

- **CODE**: 200
- **HEADERS**: `{Content-Type:application/json}`
- **BODY**:
```json
{
  "data": {},
  "status_code": 1000,
  "status_message": "Success",
  "timestamp": "2020-01-21T10:53:12Z"
}
```

### ToIOP_PATCH_cpo_tokens_2.1.1

*URL*:

`/ocpi/cpo/2.1.1/tokens/FR/EMP/AAAAAAAA`
*(FR and EMP refer to the country_code and party_id of the eMSP which owns the Token cf. 2.1.3 Client owned object push)*

*Request*:

- **VERB: PATCH**
- **HEADERS**: `{Authorization:Token xxx-xxx-xxx}{Connection:close}{Accept:application/json}{Content-Type:application/json}`
- **BODY**:
```json
{
  "visual_number": "2222222222",
  "last_updated": "2020-01-20T23:34:53Z"
}
```

*Response*:

- **CODE**: 200
- **HEADERS**: `{Content-Type:application/json}`
- **BODY**:
```json
{
  "data": {},
  "status_code": 1000,
  "status_message": "Success",
  "timestamp": "2020-01-21T10:53:12Z"
}
```
