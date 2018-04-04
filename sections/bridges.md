# Bridges

The easiest way to connect any machine to Fabman is [via Fabman bridges](http://help.fabman.io/article/15-pairing). But you can use the bridge API to build custom integrations.

All [bridge API endpoints](#endpoints) require an API key for authentication. You can create an API key for every equipment [via the Fabman web application](http://help.fabman.io/article/32-create-a-bridge-api-key) or [via the equipment API](equipment.md#endpoints). Once you have a key, add it to every request as part of the `Authorization` header:

```
Authorization: Bearer 8d29ff56-b9e3-40b5-9a86-f423fe959b93
```
**Don’t forget the `Bearer` prefix!** Replace `8d29ff56-b9e3-40b5-9a86-f423fe959b93` with the API key for your equipment.

When using [the Live API page](https://fabman.io/api/v1/documentation), you can click on the "Authorize" button at the top to enter your API key.


## Heartbeats
Fabman expects regular `/heartbeat` request for every equipment. They’re used to sync configuration changes to the bridge and recognize when bridges go offline. By default, Fabman expects a heartbeat request every 60 seconds, but you can increase the `heartbeatInterval` via the equipment’s [bridge endpoint](equipment.md#endpoints).

A bridge is considered "offline" if it doesn’t send any request (`/heartbeat`, `/access`, or `/stop`) for 3 consecutive heartbeat intervals, (i.e. for 3 minutes by default). You can disable this offline detection for an equipment by setting `heartbeatInterval: null`.

## Checking access & turning on the equipment

Use the `/access` endpoint to check whether a member is allowed to use the given equipment at that moment. Check the `type` field of the response to distinguish between possible results:

* If the member is allowed to use the equipment, Fabman will respond with a `type: "allowed"` and start a new [usage session](#usage-sessions) in the activity log. The response will contain (among other things):
	* the member’s ID
	* their name (in the `message` field)
	* the `sessionId` for this [usage session](#usage-sessions)
	* the maximum duration (in seconds) the member is allowed to use this equipment, e.g., until an upcoming booking or the end of opening hours. The equipment _must_ shut down when this time has passed and send a `/stop` request to the server to close the [usage session](#usage-sessions) (unless the `stopped` flag is set).
	* a `stopped` flag. If this flag is set, the new usage session was implicitly stopped and is already closed. (See [usage session](#usage-sessions) below.)


* If the member is not allowed to use the equipment, Fabman will respond with a `type: "denied"` response. This _must not_ affect the current session (if any). The response will contain the reason for the rejection. The reason should be displayed to the user but the bridge should not change the status of the current usage session (if any) or any deadman controls or alerts (if applicable).

* Fabman may also respond with `type: "message"` when the access request was neither allowed nor rejected but was intercepted by a different process, e.g., a key assignment. The bridge should display the contained messages and then continue operation as if nothing happened.

## Usage sessions

Every `allowed` access response contains a usage session ID. **This session ID _must_ be included in all subsequent `/access` or `/stop` requests until a successful `/stop` request has been submitted.** So whenever your equipment sends a `/access` or `/stop` request, the most recently received `sessionId` must be submitted in the `currentSession.id` field.

It’s possible that the next `/access` request returns a `sessionId` that’s different from the submitted one. (For example when a different member takes over while the machine is still running.) In this case, the bridge should discard the old ID and use the new `sessionId` for all subsequent requests.

Once a `sessionId` has been submitted in a `/stop` request _and_ was confirmed with a `2xx` response status code, the bridge should discard this `sessionId` and don’t submit a `currentSession.id` with subsequent requests – until it receives another `sessionId` from the server.

If the `allowed` access response contains `stopped: true`, then usage session was implicitly stopped (as if the bridge had sent a `/stop` request right after the `/access` request). The bridge should behave as if it just submitted a successful `/stop` request, (i.e., discard the new `sessionId` and don’t send a `/stop` request for this session). This feature is used for door bridges to avoid having to send two requests whenever someone opens the door. The bridge can simply send a `/access` request and, if it receives a `allowed` response, open the door for a few seconds without having to send a separate `/stop` request.

Access responses of any other type (e.g. `denied` or `message`) will _never_ contain a `sessionId` and should never affect the session ID that’s currently stored on the bridge.

## Bridge configuration

Every `/access` or `/heartbeat` request expects a `configVersion` parameter. If a bridge has not yet received any configuration (or does not remember it), it should submit `configVersion: null` or `configVersion: 0`.

If the submitted `configVersion` is outdated (or `null`/`0`), the response will contain the current bridge configuration in the `config` field. In contains (among other things):

* the `configVersion` for this configuration
* the type of equipment the bridge is connected to (Laser cutter, door, 3D printer, …)
* the current name of the equipment
* I/O pin and relais configuration
* safety settings such as dead man intervals

See the [/heartbeat](https://fabman.io/api/v1/documentation/#!/bridge/postBridgeHeartbeat) or [/access](https://fabman.io/api/v1/documentation/#!/bridge/postBridgeAccess) endpoint documentation for details.

The bridge can ignore any configuration parameters it does not understand or support (degrading gracefully, where possible), but it must save the `configVersion` together with any configuration settings it uses. The bridge should submit this `configVersion` as part of all subsequent `/access` or `/heartbeat` requests to avoid transmitting the (quite exhaustive) configuration object with every response.


## Live API documentation
[https://fabman.io/api/v1/documentation#/bridge](https://fabman.io/api/v1/documentation#/bridge)

## Endpoints

- [Send heartbeat](https://fabman.io/api/v1/documentation/#!/bridge/postBridgeHeartbeat) will update the bridge’s online status and sync any [configuration changes](#bridge-configuration), if needed.
- [Request access](https://fabman.io/api/v1/documentation/#!/bridge/postBridgeAccess) checks if a member (identified by their access key or their ID) is allowed to turn on the equipment right now. If the bridge’s configuration is not up to date, the response will also contain the latest [configuration](#bridge-configuration).
- [Stop](https://fabman.io/api/v1/documentation/#!/bridge/postBridgeStop) tells Fabman that a user has stopped using the equipment.
