# Fabman API

**Welcome!** If you're trying to integrate your existing applications and tools with [Fabman](https://fabman.io) or create your own application working together with Fabman, you're in the right place!

## The basics

The Fabman API is available via `https` and expects and returns data in JSON format. Every request URL starts with **`https://fabman.io/api/v1/`** and most requests require [authentication](#Authentication).

### Live documentation
You can browse all API endpoints, see required and optional fields and their default values at [https://fabman.io/api/v1/documentation](https://fabman.io/api/v1/documentation) — *you can even try out every endpoint right in your browser*. This is probably the easiest way to experiment and interact with the API.

### cURL
cURL examples in this documentation can be copy & pasted into a shell prompt to try out requests.

## Authentication

Most requests require authentication. At the moment only cookie authentication is avaiable, but other methods will follow soon.

### Cookie authentication

The simplest way to authenticate is by obtaining an authentication cookie using your email address and password. The cookie acts as a bearer token, so anyone who has that cookie can see and change everything you have access — so you want to guard it as well as you guard your password.

If you're using the [interactive documentation page](#basics), you can simply sign in via [the web form](https://fabman.io/login) and your browser will automatically use that cookie for all API requests.

You can also obtain it via cURL: First define where you want the cookie to be stored:

``` shell
export FABMAN_COOKIE=<path to store the cookie>
# for example
export FABMAN_COOKIE=~/.fabman-cookie
```

Then request a new cookie by `POST`ing to `/user/login`:

``` shell
export FABMAN_EMAIL=<your email address>
export FABMAN_PASSWORD=<your password>
curl --cookie-jar $FABMAN_COOKIE --header 'Content-Type: application/json' -d '{"emailAddress": "$FABMAN_EMAIL", "password": "$FABMAN_PASSWORD"}' https://fabman.io/api/v1/user/login
```

You can use this cookie for any subsequent request by adding the `--cookie` param:

``` shell
curl --cookie $FABMAN_COOKIE -X GET https://fabman.io/api/v1/members
```

## JSON data format

We use JSON for all API data. This means that you have to send the `Content-Type` header `Content-Type: application/json` for all `POST` or `PUT` requests — otherwise you'll receive a `415 Unsupported Media Type` response error.

All data is encoded in UTF-8.

## Pagination

Most collection APIs paginate their results. The first request returns up to 50 records by default, but you can change the page size by specifying a 'limit' query parameter.

The Fabman API follows the [RFC5988 convention](https://tools.ietf.org/html/rfc5988) of using the `Link` header to provide URLs for the `next` page. Follow this convention to retrieve the next page of data — please don't build the pagination URLs yourself!

If the `Link` header is blank, that's the last page. The Fabman API also provides the `X-Total-Count` header, which displays the total number of resources in the collection you are fetching.

## Rate limiting

You can only perform a limited number of requests per second per IP. If you exceed the limit, you'll receive a **[429 Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)** response. In such a case wait at least two seconds before retrying a request.

We recommend baking 429 response-handling in to your HTTP handling at a low level so your can handle retries gracefully and automatically.

## Create and update entities

Whenever you create a new entity (usually by `POST`ing to a collection endpoint), you can omit any non-required fields to set them to their default values. If you provide additional fields that are not expected by the API, you will receive a **400 Bad Request** response.

Whenever you update an entity (usually by `PUT`ing to the URL of the entity), you can omit any unchanged fields. If you submit any additional or read-only fields that are not expected by the API, they will be silently ignored. This allows you to GET an entity, change it and submit it without having to filter read-only fields returned by the original GET request.

## Optimistic locking
Most entities contain a `lockVersion` attribute that is used for optimistic locking. The field is automatically incremented whenever someone updates the entity.

Whenever you want to change such an entity, **you must submit the `lockVersion` field unchanged.** If someone else has modified the entity since you last saw it, the update will fail with a **409 Conflict** response to avoid unintentionally overwriting their changes. Simply load the new version of the entity, merge your changes and send another update request.

## Rich text content

Many resources such as Members, Equipment and Packages have attributes that can contain rich text as HTML. Rich text attributes may contain lists, block quotes, links and simple text formatting.

See the [rich text](sections/rich_text.md) section for details.

## Embedding and resolving
Some endpoints allow you to embed related entities into the response to reduce the number of requests needed. For example, when fetching a list of [members](sections/members.md), you can embed each member's active [packages](sections/packages.md). Otherwise you'd have to send a separate request for each member to determine who currently has which package.

Resolving is a similar technique for reducing the number of requests needed. See the [embedding and resolving](sections/embedding_resolving.md) section for more details.

## Entities

- [Accounts](sections/accounts.md)
- [Spaces](section/spaces.md)
- [Members](sections/members.md)
- [Packages](sections/packages.md)
- [Equipment](section/equipment.md)
- [Activity log](sections/log.md)
- [Bookings](sections/bookings.md)
- [Users](sections/users.md)

## License

These API docs are licensed under [Creative Commons (CC BY-SA 4.0)](http://creativecommons.org/licenses/by-sa/4.0/). Please share, remix, and distribute as you see fit.

A big shout-out to the guys at [Basecamp](https://basecamp.com). Some sections and lots of inspiration for this documentation was taken from the [Basecamp 3 API documentation](https://github.com/basecamp/bc3-api).

-----

If you have a specific feature request or find a bug, [please open a GitHub issue](https://github.com/fabman/fabman-api/issues/new).


