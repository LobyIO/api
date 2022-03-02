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

## Organization

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `id`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | Unique identifier | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `name`* | `string` | | Name of ticket | `"Acme Museum"` |
| `currency`* | `string` ([ISO 4217](https://en.wikipedia.org/wiki/ISO_4217)) | `null` | Default currenct of the organization | `"EUR"` |
| `slug` | `string` | `null` | Slug handle that can be used for creating nice url's | `"acme-museum"` |
| `defaultLanguage`* | `string` ([ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)) | | Default language code for the entire organization | `"da"` |
| `defaultEventFields` | `array` | [] | List of `Field` objects. These fields will always be asked for the event. |

## TicketType

A `TicketType` represents the type of ticket, that you want to make available for your customers.


| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `id`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | Unique identifier | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `name`* | `string` | | Name of ticket |  `"Adult 18+ ticket"` |
| `createdAt`* | `number` | | Date for when the TicketType has been created. |  `1646071663704` |
| `updatedAt`* | `number` | | Date for when the TicketType has been updated. |  `1646071663704` |
| `deletedAt` | `number` | `null` | Date for when the TicketType has been deleted. | `1646071663704` |
| `currency` | `string` ([ISO 4217](https://en.wikipedia.org/wiki/ISO_4217)) | `null` | Currency of ticket. Defaults to the currency of the organization | `"EUR"` |
| `slug` | `string` | `null` | Slug handle that can be used for creating nice url's | `"adult"` |
| `price` | `number` | `null` | Price of ticket in lowest unit. Cents for `EUR`, `USD` etc. 0 = FREE | `12500` |
| `description` | `string` | `null` | Description facing the customer.  | `"You need to be 18 years old to buy this"` |
| `translations` | `array` | `[]` | A list of translation objects `TicketTypeTranslation` for the object | `[{ language: "da", name: "Voksen billet" }]`
| `sortIndex` | `number` | `null` | Index is used for sorting TicketTypes. It will order it Ascending | `3` |
| `enableUpgradeToMembership` | `number` | `true` | If you sell tickets and memberships this will enable customers to deduct the price from their membership purchase | `true` |
| `props` | `object` | `null` | A custom object, where anything can be stored. | `{ "foreignRelevantKey": "bd4a0997-39db-41d9-883a-cdfa83e2101f" }` |

#### TicketTypeTranslation

A `TicketTypeTranslation` contains translated fields for the object.

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `language`* | `string` ([ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)) | | Language code for the translation |  `"da"` |
| `name` | `string` | `null` | See description of field in `TicketType` |  
| `description` | `string` | `null` | See description of field in `TicketType` |
| `slug` | `string` | `null` | See description of field in `TicketType |

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
| `createdAt`* | `number` | | Date for when the Ticket has been created. | `1646071663704` |
| `updatedAt`* | `number` | | Date for when the Ticket has been updated. | `1646071663704` |
| `number`* | `number` | | A chronological auto incremental integer unique for the ticket  | `1564` |
| `qrUrl`* | `string` | | The URL to show via a QR Code, that can be used to reference the Ticket. The data is encrypted and the id of the ticket cannot be derived unless the correct decryption key is being used. | `https://acme-museum.com/tickets/34?qr=qouireja90uoijw3rtef9oij` |
| `iOSPassUrl`* | `string` | | The URL for the wallet pass | `https://pass.loby.io/438ue8349ur` |
| `androidPassUrl`* | `string` | | The URL for the Android wallet pass | `https://android-pass.loby.io/438ue8349ur` |
| `pdfUrl`* | `string` | | The URL for the the PDF that is generated and customized via Loby | `https://pdf-ticket.api.loby.io/438ue8349ur` |
| `ticketTypeId`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | A reference to a `TicketType` |  `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `upgradeCode`* | `string` | | A 6 character code, that is automaticcally generated. The number of characters and which characters to use, can be changed in the settings. This code can later be used when creating a `MembershipOrder` to get a discount if eligible. | `"E73J8D"` |
| `orderId` | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | `null` | A reference to an `Order` |  `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `userId` | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | `null` | A reference to a `User` |  `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |-cdfa83e2101f"` |
| `upgradeOrderId` | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | `null` | A reference to the `Order` that has been used to create a `Membership` |  `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `props` | `object` | `null` | An optional custom object, where anything can be stored. | `{ "foreignRelevantKey": "bd4a0997-39db-41d9-883a-cdfa83e2101f" }` |

### Create a Ticket

`POST /tickets`

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

### Ticket email

`GET /tickets/:id/email`

Returns an `Email` object, that contains all the relevant information for the ticket and that can be send to the customer. The email has been enabled and costumized via Loby Cloud.

## Email

A `Email` contains translated fields for the object.

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `language`* | `string` | | The language for the email |  `"da"` |
| `subject`* | `string` | | The subject for the email |  `"Here is your ticket"` |
| `html`* | `string` | | The complete HTML to be used for sending | `"<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Tran..."` |
| `plainText`* | `string` | | Plain text version of email for email clients, that don't support html | `"Hello there! Here is your ticket"`

## MembershipType

A `MembershipType` represents the type of membership, that you want to make available for your customers.


| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `id`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | Unique identifier | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `title`* | `string` | | Title of membership |  `"All access pass"` |
| `subTitle` | `string` | | Sub title of membership |  `"So great and beneficial!"` |
| `createdAt`* | `number` | | Date for when the MembershipType has been created. |  `1646071663704` |
| `updatedAt`* | `number` | | Date for when the MembershipType has been updated. |  `1646071663704` |
| `deletedAt` | `number` | `null` | Date for when the MembershipType has been deleted. | `1646071663704` |
| `currency` | `string` ([ISO 4217](https://en.wikipedia.org/wiki/ISO_4217)) | `null` | Currency of membership type. Defaults to the currency of the organization | `"EUR"` |
| `slug` | `string` | `null` | Slug handle that can be used for creating nice url's | `"access"` |
| `price` | `number` | `null` | Price of ticket in lowest unit. Cents for `EUR`, `USD` etc. 0 = FREE | `54000` |
| `period` | `string` | `YEAR` | For how long the membership will be active until it needs to be renewed. Can be either `YEAR` or `MONTH`. | '"MONTH"'
| `description` | `string` | `null` | The description of the event. This is a `RichText` object and needs to be parsed |
| `translations` | `array` | `[]` | A list of translation objects `MembershipTypeTranslation` for the object | `[{ language: "da", name: "Et adgang til alt..." }]`
| `sortIndex` | `number` | `null` | Index is used for sorting TicketTypes. It will order it Ascending | `3` |
| `benefits` | `array` | `null` | An array of benefits for the mmembership, that can be shown different places | [] |
| `benefits.description` | `string` | `null` | The description of the event. This is a `RichText` object and needs to be parsed | [] |
| `benefits.translations` | `array` | `null` | Translations of the benefit | [] |
| `benefits[].translations[].language` | `string` ([ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)) | `null` | Language code for the translation | `[{ language: 'da', value: 'RED' },...]` |
| `benefits[].translations[].description` | `string` | `null` | See `benefits[].description` description | |
| `availableAsGiftCard` | `number` | `true` | Set to true if you want to sell the membership type as gift card as well | `true` |
| `props` | `object` | `null` | A custom object, where anything can be stored. | `{ "foreignRelevantKey": "bd4a0997-39db-41d9-883a-cdfa83e2101f" }` |

#### MembershipTypeTranslation

A `MembershipTypeTranslation` contains translated fields for the object.

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `language`* | `string` ([ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)) | | Language code for the translation |  `"da"` |
| `title` | `string` | `null` | See description of field in `MembershipType` |  
| `subTitle` | `string` | `null` | See description of field in `MembershipType` |  
| `description` | `string` | `null` | See description of field in `MembershipType` |
| `description` | `string` | `null` | See description of field in `MembershipType` |
| `slug` | `string` | `null` | See description of field in `MembershipType |

## Membership

A `Membership` represents an entered membership, that has a MembershipType. A Membership can e.g. be used to get benefits or to get access to the museum.


| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `id`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | Unique identifier | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `createdAt`* | `number` | | Date for when the Ticket has been created. | `1646071663704` |
| `updatedAt`* | `number` | | Date for when the Ticket has been updated. | `1646071663704` |
| `renewedAt` | `number` | `null` | Date for when the membership has been renewed | `1646071663704` |
| `expiresAt` | `number` | `null` | Date for when the membership will expire | `1646071663704` |
| `qrUrl`* | `string` | | The URL to show via a QR Code, that can be used to reference the Ticket. The data is encrypted and the id of the ticket cannot be derived unless the correct decryption key is being used. | `https://acme-museum.com/tickets/34?qr=qouireja90uoijw3rtef9oij` |
| `iOSPassUrl`* | `string` | | The URL for the wallet pass | `https://pass.loby.io/438ue8349ur` |
| `androidPassUrl`* | `string` | | The URL for the Android wallet pass | `https://android-pass.loby.io/438ue8349ur` |
| `membershipTypeId`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | A reference to a `MembershipType` |  `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `orderId` | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | `null` | A reference to an `Order` |  `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `userId` | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | `null` | A reference to a `User` |  `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |-cdfa83e2101f"` |
| `autoRenew` | `boolean` | `false` | If the membership has been set to auto renew | `true` |
| `props` | `object` | `null` | An optional custom object, where anything can be stored. | `{ "foreignRelevantKey": "bd4a0997-39db-41d9-883a-cdfa83e2101f" }` |

## Email

A `Email` contains translated fields for the object.

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `language`* | `string` | | The language for the email |  `"da"` |
| `subject`* | `string` | | The subject for the email |  `"Here is your ticket"` |
| `html`* | `string` | | The complete HTML to be used for sending | `"<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Tran..."` |
| `plainText`* | `string` | | Plain text version of email for email clients, that don't support html | `"Hello there! Here is your ticket"`

## Event

An `Event` object represents an event, which can have tickets, that can be sold.


| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `id`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | Unique identifier | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `createdAt`* | `number` | | Date for when the object has been created. | `1646071663704` |
| `updatedAt`* | `number` | | Date for when the object has been updated. | `1646071663704` |
| `deletedAt` | `number` | `null` | Date for when the object has been deleted. | `1646071663704` |
| `startDate`* | `number` | | Date for when the event will take place. | `1646071663704` |
| `endDate`* | `number` | | Date for when the event will end. | `1646071663704` |
| `publishedAt` | `number` | `null` | Date for when the object has been published | `1646071663704` |
| `title` | `string` | `null` | Title of the event | `"Maria Abramovic conversation..."` |
| `slug` | `string` | `null` | Slug handle that can be used for creating nice url's | `"maria-abramovic-conversation"` |
| `description` | `string` | `null` | The description of the event. This is a `RichText` object and needs to be parsed | `"[{\"type\":\"paragraph\",\"children\":[{\"text\":\"Kom til ..."` |
| `imageId` | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | `null` | The id of the image, so you can generate an image url.  | `bd4a0997-39db-41d9-883a-cdfa83e2101f` |
| `enableTickets` | `boolean` | `false` | Whether tickets are enabled. Even though ticket types for this object has been added, it can be disabled by disabling this property. | `true` |
| `ignoreDefaultFields` | `boolean` | `false` | This way you can ignore the default fields and overwrite them. | `true` |
| `ticketTypes` | `array` | [] | List of `EventTicketType` objects. |
| `products` | `array` | [] | List of `EventProduct` objects. |
| `targets` | `array` | [] | List of `Target` objects. |
| `fields` | `array` | [] | List of `Field` objects, that will be appended to the default fields from the organization.  |
| `props` | `object` | `null` | An optional custom object, where anything can be stored. | `{ "foreignRelevantKey": "bd4a0997-39db-41d9-883a-cdfa83e2101f" }` |

#### EventTranslation

An `EventTranslation` contains translated fields for the object.

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `language`* | `string` ([ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)) | | Language code for the translation |  `"da"` |
| `title` | `string` | `null` | See description of field in `Event` |  
| `description` | `string` | `null` | See description of field in `Event` |

### Create an Event

`POST /events`

## EventTicketType

An `EventTicketType` object represents all the types of tickets for a particular event.

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `id`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | Unique identifier | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `createdAt`* | `number` | | Date for when the object has been created. | `1646071663704` |
| `updatedAt`* | `number` | | Date for when the object has been updated. | `1646071663704` |
| `deletedAt` | `number` | `null` | Date for when the object has been deleted. | `1646071663704` |
| `active` | `boolean` | `true` | Only active types can be sold. Otherwise they are paused. | `false` |
| `price`* | `number` | | The price for a ticket in lowest unit. Cents for `EUR`, `USD` etc. 0 = FREE | `4500` |
| `name` | `string` | `null` | Optional name for the type of ticket. | `"General Ticket for event"` |
| `description` | `string` | `null` | Optional description for the type of ticket. | `"What an opportunity?! This is so amazing..."` |
| `products` | `array` | [] | List of `EventProduct` objects. |
| `fields` | `array` | [] | List of `Field` objects. |
| `quantity` | `number` | `null` | The number of available tickets for this type. |
| `minQuantityPerOrder` | `number` | `1` | The minimum quantity pr. order. | `2` |
| `maxQuantityPerOrder` | `number` | `null` | The maximum quantity pr. order | `3` |
| `salesStart` | `number` | `null` | Date for when the the sale of these tickets will start. | `1646071663704` |
| `salesEnd` | `number` | `null` | Date for when the the sale of these tickets will end. | `1646071663704` |
| `targets` | `array` | [] | List of `Target` objects. |
| `props` | `object` | `null` | An optional custom object, where anything can be stored. | `{ "foreignRelevantKey": "bd4a0997-39db-41d9-883a-cdfa83e2101f" }` |

#### EventTicketTypeTranslation

A `EventTicketTypeTranslation` contains translated fields for the `EventTicketType` object.

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `language`* | `string` ([ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)) | | Language code for the translation |  `"da"` |
| `name` | `string` | `null` | See description of field in `EventTicketType` |  

### Create a ticket type

`POST /events/:id/ticket-type`

### Examples for ticket types

#### Use Case: Free

1. Create an `Event`
2. Create an `EventTicketType` where `price` = `0`

#### Use Case: Free for members, but paid for public

#### Use Case: Access only for members and hidden for others

#### Use Case: Paid for public, discount for members


## EventProduct

An `EventProduct` object represents a product, that can be purchased along with the event.

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `id`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | Unique identifier | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `createdAt`* | `number` | | Date for when the object has been created. | `1646071663704` |
| `updatedAt`* | `number` | | Date for when the object has been updated. | `1646071663704` |
| `deletedAt` | `number` | `null` | Date for when the object has been deleted. | `1646071663704` |
| `name` | `string` | `null` | Optional name for the product. | `"A coffee"` |
| `price`* | `number` | | The price for a product in lowest unit. Cents for `EUR`, `USD` etc.  | `4500` |
| `quantity` | `number` | `null` | The number of available products for this type. |
| `minQuantityPerOrder` | `number` | `1` | The minimum quantity pr. order. | `2` |
| `maxQuantityPerOrder` | `number` | `null` | The maximum quantity pr. order | `3` |
| `fields` | `array` | [] | List of `Field` objects. |
| `targets` | `array` | [] | List of `Target` objects. |
| `props` | `object` | `null` | An optional custom object, where anything can be stored. | `{ "foreignRelevantKey": "bd4a0997-39db-41d9-883a-cdfa83e2101f" }` |

#### EventProductTranslation

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `language`* | `string` ([ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)) | | Language code for the translation |  `"da"` |
| `name` | `string` | `null` | See description of field in `EventProduct` |  

### Create an event product

`POST /events/{{eventId}}/product`

## Target

A `Target` where you can filter your audience.

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `role` | `string` | `null` | Choose between some of the standard roles `ALL`, `MEMBERS`, `TICKET_BUYERS` | `MEMBERS`
| `id` | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | `null` | Unique identifier | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `createdAt` | `number` | `null` | Date for when the object has been created. | `1646071663704` |
| `updatedAt` | `number` | `null` | Date for when the object has been updated. | `1646071663704` |
| `deletedAt` | `number` | `null` | Date for when the object has been deleted. | `1646071663704` |
| `name` | `string` | `null` | Name of the target | `"Members, who haven't visited within the last 3 months"` |
| `filter` | `string` | `null` | Filter object to make the query on your audience | `{ $and: { numberOfVisits: ... } }` |


## Field

A `Field` that can be used to get additional information.

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `role` | `string` | `null` | Choose between some of the standard roles `EMAIL`, `FIRST_NAME`, `LAST_NAME`, `FULL_NAME`, `ADDRESS`, `ADDRESS2`, `POSTAL_CODE`, `CITY`, `COUNTRY`, `PHONE` | `FIRST_NAME` |
| `id` | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | `null` | Unique identifier | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `type` | `string` | `null` | Choose between one of these types: `TEXT`, `BIG_TEXT`, `RADIO`, `LIST`, `CHECKBOX` | `TEXT` |
| `createdAt` | `number` | `null` | Date for when the object has been created. | `1646071663704` |
| `updatedAt` | `number` | `null` | Date for when the object has been updated. | `1646071663704` |
| `deletedAt` | `number` | `null` | Date for when the object has been deleted. | `1646071663704` |
| `label` | `string` | `null` | Label of the field | `"Allergies"` |
| `placeholder` | `string` | `null` | Optional placeholder for the field  | `"Type in your allergies"` |
| `note` | `string` | `null` | A note for the field | `"We need to know about your allergies, so we can..."` |
| `required ` | `boolean` | `false` | Whether the field is required | `true` |
| `options` | `array` | `null` | Needed for types `RADIO` and `LIST`. | `[{ label: 'Red', value: 'RED' },...]` |
| `options[].label` | `string` | `null` | The label of the option | `"Green"` |
| `options[].value` | `string` | `null` | The value of the option, that is used behind the scenes | `"GREEN"` |
| `options[].translations` | `array` | `null` | To add translations to each option | `[{ language: 'da', value: 'RED' },...]` |
| `options[].translations[].language` | `string` ([ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)) | `null` | Language code for the translation | `[{ language: 'da', value: 'RED' },...]` |
| `options[].translations[].value` | `string` | `null` | See `options[].value` description | |
| `defaultOption` | `string` | `null` | Whether an option should be preselected. | `"RED"` |
| `props` | `object` | `null` | An optional custom object, where anything can be stored. | `{ "foreignRelevantKey": "bd4a0997-39db-41d9-883a-cdfa83e2101f" }` |

#### FieldTranslation

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `language`* | `string` ([ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)) | | Language code for the translation |  `"da"` |
| `label` | `string` | `null` | See description of field in `Field` |  
| `placeholder` | `string` | `null` | See description of field in `Field` |  
| `note` | `string` | `null` | See description of field in `Field` |  


## Price Variation

A `PriceVariation` can be used to differentiate prices among targets

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `targets` | `array` | [] | List of `Target` objects. |
| `name`* | `string` | | The name of the variation, so users know what this price variation is about. | `"Free for all members!!"` |
| `type`* | `string` |  | Choose between the following types: `ABSOLUTE`, `DISCOUNT`, `DISCOUNT_PERCENTAGE` | `"DISCOUNT_PERCENTAGE"` |
| `value` | `number` | `null` | The value. If `ABSOLUTE`, has been selected, this is the new price. If `DISCOUNT` has been selected, then set how much discount the target gets. If `DISCOUNT_PERCENTAGE` has been selected, then set the percentage. | `0.4` |

#### PriceVariationTranslation

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `language`* | `string` ([ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)) | | Language code for the translation |  `"da"` |
| `name` | `string` | `null` | See description of field in `PriceVariation` |  

## UserActivity

A general `UserActivity` object to contain data about relevant activities for the user

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `id`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | Unique identifier | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `userId`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | Reference to the user | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `description` | `string` | `null` | Something in plain language describing the activity. Templating can be applied with the activity object |  `"Here is your ticket"` |
| `type` | `string` | `null` | Choose between `PURCHASE`, `VISIT`, etc | `PURCHASE` |
| `props` | `object` | `null` | An optional custom object, where anything can be stored. There are standard fields for each type. | `{ "foreignRelevantKey": "bd4a0997-39db-41d9-883a-cdfa83e2101f" }` |

#### Standard fields for activity types

#### `PURCHASE`
```json
"props": [
  {
    "lines": [
      {
        "price": ,
        "currency": ,
        "description": ,
        "quantity": ,
      }
    ]
  }
]
```


# Images

Loby provides a fully functional image generator, that dynamically generates images for requested sizes. The images are quickly generated and cached for fast reuse.

### Url format

Use the following url for generating images:

`https://img.loby.io/{{imageId}}_{{sizingType}}{{size}}.{{format}}`

| component | Description | Example |
| - | - | - |
| `imageId` | The id of the image | `6ef7d631-b898-40a4-ba27-b8c772f6db03`  |
| `sizingType` | There are 3 options: `w`, `h` and `s`. By choosing `w`, image is sized according to width. `h` according to height and `s` will square crop the image and size according to both sides | `w` |
| `size` | The size according to the sizing type | `1280` |
| `format` | You can choose between `jpg` and `png` | `jpg` |


### Example

`https://img.loby.io/6ef7d631-b898-40a4-ba27-b8c772f6db03_w1280.jpg`




# QR Code Specification

naevne noget om hvordan de er formatteret

# Webhooks

Listen for events on your Loby account so your integration can automatically trigger reactions.
Loby uses webhooks to notify your application when an event happens in your account.

## Steps to receive webhooks
1. Create a webhook endpoint as a publicly accessible HTTPS URL.
2. Register the endpoint to Loby via the Dashboard.
3. Identify the events you want to monitor and the event payloads to parse.
4. Handle requests from Loby by parsing each event object and returning 2xx response status codes.

## Event types

| Type | Included object type | Description |
| - | - | - |
| `EVENT_CREATED` | `Event` |  |
| `EVENT_UPDATED` | `Event` |  |
| `EVENT_DELETED` | `Event` |  |
| `EVENT_TICKET_CREATED` | `EventTicket` |  |
| `TICKET_CREATED` | `Ticket` |  |
| `TICKET_TYPE_CREATED` | `TicketType` |  |
| `TICKET_TYPE_UPDATED` | `TicketType` |  |
| `TICKET_TYPE_DELETED` | `TicketType` |  |


# Scanner Hook

You can intercept the default behaviour when scanning QR codes with the Loby Scanner. This can be particularly useful when you want to consume information from the QR codes and use it in other applications.

## Example Use Case
