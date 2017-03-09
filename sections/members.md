# Members

Members belong to an [account](accounts.md), have a home [space](spaces.md) and may have a corresponding [user](users.md) that allows them to sign in and use the API amd the member or admin app.

The privileges of a member (`member`, `admin` or `owner`) determine what their user is allowed to read or change via the API or web interface.

The fields `notes` and `billingInvoiceText` allow [rich text content](rich_text.md).

##  Live API documentation

[https://fabman.io/api/v1/documentation#/members](https://fabman.io/api/v1/documentation#/members)

## Endpoints
- [List members](https://fabman.io/api/v1/documentation#!/members/getMembers) will return a list of all equipment the current user has access to.
- [Add a member](https://fabman.io/api/v1/documentation#!/members/postMembers) lets you add a member to your account.
- [Get a member](https://fabman.io/api/v1/documentation#!/members/getMembersId) returns the member with the given ID.
- [Update a member](https://fabman.io/api/v1/documentation#!/members/putMembersId) allows you to change the member's fields and lock/unlock their access to the account.
- Privileges
	- [Get a member's current privileges](https://fabman.io/api/v1/documentation#!/members/getMembersIdPrivileges). Defaults to `member`, but can be `admin` or `owner`.
	- [Change a member's privileges](https://fabman.io/api/v1/documentation#!/members/putMembersIdPrivileges) to make them Admins or Owners of your account.
- Invitation
	- [Invite the member to create a user](https://fabman.io/api/v1/documentation#!/members/postMembersIdInvitation) sends an invitation email the member’s email address. (Calling this for an already-invited member re-sends the invitation email.)
	- [Get invitation status](https://fabman.io/api/v1/documentation#!/members/getMembersIdInvitation) to see if a member was invited and whether they have already accepted the invitation.
- Keycard
	- Assign a keycard to a member via the [key assignment](key_assignments.md) process
	- [Get a member's key status](https://fabman.io/api/v1/documentation#!/members/getMembersIdKey) to check whether their key is active or disabled.
	- [Change a member's key status](https://fabman.io/api/v1/documentation#!/members/putMembersIdKey) to enable or disable a key.
	- [Remove a member's current key](https://fabman.io/api/v1/documentation#!/members/deleteMembersIdKey) to replace it or assign the key to someone else.
- Packages
	- [List a member's packages](https://fabman.io/api/v1/documentation#!/members/getMembersIdPackages) – current and past.
	- [Assign a member a new package](https://fabman.io/api/v1/documentation#!/members/postMembersIdPackages) to allow them to use equipment
	- [Get a member's package](https://fabman.io/api/v1/documentation#!/members/getMembersIdPackagesMemberpackageid), eg. to see the start and end date.
	- [Update a member's package](https://fabman.io/api/v1/documentation#!/members/putMembersIdPackagesMemberpackageid), eg. to set the end date.
	- [Delete a member's package](https://fabman.io/api/v1/documentation#!/members/deleteMembersIdPackagesMemberpackageid) as if it had never existed. (You might want to set an end date instead…)

