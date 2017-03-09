# Spaces

For now, every [account](accounts.md) has exactly one space that is created when you set up the account. But we'll allow multiple spaces per account pretty soon.

Every space has opening hours and a list of holidays, both of which can be used to limit access to its [equipment](equipment.md).

A space has billing settings like currency, invoice templates, invoice number format, …

## Live API documentation
[https://fabman.io/api/v1/documentation#/spaces](https://fabman.io/api/v1/documentation#/spaces)

## Endpoints
- [List spaces](https://fabman.io/api/v1/documentation#!/spaces/getSpaces) will return a list of all spaces the current user has access to.
- [Add a space](https://fabman.io/api/v1/documentation#!/spaces/postSpaces) lets you add a space to your account.
- [Get a space](https://fabman.io/api/v1/documentation#!/spaces/getSpacesId) returns the space with the given ID.
- [Update a space](https://fabman.io/api/v1/documentation#!/spaces/putSpacesId) allows you to change the space's fields and booking settings.
- Opening hours
	- [Get a space's current opening hours](https://fabman.io/api/v1/documentation#!/spaces/getSpacesIdOpeninghours) returns the opening hours for the space with the given ID.
	- [Set a space’s opening hours](https://fabman.io/api/v1/documentation#!/spaces/putSpacesIdOpeninghours) to replace its previous opening hours.
- Holidays
	- [List a space's holidays](https://fabman.io/api/v1/documentation#!/spaces/getSpacesIdHolidays) returns only *upcoming* holidays.
	- [Add a holiday](https://fabman.io/api/v1/documentation#!/spaces/postSpacesIdHolidays) to a particular space.
	- [Get a space's holiday](https://fabman.io/api/v1/documentation#!/spaces/getSpacesIdHolidaysHolidayid), eg. to see the start and end date.
	- [Update a space’s holiday](https://fabman.io/api/v1/documentation#!/spaces/putSpacesIdHolidaysHolidayid), eg. to change the date.
	- [Delete a space's holiday](https://fabman.io/api/v1/documentation#!/spaces/deleteSpacesIdHolidaysHolidayid) as if it had never existed.
- Billing settings
	- [Get a space’s current billing settings](https://fabman.io/api/v1/documentation#!/spaces/getSpacesIdBillingsettings) returns the current billing settings for the space with the given ID.
	- [Update a space’s billing settings](https://fabman.io/api/v1/documentation#!/spaces/putSpacesIdBillingsettings) to change the tax rate, invoice number format, etc.
