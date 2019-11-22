# Unit events

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
| `event`           | string  | **Required** | Always "Unit" for unit events.
| `date`            | string  | **Required** | Date and time for the action that triggered the event. In format yyyy-MM-dd HH:mm:ss.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.

# Unit Created 

## Preconditions
- only sent once (any later update to a Unit triggers a "Unit updated" event


## Properties
### data

`object` with following properties:

| Property                | Type    | Required     | Description |
| ----------------------- | ------- | ------------ | ------- |
| `id`                    | string  | **Required** | GUID. Must be generated and can't be changed.
| `code`			      | string | **Required**  | Unit abbreviation eg. "CM". This can't change.
| `name`           		  | string  | **Required** | Descriptive name eg. "CENTIMETER"
| `uneceCode`             | string  | **Optional** | Corresponding code from the UNECE standard


## Sample json

```json
{
	"metadata": {
		"eventType": "Create", // string
		"event": "Unit", // string
		"date": "2019-09-30 19:34:56", // datetime in yyyy-MM-dd HH:mm:ss
		"author": "Byggtjeneste AS" // string
	},
	
	"data": {
		// identifiers
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"code": "STK",

		// other fields
		"name": "STYKK", // string
		"uneceCode": "EA" // string, optional
	}
}

```

# Unit Updated 

## Preconditions
- the "Unit created" event has already been sent

## Note
The identifiers must be part of the event data. Otherwise, only changed fields can be part of the event data. 

## Properties
### data

`object` with following properties:

| Property                | Type    | Required     | Description |
| ----------------------- | ------- | ------------ | ------- |
| `id`                    | string  | **Required** | GUID. Must be generated and can't be changed.
| `code`			      | string | **Required** | Unit abbreviation eg. "CM". This can't change.
| `name`          		  | string  | **Optional** | Descriptive name eg. "CENTIMETER"
| `uneceCode`             | string  | **Optional** | Corresponding code from the UNECE standard


# Sample json

```json
{
	"metadata": {
		"eventType": "Update", // string
		"event": "Unit", // string
		"date": "2019-09-30 18:34:56", // datetime in yyyy-MM-dd HH:mm:ss
		"author": "Byggtjeneste AS" // string
	},
	
	"data": {
		// identifiers
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"code": "CM",

		// example of change in name
		"name": "CENTIMETER"
	}
}

```
