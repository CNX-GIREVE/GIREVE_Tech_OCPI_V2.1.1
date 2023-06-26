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

For more informations, [See OCPI client owned object](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/transport_and_format.md)
