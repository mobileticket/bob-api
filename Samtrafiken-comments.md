# Comments from Samtrafiken

## General

- All APIs will of course need to be synced with latest available APIs.
- Objects seem to be missing required fields

## Product API

## Ticket API

- A ticket can be in many states, too many? <code>active, suspended, refunded, refundable, hindered</code>
- <code>/definitions/ticketActivationStatus/remainingActivations</code> - should it really be product set?
- <code>x-nullable</code> has been used. Should it?

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
- Should title and description in object <code>ticket</code> really be "Product title" and "Product description"?
- <code>paymentMeans</code> have fields that starts with paymentMeans... We changed some of them, but not all. We think that is litte bit of "kaka-på-kaka", but the BoB APIs are not consequent on this topic!