# Accounts

An account is created for every Fabman customer. You'll need your account ID for creating entities like [members](members.md) or [equipment](equipment.md).

A [user](users.md) can have access to multiple accounts. 

## Live API documentation
[https://fabman.io/api/v1/documentation#/accounts](https://fabman.io/api/v1/documentation#/accounts)

## Endpoints

- [List accounts](https://fabman.io/api/v1/documentation#!/accounts/getApiV1Accounts) will return a list of all accounts the current user has acess to
- [Create an account](https://fabman.io/api/v1/documentation#!/accounts/postApiV1Accounts) lets you create a new account. The current user will be the owner of the new account.
- [Get an account](https://fabman.io/api/v1/documentation#!/accounts/getApiV1AccountsId) returns a single account with the given ID
- [Update an account](https://fabman.io/api/v1/documentation#!/accounts/putApiV1AccountsId) allows you to change the settings of an account.
