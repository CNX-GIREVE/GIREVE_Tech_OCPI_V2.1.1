# Contents

* [Technical](#technical)
  - Supported OCPI versions
  - Security
  - Client owned object push
  - Pagination
  - IOP HTTP headers
* [OCPI modules implemented by IOP](#ocpi-modules-implemented-by-iop)
* [Gireve management of Locations data](#gireve-management-of-locations-data)
* [Roaming](#roaming)
  - General workflow
  - New attribute « authorization_id »
  - Management of B2B tariffs
  - RFID Tokens
  - Custom OCPI flow to prevent eMSP Tokens download by CPOs
 
*** 

# `Technical`

### Supported OCPI versions

IOP OCPI implementation has begun with version 2.1.1. IOP does not support previous versions.

### Security

IOP follows the security standard mechanisms of OCPI. At the connection, Gireve provides new operator connecting with temporary Token(s) to register as CPO and/or eMSP. The Token should be used when the connection is initiated by the operater.

In the meantime, the operator can provide Gireve with temporary Token(s) for IOP to initiate the connection. It is not mandatory as the operator is able to launch the Connection & Register process.

During the Connection & Register process, IOP and the operator exchange final Tokens to request each other, and endpoints of their respective OCPI modules. **<ins>Gireve requires all endpoints to be in HTTPS in order to secure communication between the two backends.</ins>**

### Client owned object push

OCPI introduces a specific use of resource identification mechanism, to manage situation where resource belongs to servers and situation where resource belongs to client. [See OCPI client owned object.](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/transport_and_format.md)

In OCPI, Objects managed through Rest protocol are owned by the CPO or the eMSP:

-   Tokens are owned by the eMSP
-   Locations are owned by the CPO
-   Sessions are owned by the CPO
-   CDRs are owned by the CPO
-   Tarifs are owned by the CPO

IOP acts as a hub which routes the messages between CPOs and eMSPs, so IOP does not own objects exchanged by these two actors during their communication.

**<ins>IOP is not the owner of the exchanged resources. Following this principle, IOP uses “country-code” and “partner-id” of the object owner during communication with other operators.</ins>**

For example, IOP pushes Locations of a CPO to an eMSP using “country-code/party-id” of the CPO in the URL.

![image](https://github.com/CNX-GIREVE/GIRVE_Tech_OCPI_V2.1.1/assets/137178502/4c52c925-a330-40a8-90f5-55f7fdf3e41a)

### Pagination

IOP implements pagination mechanisms described by OCPI. [*See OCPI pagination mechanism.*](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/transport_and_format.md#pagination)

IOP requires operators to use pagination when they pull resources requesting IOP. If the operator does not use pagination arguments in its request, IOP will force it answering with the first **X** items and a Link to the second page if any.

Finally, IOP for each module has its own max size limit per page (20 Locations, 1000 Tokens, …). These limits can change with IOP evolutions, the operator implementation might be flexible regarding these limits.

### IOP HTTP headers

The next version of OCPI, OCPI 2.2, integrates new extra headers enabling the sharing of a single OCPI connection to multiple operators.

Gireve has begun to deploy these extra headers in its OCPI version 2.1.1 but it is an ongoing action.

**<ins>These extra headers should not be considered yet in OCPI 2.1.1, except for “PULL Tokens: Retrieve Tokens of a single given eMSP” (see chapter 3.7.4 page 22) and “PULL Tokens: Retrieve Locations of a single given CPO” (see chapter 4.4.3 page 29)</ins>**

-   ocpi-to-country-code
-   ocpi-to-party-id
-   ocpi-from-country-code
-   ocpi-from-party-id

## `OCPI modules implemented by IOP`

All OCPI modules have been implemented in IOP.
List of modules implemented by IOP :

| Module | Usage |
| ----------- | ----------- |
| **Credentials** | Initialise and update IT connection information (endpoints, OCPI versions, …) |
| **Locations** | Exchange charge infrastructure information |
| **Tokens** | Manage Tokens and local authorizations |
| **Commands** | Start a charge remotely, Send commands during charge |
| **Sessions** | Exchange information during charge |
| **CDRs** | Send the final charge report |
| **Tariffs** | Exchange Tariffs information |

## `Gireve management of Locations data`

Gireve and its systems distinguish two natures of Location properties:

-   <ins>**Static data :**</ins> Locations properties which never or almost never change.
-   <ins>**Dynamic data :**</ins> Locations properties which can change frequently (e.g., EVSE status, Connector tariff_id)

All Location properties are considered as static data except for the status of the EVSE and the tariff_id attached to a connector.

Gireve performs a specific process to first integrate static data of CPO Locations in its charge point repository then to integrate static data change like change on a Locations of a CPO or new Locations or EVSEs. This process implies data quality tests and data completion of the CPO Locations.

This process is asynchronous from the standard connection of the CPO with the Gireve IOP platform, meaning that new Locations of the CPO or updates on them can be seen in the Gireve charge point repository several days after the first **PUSH** from the CPO to the Gireve IOP platform.

## `Roaming`

### General workflow

An OCPI Roaming session through IOP should run the following workflow :

#### Authorisation then initialise Session object

![image](https://github.com/CNX-GIREVE/GIRVE_Tech_OCPI_V2.1.1/assets/137178502/b31cb733-abe7-420c-8784-ddba1d846a46)

#### Session status information exchange, stop then Charge Detail Record

![image](https://github.com/CNX-GIREVE/GIRVE_Tech_OCPI_V2.1.1/assets/137178502/ac509c35-6d0b-4e82-b83a-048aa302c283)

### New attribute authorization_id 

**<ins>IOP uses a new attribute « authorization_id » for Session, CDR, AuthorizationInfo and StartSession objects.</ins>**

This information is used to associate the « authorization » given by the eMSP at the beginning of a charging session with Sessions and CDR related to. It answers to real business cases, for example an eMSP giving 2 authorisations and receiving 3 CDRs. Which ones should it pay?

This new property must be provided by the eMSP during the authorisation process, then used by the CPO when it sends Sessions and CDRs related to the authorisation.

**<ins>This property is implemented in OCPI 2.2 under the name “authorization_reference”.</ins>**

#### “authorization_id” in local authorization

![image](https://github.com/CNX-GIREVE/GIRVE_Tech_OCPI_V2.1.1/assets/137178502/ec32c777-cca6-4f69-ad2d-9bc2121d69d1)

#### “authorization_id” in remote authorization

![image](https://github.com/CNX-GIREVE/GIRVE_Tech_OCPI_V2.1.1/assets/137178502/a9ec03ea-c428-4ece-aaf4-250ca84d0723)

#### “authorization_id” in Sessions

![image](https://github.com/CNX-GIREVE/GIRVE_Tech_OCPI_V2.1.1/assets/137178502/e0f0d1a6-4c65-4e05-913c-1cb9926aa2ec)

#### “authorization_id” in CDRs

![image](https://github.com/CNX-GIREVE/GIRVE_Tech_OCPI_V2.1.1/assets/137178502/4c43625c-efb0-4daf-b7ef-a4e7d1128223)


## `Management of B2B tariffs`

The CPO connected to Gireve through OCPI have two options to manage and “publish” B2B tariffs:

**<ins>B2B tariffs are described in roaming agreements</ins>**: The description and the commitment related to tariffs are contained in roaming agreements signed on the Gireve connect place:

-   Tariffs applied for a contract between an eMSP and a CPO are defined and negotiated, using the Gireve connect place, before signature of the roaming agreement by both parts.
-   In case of tariff updates, the CPO and the eMSP must sign an amendment whose management is fully automated.

In this case (tariffs defined in roaming agreements), the CPO should not use the OCPI Tariffs module.

**<ins>B2B tariffs are not described in roaming agreements</ins>**: Tariffs are not described in roaming agreements and the CPO transfers them through the OCPI Tariffs module.

-   In this case, the CPO manages its tariffs on its own and eMSPs cannot give their insights or validation about applied tariffs.
-   Moreover, the CPO is not able to differentiate its tariffs according to eMSPs. In other words, the tariff is unique whatever the eMSP.

## `RFID Tokens`

The current typical situation for identification is swiping a MIFARE badge. In this case, the relevant RFID tag in such a situation is a character string that shall contain the hexadecimal representation of the 4- or 7-bytes RFID UID (sector 0). Please note that the 7 bytes UID is preferred for interoperability reason.

As an example: “1A2B3C4D5E6F70” shall be interpreted as

-   Lowest address byte contains “1A” and “1” is the most significant nibble (half byte) and “A” is the least significant nibble.
-   Highest address byte contains “70”
-   Equivalent decimal value is “7365887390543728”

The RFID is not case sensitive. We recommend using uppercase characters. Leading zeros must be provided to reach 8 characters in case of 4 bytes UID or 14 characters in case of 7 bytes UID.

### Custom OCPI flow to prevent eMSP Tokens download by CPOs

> :warning: <ins>**In OCPI 2.1.1, CPOs must send the “auth_id” of a Token in Sessions and CDRs flows.**</ins>

The only way for CPOs to get this information is to download all Tokens of all eMSPs it is in contract with. CPOs must therefore manage millions of Tokens in their backends, sometime duplicated in case of a backend managing multiple CPOs (a single Token can be downloaded through each CPO connection).

<ins>**To prevent this complexity and make life easier for CPOs, IOP provides a new OCPI flow enabling a CPO to get the full description of a Token, including the “auth_id” information, by requesting IOP with the “Token’s uid”.**</ins>

Using this new flow, the CPO can decide to:

-   Send an authorization request to IOP without knowledge of the Token. If the authorization is accepted the CPO gets the Token, including the “auth_id”, with the new flow then send Sessions and CDRs.  
      
     ![image](https://github.com/CNX-GIREVE/GIRVE_Tech_OCPI_V2.1.1/assets/137178502/12bbe067-d4e7-45bd-8cef-3a7195115ca6)
-   Or request this new flow to get the Token description and the confirmation that this Token is owned by an eMSP behind Gireve, then send the authorization request to Gireve.  
      
    ![image](https://github.com/CNX-GIREVE/GIRVE_Tech_OCPI_V2.1.1/assets/137178502/5f86964a-3b30-41a1-b8cf-a9a62b1b6ac0)

This new flow can also be used as a fallback for CPOs which download all Tokens and have a local authorization for an unknown Token.

Description of this new flow in chapter [PULL Tokens by uid: Retrieve a unique Token](cpo_tokens.md#pull-tokens-by-uid-retrieve-a-unique-token)
