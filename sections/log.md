# Activity log

The activity log contains information about who used which equipment when and for how long.

## Live API documentation
[https://fabman.io/api/v1/documentation#/resource-logs](https://fabman.io/api/v1/documentation#/resource-logs)

## Endpoints

- [List logs entries](https://fabman.io/api/v1/documentation#!/resource45logs/getResourcelogs) will return a list of all log entries the current user has access to (and can be filtered by account, equipment, type, …)
- [Get a log entry](https://fabman.io/api/v1/documentation#!/resource45logs/getResourcelogsId) returns a single entry with the given ID.
- [Update a log entry](https://fabman.io/api/v1/documentation#!/resource45logs/putResourcelogsId) allows you to attach up to 1kb of JSON metadata to a log entry.
