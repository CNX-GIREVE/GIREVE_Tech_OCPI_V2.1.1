# Things to know

## 1. Definitions and Abbreviations

| Word | Meaning |
| ----------- | ----------- |
| IOP | Inter-operation Platform. IOP is the acronym of the GIREVE’s eMobility Services Platform. |
| RPC | Référentiel des Points de Charge (in French) = Charge Points Repository (in English) The RPC is a system, built around a database that contains Electric Vehicles Charge Infrastructure (EVCI) description. It is connected to IOP. IOP’s interfaces (eMIP, OCPI …) are the only way to access RPC, for partners systems. |
| ToIOP | Referring to flows for which an operator requests GIREVE platform IOP. Partner system is client. IOP is server |
| FromIOP | Referring to flows for which GIREVE platform IOP requests an operator. IOP is client. Partner system is server |

## 2. Who owns what ? 

- Tokens are owned by the eMSP
- Locations are owned by the CPO
- Sessions are owned by the CPO
- CDRs are owned by the CPO
- Tarifs are owned by the CPO

For more informations, [See OCPI client owned object.](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/transport_and_format.md)

## 3. OCPI modules implemented by IOP

| Module | Usage |
| ----------- | ----------- |
| Credentials | Initialise and update IT connection information (endpoints, OCPI versions, …) |
| Locations | Exchange charge infrastructure information |
| Tokens | Manage Tokens and local authorizations |
| Commands | Start a charge remotely, Send commands during charge |
| Sessions | Exchange information during charge |
| CDRs | Send the final charge report |
| Tariffs | Exchange Tariffs information |

## 4. IOP HTTP headers

The next version of OCPI, OCPI 2.2, integrates new extra headers enabling the sharing of a single OCPI connection to multiple operators.
GIREVE has begun to deploy these extra headers in its OCPI version 2.1.1 but it is an ongoing action.

These extra headers should not be considered yet in OCPI 2.1.1, ==except for “PULL Tokens: Retrieve Tokens of a single given eMSP” (see chapter 3.7.4 page 22) and “PULL Tokens: Retrieve Locations of a single given CPO” (see chapter 4.4.3 page 29)==:
- ocpi-to-country-code
- ocpi-to-party-id
- ocpi-from-country-code
- ocpi-from-party-id

## 5. _New_attribute « authorization_id »

**IOP uses a new attribute « authorization_id » for Session, CDR, AuthorizationInfo and StartSession objects.**

This information is used to associate the « authorization » given by the eMSP at the beginning of a charging session with Sessions and CDR related to.
It answers to real business cases, for example an eMSP giving 2 authorisations and receiving 3 CDRs. Which ones should it pay ?

This new property must be provided by the eMSP during the authorisation process, then used by the CPO when it sends Sessions and CDRs related to the authorisation.

**This property is implemented in OCPI 2.2 under the name “authorization_reference”.**

