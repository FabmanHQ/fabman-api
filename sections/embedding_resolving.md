# Embedding and resolving

[Embedding](#embedding) and [resolving](#resolving) are optimization techniques provided by the Fabman API.

## Embedding

Some endpoints allow you to embed related entities into the response to reduce the number of requests needed.
For example, when fetching a list of [members](sections/members.md), you can embed each member's active [packages](sections/packages.md) by adding the query parameter `embed=activePackages`. Without this feature you'd have to send a separate request for each member to determine what packages the currently have.

Embedded entities are returned as children of the `_embedded` attribute within the original entity.

### Example: embedding active packages

Try the following request:

``` shell
curl --cookie $FABMAN_COOKIE -X GET https://fabman.io/api/v1/members?embed=activePackages
```

It returns every member's packages as a list beneath the `_embedded.memberPackages` attribute and even adds the corresponding `package` base record:

``` json
[
	{
		"id": 2916,
		"memberNumber": "5",
		"firstName": "John",
		"lastName": "Doe",
		… remaining member attributes …
		"_embedded": {
			"memberPackages": [
				{
					"id": 5078,
					"package": 15,
					"fromDate": "2016-06-21",
					"untilDate": null,
					… remaining member package attributes…
					"_embedded": {
						"package": {
							"id": 15,
							"name": "All-inclusive",
							… remaining package attributes…
						}
					}
				}
			]
		},
	},
	{
		"id": 2998,
		"memberNumber": "6",
		"firstName": "John",
		"lastName": "Dull",
		… remaining member attributes …
		"_embedded": {
			"memberPackages": [
				{
					"id": 5072,
					"package": 16,
					"fromDate": "2016-06-16",
					"untilDate": null,
					… remaining member package attributes…
					"_embedded": {
						"package": {
							"id": 16,
							"name": "Small",
							… remaining package attributes…
						}
					}
				},
				{
					"id": 5074,
					"package": 18,
					"fromDate": "2016-06-16",
					"untilDate": null,
					… remaining member package attributes…
					"_embedded": {
						"package": {
							"id": 18,
							"name": "Infinite coffee",
							… remaining package attributes…
						}
					}
				},
			]
		},
	},
	…
]
```

## Resolving

Resolving is a similar technique for reducing the number of requests needed for large record sets like reports. Instead of adding complete entities to the result, the just replace the ID of a relation with some fields of the referenced entity to avoid subsequent fetches.

### Example: activiy log

When you want to display the activity log of equipment, it's only moderately useful when the corresponding member is only referenced by ID:

``` shell
curl --cookie $FABMAN_COOKIE -X GET https://fabman.io/api/v1/resource-logs

Result:
[
  {
    "id": 1118231,
    "resource": 245,
    "type": "keyAssigned",
    "member": 2999,
    "stoppedAt": null,
    …
  },
  …
]
```

Instead, you can resolve the relation to include the member's name and status. This allows you display the log without subsequent requests to fetch each member:

``` shell
curl --cookie $FABMAN_COOKIE -X GET https://fabman.io/api/v1/resource-logs?resolve=member

Result:
[
  {
    "id": 1118231,
    "resource": 245,
    "type": "keyAssigned",
    "member": {
      "id": 2999,
      "memberNumber": "9",
      "firstName": "Jack",
      "lastName": "Black",
      "state": "active"
    },
    "stoppedAt": null,
    …
  },
  …
]
```

It's not a full member record – to avoid bloating the response too much – but enough information to give a meaningful context.