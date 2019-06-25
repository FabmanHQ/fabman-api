# Key assignments

Assigning a keycard to a member works by selecting a member and equipment and swiping an unassigney key over the equipment's bridge.

1. [Create a new key assignment](https://fabman.io/api/v1/documentation#!/key-assignments/postKeyassignments) by submitting the ID of the member that's about to receive a new key. You'll receive a key assignment ID.
2. [Select an equipment to use](https://fabman.io/api/v1/documentation#!/key-assignments/putKeyassignmentsId) by `PUT`ing the equipment ID to your assignment endpoint. This request will reserve the given equipment for about 15 seconds.

Existing members can continue to use the reserved equipment as normal. Only the _first unknown_ keycard that's swiped accross the equipment's bridge during this time will be assigned to your chosen member.


The `PUT` request will stall for a few seconds. As soon as a keycard was assigned, it will return a **204 No Content** response and you're done!

If no unknown keycard is swiped accross the bridge for 5 seconds, the request will return a **202 Accepted** response, but will keep the equipment reserved for 10 more seconds. Just repeat `PUT`ing the equipment ID during this time to maintain your reservation or submit a different equipment ID to switch equipment. If a keycard is successfully assigned while no `PUT` requests is active, you'll receive a **204 No Content** response to your next `PUT` request to let you know that it succeeded.

If you want to abort the process, you can either let the reservation time out or [delete your key assignment](https://fabman.io/api/v1/documentation#!/key-assignments/deleteKeyassignmentsId) (recommended).
