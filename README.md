# ifeelgoods-platform-fulfillment-api-documentation
Ifeelgoods Platform Fulfillment API Documentation

# Introduction

The Ifeelgoods Fulfillment API is a __Rewards Delivery service__ which is one component of the __Ifeelgoods Platform__. It allows you to grab digital and physical rewards on-demand from a wide range of providers as listed on our [Catalog](https://catalog.ifeelgoods.com) and send these to your end users.

This is a __RESTful JSON-based API__ with a focus on practicality and ease of use. Here you will find the reference, guides and how-to useful to integrate the Ifeelgoods Fulfillment API as quickly as possible.

### URLs & Versioning

The current API version  is version 2.

- __API Endpoint:__ https://api.ifeelgoods.com/fulfillment/v2/
- __OAuth Endpoint__ : https://oauth.ifeelgoods.com/

The API URL includes the version of the API. When changes do not allow backward compatibility, the API URL will be updated with a new version number in order to not break applications using the previous version.

### API Status

We provide a [Status Page](http://status.ifeelgoods.com/) where you can stay up to date on our operational status. We will communicate about maintenance windows, incidents and API updates through that page so we encourage you to subscribe to one of the notification channels provided (Email, RSS, etc)

# Authentication

Credentials consist of

- __API Key__ : API Client identifier, can be shared.
- __API Secret__ : Shared secret between client and Ifeelgoods.

These credentials are specific to each environment (as described above) and allow you access your resources exclusively. To get your credentials, please reach out to your Ifeelgoods contact at <apisupport@ifeelgoods.com>. Your API Secret acts as a  strong password and carry many privileges, so never disclose it publicly!

Two authentication methods are possible :

- __Header-based authentication__ (i.e embedding __Api-Key__ and __Api-Secret__ in headers on every request)
- __Token-based authentication__ (i.e embedding an Auth Token in the headers on every request)

## API Key-based Authentication

A simple approach is to include the API credentials in the headers directly, using __Api-Key__ and __Api-Secret__.

```http
Api-Key: <IFEELGOODS_API_KEY>
Api-Secret: <IFEELGOODS_API_SECRET>
```

## OAuth2 tokens

There's no reinventing the wheel here so we were inspired by the OAuth2 authorization framework to use the service accounts approach, albeit in a simplified form.
Tokens need to be included in all requests made to the API using the __Authorization__ header.

```http
Authorization: Bearer <IFEELGOODS_OAUTH_TOKEN>
```

<!-- In order to obtain a token, please refer to the guide. -->

## OAuth2 tokens vs API Key-based Authentication

API Key-based authentication is meant solely for Server-to-Server interactions with this API since it requires sending the API secret on every request. The Token-based authentication can be used for hitting the API from client side applications (web or mobile).

Please contact us at <apisupport@ifeelgoods.com> for more information.

# Responses & Errors

## Data formats

- __Date and time__ will be expressed as ISO8601 date and time in UTC (e.g., "2015-09-23T16:01:35Z").
- __Floats__ will be represented as Strings with decimals separated by a dot "." (e.g., "169.99").

## HTTP Status Codes

The Ifeelgoods API attempts to use appropriate conventional HTTP response codes. `2xx` codes indicate succesfull requests, `4xx` codes are intended for cases in which there is an error in the request and `5xx` when the Ifeelgoods' servers are experiencing troubles (which is rare :wink:).

|Status Code|Text|Description|
|---|---|---|
|200|OK|Everything is OK!|
|201|Created|A resource has been successfully created after a POST request.|
|400|Bad Request|The request is malformed and cannot be understood.|
|401|Unauthorized|You need to be signed-in to perform this action.|
|403|Forbidden|You are not allowed to perform this action.|
|404|Not Found|The resource or action does not exist.|
|406|Not Acceptable|The media type specified by the Accept header is not acceptable.|
|409|Conflict|The resource has already been modified, please reload before submitting again.|
|412|Precondition Failed|Version specified in If-Match header does not match with database one. Please reload the resource.|
|422|Unprocessable Entity|The request is not valid and cannot be processed, please check parameter requirements and format in the documentation.|
|429|Too Many Requests|You sent too many requests in a given amount of time.|
|500|Internal Server Error|An unexpected error has happened.|

## Responses for successful requests

### Single resource

Every responses including a single resource provide a `data` section which contains the detailled resource object. In addition, some resources contain a `version` top-level attribute that helps prevent simultaneous updates of a resource from overwriting each other. When a PUT request is sent, the value should match the one stored in order to be accepted.

```json
{
  "data": {...},
  "version": 42
}
```

### Resources collection & pagination

```json
{
  "data": [
    {...}
  ],
  "paging": {
    "page": 1,
    "per_page": 20,
    "total": 214
  }
}
```

## Response for errors

When the Ifeelgoods API returns error messages, response looks like this:

```json
{
  "error_name": "not_found",
  "error_message": "The resource or action does not exist."
}
```

## Error types

|Error name|Message|
|---|---|
|authentication_system_not_supported|The authentication system provided to perform the redemption has not been activated in the promotion settings.|
|bad_request|The request is malformed and cannot be understood.|
|conflict|The resource has already been modified, please reload before submitting again.|
|conflict_uniqueness|The resource does not satisfy a uniqueness constraint.|
|forbidden|You are not allowed to perform this action.|
|invalid_request|The request is not valid and cannot be processed, please check parameter requirements and format in the documentation.|
|invalid_token|The token provided to perform the redemption is not valid.|
|invalid_user|The user provided to perform the redemption is not valid.|
|not_acceptable|The media type specified by the Accept header is not acceptable.|
|not_found|The resource or action does not exist.|
|operation_refused|This operation is not allowed.|
|precondition_failed|Version specified in If-Match header does not match with database one. Please reload the resource.|
|sorting_limit_exceeded|The number of returned rows exceeds the sorting limit (5000 rows). Please adjust the query parameters to reduce the size.|
|unauthorized|You need to be signed-in to perform this action.|
|user_mismatch|The user provided to perform the redemption does not match the user associated to the redemption.|

# Sandbox

This environment provides a playground for devs to test their integration with the Ifeelgoods Fulfillment API and even run automated integration tests against it without making calls to the production. The environment runs the same code base as the production environment and is maintained in lock step with it, except that the data is not real and all activity may be erased from time to time. __There are no financial side effects to any actions on this environment (i.e only placeholder/fake rewards are delivered through this API) and at this time, no quotas are enforced.__ API credentials provided for this environment do NOT work in other environments.

- __API Endpoint__ : https://api-sandbox.ifeelgoods.com/fulfillment/v2/
- __OAuth Endpoint__ : https://oauth-sandbox.ifeelgoods.com/
