# Spaces

For now, every [account](accounts.md) has exactly one space that is created when you set up the account. But we'll allow multiple spaces per account pretty soon.

Every space has opening hours and a list of holidays, both of which can be used to limit access to its [equipment](equipment.md).

## Live API documentation
[https://fabman.io/api/v1/documentation#/spaces](https://fabman.io/api/v1/documentation#/spaces)

## Endpoints
- [List spaces](https://fabman.io/api/v1/documentation#!/spaces/getApiV1Spaces) will return a list of all spaces the current user has access to.
- [Add a space](https://fabman.io/api/v1/documentation#!/spaces/postApiV1Spaces) lets you add a space to your account.
- [Get a space](https://fabman.io/api/v1/documentation#!/spaces/getApiV1SpacesId) returns the space with the given ID.
- [Update a space](https://fabman.io/api/v1/documentation#!/spaces/putApiV1SpacesId) allows you to change the space's fields and booking settings.
- Opening hours
	- [Get a space's current opening hours](https://fabman.io/api/v1/documentation#!/spaces/getApiV1SpacesIdOpeninghours) returns the opening hours for the space with the given ID.
	- [Set a space’s opening hours](https://fabman.io/api/v1/documentation#!/spaces/putApiV1SpacesIdOpeninghours) to replace its previous opening hours.
- Holidays
	- [List a space's holidays](https://fabman.io/api/v1/documentation#!/spaces/getApiV1SpacesIdHolidays) returns only *upcoming* holidays.
	- [Add a holiday](https://fabman.io/api/v1/documentation#!/spaces/postApiV1SpacesIdHolidays) to a particular space.
	- [Get a space's holiday](https://fabman.io/api/v1/documentation#!/spaces/getApiV1SpacesIdHolidaysHolidayid), eg. to see the start and end date.
	- [Update a space’s holiday](https://fabman.io/api/v1/documentation#!/spaces/putApiV1SpacesIdHolidaysHolidayid), eg. to change the date.
	- [Delete a space's holiday](https://fabman.io/api/v1/documentation#!/spaces/deleteApiV1SpacesIdHolidaysHolidayid) as if it had never existed.
