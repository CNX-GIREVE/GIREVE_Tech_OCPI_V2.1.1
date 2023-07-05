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

## 3 Tokens module specifications

IOP follows the OCPI standard for Tokens module. [See OCPI specifications](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_tokens.md).

### 3.1 GET Tokens To/FromIOP

IOP is not able to PUSH Tokens to CPO backend. CPO should PULL IOP to get Tokens.

### 3.2 PULL Tokens: Who is the eMSP ?

The eMSP of a Token is not a direct property of the Token object. So, a CPO pulling IOP to get Tokens does not know to which eMSP the Token refers to. That is why IOP replaces the property *“Token.issuer”* by the eMI3 Id of the eMSP when a CPO get a Token. Using this property, the CPO is able to know who is the eMSP of the Token.

### 3.3 PULL Tokens by uid: Retrieve a unique Token

IOP adds a new OCPI feature enabling a CPO to retrieve the full description of a Token through the Tokens.uid :

| Property |  Content ? |
| ----------- | ----------- |
| WS Name | ToIOP_GET_emsp_Tokens-unitary |
| HTTP Verb | GET |
| URL | `{GIREVE_URL}/ocpi/emsp/2.1.1/tokens/{uid}?type={tokenType}` **uid** : Mandatory **tokenType** : Optional (default value: RFID) |
| Request's body | Empty |
| Response's body | Standard OCPI response including the Token description in data field. In case of an unknown Token or Token not visible by the CPO, the status_code “2000” is returned |

If the CPO is allowed to get Tokens of the eMSP owner, the response includes the full description of the Token.

This new flow prevents CPOs to download all Tokens of all eMSPs. For more description, see [? Custom OCPI flow to prevent eMSP Tokens download by CPOs]().

### 3.4 PULL Tokens: Retrieve Tokens of a single given eMSP

The standard OCPI 2.1.1 Tokens pulling allows CPOs to get Tokens of all eMSPs in contract with them.
In some cases, CPOs need only Tokens of a specific given eMSP. For example, when the CPO initializes data of an eMSP after signature of a new roaming agreement.
GIREVE provides a new OCPI 2.1.1 feature by allowing the CPO to get Tokens of a unique eMSP by filling two dedicated OCPI headers in their “GET Tokens” request to GIREVE :

- ocpi-to-country-code: The country code of the targeted eMSP.
- ocpi-to-party-id: The party id of the targeted eMSP.
  
Therefore, CPOs can request GIREVE without these headers to get Tokens of all eMSPs or including these headers to get Tokens of a unique eMSP.
For information, these headers have been included in the version 2.2 of the OCPI standard.

### 3.5 POST Authorize request: LocationReferences mandatory

In OCPI, the body of a POST Authorize request can contain a LocationReferences object.
**For IOP, the LocationReferences object is mandatory and must contain one and only one EVSE.**

If « LocationReferences » object contains 0 EVSE, IOP responds with a 2002 OCPI error (« Missing EVSE Id »).
If « LocationReferences » object contains more than 1 EVSE, IOP responds with a 2001 OCPI error (« Invalid or missing parameters: IOP does not support authorization request on multiple EVSE »).

### 3.6 POST Authorize request: new attribute “authorization_id”

IOP answers to a POST Token Authorize request with a new attribute of AuthorizationInfo object, the “authorization_id”. Please refer to paragraph 5 [New attribute « authorization_id »](checkup_edits.md).

**The CPO must store this information to send it in Sessions and CDRs related to this Authorization.**

## 4 Exemples

### 4.1 ToIOP_GET_emsp_tokens_2.1.1, FromIOP_GET_emsp_tokens_2.1.1

*URL* :

`/2.1.1/tokens?date_from=2020-01-13T12%3A54%3A25&offset=0&limit=50`
*(Date_from parameter shall be the date of the last successful request)*

*Request* :

- **VERB : GET**
- **HEADERS** : `{Authorization:Token xxx-xxx-xxx}{Connection:close}{Accept:application/json}`
- **BODY** :

*Response* :

- **CODE** : 200
- **HEADERS** : `{Link:<https://xxx.nnn.com/ocpi/emsp/2.1.1/tokens?date_from=2020-01-13T12:54:25Z&offset=50&limit=50>;rel="next"}{X-Limit:50}{Content-Type:application/json}{X-Total-Count:10627}`
- **BODY** :  
```json

  {
"data": [
        {
          "uid": "aaa",
          "valid": true,
          "issuer": "FR*AAA",
          "auth_id": "AZERTY",
          "last_updated": "2020-01-13T12:55:01Z",
          "language": "FR",
          "type": "OTHER",
          "whitelist": "ALLOWED_OFFLINE"
},
{
          "uid": "bbb",
          "valid": true,
          "issuer": "FR*BBB",
          "auth_id": "QSDFG",
          "last_updated": "2020-01-13T13:01:53Z",
          "type": "RFID",
          "whitelist": "NEVER",
          "visual_number": "AZSCV"
}
],
"status_code": 1000,
"status_message": "Success",
"timestamp": "2020-01-17T09:54:25Z"
}

```
