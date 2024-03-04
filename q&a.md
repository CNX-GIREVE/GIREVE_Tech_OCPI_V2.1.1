### [<- Back to menu](README.md)


# Contents

* [Preproduction modules limit](#preproduction-modules-limit)
* [How to test pagination in preprodution environment](#how-to-test-pagination-in-preproduction-environment)
* [Tokens : Difference between Full and Delta PULL](#tokens-difference-between-full-and-delta-pull)
* [Tokens by uid : What's the difference](#tokens-by-uid)
* [Store and Forward Mechanism](#store-and-forward-mechanism)
 
*** 

## `Preproduction modules limit`

Gireve's preproduction environment has **<ins>defined limits</ins>** when retrieving your data :

- **PULL** Locations : 20
- **PULL** CDRs : 20
- **PULL** Tokens : 1000 
- **PULL** Tariffs : 100


## `How to test pagination in preproduction environment`

To test the pagination of various modules, your system must contain a minimum number of elements : 

- **Location Module :** 21 locations
- **CDRs Module :** 21 Cdrs
- **Tariffs Module :** 101 tariffs

> :warning: **<ins>If you cannot integrate the recommended minimum number into your system, you must be able to modify your default PULL limit.
IOP will consider this new limit.</ins>**


## `Tokens Difference between Full and Delta PULL`

As a CPO, you can retrieve tokens from eMSPs with whom you have a contract.
To do this, you have two ways :

- **Full** : 1 time per month.
- **Delta** : 1 time per hour.

> :warning: In Delta the CPO doesn't retrieve the Tokens of new eMSPs contracts. 
If you have a contract with a new eMSP, there are two ways to recover tokens :

- New FULL
- Gireve's new Web Service : [Tokens by single eMSP](cpo_tokens.md#pull-tokens-retrieve-tokens-of-a-single-given-emsp)

> :bulb: **<ins>Depending on your contract with Gireve, these periodicities can change. For more information, please contact your acccount representative.</ins>**

## `Tokens by uid`

You can retrieve tokens from an eMSP using either the FULL or DELTA method. However, integrating tokens at the CPO's end can be complicated if the eMSP contains a significant quantity of tokens. To address this, Gireve has implemented a new service : **"GET_Tokens_unitary."**

This service allows the CPO to retrieve the complete description of a token using only the UID.

> :memo: **<ins>The most important thing is to be able to retrieve the "auth_id" attribute of the token.</ins>**

> :bulb: **<ins>"auth_id" attribute is the contract ID between the eMSP and the user's token</ins>**

## `Store and Forward mechanism`

> :warning: **<ins>A Store and Forward mechanism must be implemented to ensure that no data upload may be lost, in case of a connection loss. Any data upload that didn’t get a correct response (HTTP code : 200) from the GIREVE platform IOP must be stored on CPO side and a retry process must be active.
> After the connection recovery, the Data Upload messages must be resent in a FIFO manner.</ins>**

