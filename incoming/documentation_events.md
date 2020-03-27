# Documentation events

These events relates to documentation that will be maintained in the DAM domain within Riversand. Events will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

[Documentation Created](#Documentation-Created)

[Documentation Updated](#Documentation-Updated)

[Documentation Deleted](#Documentation-Deleted)

## Message properties

### SessionID: 	<data.id>

## Payload properties

| Property              | Type     | Required     | Nullable | Description                                |
| --------------------- | -------- | ------------ | -------- | ------------------------------------------ |
| [data](#data)         | `object` | **Required** | No       |         |
| [metadata](#metadata) | `object` | **Required** | No       |         |

### metadata

`object` with the following properties:

| Property          | Type    | Required     | Description |
| ------------------| ------- | ------------ | ------- |
| `eventType`       | string  | **Required** | Either "Create", "Update" or "Delete".
| `event`           | string  | **Required** | Always "Documentation" for documentation events.
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.


# Documentation Created 

## Preconditions
- The supplier has uploaded a valid documentation file in DAM, and selected a valid documentation type.

## Dependencies
- Item Created (for `referencedByItems`)
- Module Created (for `referencedByModules`)
- Product Group Created (for `referencedByProductGroups`)
- MediaType Created (for `type`)
- Supplier Created (for `participantNumber`)

## Properties

### data

`object` with following properties:

| Property            | Type    | Required     | Description |
| ------------------- | ------- | ------------ | ----------- |
| `id`                | string  | **Required** | GUID. Must be generated and can't be changed.
| `contentLanguage`   | string  | **Optional** |
| `copyright`         | string  | **Optional** |
| `copyrightOwner`    | string  | **Optional** |
| `description`       | string  | **Optional** |
| `fileName`          | string  | **Required** | Documentation file name.
| `fileType`          | string  | **Optional** |
| `group`             | string  | **Optional** | Categorization of documentation types.
| `participantNumber` | integer | **Required** | Participant number of documentation owner.
| `referencedByItems` | array of integers | **Optional** | NOBB numbers of items that references this documentation.
| `referencedByModules` | array of integers | **Optional** | Module numbers of modules that references this documentation.
| `referencedByProductGroups` | array of strings | **Optional** | Product group numbers of product groups that references this documentation.
| `size`              | string  | **Optional** |
| `title`             | string  | **Optional** |
| `type`              | string  | **Required** | Type of documentation, from reference data. E.g. "FDV".
| `url`               | string  | **Required** | Full URL to documentation file.
| `validFromDate`     | string  | **Optional** | Date in format yyyy-MM-dd.
| `validToDate`       | string  | **Optional** | Date in format yyyy-MM-dd.


## Sample JSON

```json
{
	"metadata": {
		"eventType": "Create",
		"event": "Documentation",
		"date": "2019-09-30T16:34:56",
		"author": "Glava AS"
	},	
	"data": {
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"fileName": "rormansjett.pdf",
		"participantNumber": 51128,
		"referencedByItems": [ 49831963, 49831952 ],
		"referencedByProductGroups": [ "0930400" ],
		"type": "FDV",
		"url": "https://blobs.riversand.com/docs/e7befba3-0f92-44c6-9b5a-1e88b33b83f3"
	}
}
```

# Documentation Updated 

## Preconditions
- the "Documentation created" event has already been sent

## Dependencies
- Documentation Created
- Other dependencies depends on the data being updated (see dependencies for Documentation Created event)

## Note
The identifier must be part of the event data. Otherwise, only changed fields can be part of the event data. If there is a change inside a list field (like "referencedByItems"), the whole list must be part of the event data.

## Properties

### data

`object` with following properties:

| Property            | Type    | Required     | Description |
| ------------------- | ------- | ------------ | ----------- |
| `id`                | string  | **Required** | GUID. Must be generated and can't be changed.
| `contentLanguage`   | string  | **Optional** |
| `copyright`         | string  | **Optional** |
| `copyrightOwner`    | string  | **Optional** |
| `description`       | string  | **Optional** |
| `fileName`          | string  | **Optional** | Documentation file name.
| `fileType`          | string  | **Optional** |
| `group`             | string  | **Optional** | Categorization of documentation types.
| `participantNumber` | integer | **Optional** | Participant number of documentation owner.
| `referencedByItems` | array of integers | **Optional** | NOBB numbers of items that references this documentation.
| `referencedByModules` | array of integers | **Optional** | Module numbers of modules that references this documentation.
| `referencedByProductGroups` | array of strings | **Optional** | Product group numbers of product groups that references this documentation.
| `size`              | string  | **Optional** |
| `title`             | string  | **Optional** |
| `type`              | string  | **Optional** | Type of documentation, from reference data. E.g. "FDV".
| `url`               | string  | **Optional** | Full URL to documentation file.
| `validFromDate`     | string  | **Optional** | Date in format yyyy-MM-dd.
| `validToDate`       | string  | **Optional** | Date in format yyyy-MM-dd.


## Sample JSON

Example of when the image has been referenced from one more product group:

```json
{
	"metadata": {
		"eventType": "Update",
		"event": "Documentation",
		"date": "2019-09-30T17:34:56",
		"author": "Glava AS"
	},	
	"data": {
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"referencedByProductGroups": [ "0930400", "0930500" ]
	}
}
```

# Documentation Deleted 

## Preconditions
- the "Documentation created" event has already been sent

## Note
The identifier must be part of the event data. 

## Properties

### data

`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | -------     |
| `id`                        | string  | **Required** |  


## Sample json

```json
{
	"metadata": {
		"eventType": "Delete",
		"event": "Documentation",
		"date": "2019-09-30T19:34:56",
		"author": "Glava AS"
	},
	"data": {
		"id": "4f214662-ba42-491c-b230-37b1420a4db9"
	}
}
```