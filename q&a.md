# Contents

* [Preproduction modules limit](#preproduction-modules-limit)
* [How to test pagination in preprodution environment](#how-to-test-pagination-in-preproduction-environment)
* [Tokens : Difference between Full and Delta PULL](#tokens-difference-between-full-and-delta-pull)
* [Tokens by uid : What's the difference](#tokens-by-uid)
* [CDR : « total_cost » attribute](#cdr-attribute)
 
*** 

## `Preproduction modules limit`

Gireve's preproduction environment has **<ins>defined limit</ins>** when retrieving your data :

- **PULL** Locations : 20
- **PULL** CDRs : 20
- **PULL** Tokens : 1000 
- **PULL** Tariffs : 100


## `How to test pagination in preproduction environment`

To test the pagination of various modules, your system must contain a minimum number of elements : 

- **Location Module :** 21 locations
- **CDRs Module :** 21 Cdrs
- **Tariffs Module :** 101 tariffs

> :warning: **<ins>If you cannot integrate the recommended minimum number into your system, you must be able to modify your default PUSH limit.
IOP will consider this new limit.</ins>**


## `Tokens Difference between Full and Delta PULL`

As a CPO, you can retrieve tokens from eMSPs with whom you have a contract.
To do this, you have two ways :

- **Full** : 1 time per month.
- **Delta** : 1 time per day.

> :warning: In Delta the CPO don't retrieve the Tokens of new eMSPs contracts. **<ins>New contract = New FULL</ins>**

> :bulb: **<ins>Depending on your contract with Gireve, these periodicities can change. For more information, please contact your acccount representative.</ins>**

## `Tokens by uid`

You can retrieve tokens from an eMSP using either the FULL or DELTA method. However, integrating tokens at the CPO's end can be complicated if the eMSP contains a significant quantity of tokens. To address this, Gireve has implemented a new service : **"GET_Tokens_unitary."**

This service allows the CPO to retrieve the complete description of a token using only the UID.

> :bulb: **<ins>It is not mandatory to implement the full or delta. The most important thing is to be able to retrieve the "auth_id" attribute of the token.</ins>**



## `CDR attribute`

The field "total_cost" is **mandatory**. It must always be associated with a value, whether it is true or false.
