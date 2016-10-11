# Members

Members belong to an [account](account.md) and may have a corresponding [user](user.md) that allows them to sign in.

The privileges of a member (`member`, `admin` or `owner`) determine what their user is allowed to read or change via the API or web interface.

The field `notes` allows [rich text content](rich_text.md).

##  Live API documentation

[https://fabman.io/api/v1/documentation#/members](https://fabman.io/api/v1/documentation#/members)

## Endpoints
- [List members](https://fabman.io/api/v1/documentation#!/members/getApiV1Members) will return a list of all equipment the current user has access to.
- [Add a member](https://internal.fabman.io/api/v1/documentation#!/members/postApiV1Members) lets you add a member to your account.
- [Get a member](https://fabman.io/api/v1/documentation#!/members/getApiV1MembersId) returns the member with the given ID.
- [Update a member](https://fabman.io/api/v1/documentation#!/members/putApiV1MembersId) allows you to change the member's fields and lock/unlock their access to the account.
- [Get a member's current privileges](https://fabman.io/api/v1/documentation#!/members/getApiV1MembersIdPrivileges). Defaults to `member`, but can be `admin` or `owner`.
- [Change a member's privileges](https://fabman.io/api/v1/documentation#!/members/putApiV1MembersIdPrivileges) to make them Admins or Owners of your account.
- Keycards
	- Assign a keycard to a member via the [key assignment](key_assignments.md) process
	- [Get a member's key status](https://fabman.io/api/v1/documentation#!/members/getApiV1MembersIdKey) to check whether their key is active or disabled.
	- [Change a member's key status](https://fabman.io/api/v1/documentation#!/members/putApiV1MembersIdKey) to enable or disable a key.
	- [Remove a member's current key](https://fabman.io/api/v1/documentation#!/members/deleteApiV1MembersIdKey) to replace it or assign the key to someone else.
- Packages
	- [List a member's packages](https://fabman.io/api/v1/documentation#!/members/getApiV1MembersIdPackages) – current and past.
	- [Assign a member a new package](https://fabman.io/api/v1/documentation#!/members/postApiV1MembersIdPackages) to allow them to use equipment
	- [Get a member's package](https://fabman.io/api/v1/documentation#!/members/getApiV1MembersIdPackagesMemberpackageid), eg. to see the start and end date.
	- [Update a member's package](https://fabman.io/api/v1/documentation#!/members/putApiV1MembersIdPackagesMemberpackageid), eg. to set the end date.
	- [Delete a member's package](https://fabman.io/api/v1/documentation#!/members/deleteApiV1MembersIdPackagesMemberpackageid) as it had never existed. (You might want to set an end date instead…)

