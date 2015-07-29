# Webhooks and Subscriptions

```json
{
  "_links": {},
  "id": "string",
  "topic": "string",
  "accountId": "string",
  "eventId": "string",
  "subscriptionId": "string",
  "attempts": [
    {
      "id": "string",
      "request": {
        "timestamp": "2015-07-23T14:19:36.981Z",
        "url": "string",
        "headers": [
          {
            "name": "string",
            "value": "string"
          }
        ],
        "body": "string"
      },
      "response": {
        "timestamp": "2015-07-23T14:19:36.981Z",
        "headers": [
          {
            "name": "string",
            "value": "string"
          }
        ],
        "statusCode": 0,
        "body": "string"
      }
    }
  ]
}
```

A webhook notification is sent as a `POST` request to a user defined destination. The whitelabel API will send webhooks to notify an application as to the status of a transfer. As required in the White Label TOS, you will use webhooks to notify your customers via email on the status of a transfer.

### Available notifications
| Event              | Description                                                                                                       |
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| customer_created   | A customer record was created.                                                                                    |
| transfer_created   | Transfer was created.                                                                                             |
| transfer_cancelled | A pending transfer has been cancelled, and will not process further.                                              |
| transfer_failed    | Transaction failed to clear successfully. Usually, this is a result of an ACH failure (insufficient funds, etc.). |
| transfer_completed | Transfer has cleared successfully.                                                                                |
| transfer_reclaimed | The transfer was returned to the sender after being unclaimed by the recipient for a period of time.              |

### Acknowledgement and retries
For webhook notifications, your server should respond with a HTTP 2xx status code to indicate you have successfully received the message. If Dwolla receives a status code greater than a HTTP 400, or you fail to respond within 20 seconds of the attempt, we will retry the call to your subscribed webhook URL. 

