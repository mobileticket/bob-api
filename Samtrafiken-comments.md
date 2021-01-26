# Comments from Samtrafiken

The comments are unordered and unprioritised.

## General

- All APIs will of course need to be synced with latest available APIs.
- Objects seem to be missing required fields (we've added a few).
- All new fields should have example values (in english where applicable).
- HTTP status codes should be added. One example is:
  - 400 Bad Request - in some places, where maybe certain combination of parameters don't work together

## Product API

- <code>temporalValidity</code>
  - Clearer description of <code>absoluteValidity</code> where period is used in description...
  - <code>active</code> is not the only parameter that can set a ticket to active. <code>issueMtb</code> is also relevant.
  - <code>numberOfActivations</code>, dormant is not a state and should probably be removed.
    - Maybe the last part "...if issued in dormant (inactive) state" should be removed?
    - Don't know if it's possible, but say one issue a ticket in active state, then make it inactive. Then numberOfActivations could tell if it's possible to activate the ticket again.

## Ticket API

- A ticket can be in many states, too many? <code>active, inactive, suspended, refunded, refundable, hindered</code>
- <code>/definitions/ticketActivationStatus/remainingActivations</code> - should it really be product set?
- <code>x-nullable</code> has been used. Should it?
- <code>ticketUpdateRequest</code>, the text for startOfValidity seems to be copy-pasted from somewhere. There is in essence no manifest here.
- <code>PATCH /ticket/{ticketId}</code> says "..., issued by this server.". The word server should be replaced. This an API documentation.
- Somewhere there needs to be a description of which system/function that set suspension status, that later can be retrived from <code>GET /ticket/{ticketId}/suspendedStatus</code>
- In object <code>ticketActivationStatus</code> relativeValidity is described with "Validity period ...". Don't get us started :-) The subject is complex, but the right words/phrases must be used!

## Token API

- hint
  - More descriptive info about what a hint is somewhere in the API description
  - Could there be another word that better conveys the information that this is a participant?
- <code>GET /token/{thumbprint}/hint</code> in the end returns hintCompact that only contains the pid, but <code>GET /token/{thumbprint}/hint/{hintId}</code> returns hint that has both pid and expired. Any reason for not returning hint also when getting all hints for a thumbprint?
- Endpoints <code>GET /token/{thumbprint}/hint</code> and <code>GET /token/{thumbprint}/hint/{hintId}</code> can be called without X-BoB-AuthToken. Is that intentional?
  
## Traveller API

- <code>PATCH /token/{tokenId}/wallet/transaction</code> - it feels strange to be able to update a transaction. Shouldn't a transaction be atomic?
- <code>productProperty</code> and <code>travellersPerCategory(Item)</code> should be synced as well as possible with corresponding objects in Product API
- Should timeOfIssue in <code>timeConstraints</code> really be there? We don't think it's a constraint.
  - Move it up/out one level?
- Should title and description in object <code>ticket</code> really be "Product title" and "Product description"?
- <code>paymentMean</code> (was paymentMean<b>s</b>) have fields that starts with paymentMean... We removed the prefix. We think that is little bit of "kaka-på-kaka", but the BoB APIs are not consequent on this topic!
- Should the format for DOB, date-of-birth, be specified in MTS8?
- Somewhere describe when a wallet is created? By which system?
- The type enum in <code>paymentMean</code> has purse as one of it's allowed values. What is a purse (in this context)? Would cash be a better word?
- Changed <code>transactionUpdate</code> to use an enum instead. To avoid the possibility of setting both to true or both to false.
- travellersPerCategory is not exactly the same as in Product API, which is ok. But maybe <code>travellersPerCategoryItem</code> should be renamed to travellersPerCategoryInformation?
- <code>productSetInformation</code>
  - Shoud it really be a product set? Maybe rename to ticket set? Althought is awfully close to ticket bundle.
  - What does <code>productSetId</code> refer to? producetSetAlternatives in Product API doesn't have an id.
- <code>ride</code> should maybe be renamed to trip, to more align with NeTEx / Transmodel? But we haven't found a definitive answer.  