# Activity log

The activity log contains records of all equipment activity in Fabman, including information about who used which equipment when and for how long.

## Live API documentation
[https://fabman.io/api/v1/documentation#/resource-logs](https://fabman.io/api/v1/documentation#/resource-logs)

## Possible field values

An activity log record contains multiple enumeration fields:

* `type` can be one of:
	* `'denied'`: Access to an equipment was denied.
	* `'allowed'`: Access to an equipment was granted.
	* `'pairing'`: A new bridge was connected to an equipment.
	* `'keyAssigned'`: A new key was assigned to a member.
	* `'checkIn'`: Someone checked in.
	* `'reboot'`: A bridge was restarted.
	* `'offline'`: A bridge was offline.
	* `'resourceDisabled'`: An equipment was put out of order by an admin.
	* `'resourceEnabled'`: An equipment was re-enabled by an admin.
    * `'authorizationCheck'`: An [authorization check](authorization-checks.md) was performed on an equipment.

* `stopType` can be either `null` (if not appilcable or not yet stopped) or one of:
	* `'normal'`: The activity was stopped manually by the user.
	* `'automatic'`: The activity was stopped automatically (usually because of a dead-man timeout).
	* `'offline'`: The activity was stopped while the equipment was offline. (So the time might not be 100% accurate.)
	* `'takeover'`: The next user took over while the equipment was running.
	* `'filter'`: The activity was stopped due to a filter alert.
	* `'permission'`: The activity was stopped because the user’s permission expired.
	* `'race'`: Exists for technical reasons as a precaution, but should never happen in practice.

* `reason` contains the reason why an activity was `'denied'` and can be either `null` (if not appilcable) or one of:
	* `'booking'`: The equipment was booked by someone else.
	* `'exclusiveResource'`: Another exclusive equipment was currently in use.
	* `'holiday'`: The equipment was unavailable due to a holiday.
	* `'inactive'`: The equipment was out of order.
	* `'keyInactive'`: The member’s key was disabled.
	* `'memberInactive'`: The member account was locked.
	* `'noPackage'`: The member doesn’t have a package for this equipment.
	* `'noPermission'`: The member doesn’t have training for this equipment.
	* `'openingHours'`: The activity was outside opening hours and the member’s package doesn’t allow that.
	* `'prepaidBalance'`: The member’s prepaid balance was insufficient.
	* `'requiresBooking'`: This equipment must be booked before it can be used.
	* `'timeTable'`: The time was outside the time frame allowed by the member’s package.
	* `'unknownKey'`: The key doesn't belong to any known member.
	* `'unknownMember'`: The member ID provided in the access request was unknown.
    * `'tooManyAttempts'`: Too many rejected access attempts happended within a short amount of time.
    <!-- Not logged atm: * `'commandTimeout'`: An authorization check has timed out. -->
    <!-- Not logged atm: * `'noTransaction'`: No authorization check was active on an equipment with `controlType = 'pos'`. -->
    * `'thirdPartyError'`: Fabman has encountered an error while communicating with a third-party server (eg. to control a Nuki Smart lock that was set up for this equipment).
    * `'notCheckedIn'`: The member was not checked in but the equipment requires members to check-in before using it.