Dwolla will attempt to retry each webhook 8 times over the course of 72 hours. If a webhook was successful but you would like information again, you can call [retrieve webhook by ID](#retrieve-webhook-by-id). Failed webhooks are retried on the following backoff schedule:

| Retry number | Interval (relative to last retry) | Interval (relative to original attempt) |
|:------------:|:---------------------------------:|:---------------------------------------:|
|       1      |              15 min               |                  15 min                 |
|       2      |              45 min               |                   1 h                   |
|       3      |               2 h                 |                   3 h                   |
|       4      |               3 h                 |                   6 h                   |
|       5      |               6 h                 |                  12 h                   |
|       6      |              12 h                 |                  24 h                   |
|       7      |              24 h                 |                  48 h                   |
|       8      |              24 h                 |                  72 h                   |

### Webhook Resource

| Parameter      | Description                                       |
|----------------|---------------------------------------------------|
| id             | Unique UUIDv4 identifier.                         |
| topic          | Type of webhook subscription.                     |
| accountId      | Account associated with the webhook notification. |
| eventId        | Event ID for this webhook.                        |
| subscriptionId | Webhook subscription ID for this event.           |
| attempts       | Array of Attempt JSON object.                     |

### Attempt JSON Object

| Parameter      | Description                             |
|----------------|-----------------------------------------|
| id             | Unique ID of webhook delivery attempt.  |
| request        | Request JSON object                     |
| response       | Response JSON object                    |

### Request/Response JSON Object

| Parameter      | Description                                                                   |
|----------------|-------------------------------------------------------------------------------|
| timestamp      | ISO-8601 timestamp.                                                           |
| url            | URL where data was sent to/received from.                                     |
| headers        | Array of objects with keys `name` and `value` representative of HTTP headers. |
| body           | Event ID for this webhook.                                                    |

## Subscribe to a Webhook

> Request:

```shell
POST /webhook-subscriptions
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
```

```json
{
  "url": "http://deliver.webhooks/here",
  "secret": "shush!"
}
```

> Response:

```shell
HTTP/1.1 201 Created
Location: https://api.dwolla.com/webhook-subscriptions/10d4133e-b308-4646-b276-40d9d36def1c
```

Create a webhook subscription to deliver webhooks to a desired URL. 

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `ManageCustomers` scope.</aside>

### HTTP Request
`
POST https://api.dwolla.com/customers/webhook-subscriptions
`

### Request Parameters

Parameter | Description
----------|------------
url | Where Dwolla should deliver the webhook notification.
secret | Your application secret. 

### Errors
| HTTP Status | Message |
|--------------|-------------|
| 404 | No active customer record |

## Unsubscribe from a Webhook

> Request:

```shell
DELETE /webhook-subscriptions/10d4133e-b308-4646-b276-40d9d36def1c
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
```

> Response:

```shell
HTTP 200
```

Fetch a list of webhooks that the authorized user is currently subscribed to. 

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `ManageCustomers` scope.</aside>

### HTTP Request
`
DELETE https://api.dwolla.com/customers/webhook-subscriptions/{id}
`

### Request Parameters

Parameter | Description
----------|------------
id | Webhook UUID.


### Errors
| HTTP Status | Message |
|--------------|-------------|
| 404 | No active customer record |

## List Webhook Subscriptions

> Request:

```shell
GET /webhook-subscriptions
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
```

> Response:

```json
{
  "_links": {
    "self": {
      "href": "https://api.dwolla.com/webhook-subscriptions"
    }
  },
  "total": 1,
  "items": [
    {
      "_links": {
        "self": {
          "href": "https://api.dwolla.com/webhook-subscriptions/10d4133e-b308-4646-b276-40d9d36def1c"
        }
      },
      "id": "10d4133e-b308-4646-b276-40d9d36def1c",
      "url": "http://destination.url",
      "createdDate": "2015-07-23T14:19:36.993Z"
    }
  ]
}
```

Fetch a list of webhooks that the authorized user is currently subscribed to. 

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `ManageCustomers` scope.</aside>

### HTTP Request
`
GET https://api.dwolla.com/customers/webhook-subscriptions
`

### Errors
| HTTP Status | Message |
|--------------|-------------|
| 404 | No active customer record |

## Retrieve Webhook by ID

> Request:

```shell
GET /hooks/76ce47b9-5b3c-4ac8-a743-ce318afbaecd
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
```

> Response:

```json
{
  "_links": {
    "self": {
      "href": "https://api.dwolla.com/hooks/76ce47b9-5b3c-4ac8-a743-ce318afbaecd"
    }
  },
  "id": "76ce47b9-5b3c-4ac8-a743-ce318afbaecd",
  "topic": "string",
  "accountId": "string",
  "eventId": "string",
  "subscriptionId": "string",
  "attempts": [
    {
      "id": "string",
      "request": {
        "timestamp": "2015-07-23T14:19:36.981Z",
        "url": "string",
        "headers": [
          {
            "name": "string",
            "value": "string"
          }
        ],
        "body": "string"
      },
      "response": {
        "timestamp": "2015-07-23T14:19:36.981Z",
        "headers": [
          {
            "name": "string",
            "value": "string"
          }
        ],
        "statusCode": 0,
        "body": "string"
      }
    }
  ]
}
```

Fetch a list of webhooks that the authorized user is currently subscribed to. 

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `ManageCustomers` scope.</aside>

### HTTP Request
`
GET https://api.dwolla.com/customers/webhook-subscriptions
`

### Errors
| HTTP Status | Message |
|--------------|-------------|
| 404 | No active customer record |

## Retrieve Subscription by ID

> Request:

```shell
GET /webhook-subscriptions/10d4133e-b308-4646-b276-40d9d36def1c
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
```

> Response:

```json
{
  "_links": {
    "self": {
      "href": "https://api.dwolla.com/webhook-subscriptions/10d4133e-b308-4646-b276-40d9d36def1c"
    }
  },
  "id": "10d4133e-b308-4646-b276-40d9d36def1c",
  "url": "http://destination.url",
  "createdDate": "2015-07-23T14:19:36.993Z"
}
```

Fetch a webhook subscription by its ID.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `ManageCustomers` scope.</aside>

### HTTP Request
`
GET https://api.dwolla.com/customers/webhook-subscriptions/{id}
`

### Errors
| HTTP Status | Message |
|--------------|-------------|
| 404 | No active customer record |

## Hooks Belonging to Subscription

> Request:

```shell
GET /webhook-subscriptions/10d4133e-b308-4646-b276-40d9d36def1c/hooks
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
```

> Response:

```json
{
  "_links": {},
  "total": 0,
  "items": [
    {
      "_links": {},
      "id": "string",
      "topic": "string",
      "accountId": "string",
      "eventId": "string",
      "subscriptionId": "string",
      "attempts": [
        {
          "id": "string",
          "request": {
            "timestamp": "2015-07-23T14:19:37.006Z",
            "url": "string",
            "headers": [
              {
                "name": "string",
                "value": "string"
              }
            ],
            "body": "string"
          },
          "response": {
            "timestamp": "2015-07-23T14:19:37.006Z",
            "headers": [
              {
                "name": "string",
                "value": "string"
              }
            ],
            "statusCode": 0,
            "body": "string"
          }
        }
      ]
    }
  ]
}
```

View all fired webhooks that are associated to a specific subscription, via that subscription's ID. 

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `ManageCustomers` scope.</aside>

### HTTP Request
`
GET https://api.dwolla.com/customers/webhook-subscriptions/{id}/hooks
`

### Errors
| HTTP Status | Message |
|--------------|-------------|
| 404 | No active customer record |