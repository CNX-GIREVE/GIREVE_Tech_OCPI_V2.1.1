## Use cases covered by IOP

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

In case of connection to GIREVE, here is the list of use cases that a CPO can implement :


## 1. Technical Use cases


| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Register | FromIOP | IOP initialises the connection to a CPO beginning by a GET_Versions, then exchanging endpoints and Authorization-token. |
| Register | ToIOP | A CPO initialises the connection to IOP beginning by a GET_Versions, then exchanging endpoints and Authorization-token. |
| Update CPO credentials | ToIOP | A CPO already registered requests IOP to update its Credentials (endPoints, versions, authorization-token). |
| Unregister | FromIOP | IOP unregisters a CPO by requesting a DELETE Credentials. |
| Unregister | ToIOP | A CPO unregisters IOP by requesting a DELETE Credentials. |

**If a CPO manages several operations, each operation must go through the OCPI connection process (handshake).**


## 2. Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE :

### 2.1 Always required

| Use case |  Why ? | 
| ----------- | ----------- |
| Register/FromIOP OR Register/ToIOP |  These use cases are needed to initialise connection between an operator and GIREVE. | 

### 2.2 Connection & Register specifications

IOP follows the OCPI standard for Connection & Register process. [See OCPI specifications](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/credentials.md).

### 2.2.1 Update Credentials FromIOP

IOP is not able to send a **PUT** Credentials to update its Credentials on operator backend. In order to update them, IOP sends a “**DELETE** Credentials” to delete the OCPI connection then starts a new Connection & Register process beginning by a **POST** Credentials sent by IOP or by the operator.

# 3. Exemples 

# 3.1 ToIOP_GET_emsp_versions, FromIOP_GET_emsp_versions

*Request* :

- **VERB : GET**
- **HEADERS** : `{Transfer-Encoding:chunked}{Date:Wed, 08 Jan 2020 10:24:52 GMT}{Connection:Close}{Content-Type:application/json}`
- **BODY** : EMPTY

*Response* :

- **CODE** : 200
- **HEADERS** : `{Transfer-Encoding:chunked}{Date:Wed, 08 Jan 2020 10:24:52 GMT}{Connection:Close}{Content-Type:application/json}`
- **BODY** :  
```json
{
    "data": [
        {
            "version": "2.1.1",
            "url": "https://xxx.yyy.com/ocpi/emsp/2.1.1"
        }
    ],
    "status_code": 1000,
    "status_message": "Success",
    "timestamp": "2020-01-08T10:24:52Z"
}

```

### 3.2 ToIOP_GET_emsp_version_detail_2.1.1, FromIOP_GET_emsp_version_detail_2.1.1

*Request* :

- **VERB : GET**
- **HEADERS** : `{connection:close}{authorization:Token xxx-xxx-xxx}`
- **BODY** : EMPTY

*Response* :

- **CODE** : 200
- **HEADERS** : `{Date:Thu, 16 Jan 2020 07:30:16 GMT}{Connection:close}{Content-Type:application/json}`
- **BODY** :
```json

{
    "data": {
        "endpoints": [
            {
                "identifier": "credentials",
                "url": "https:// ccc.yyy.com/ocpi/emsp/2.1.1/credentials"
            },
            {
                "identifier": "locations",
                "url": "https:// ccc.yyy.com/ocpi/emsp/2.1.1/locations"
            },
            {
                "identifier": "tokens",
                "url": "https:// ccc.yyy.com/ocpi/emsp/2.1.1/tokens"
            },
            {
                "identifier": "commands",
                "url": "https:// ccc.yyy.com/ocpi/emsp/2.1.1/commands"
            },
            {
                "identifier": "sessions",
                "url": "https:// ccc.yyy.com/ocpi/emsp/2.1.1/sessions"
            },
            {
                "identifier": "cdrs",
                "url": "https:// ccc.yyy.com/ocpi/emsp/2.1.1/cdrs"
            }
        ],
        "version": "2.1.1"
    },
    "status_code": 1000,
    "status_message": "Success",
    "timestamp": "2020-01-08T10:24:52Z"
}

```
