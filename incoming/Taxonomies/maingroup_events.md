# Main Group events

The events related to main groups will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste. Main groups (a.k.a. "hovedgrupper") is the second level in the NOBB group hierarchy, below super groups ("overgrupper"), and above product groups ("varegrupper").

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
| `event`           | string  | **Required** | Always "MainGroup" for main group events.
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.

# Main Group Created

## Preconditions
- the super group the main group belongs to must have been published
- only sent once (any later update to a main group triggers a "Main Group updated" event

## Dependencies
- SuperGroupCreated

## Properties

### data

`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | ------------
| `id`                        | string  | **Required** | GUID. Must be generated and can't be changed.
| `groupNumber`               | string  | **Required** | A unique 4 digit number. The type is string because of potential leading zeros.
| `description`               | string  | **Required** |
| `superGroupNumber`          | string  | **Required** | 2 digit identifier to the parent super group.

## Sample JSON

```json
{
	"metadata": {
		"eventType": "Create",
		"event": "MainGroup",
		"date": "2019-09-30T16:34:56",
		"author": "Glava AS"
	},
	
	"data": {
		// identifiers
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"groupNumber": "0502",
		
		// other data fields
		"description": "Konstruksjonsvirke",
		"superGroupNumber": "05"
	}
}
```

# Main Group updated

## Preconditions
- the "Main Group created" event has already been sent

## Dependencies
- MainGroupCreated

## Note
The identifiers must be part of the event data. Otherwise, only changed fields can be part of the event data.

## Properties

### data

`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | ------------
| `id`                        | string  | **Required** | GUID. Must be generated and can't be changed.
| `groupNumber`               | string  | **Required** | A unique 4 digit number. The type is string because of potential leading zeros.
| `description`               | string  | **Optional** |


## Sample JSON

```json
{
	"metadata": {
		"eventType": "Update",
		"event": "MainGroup",
		"date": "2019-09-30T16:34:56",
		"author": "Glava AS"
	},
	
	"data": {
		// Identifiers must always be part of the event and can't change value.
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"groupNumber": "0502",

		// An example of updating description.
		"description": "Konstruksjonsvirke Furu"
	}
}
```
