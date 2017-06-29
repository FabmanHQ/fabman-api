# Webhooks

Fabman can notify your application through webhooks when something is changed. A webhook consists of an URL to be called, which must be HTTPS, and a list of event categories that'll trigger calls.

For each event, Fabman will POST to the webhook URL with a JSON body that contains details about the event. Fabman will only consider HTTP status codes in the 2xx range to be a successful response. We will not follow a 3xx redirect.

In case of an unsuccessful response, Fabman will attempt to call the webhook URL multiple times before deactivating the webhook. The duration between attempts will grow exponentially longer to give your service time to recover.

A webhook can be subscribed to updates from the following categories:

* [Member](#member)
* [Member package](#member-package)
* [Member key](#member-key)
* [Member training](#member-training)
* [Equipment](#equipment)
* [Activity log](#activity-log)
* [Booking](#booking)
* [Charge](#charge)
* [Invoice](#invoice)

All JSON payloads follow the same format:

```
{
	"id": 9007199254741210,
	"type": "member_created",
	"createdAt": "2016-08-20T15:08:35.384Z",
	"details": {
		… // depends on the event type – see below for details
	}
}
```

where

* `id` is a unique id for this event
* `type` denotes the [type of the event](#event-types)
* `createdAt` shows when the event was created (which might be different from when the event was finally deliviered)
* `details` contains detailed information about the event. The exact contents depends on the event type.

After sending the event, Fabman will record the interaction with your application so it can be introspected for debugging.

## Paused or deactivated webhooks

Paused webhooks (or webhooks deactivated after unsuccessful delivery) do not receive any events. But events from the selected categories will still be queued and will be submitted once you un-pause the webhook.

## Event types
### Member

* `member_created`
* `member_updated`
* `member_deleted`

```
{
	…, // common fields
	"details": {
		"member": {…} // the created, updated or deleted member
	}
}
```

### Member package

* `memberPackage_created`
* `memberPackage_updated`
* `memberPackage_deleted`

```
{
	…, // common fields
	"details": {
		"memberPackage": {…}, // the details of this member’s package (fromDate, untilDate, …)
		"member": {…}, // the affected member
		"package": {…} // the package that was assigned to the member
	}
}
```

### Member key

* `memberKey_created`
* `memberKey_updated`
* `memberKey_deleted`

```
{
	…, // common fields
	"details": {
		"key": {…}, // the key including its type and ID token
		"member": {…}, // the member whose key changed
	}
}
```
### Member training

* `memberTraining_created`
* `memberTraining_updated`
* `memberTraining_deleted`

```
{
	…, // common fields
	"details": {
		"memberTraining": {…}, // the created, updated or deleted training record
		"member": {…}, // the affected member
	}
}
```

### Equipment

* `resource_created`
* `resource_updated`
* `resource_deleted`

```
{
	…, // common fields
	"details": {
		"resource": {…} // the created, updated or deleted equipment
	}
}
```
	
### Activity log

* `resourceLog_created` A new entry in the activity log was created (because someone turned on a machine, asigned a key, …)
* `resourceLog_updated` An existing log entry was modified, eg. because someone extended their session or stopped the machine.

```
{
	…, // common fields
	"details": {
		"log": {…}, // the created or updated log entry
		"resource": {…}, // the affected resource
		"member": {…} // member, if applicable. Otherwise null
	}
}
```
	
### Booking

* `booking_created`
* `booking_updated`
* `booking_deleted`

```
{
	…, // common fields
	"details": {
		"booking": {…}, // the created, updated or deleted booking
		"resource": {…}, // the affected resource
		"member": {…} // the member who booked the equipment. (null if booked as "staff only")
	}
}
```
	
### Charge

* `charge_created`
* `charge_updated`
* `charge_deleted`

```
{
	…, // common fields
	"details": {
		"charge": {…}, // the created, updated or deleted charge
		"member": {…} // the affected member
	}
}
```
	
### Invoice

* `invoice_created`
	
	*Note: Creating an invoice will trigger `charge_updated` events for every existing charge that’s added to the invoice.*
* `invoice_updated`

```
{
	…, // common fields
	"details": {
		"invoice": {…}, // the created or updated invoice
		"charges": […], // all of the invoice’s charges
		"member": {…} // the affected member
	}
}
```
	
### Other events
* `test` is sent when you trigger a webhook test [via the API](https://fabman.io/api/v1/documentation#!/webhooks/postWebhooksIdTest) or the admin UI.

	```
	{
		…, // common fields
		"details": {
			"message": "…", // A message with the name of the member who triggered the test event
			"createdBy": {…} // The member who triggered the test event
		}
	}
	```

## Live API documentation
[https://fabman.io/api/v1/documentation#/webhooks](https://fabman.io/api/v1/documentation#/webhooks)


## Endpoints
- [List webhooks](https://fabman.io/api/v1/documentation#!/webhooks/getWebhooks) will return a list of all webhooks the current user has access to.
- [Add a webhook](https://fabman.io/api/v1/documentation#!/webhooks/postWebhooks) lets you add a webhook to your account.
- [Get a webhook](https://fabman.io/api/v1/documentation#!/webhooks/getWebhooksId) returns the webhook with the given ID.
- [Update a webhook](https://fabman.io/api/v1/documentation#!/webhooks/putWebhooksId) allows you to change the webhooks's url, label, categories and status (to pause and resume event delivery).
- [Test a webhook](https://fabman.io/api/v1/documentation#!/webhooks/postWebhooksIdTest) sends a test event (see above) to see if the webhook is configured correctly.
- [Get recent events](https://fabman.io/api/v1/documentation#!/webhooks/getWebhooksIdEvents) returns the last few events that were sent to the webhook URL – together with a recording of the response that was received. Useful for debugging.
- [Delete a webhook](https://fabman.io/api/v1/documentation#!/webhooks/deleteWebhooksId) to stop Fabman from sending new events to the URL.
