# Loby API

The base URL to send all API requests is https://api.loby.io. HTTPS is required for all API requests.

The Loby API follows RESTful conventions when possible, with most operations performed via GET, POST, PATCH, and DELETE requests. Request and response bodies are encoded as JSON.

Use Loby API to retrieve, create or modify data stored in your Loby account programmatically and to create custom integrations.

# Versions

There is currently one version, which is reachable via: `https://api.loby.io/v1`

## Error Codes

## Pagination


## Authentication

Loby API uses API access tokens to authenticate requests. You can view and manage your API access tokens in Loby.

Your API access tokens carry many privileges, so be sure to keep them secure! Do not share your secret API access tokens in publicly accessible areas such as GitHub, client-side code, and so forth.

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

The API access token should be passed as a Bearer authorization header, e.g.:

```
Authorization: Bearer {{accessToken}}
```

Replace `{{accessToken}}` with the access token from Loby.

# Resources

## TicketType

A `TicketType` represents the type of ticket, that you want to make available for your customers.


| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `id`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | Unique identifier | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `name`* | `string` | | Name of ticket |  `"Adult 18+ ticket"` |
| `createdAt`* | `number` | | Date for when the TicketType has been created. |  `1646071663704` |
| `updatedAt`* | `number` | | Date for when the TicketType has been updated. |  `1646071663704` |
| `currency` | `string` ([ISO 4217](https://en.wikipedia.org/wiki/ISO_4217)) | `null` | Currency of ticket. Defaults to the currency of the organization | `"EUR"` |
| `price` | `number` | `null` | Price of ticket in lowest unit. Cents for `EUR`, `USD` etc.  | `"Adult 18+ ticket"` |
| `description` | `string` | `null` | Description facing the customer.  | `"You need to be 18 years old to buy this"` |
| `sortIndex` | `number` | `null` | Index is used for sorting TicketTypes. It will order it Ascending | `3` |
| `enableUpgradeToMembership` | `number` | `true` | If you sell tickets and memberships this will enable customers to deduct the price from their membership purchase | `true` |
| `props` | `object` | `null` | A custom object, where anything can be stored. | `{ "foreignRelevantKey": "bd4a0997-39db-41d9-883a-cdfa83e2101f" }` |

### Create a TicketType

`POST /ticket-types`

| Property | Required |
| - | - |
| `name` | Yes |
| `currency` | No |
| `price` | No |
| `description` | No |
| `sortIndex` | No |
| `enableUpgradeToMembership` | No |
| `props` | No |

Example
```json
{
  "name": "Ticket",
  "currency": "DKK",
  "price": 13500,
}
```

Returns a `TicketType` object.

### Update a TicketType

`PATCH /ticket-types/:id`

## List TicketType

`GET /ticket-types`

This returns a list of

Example:
```json
[
  {
    "id": "",
    "name": "Adult 18+ ticket",
    "createdAt": 1646071663704,
    "updatedAt": 1646071663704,
    "currency": "DKK",
    "price": 13500,
    "description": "You need to be 18 years old to buy this",
    "sortIndex": 0,
    "enableUpgradeToMembership": true,
  }
]
```

## Ticket

A `Ticket` represents an issued ticket, that has a TicketType. A Ticket can e.g. be used as general admission.


| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `id`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | Unique identifier | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `createdAt`* | `number` | | Date for when the Ticket has been created. |  `1646071663704` |
| `updatedAt`* | `number` | | Date for when the Ticket has been updated. |  `1646071663704` |
| `number`* | `number` | | A chronological auto incremental integer unique for the ticket  | `1564` |
| `qrUrl`* | `string` | | The URL to show via a QR Code, that can be used to reference the Ticket  | `https://acme-museum.com/tickets/34?qr=qouireja90uoijw3rtef9oij` |
| `ticketTypeId`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | A reference to a `TicketType` |  `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `upgradeCode`* | `string` | | A 6 character code, that is automaticcally generated. The number of characters and which characters to use, can be changed in the settings. This code can later be used when creating a `MembershipOrder` to get a discount if eligible. | `"E73J8D"` |
| `orderId` | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | `null` | A reference to an `Order` |  `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `userId` | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | `null` | A reference to a `User` |  `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |-cdfa83e2101f"` |
| `upgradeOrderId` | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | `null` | A reference to the `Order` that has been used to create a `Membership` |  `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `props` | `object` | `null` | A custom object, where anything can be stored. | `{ "foreignRelevantKey": "bd4a0997-39db-41d9-883a-cdfa83e2101f" }` |

### Create a Ticket

`POST /ticket`

| Property | Required |
| - | - |
| `ticketTypeId` | Yes |
| `orderId` | No |
| `userId` | No |
| `upgradeOrderId` | No |
| `props` | No |

Example
```json
{
  "ticketTypeId": "bd4a0997-39db-41d9-883a-cdfa83e2101f"
}
```

# QR Code Specification


# Webhooks

Listen for events on your Loby account so your integration can automatically trigger reactions.
Loby uses webhooks to notify your application when an event happens in your account.

## Steps to receive webhooks
1. Create a webhook endpoint as a publicly accessible HTTPS URL.
2. Register the endpoint to Loby via the Dashboard.
3. Identify the events you want to monitor and the event payloads to parse.
4. Handle requests from Loby by parsing each event object and returning 2xx response status codes.

# Scanner Hook

You can intercept the default behaviour when scanning QR codes with the Loby Scanner. This can be particularly useful when you want to consume information from the QR codes and use it in other applications.

## Example Use Case
