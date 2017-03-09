# Equipment

Equipment belongs to a [space](spaces.md) and an [account](accounts.md) and can be everything from machines to doors, check-in terminals or meeting rooms.

They are called `resources` in the API for historical reasons. So whenever you see a `resource` attribute (eg. on a [booking](bookings.md) or in the [access log](log.md)) it means "equpiment".

The fields `description` and `notes` allow [rich text content](rich_text.md).

## Live API documentation
[https://fabman.io/api/v1/documentation#/resources](https://fabman.io/api/v1/documentation#/resources) and [https://fabman.io/api/v1/documentation#/resource-types](https://fabman.io/api/v1/documentation#/resource-types)

## Endpoints

- [List equipment](https://fabman.io/api/v1/documentation#!/resources/getApiV1Resources) will return a list of all equipment the current user has access to.
- [Add equipment](https://fabman.io/api/v1/documentation#!/resources/postApiV1Resources) lets you add equipment to a space.
- [Get equipment](https://fabman.io/api/v1/documentation#!/resources/getApiV1ResourcesId) returns an equipment with the given ID.
- [Update equipment](https://fabman.io/api/v1/documentation#!/resources/putApiV1ResourcesId) allows you to change the settings of equipment.
- [Pair a bridge to an equipment](https://fabman.io/api/v1/documentation#!/resources/putApiV1ResourcesIdBridge) by submitting the pairing code that's shown on the bridge display.
- [Get information about an equipment's bridge](https://fabman.io/api/v1/documentation#!/resources/putApiV1ResourcesIdBridge) with the given equipment ID – like whether it is currently in use, idle or offline.
- [Remove a bridge](https://fabman.io/api/v1/documentation#!/resources/deleteApiV1ResourcesIdBridge) from the equipment with the given ID.
