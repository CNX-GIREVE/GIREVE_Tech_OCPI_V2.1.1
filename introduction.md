# Contents
## 1 Introduction
* [1.1 Aims](#1-1-aims)
* [1.2 Intended Audience](#1-2-intended-audience)
* [1.3 Definitions and Abbreviations](#1-3-definitions-and-abbreviations)
* [1.4 Cardinality expression](#1-4-cardinality-expression)

(## 1.1 Aims {#1-1-aims})

This document describes guidelines to perform a proper connection with the GIREVE’s platform using the OCPI protocol.

## 1.2 Intended Audience

This document is dedicated to technical teams (system administrators, developers, etc.) of systems connected or to be connected to the GIREVE’s platform through OCPI.

This document also covers some operational requirements that must be considered when implementing the OCPI 2.1.1 protocol.

## 1.3 Definitions and Abbreviations

| Word | Meaning |
| ----------- | ----------- |
| IOP | Inter-operation Platform. IOP is the acronym of the GIREVE’s eMobility Services Platform. |
| RPC | Référentiel des Points de Charge (in French) = Charge Points Repository (in English) The RPC is a system, built around a database that contains Electric Vehicles Charge Infrastructure (EVCI) description. It is connected to IOP. IOP’s interfaces (eMIP, OCPI …) are the only way to access RPC, for partners systems. |
| ToIOP | Referring to flows for which an operator requests GIREVE platform IOP. Partner system is client. IOP is server |
| FromIOP | Referring to flows for which GIREVE platform IOP requests an operator. IOP is client. Partner system is server |

## 1.4 Cardinality expression

To designate the cardinality of fields on data structures the following symbols are used:

![](media/ebc894d041323455718e436089619559.png)

**Note that some attributes may not be mandatory by the OCPI standard but are required by Gireve for quality reasons.**

Gireve’s data integration process includes quality controls that ensure the consistency of its data referential. The standard data quality level of Gireve is sustained by numerous mandatory attributes that are not all mandatory by the OCPI standard.

**For further information about the cardinality of attributes by Gireve’s quality standard, please refer to the document “Starting roaming operations as a CPO”.**
