# Contents
## [Introduction](#introduction)
* [Aims](#aims)
* [Intended Audience](#intended-audience)
* [Definitions and Abbreviations](#definitions-and-abbreviations)
* [Cardinality expression](#cardinality-expression)

***

# Introduction

## `Aims`

This document describes guidelines to perform a proper connection with the Gireve’s platform using the OCPI protocol.

## `Intended Audience`

This document is dedicated to technical teams (system administrators, developers, etc.) of systems connected or to be connected to the Gireve’s platform through OCPI.

This document also covers some operational requirements that must be considered when implementing the OCPI 2.1.1 protocol.

## `Definitions and Abbreviations`

| Word | Meaning |
| ----------- | ----------- |
| **IOP** | Inter-operation Platform. **IOP** is the acronym of the Gireve’s eMobility Services Platform. |
| **RPC** | Référentiel des Points de Charge (in French) = Charge Points Repository (in English) The **RPC** is a system, built around a database that contains Electric Vehicles Charge Infrastructure (EVCI) description. It is connected to **IOP**. **IOP**’s interfaces (eMIP, OCPI …) are the only way to access **RPC**, for partners systems. |
| **ToIOP** | Referring to flows for which an operator requests Gireve platform **IOP**. Partner system is client. IOP is server |
| **FromIOP** | Referring to flows for which Gireve platform **IOP** requests an operator. **IOP** is client. Partner system is server |

## `Cardinality expression`

To designate the cardinality of fields on data structures the following symbols are used:

| Symbol | Description |
| ----------- | ----------- |
| ? | An optional object. If not set, it might be null, or the field might be omitted. When the field is omitted and it has a default value, the value is the default value. |
| 1 | Required object. |
| * | A list of zero or more objects. If empty, it might be null, [] or the field might be omitted. |
| + | A list of at least one object. |

<ins>**Note that some attributes may not be mandatory by the OCPI standard but are required by Gireve for quality reasons.**</ins>

Gireve’s data integration process includes quality controls that ensure the consistency of its data referential. The standard data quality level of Gireve is sustained by numerous mandatory attributes that are not all mandatory by the OCPI standard.
