# Authorization checks

Authorization checks are an advanced integration feature. They allow you to ask members to authenticate themselves on a Fabman Bridge to confirm something, like a transaction for a PoS system.

The general flow works like this:

1. Create an authorization check via [`POST /api/v1/authorization-checks`](https://fabman.io/api/v1/documentation#/authorization-checks/postAuthorizationchecks).

    - The bridge will display `message` (eg. a transaction amount to confirm) and wait for a user to swipe their card or scan the QR code. `successMessage` is displayed when a user successfully interacts with the bridge. 

    - You can update these messages at any time via [`PUT /api/v1/authorization-checks/{id}`](https://fabman.io/api/v1/documentation#/authorization-checks/putAuthorizationchecksId), for example if the user has scanned another item an the transaction amount has changed.
    - Cancel a pending check via [`POST /api/v1/authorization-checks/{id}](https://fabman.io/api/v1/documentation#/authorization-checks/postAuthorizationchecksIdCancel).

2. Continuously poll `GET /api/v1/authorization-checks/{id}` to keep the check active and get the current status of the check.

    - You probably want to add `wait=true` to use long-polling, i.e. the response is delayed until either the state of the check changes or a long-poll timeout occurs (in which case you can repeat the request to start another long-poll).
    - You probably also want to add `renew=true` to keep the authorization check active and prevent it from being cancelled due to inactivity.
    - Adding `embed=authorizedMember` allows you to get back the member who authorized the check without having to fetch it with a separate request.

You can query whether there is a pending authorization check for a particular resource by fetching the resource with [`GET /api/v1/resources/{id}?embed=authorizationCheck`](https://fabman.io/api/v1/documentation#/resources/getResourcesId).

## Live API documentation
[https://fabman.io/api/v1/documentation#/authorization-checks](https://fabman.io/api/v1/documentation#/authorization-checks)

