# User events

The events related to users will be sent from Byggtjeneste's Subscription System to an Azure Service Bus queue for consumption by the Avensia middleware.

## Properties

| Property              | Type     | Required     | Nullable |
| --------------------- | -------- | ------------ | -------- |
| `data`                | `object` | **Required** | No       |
| `metadata`            | `object` | **Required** | No       |

### metadata

`object` with the following properties:

| Property          | Type    | Required     | Description |
| ------------------| ------- | ------------ | ------- |
| `eventType`       | string  | **Required** | Either "Create", "Update", or "Delete".
| `event`           | string  | **Required** | Always "User" for user events.
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.

# User created

## Preconditions
- The user must exist in Auth0 and be assigned to a company.
- This event won't appear unless a user is added to a subscription.

## Properties

### data

`object` with the following properties:

| Property                | Type    | Required     | Description |
| ----------------------- | ------- | ------------ | ------- |
| `id`                    | string  | **Required** | ID from Auth0
| `email`                 | string  | **Required** |
| `firstName`             | string  | **Required** |
| `surname`               | string  | **Required** |
| `ownerships`            | array of integer | **Optional** | NOBB participant numbers. Only required for role "supplier".
| `roles`                 | array of string | **Required** | One or more of the available roles: "supplier", "nobbadmin", and "nobbsuperadmin".

## Sample JSON

```json
{
    "metadata": {
        "eventType": "Create",
        "event": "User",
        "date": "2019-09-30T12:34:56",
        "author": "Glava AS"
    },
    
    "data": {
        "id": "auth0|103547991597142817347",
        "email": "ola@nordmann.no",
        "firstName": "Ola",
        "surname": "Nordmann",
        "ownerships": [51128, 206198],
        "roles": ["supplier"]
    }
}

```

# User updated

## Preconditions
- The user must be assigned to a subscription.

## Properties

### data

`object` with following properties:

| Property                | Type    | Required     | Description |
| ----------------------- | ------- | ------------ | ------- |
| `id`                    | string  | **Required** | The identifier to the user that should be updated
| `email`                 | string  | **Required** |
| `firstName`             | string  | **Optional** |
| `surname`               | string  | **Optional** |
| `ownerships`            | array of integer  | **Optional** | Any change to this field must include the whole array
| `roles`                 | array of string  | **Optional** | Any change to this field must include the whole array




## Sample JSON
Example of updating roles:
```json
{
    "metadata": {
        "eventType": "Update",
        "event": "User",
        "date": "2019-09-30T12:34:56",
        "author": "Glava AS"
    },
    
    "data": {
        "id": "auth0|103547991597142817347",
        "email": "ola@nordmann.no",
        "roles": ["supplier"]
    }
}

```


# User deleted

## Preconditions
- The user must be assigned to a subscription.

## Properties

### data

`object` with following properties:

| Property                | Type    | Required     | Description |
| ----------------------- | ------- | ------------ | ------- |
| `id`                    | string  | **Required** | The identifier to the user that should be deleted
| `email`                 | string  | **Required** |



## Sample JSON

```json
{
    "metadata": {
        "eventType": "Delete",
        "event": "User",
        "date": "2019-09-30T12:34:56",
        "author": "Glava AS"
    },
    
    "data": {
        "id": "auth0|103547991597142817347",
        "email": "ola@nordmann.no"
    }
}

```