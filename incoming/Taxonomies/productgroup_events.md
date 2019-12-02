# Product Group events

The events related to product groups will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste. Product groups (a.k.a. "varegrupper") is the third level in the NOBB group hierarchy, below main groups ("hovedgrupper") and super groups ("overgrupper").

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
| `event`           | string  | **Required** | Always "ProductGroup" for product group events.
| `date`            | string  | **Required** | Date and time for the action that triggered the event. In format yyyy-MM-dd HH:mm:ss.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.



# Product Group Created

## Preconditions
- the main group the product group belongs to must have been published
- only sent once (any later update to a product group triggers a "Product Group updated" event

## Dependencies
- MainGroupCreated
- UnitCreated (primaryPseUnit, secondaryPseUnit)

## Properties

### data





`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | ------------
| `id`                        | string  | **Required** | GUID. Must be generated and can't be changed.
| `groupNumber`               | string  | **Required** | A unique 7 digit number. The type is string because of potential leading zeros.
| `description`               | string  | **Required** |
| `mainGroupNumber`           | string  | **Required** | 4 digit identifier to the parent main group.
| `primaryPseUnit`            | string  | **Optional** | One from units reference data.
| `recommendedDocumentation`  | array of string | **Optional** | Documentation types, e.g. "FDV"
| `secondaryPseUnit`          | string  | **Optional** | One from units reference data.
| `unspscNumber`              | integer | **Optional** |


## Sample JSON

```json
{
	"metadata": {
		"eventType": "Create",
		"event": "ProductGroup",
		"date": "2019-09-30 16:34:56",
		"author": "Glava AS"
	},
	
	"data": {
		// identifiers
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"groupNumber": "0502100",
		
		// other data fields
		"description": "K-Virke Furu/Gran",
		"mainGroupNumber": "0502",
		"primaryPseUnit": "LM",
		"recommendedDocumentation": [ "FDV" ],
		"secondaryPseUnit": null,
		"unspscNumber": 30103601
	}
}
```


# Product Group updated

## Preconditions
- the "Product Group created" event has already been sent

## Dependencies
- ProductGroupCreated

## Note
The identifiers must be part of the event data. Otherwise, only changed fields can be part of the event data. If there is a change inside a list field (like "recommendedDocumentation"), the whole list must be part of the event data.


## Properties

### data

`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | ------------
| `id`                        | string  | **Required** | GUID. Must be generated and can't be changed.
| `groupNumber`               | string  | **Required** | A unique 7 digit number. The type is string because of potential leading zeros.
| `description`               | string  | **Optional** |
| `primaryPseUnit`            | string  | **Optional** | One from units reference data.
| `recommendedDocumentation`  | array of string | **Optional** | Documentation types, e.g. "FDV"
| `secondaryPseUnit`          | string  | **Optional** | One from units reference data.
| `unspscNumber`              | integer | **Optional** |



## Sample JSON

```json
{
	"metadata": {
		"eventType": "Update",
		"event": "ProductGroup",
		"date": "2019-09-30 16:34:56",
		"author": "Glava AS"
	},
	
	"data": {
		// Identifiers must always be part of the event and can't change value.
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"groupNumber": "0502100",

		// An example of updating description.
		"description": "K-Virke Furu",

		// An example of adding a recommended documentation type.
		// Must supply the whole list for any changes inside the list.
		"recommendedDocumentation": [ "FDV", "SER" ]
	}
}
```
