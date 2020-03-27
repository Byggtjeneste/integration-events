# Media type events

Media types are reference data that represents the available document and image types a supplier can upload in DAM.

## Message properties

### SessionID: 	<data.id>

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
| `event`           | string  | **Required** | Always "MediaType" for media type events.
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.



# MediaType Created 

## Preconditions
- only sent once (any later update to a MediaType triggers a "MediaType updated" event

## Properties
### data

`object` with following properties:

| Property                | Type    | Required     | Description |
| ----------------------- | ------- | ------------ | ------- |
| `id`                    | string  | **Required** | GUID. Must be generated and can't be changed.
| `code`			      | string | **Required**  | Media type code. Eg. "FDV". This can't change.
| `name`           		  | string  | **Required** | Descriptive name eg. "Forvaltning, drift og vedlikehold"
| `description`           | string  | **Optional** | Complementary description 


## Sample json

```json
{
	"metadata": {
		"eventType": "Create", // string
		"event": "MediaType", // string
		"date": "2019-09-30T17:34:56",
		"author": "Byggtjeneste AS" // string
	},
	
	"data": {
		// identifiers
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"code": "FDV",

		// other fields
		"name": "Forvaltning, drift og vedlikehold",
		"description": "Denne dokumentasjonen er nøye knyttet til ferdigattesten for nye bygg. Dokumentasjonen skal fortelle hvordan man forvalter, drifter og vedlikeholder produktet og er grunnlaget for FDV-dokumentasjonen for hele bygget."
	}
}

```

# MediaType Updated 

## Preconditions
- the "MediaType created" event has already been sent

## Dependencies
- MediaTypeCreated

## Note
The identifiers must be part of the event data. Otherwise, only changed fields can be part of the event data. 

## Properties
### data

`object` with following properties:

| Property                | Type    | Required     | Description |
| ----------------------- | ------- | ------------ | ------- |
| `id`                    | string  | **Required** | GUID. Must be generated and can't be changed.
| `code`			      | string | **Required**  | Media type code. Eg. "FDV". This can't change.
| `name`           		  | string  | **Optional** | Descriptive name eg. "Forvaltning, drift og vedlikehold"
| `description`           | string  | **Optional** | Complementary description 




# Sample json

```json
{
	"metadata": {
		"eventType": "Update", // string
		"event": "MediaType", // string
		"date": "2019-09-30T19:34:56",
		"author": "Byggtjeneste AS" // string
	},
	
	"data": {
		// identifiers
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"code": "FDV",

		// Example of change of description
		"description": "Denne dokumentasjonen er nøye knyttet til ferdigattesten for nye bygg. Dokumentasjonen skal fortelle hvordan man forvalter, drifter og vedlikeholder produktet og er grunnlaget for FDV-dokumentasjonen for hele bygget."
	}
}

```
