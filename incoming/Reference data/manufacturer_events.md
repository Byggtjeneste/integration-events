# Manufacturer events

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
| `event`           | string  | **Required** | Always "Manufacturer" for manufacturer events.
| `date`            | string  | **Required** | Date and time for the action that triggered the event. In format yyyy-MM-dd hh:mm:ss.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.

# Manufacturer Created 


## Properties
### data

`object` with following properties:

| Property                | Type    | Required     | Description |
| ----------------------- | ------- | ------------ | ------- |
| `id`                    | string  | **Required** | GUID. Must be generated and can't be changed.
| `number`			      | integer | **Required** | Unique NOBB manufacturer number. Must be generated and can´t be  changed.
| `name`          		  | string  | **Required** | 



## Sample json

```json
{
	"metadata": {
		"eventType": "Create", // string
		"event": "Manufacturer", // string
		"date": "2019-09-30 12:34:56", // datetime in yyyy-MM-dd hh:mm:ss
		"author": "Glava AS" // string
	},
	
	"data": {
		// identifiers
		"id": "4f214662-ba42-491c-b230-37b1420a4db9", // GUID (must be generated and can't be changed)
		"number": 2002, // integer
		
		// other data fields
		"name": "Manufacturer AS" // string
	}
}

```

# Manufacturer Updated 


## Properties
### data

`object` with following properties:

| Property                | Type    | Required     | Description |
| ----------------------- | ------- | ------------ | ------- |
| `id`                    | string  | **Required** | GUID. Must be generated and can't be changed.
| `number`			      | integer | **Required** | Unique NOBB manufacturer number. Must be generated and can´t be  changed.
| `name`          		  | string  | **Required** | 



## Sample json

```json
{
	"metadata": {
		"eventType": "Update", // string
		"event": "Manufacturer", // string
		"date": "2019-09-30 12:34:56", // datetime in yyyy-MM-dd hh:mm:ss
		"author": "Glava AS" // string
	},
	
	"data": {
		// identifiers must always be part of the event and can't change value
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"number": 2002,
		
		// an example of updating name
		"name": "Updated Manufacturer AS"
	}
}


```
