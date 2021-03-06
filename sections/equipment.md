# Equipment

Equipment can be everything from machines to doors, check-in terminals or meeting rooms. Each equipment belongs to a [space](spaces.md) and an [account](accounts.md).

They are called `resources` in the API for historical reasons. So whenever you see a `resource` attribute (eg. on a [booking](bookings.md) or in the [access log](log.md)) it means "equpiment".

The fields `description` and `notes` allow [rich text content](rich_text.md).

## Live API documentation
[https://fabman.io/api/v1/documentation#/resources](https://fabman.io/api/v1/documentation#/resources) and [https://fabman.io/api/v1/documentation#/resource-types](https://fabman.io/api/v1/documentation#/resource-types)

## Endpoints

- [List equipment](https://fabman.io/api/v1/documentation#!/resources/getResources) will return a list of all equipment the current user has access to.
- [Add equipment](https://fabman.io/api/v1/documentation#!/resources/postResources) lets you add equipment to a space.
- [Get equipment](https://fabman.io/api/v1/documentation#!/resources/getResourcesId) returns an equipment with the given ID.
- [Update equipment](https://fabman.io/api/v1/documentation#!/resources/putResourcesId) allows you to change the settings of equipment.
- [Bridge configuration](https://fabman.io/api/v1/documentation#!/resources/putResourcesIdBridge) lets you
    - pair a bridge by submitting the pairing code that's shown on the bridge display
    - create a bridge API key for this equipment to authenticate against the [bridge API](bridges.md).
    - change the expected heartbeat interval for bridges.
- [Get information about an equipment's bridge](https://fabman.io/api/v1/documentation#!/resources/putResourcesIdBridge) with the given equipment ID – like whether it is currently in use, idle or offline.
- [Remove a bridge](https://fabman.io/api/v1/documentation#!/resources/deleteResourcesIdBridge) from the equipment with the given ID.
