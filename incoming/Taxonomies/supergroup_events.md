# Super Group events

The events related to super groups will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste. Super groups (a.k.a. "overgrupper") is the top level in the NOBB group hierarchy, above main groups ("hovedgrupper") and product groups ("varegrupper").

## Properties

| Property              | Type     | Required     | Nullable |
| --------------------- | -------- | ------------ | -------- |
| `data`                | `object` | **Required** | No       |
| `metadata`            | `object` | **Required** | No       |

### metadata

`object` with the following properties:

| Property          | Type    | Required     | Description |
| ------------------| ------- | ------------ | ------- |
| `eventType`       | string  | **Required** | Either "Create" or "Update".
| `event`           | string  | **Required** | Always "SuperGroup" for super group events.
| `date`            | string  | **Required** | Date and time for the action that triggered the event. In format yyyy-MM-dd HH:mm:ss.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.

# Super Group Created

## Preconditions
- only sent once (any later update to a super group triggers a "Super Group updated" event


## Properties

### data

`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | ------------
| `id`                        | string  | **Required** | GUID. Must be generated and can't be changed.
| `groupNumber`               | string  | **Required** | A unique 2 digit number. The type is string because of potential leading zeros.
| `description`               | string  | **Required** |

## Sample JSON

```json
{
	"metadata": {
		"eventType": "Create",
		"event": "SuperGroup",
		"date": "2019-09-30 16:34:56",
		"author": "Glava AS"
	},
	
	"data": {
		// identifiers
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"groupNumber": "05",
		
		// other data fields
		"description": "Trelast"
	}
}
```

# Super Group updated

## Preconditions
- the "Super Group created" event has already been sent

## Note
The identifiers must be part of the event data. Otherwise, only changed fields can be part of the event data.

## Properties

### data

`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | ------------
| `id`                        | string  | **Required** | GUID. Must be generated and can't be changed.
| `groupNumber`               | string  | **Required** | A unique 2 digit number. The type is string because of potential leading zeros.
| `description`               | string  | **Optional** |


## Sample JSON

```json
{
	"metadata": {
		"eventType": "Update",
		"event": "SuperGroup",
		"date": "2019-09-30 16:34:56",
		"author": "Glava AS"
	},
	
	"data": {
		// Identifiers must always be part of the event and can't change value.
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"groupNumber": "05",

		// An example of updating description.
		"description": "Trelast Furu"
	}
}
```
