# Transfers

```shell
{
  "_links": {},
  "_embedded": {},
  "id": "string",
  "status": "string",
  "amount": {
    "value": "string",
    "currency": "string"
  },
  "created": "2015-10-02T19:48:40.485Z",
  "metadata": {}
}
```

A transfer represents money being transferred from a `source` to a `destination`. Transfers are available to the `Customer` and `Account` resources. 

### Transfer Resource 

| Parameter | Description
|-----------|------------|
|id | Transfer unique identifier.
|status | Either `processed`, `pending`, `cancelled`, `failed`, or `reclaimed`
|amount| An amount JSON object. See below. 
|created | ISO-8601 timestamp.
|metadata | A metadata JSON object.

### Amount JSON Object

| Parameter | Description
|-----------|------------|
|value | Amount of money. 
|currency | String, `USD`

## Initiate Transfer

> Request:

```shell
POST /transfers
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
```

```json
{
    "_links": {
        "destination": {
            "href": "https://api.dwolla.com/customers/07D59716-EF22-4FE6-98E8-F3190233DFB8"
        },
        "source": {
            "href": "https://api.dwolla.com/funding-sources/707177c3-bf15-4e7e-b37c-55c3898d9bf4"
        }
    },
    "amount": {
        "currency": "USD",
        "value": "1.00"
    },
    "metadata": {
        "foo": "bar",
        "baz": "boo"
    }
}
```

> Response:

```shell
HTTP/1.1 201 Created
Location: https://api.dwolla.com/transfers/74c9129b-d14a-e511-80da-0aa34a9b2388
```

Initiate a transfer for either an account or customer resource. 

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Send` [scope](#oauth-scopes).</aside>

### HTTP Request
`POST https://api.dwolla.com/transfers`

### Errors
| HTTP Status | Message |
|--------------|-------------|
| 400 | Transfer failed. |
| 403 | OAuth token does not have Send scope. |

## Get Transfers (Customer) 

> Request:

```shell
GET /customers/01B47CB2-52AC-42A7-926C-6F1F50B1F271/transfers
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
```

> Response:

```json
{
  "_links": {
    "first": {
      "href": "https://api.dwolla.com/customers/01b47cb2-52ac-42a7-926c-6f1f50b1f271/transfers?limit=25&offset=0"
    },
    "last": {
      "href": "https://api.dwolla.com/customers/01b47cb2-52ac-42a7-926c-6f1f50b1f271/transfers?limit=25&offset=0"
    },
    "self": {
      "href": "http://api.dwolla.com/customers/01B47CB2-52AC-42A7-926C-6F1F50B1F271/transfers"
    }
  },
  "_embedded": {
    "transfers": [
      {
        "_links": {
          "self": {
            "href": "https://api.dwolla.com/transfers/4C8AD8B8-3D69-E511-80DB-0AA34A9B2388"
          },
          "source": {
            "href": "https://api.dwolla.com/accounts/AD5F2162-404A-4C4C-994E-6AB6C3A13254"
          },
          "destination": {
            "href": "https://api.dwolla.com/customers/01B47CB2-52AC-42A7-926C-6F1F50B1F271"
          }
        },
        "id": "4C8AD8B8-3D69-E511-80DB-0AA34A9B2388",
        "status": "pending",
        "amount": {
          "value": "225.00",
          "currency": "USD"
        },
        "created": "2015-10-02T19:42:32.950Z",
        "metadata": {
          "foo": "bar",
          "baz": "foo"
        }
      },
      {
        "_links": {
          "self": {
            "href": "https://api.dwolla.com/transfers/9DC99076-3D69-E511-80DB-0AA34A9B2388"
          },
          "source": {
            "href": "https://api.dwolla.com/accounts/AD5F2162-404A-4C4C-994E-6AB6C3A13254"
          },
          "destination": {
            "href": "https://api.dwolla.com/customers/01B47CB2-52AC-42A7-926C-6F1F50B1F271"
          }
        },
        "id": "9DC99076-3D69-E511-80DB-0AA34A9B2388",
        "status": "pending",
        "amount": {
          "value": "225.00",
          "currency": "USD"
        },
        "created": "2015-10-02T19:40:41.437Z",
        "metadata": {
          "foo": "bar",
          "baz": "foo"
        }
      }
    ]
  },
  "total": 2
}
```

Retrieve a Customer's list of transfers.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `ManageCustomers` [scope](#oauth-scopes).</aside>

### HTTP Request
`GET https://api.dwolla.com/customers/{id}/transfers`

### Request Parameters

Parameter | Optional? | Description
----------|------------|-------------
id | no | Customer unique identifier to get transfers for.
limit | yes | How many results to return.
offset | yes | How many results to skip.

### Errors
| HTTP Status | Message |
|--------------|-------------|
| 403 | Not authorized to list transfers. |
| 404 | Customer not found. |

## Get Transfers (Account) [DRAFT]

> Request:

```shell
GET https://api.dwolla.com/customers/99bfb139-eadd-4cdf-b346-7504f0c16c60/transfers
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
```

> Response:

```json
{
  "_links": {
    "self": {
      "href": "https://api.dwolla.com/accounts/99bfb139-eadd-4cdf-b346-7504f0c16c60/transfers"
    }
  },
  "total": 1,
  "items": [
    {
      "_links": {
        "self": {
          "href": "https://api.dwolla.com/accounts/99bfb139-eadd-4cdf-b346-7504f0c16c60/transfers/a63526e1-cc8d-4a62-9d26-8f04a39da4f3"
        }
      },
      "id": "string",
      "status": "string",
      "amount": {
        "value": 0,
        "currency": "string"
      },
      "created": "2015-07-23T14:19:36.987Z"
    }
  ]
}
```
<aside class="warning">This endpoint is not yet implemented. The following specification is subject to change.</aside>

Retrieve an Account's list of transfers.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Transactions` [scope](#oauth-scopes).</aside>

### HTTP Request
`GET https://api.dwolla.com/accounts/{id}/transfers`

### Request Parameters

Parameter | Optional? | Description
----------|------------|-------------
id | no | Account unique identifier to get transfers for.

### Errors
| HTTP Status | Message |
|--------------|-------------|
| 404 | Account not found. |

## Get Transfers by ID 

> Request:

```shell
GET /transfers/38242332-374b-e511-80da-0aa34a9b2388
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
```

> Response:

```json
{
  "_links": {
    "self": {
      "href": "https://api.dwolla.com/transfers/4C8AD8B8-3D69-E511-80DB-0AA34A9B2388"
    },
    "source": {
      "href": "https://api.dwolla.com/accounts/AD5F2162-404A-4C4C-994E-6AB6C3A13254"
    },
    "destination": {
      "href": "https://api.dwolla.com/customers/01B47CB2-52AC-42A7-926C-6F1F50B1F271"
    }
  },
  "id": "4C8AD8B8-3D69-E511-80DB-0AA34A9B2388",
  "status": "pending",
  "amount": {
    "value": "225.00",
    "currency": "USD"
  },
  "created": "2015-10-02T19:42:32.950Z",
  "metadata": {
    "foo": "bar",
    "baz": "foo"
  }
}
```

Retrieve a Transfer belonging to an Account or Customer by its ID.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Transactions` [scope](#oauth-scopes).</aside>

### HTTP Request
`GET https://api.dwolla.com/transfers/{id}`

### Request Parameters

Parameter | Optional? | Description
----------|------------|-------------
id | no | Account or Customer unique identifier to get transfers for.

### Errors
| HTTP Status | Message |
|--------------|-------------|
| 404 | Transfer not found. |