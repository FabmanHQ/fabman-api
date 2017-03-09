# Accounts

An account is created for every Fabman customer. Every account has at least one [space](spaces.md). You'll need your account ID for creating every other entity like [members](members.md), [equipment](equipment.md) or [packages](packages.md).

A [user](users.md) can have access to multiple accounts.

## Live API documentation
[https://fabman.io/api/v1/documentation#/accounts](https://fabman.io/api/v1/documentation#/accounts)

## Endpoints

- [List accounts](https://fabman.io/api/v1/documentation#!/accounts/getAccounts) will return a list of all accounts the current user has access to
- [Create an account](https://fabman.io/api/v1/documentation#!/accounts/postAccounts) lets you create a new account. The current user will be the owner of the new account.
- [Get an account](https://fabman.io/api/v1/documentation#!/accounts/getAccountsId) returns a single account with the given ID.
- [Update an account](https://fabman.io/api/v1/documentation#!/accounts/putAccountsId) allows you to change the settings of an account.
