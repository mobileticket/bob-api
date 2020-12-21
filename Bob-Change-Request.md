# BoB Change Request

This document describes a change request for BoB Traveller, Ticket, Token and Product API.

## Date

2020-12-21

## Requested by

Fredrik Ljunggren, Kirei AB
fredrik@kirei.se

Paul Ulvinius, Stratiteq Sweden AB 
paul.ulvinius@stratiteq.com

On behalf of Skånetrafiken, Blekingetrafiken, Hallandstrafiken, Jönköpings Länstrafik, Länstrafiken Kronoberg and Kalmar länstrafik.

## Current situation

Skånetrafiken, Blekingetrafiken, Hallandstrafiken, Jönköpings Länstrafik, Länstrafiken Kronoberg and Kalmar länstrafik are working towards completing the implementation of seamless cross-border travels.

During the design of functionality relating to Ticket Vending Machines (TVMs) a number of use-cases has been unravelled which requires extended functionality in the communication between the sales systems. Examples of such use-cases includes:

-	Activation of dormant cross-border tickets
-	Use of funds held in purse managed by a foreign sales system
-	Ability to re-purchase (previously bought) products using foreign sales system

Some of these requirements are also outlined in the Sydtaxa 2.0, the foundational agreement between these regions for establishing functionality for cross-border ticketing.

This CR also includes the functionality already discussed in BOBX-16, as it is closely related to the proposed functionality. BOBX-16 did not, however, take ID-based travelling into account. The purpose of BOBX-16 and its updated functionality in this CR is to provide human-readable descriptions of a cross-border set of tickets which can be used for example in the following use-cases:

-	During validation or inspection, to provide information on the complete journey (not only the current leg) which is an enabler to provide better customer experience and support during the journey. It can also help in proving more detailed information if a ticket cannot be (positively) validated on a specific trip.
-	To provide a recognised and understandable description of combined tickets when they are being offered for re-purchase.

## Desired situation and purpose

The desired situation is to have the Bob APIs support these functions.

## Proposed solution

### Activation of dormant cross-border tickets

A cross-border ticket may have been sold in inactive state (being dormant) and tied to a token for ID-based travelling. As embarking on this trip, the traveller is required to activate the dormant (combined) ticket. All legs of it should be activated at once. This requires the activating party to be able to locate both what combined tickets are tied to the token and each ticket component of these combined tickets.

To enable this, it is suggested to add a simple function to the Token API where sales channels may register hints to a hint list. A hint is merely an entry in the list telling the inquirer which sales channels that may hold information on issued combined tickets. Each entry has an expiry time which should be set to the end of the absolute validity period. It should be noted that only cross-border tickets (or tickets valid at another operator) need to be registered in the hint list. It is the sales channels decision if it is relevant to register a hint or not.

By recovering the hint list a TVM (for instance) can list what combined tickets are bound to a token and offer the traveller to also active dormant combined tickets. To active them, the TVM would request the selling party to activate them through each Ticket API.

The following sequence diagram describes the flow:

[card-query-activation.txt](https://github.com/mobileticket/bob-api/blob/cr2020q1/seq/card-query-activation.txt)


### Support for multi-trip tickets

Tied to the activation of formant cross-border tickets is the functionality to support multi-trip tickets, also in ID-based scenarios. Such tickets have traditionally been offered with a discount compared to buying single-trip tickets, and as a convienient way to purchase tickets in advance without having to deal with a ticket vending machine before embarking.

Simple examples includes pre-purchasing of 10 trips, but more complex variants exists as well. Such examples are tickets which are valid for 20 trips within a time window of 60 days counting from the first trip. Another example is school-passes, which is valid for two trips per day during the semester, but only on work-days.

To support this latter use case, a new ticket state is introduced ("suspended"). A school-pass with such restrictions would typically enter the suspended state after the second trip of a school day and over the weekend. In addition, information objects providing the sales channel with the insights into the applicable restrictions have been added to the product API.


### Ability to re-purchase (previously bought) products using foreign sales system 

A traveller who has bought a travel pass in Region A which is valid for cross-border travel between Region A and Region B, may discover when being in Region B that the travel pass has expired and wants to re-new it for another period. However, the only party holding the original product search parameters is the sales channel in Region A, where the original purchase was made. In special cases, it may also be that only this sales channel has the required agreements in place to sell this specific product. The proposed solution to this challenge is to have a function to trigger the same product search again through the sales channel in Region A, using the current sales channel (for instance, a TVM in Region B). Using this functionality, the payment can also be made using payment means tied to the token in the original sales channel.

It is also proposed to add the ability to (optionally) alter some options related to the product query, these options are limited to travellersPerCategory and productProperties (only). A foreign sales channel may present the productProperties set in the productSetInformation if these are consistent across tickets, and allow the customer to alter such options. For instance, a productProperty could represent certain add-ons to the product which affects the price and may or may not be desirable for the re-purchase. It could also be possible to alter travellersPerCategory if that makes sense in the current context. For instance if the re-purchase operation references a single-journey ticket which the traveller frequently purchases. Allowing a traveller to quickly buy the same ticket for multiple travellers could be convenient. 

[card-repurchase.txt](https://github.com/mobileticket/bob-api/blob/cr2020q1/seq/card-repurchase.txt)


### Use of funds held in purse managed by a foreign sales system

To use payment means held in a foreign sales system, a similar function to the hints is proposed to be added to the Token API, enabling a sales system to locate where the traveller has the preferred payment means registered. It is proposed that only one preferred provider can be registered to minimise complexity to a manageable level. A list of such providers can lead to unpredictable behaviours, even if the list is holding a priority indicator. For instance, lack of agreements and different limits on different providers can severely complicate matters

[card-purchase.txt](https://github.com/mobileticket/bob-api/blob/cr2020q1/seq/card-purchase.txt)


### Registration of preferred payment service provider

To register a preferred payment service provider, the sales channel (which has the relationship with the payment service provider) will register its own PID with the token issuer through the Token API. Reasons for registering this with the token provider is that a foreign sales channel may have no other information than the token ID and the PID of the issuer as a travel token is presented. The most straight-forward way to recover information relating to the token is to query the Token API (which has to be queried anyway, to ensure the token is not revoked and possibly also to recover the hints list).

It should be noted that it is not a requirement to register a preferred PSP with the token provider, it is optional.

[card-registration.txt](https://github.com/mobileticket/bob-api/blob/cr2020q1/seq/card-registration.txt)


## Affected specifications and APIs

BoB Traveller API
BoB Token API
BoB Product API
BoB Ticket API


## Other affected stakeholders

Only those stakeholders who implement the Traveller API and desire to use this functionality is affected.


## Technical consequences 

The proposed change will alter some existing objects in the Traveller API, this CR therefor calls for a bump of the major version in this API. To our knowledge, no-one has implemented the current version of the Traveller API. 

All other proposed changes are backwards compatible.


## Business consequences 

The proposed functionality is critical for being able to support some quite common cross-border use-cases for ID-based travelling, while also improving the user experience when travelling across multiple operators. The functionality also enables Payment Service Providers (PSPs), holding authorisations to conduct banking or other financial business, to enter the BoB ecosystem as independent participants. This could overcome some difficult hurdles in, for instance, "Pay-as-you-go" scenarios.  

As the current Traveller API has not, to our knowledge, been implemented by any BoB Participants there should be no negative consequences.


## Time schedule

2021


## Required resources

The suggested changes are available for review at:

[https://github.com/mobileticket/bob-api/tree/cr2020q1](https://github.com/mobileticket/bob-api/tree/cr2020q1)



## Risk analysis

No risks have been identified at this point.
