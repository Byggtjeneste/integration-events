# User events

The events related to users will be sent from Byggtjeneste's Subscription System to an Azure Service Bus queue for consumption by the Avensia middleware. Note that the events can only contain information that the Subscription System has ownership of. The user information stored in Auth0 can be changed by the user without any involvement of the Subscription System, and can not be part of the events. This includes the user email address, first name, and last name. In case the Avensia middleware would require this information, it must collect it directly from Auth0.

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
| `date`            | string  | **Required** | Date and time for the action that triggered the event. In format yyyy-MM-dd hh:mm:ss.
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
| `ownerships`            | array of integer | **Required** | NOBB participant numbers
| `roles`                 | array of string | **Required** | For example "Supplier"

## Sample JSON

```json
{
	"metadata": {
		"eventType": "Create",
		"event": "User",
		"date": "2019-09-30 12:34:56",
		"author": "Glava AS"
	},
	
	"data": {
		"id": "auth0|103547991597142817347",
		"ownerships": [51128, 206198],
		"roles": ["Supplier"]
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
| `ownerships`            | array of integer  | **Optional** | Any change to this field must include the whole array
| `roles`                 | array of string  | **Optional** | Any change to this field must include the whole array




## Sample JSON

```json
{
	"metadata": {
		"eventType": "Update",
		"event": "User",
		"date": "2019-09-30 12:34:56",
		"author": "Glava AS"
	},
	
	"data": {
		"id": "auth0|103547991597142817347",
        "roles": ["Supplier"]
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




## Sample JSON

```json
{
	"metadata": {
		"eventType": "Update",
		"event": "User",
		"date": "2019-09-30 12:34:56",
		"author": "Glava AS"
	},
	
	"data": {
		"id": "auth0|103547991597142817347"
	}
}

```