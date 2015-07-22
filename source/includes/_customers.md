# Customers

```shell
{
  "id": "99bfb139-eadd-4cdf-b346-7504f0c16c60",
  "firstName": "Bob",
  "lastName": "Dole",
  "status": "active",
  "_links": {
    "self": {
      "href": "https://api.dwolla.com/customers/99bfb139-eadd-4cdf-b346-7504f0c16c60"
    }
  }
}
```

A customer represents an individual or business with whom you intend to transact with.  In order to manage a user's Customers, the `ManageCustomers` OAuth scope is required.

### Customer Resource

| Parameter | Description
|-----------|------------|
|id | Customer Record unique identifier.  UUID format.
|firstName | Customer's first name.
|lastName | Customer's last name.
|status | Either `active`, `deactivated`, or `suspended`.


## List Customers

> Request:

```shell
GET https://api.dwolla.com/customers
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
```

> Response:

```json
{
  "_links": {
    "self": {
      "href": "https://api.dwolla.com/customers"
    }
  },
  "total": 1,
  "items": [
    {
      "_links": {
        "self": {
          "href": "https://api.dwolla.com/customers/99bfb139-eadd-4cdf-b346-7504f0c16c60"
        }
      },
      "id": "99bfb139-eadd-4cdf-b346-7504f0c16c60",
      "firstName": "Bob",
      "lastName": "Dole",
      "status": "active"
    }
  ]
}
```

Fetch a list of Customers which belong to the authorized user.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `ManageCustomers` scope.</aside>

### HTTP Request
`GET https://api.dwolla.com/customers`

### Errors
| HTTP Status | Message |
|--------------|-------------|
| 404 | No active customer record |

## New Customer

> Request:

```shell
POST /customers
Content-Type: application/vnd.dwolla.v1.hal+json
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
```

```json
{
  "firstName": "Bob",
  "lastName": "Dole",
  "email": "gordon+1@dwolla.com",
  "ipAddress": "99.99.99.99"
}
```

> Response:
```shell
HTTP 201
```

```json
{
  "_links": {
    "self": {
      "href": "https://api.dwolla.com/customers/6f80efc0-b158-4df1-9b11-da85f0bffdd4"
    }
  },
  "id": "6f80efc0-b158-4df1-9b11-da85f0bffdd4",
  "firstName": "Bob",
  "lastName": "Dole",
  "status": "active"
}
```

Create a new Customer.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `ManageCustomers` scope.</aside>

### HTTP Request
`POST https://api.dwolla.com/customers`

### Request Parameters
Parameter | Description
----------|------------
firstName | Customer's first name.
lastName | Customer's last name.
email | Customer's email address.
ipAddress | Customer's IP address

### Errors
| HTTP Status | Message |
|--------------|-------------|
| 400 | A customer record with this information already exists.
| 400 | Validation errors. Another object is included the response to list the parameters and errors. 
| 401 | You do not have access to this resource.
| 500 | An unexpected error occurred.

## List Documents

> Request:

```shell
GET /customers/6f80efc0-b158-4df1-9b11-da85f0bffdd4/documents
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
```

> Response:

```json
{
  "_links": {
    "self": {
      "href": "https://api.dwolla.com/customers/6f80efc0-b158-4df1-9b11-da85f0bffdd4/documents"
    }
  },
  "total": 1,
  "items": [
    {
      "_links": {
        "self": {
          "href": "https://api.dwolla.com/customers/99bfb139-eadd-4cdf-b346-7504f0c16c60/documents/e6c141d5-0922-4d18-ad00-4789a37f288f"
        }
      },
      "id": "e6c141d5-0922-4d18-ad00-4789a37f288f",
      "mimetype": "image/png",
      "documentType": "passport"
    }
  ]
}
```

Fetch a list of Documents which belong to a Customer. 

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `ManageCustomers` scope.</aside>

### HTTP Request
`GET https://api.dwolla.com/customers/{id}/documents`

### Errors
| HTTP Status | Message |
|--------------|-------------|
| 404 | No active customer record |

## Create a Document

> Request:

```shell
POST /customers/6f80efc0-b158-4df1-9b11-da85f0bffdd4/documents
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
```

```json
{
  "mimetype": "image/png",
  "documentType": "passport"
}
```

```shell
POST /customers/6f80efc0-b158-4df1-9b11-da85f0bffdd4/documents
Accept: image/png
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY

[bytes of image]
```

> Response:
```shell
HTTP 201
```

```json
{
  "_links": {
    "self": {
      "href": "https://api.dwolla.com/customers/6f80efc0-b158-4df1-9b11-da85f0bffdd4/documents/e6c141d5-0922-4d18-ad00-4789a37f288f"
    }
  }
```

Create a Document belonging to a Customer. 

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `ManageCustomers` scope.</aside>

### HTTP Request
1: `POST https://api.dwolla.com/customers/{id}/documents` with `Document` <br />
2: `POST https://api.dwolla.com/customers/{id}/documents` with a 5MB `*.png` or `*.jpg` image file.

### Errors
| HTTP Status | Message |
|--------------|-------------|
| 404 | No active customer record |

## Retrieve Document By ID

> Request:

```shell
GET /customers/6f80efc0-b158-4df1-9b11-da85f0bffdd4/documents/e6c141d5-0922-4d18-ad00-4789a37f288f
Accept: application/vnd.dwolla.v1.hal+json
Authorization: Bearer pBA9fVDBEyYZCEsLf/wKehyh1RTpzjUj5KzIRfDi0wKTii7DqY
```

> Response:

```json
{
    "_links": {
      "self": {
        "href": "https://api.dwolla.com/customers/99bfb139-eadd-4cdf-b346-7504f0c16c60/documents/e6c141d5-0922-4d18-ad00-4789a37f288f"
      }
    },
    "id": "e6c141d5-0922-4d18-ad00-4789a37f288f",
    "mimetype": "image/png",
    "documentType": "passport"
}
```

Fetch a Document by its ID.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `ManageCustomers` scope.</aside>

### HTTP Request
`GET https://api.dwolla.com/customers/{id}/documents/{id}`

### Errors
| HTTP Status | Message |
|--------------|-------------|
| 404 | No active customer record |