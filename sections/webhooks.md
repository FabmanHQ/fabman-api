# Webhooks

Fabman can notify your application through webhooks when something has changed. A webhook consists of an URL to be called, which must be HTTPS, and a list of event categories that'll trigger calls.

For each event, Fabman will POST to the webhook URL with a JSON body that contains details about the event. Fabman will only consider HTTP status codes in the 2xx range to be a successful response. We will not follow a 3xx redirect.

A webhook can be subscribed to updates from the following categories:

* [Member](#member)
* [Member credit](#member-credit)
* [Member device](#member-device)
* [Member key](#member-key)
* [Member package](#member-package)
* [Member payment method](#member-payment-method)
* [Member training](#member-training)
* [Equipment](#equipment)
* [Activity log](#activity-log)
* [Bridge](#bridge)
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
* `createdAt` shows when the event was created (which can be different from when the event was finally deliviered)
* `details` contains detailed information about the event. The exact contents depends on the event type.

After sending the event, Fabman will record the interaction with your application so it can be introspected for debugging.


## Unsuccessful deliveries and retries

In case of an unsuccessful (i.e., non-2xx) response, Fabman will attempt to call the webhook URL multiple times before deactivating the webhook. The duration between attempts will grow exponentially longer to give your service time to recover.


## Paused or deactivated webhooks

Paused webhooks (or webhooks deactivated after unsuccessful delivery) do *not* receive any events. But events from the selected categories *will still be queued* and will be submitted once you un-pause the webhook.


## Event types
### Member

Possible events:
* `member_created`
* `member_updated`
* `member_deleted`

Event payload:
```
{
	…, // common fields (see above)
	"details": {
		"member": {…} // the created, updated or deleted member
	}
}
```

### Member credit

Possible events:
* `memberCredit_created`
* `memberCredit_updated`
* `memberCredit_deleted`
* `memberCredit_used`
* `memberCredit_restored`

Event payload:
```
{
	…, // common fields (see above)
	"details": {
		"memberCredit": {…}, // the member credit
		"member": {…}, // the affected member
        "use": {…},    // Details about the credit use, including the amount and booking or activity ID. (Only exists for "memberCredit_used" and "memberCredit_restored" events.)
	}
}
```
### Member device

A member device is a smartphone, tablet or computer that’s used to switch on equipment via QR codes.

Possible events:
* `memberDevice_created`
* `memberDevice_updated`

Event payload:
```
{
	…, // common fields (see above)
	"details": {
		"device": {…}, // the device including its automatically determined name and user agent
		"member": {…}, // the member whose device changed
	}
}
```

### Member key

Possible events:
* `memberKey_created`
* `memberKey_updated`
* `memberKey_deleted`

Event payload:
```
{
	…, // common fields (see above)
	"details": {
		"key": {…}, // the key including its type and ID token
		"member": {…}, // the member whose key changed
	}
}
```

### Member package

Possible events:
* `memberPackage_created`
* `memberPackage_updated`
* `memberPackage_deleted`

Event payload:
```
{
	…, // common fields (see above)
	"details": {
		"memberPackage": {…}, // the details of this member’s package (fromDate, untilDate, …)
		"member": {…}, // the affected member
		"package": {…} // the package that was assigned to the member
	}
}
```

### Member payment method

Possible events:
* `memberPaymentMethod_created`
* `memberPaymentMethod_updated`
* `memberPaymentMethod_deleted`

Event payload:
```
{
	…, // common fields (see above)
	"details": {
		"paymentMethod": {…}, // the created, updated or deleted payment method (including its type and type-specific details like IBAN or Stripe Id)
		"member": {…}, // the affected member
	}
}
```


### Member training

Possible events:
* `memberTraining_created`
* `memberTraining_updated`
* `memberTraining_deleted`

Event payload:
```
{
	…, // common fields (see above)
	"details": {
		"memberTraining": {…}, // the created, updated or deleted training record
		"member": {…}, // the affected member
	}
}
```

### Equipment

Possible events:
* `resource_created`
* `resource_updated`
* `resource_deleted`

Event payload:
```
{
	…, // common fields (see above)
	"details": {
		"resource": {…} // the created, updated or deleted equipment
	}
}
```

### Activity log

Possible events:
* `resourceLog_created` A new entry in the activity log was created (because someone turned on a machine, asigned a key, …)
* `resourceLog_updated` An existing log entry was modified, eg. because someone extended their session or stopped the machine.

Event payload:
```
{
	…, // common fields (see above)
	"details": {
		"log": {…}, // the created or updated log entry
		"resource": {…}, // the affected equipment
		"member": {…} // member, if applicable. Otherwise null
	}
}
```

### Bridge

Possible events:
* `bridge_create` A bridge was paired (or a new API key bridge was created)
* `bridge_deleted` A bridge was deleted (or an equipment that had a bridge connected was deleted)

Event payload:
```
{
	…, // common fields (see above)
	"details": {
		"bridge": {…}, // the created or deleted bridge
		"resource": {…}, // the affected equipment
	}
}
```

### Booking

Possible events:
* `booking_created`
* `booking_updated`
* `booking_deleted`

Event payload:
```
{
	…, // common fields (see above)
	"details": {
		"booking": {…}, // the created, updated or deleted booking
		"resource": {…}, // the affected equipment
		"member": {…} // the member who booked the equipment. (null if booked as "staff only")
	}
}
```

### Charge

Possible events:
* `charge_created`
* `charge_updated`
* `charge_deleted`

Event payload:
```
{
	…, // common fields (see above)
	"details": {
		"charge": {…}, // the created, updated or deleted charge
		"member": {…} // the affected member
	}
}
```

### Invoice

Possible events:
* `invoice_created`

	*Note: Creating an invoice will trigger `charge_updated` events for every existing charge that’s added to the invoice.*
* `invoice_updated`

Event payload:
```
{
	…, // common fields (see above)
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
		…, // common fields (see above)
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
