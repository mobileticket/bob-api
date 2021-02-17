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

2. <code>/definitions/ticketActivationStatus/remainingActivations</code> - should it really be product set?
4. <code>ticketUpdateRequest</code>, the text for startOfValidity seems to be copy-pasted from somewhere. There is in essence no manifest here.
5. <code>PATCH /ticket/{ticketId}</code> says "..., issued by this server.". The word server should be replaced. This an API documentation.
6. Somewhere there needs to be a description of which system/function that set suspension status, that later can be retrived from <code>GET /ticket/{ticketId}/suspendedStatus</code>

## Token API

1. hint
   1. More descriptive info about what a hint is somewhere in the API description
   2. Could there be another word that better conveys the information that this is a participant?
2. <code>GET /token/{thumbprint}/hint</code> in the end returns hintCompact that only contains the pid, but <code>GET /token/{thumbprint}/hint/{hintId}</code> returns hint that has both pid and expired. Any reason for not returning hint also when getting all hints for a thumbprint?
  
## Traveller API

5. Should title and description in object <code>ticket</code> really be "Product title" and "Product description"?
8. Somewhere describe when a wallet is created? By which system?
