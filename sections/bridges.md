# Bridges

The easiest way to connect any machine to Fabman is [via Fabman bridges](http://help.fabman.io/article/15-pairing). But you can use the bridge API to build a custom integration.

All [bridge API endpoints](#endpoints) require an API key for authentication. You can create an API key for every equipment [via the Fabman web application](http://help.fabman.io/article/32-create-a-bridge-api-key) or [via the equipment API](equipment.md#endpoints). Once you have a key, add it to every request via the `Authorization` header:

```
 Authorization: Bearer 8d29ff56-b9e3-40b5-9a86-f423fe959b93
```
(replace `8d29ff56-b9e3-40b5-9a86-f423fe959b93` with the API key for your equipment)


## Heartbeats
Fabman expects regular heartbeat request for every equipment. They are used to sync configuration changes to the bridge and recognize when bridges go offline. By default, Fabman expects a heartbeat request every 60 seconds, but you can change the interval via the equipment’s [bridge endpoint](equipment.md#endpoints).

A bridge is considered "offline" if it does not send any request (heartbeat, access, or stop) for 3 consecutive heartbeat intervals, ie. for 3 minutes by default.

## Checking access & usage sessions

Use the [access endpoint](#endpoints) to check whether a member is allowed to use the given equipment at that moment. Check the `type` field in the response to distinguish between possible results:

If the member is allowed to use the equipment, Fabman will respond with `type: "allowed"` and start a new usage session in the activity log. The response will contain (among other things):
* the member’s ID and (in the `message` field) their name
* maximum duration they are allowed to use the equipment (eg. limited by opening hours or an upcoming booking). The equipment _must_ shut down when this time has passed and send a `stop` request to the server.
* The session ID for this usage session. *This _must_ be sent with all subsequent `access` or `stop` requests until the user has stopped using the equipment.*

If the member is not allowed to use the equipment, Fabman will respond with `type: "denied"` response but the current session (if any) will not be affected. The response will contain the reason for the rejection. The reason should be displayed to the user but the bridge should not change the status of the current usage session (if any) or any deadman controls or alerts (if applicable).

Fabman may also respond with `type: "message"` when the access request was neither allowed nor rejected but was intercepted by a different process (eg. a key assignment). The bridge should display the contained messages and then continue operation as if nothing happened.

## Live API documentation
[https://fabman.io/api/v1/documentation#/bridge](https://fabman.io/api/v1/documentation#/bridge)

## Endpoints

- [Send heartbeat](https://fabman.io/api/v1/documentation/#!/bridge/postBridgeHeartbeat) will update the bridge’s online status and sync any configuration changes, if needed.
- [Request access](https://fabman.io/api/v1/documentation/#!/bridge/postBridgeAccess) checks if a member (identified by their access key or their ID) is allowed to turn on the equipment right now. If the bridge’s configuration is not up to date, the response will also contain the latest configuration.
- [Stop](https://fabman.io/api/v1/documentation/#!/bridge/postBridgeStop) tells Fabman that a user has stopped using the equipment.