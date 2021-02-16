# Comments from Samtrafiken

The comments are unordered and unprioritised.

## General

1. All APIs will of course need to be synced with latest available APIs.
2. Objects seem to be missing required fields (we've added a few).
3. All new fields should have example values (in english where applicable).
4. HTTP status codes should be added. One example is:
   1. 400 Bad Request - in some places, where maybe certain combination of parameters don't work together

## Product API

1. <code>temporalValidity</code>
   1. Clearer description of <code>absoluteValidity</code> where period is used in description...
   2. <code>active</code> is not the only parameter that can set a ticket to active. <code>issueMtb</code> is also relevant.
   3. <code>numberOfActivations</code>, dormant is not a state and should probably be removed.
      1. Maybe the last part "...if issued in dormant (inactive) state" should be removed?
      2. Don't know if it's possible, but say one issue a ticket in active state, then make it inactive. Then numberOfActivations could tell if it's possible to activate the ticket again.

## Ticket API

1. A ticket can be in many states, too many? <code>active, inactive, suspended, refunded, refundable, hindered</code>
2. <code>/definitions/ticketActivationStatus/remainingActivations</code> - should it really be product set?
3. <code>x-nullable</code> has been used. Should it?
4. <code>ticketUpdateRequest</code>, the text for startOfValidity seems to be copy-pasted from somewhere. There is in essence no manifest here.
5. <code>PATCH /ticket/{ticketId}</code> says "..., issued by this server.". The word server should be replaced. This an API documentation.
6. Somewhere there needs to be a description of which system/function that set suspension status, that later can be retrived from <code>GET /ticket/{ticketId}/suspendedStatus</code>
7. In object <code>ticketActivationStatus</code> relativeValidity is described with "Validity period ...". Don't get us started :-) The subject is complex, but the right words/phrases must be used!

## Token API

1. hint
   1. More descriptive info about what a hint is somewhere in the API description
   2. Could there be another word that better conveys the information that this is a participant?
2. <code>GET /token/{thumbprint}/hint</code> in the end returns hintCompact that only contains the pid, but <code>GET /token/{thumbprint}/hint/{hintId}</code> returns hint that has both pid and expired. Any reason for not returning hint also when getting all hints for a thumbprint?
3. Endpoints <code>GET /token/{thumbprint}/hint</code> and <code>GET /token/{thumbprint}/hint/{hintId}</code> can be called without X-BoB-AuthToken. Is that intentional?
  
## Traveller API

2. <code>productProperty</code> and <code>travellersPerCategory(Item)</code> should be synced as well as possible with corresponding objects in Product API
3. Should timeOfIssue in <code>timeConstraints</code> really be there? We don't think it's a constraint.
   1. Move it up/out one level?
   2. The same also for numberOfActivations and remainingActivations, which doesn't have anything to do with time.
   3. <code>other</code> is a sloppy word, use a better one.
4. Maybe also rename <code>timeConstraints</code> to temporal (compare with Product API)  
5. Should title and description in object <code>ticket</code> really be "Product title" and "Product description"?
6. <code>paymentMean</code> (was paymentMean**s**) have fields that starts with paymentMean... We removed the prefix. We think that is little bit of "kaka-på-kaka", but the BoB APIs are not consequent on this topic!
7. Should the format for DOB, date-of-birth, be specified in MTS8?
8. Somewhere describe when a wallet is created? By which system?
9. The type enum in <code>paymentMean</code> has purse as one of it's allowed values. What is a purse (in this context)? Would cash be a better word?
10. Changed <code>transactionUpdate</code> to use an enum instead. To avoid the possibility of setting both to true or both to false.
11. travellersPerCategory is not exactly the same as in Product API, which is ok. But maybe <code>travellersPerCategoryItem</code> should be renamed to travellersPerCategoryInformation?
12. <code>productSetInformation</code>
    1. Should it really be a product set? Maybe rename to ticket set? Althought is awfully close to ticket bundle.
    2. What does <code>productSetId</code> refer to? producetSetAlternatives in Product API doesn't have an id.
13. <code>ride</code> should maybe be renamed to trip, to more *align* with NeTEx / Transmodel? But we haven't found a definitive answer.
14. <code>activationData</code>
    1. No required fields?
    2. Rename <code>activationDateTime</code> to activation. We rarely use DateTime-suffix in BoB.
    3. Specify format according to MTS8 
    4. What does activating for ticketId and eventId mean?
15. We try not use verbs for endpoints (this could maybe be under general)
    1. /mtb/{issuerSignature}/activate should be .../activiation?
    2. /productSet/{productSetId}/issue should be .../issuance?
    3. /productSet/{productSetId}/recreate should be .../recreation?
16. Should the list of statuses in <code>productSetStatus</code> also contain created?
17. Change <code>PUT</code> to return 204, instead of 200. Ok?
18. <code>POST /ticketNotification<code> clearer text for 403? Which creation failed?
