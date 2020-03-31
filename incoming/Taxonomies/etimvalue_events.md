# ETIM Value events

The events related to ETIM Values will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

## Message properties

### SessionID: 	<data.id>

## Payload properties

| Property              | Type     | Required     | Nullable |
| --------------------- | -------- | ------------ | -------- |
| `data`                | `object` | **Required** | No       |
| `metadata`            | `object` | **Required** | No       |

### metadata

`object` with the following properties:

| Property          | Type    | Required     | Description |
| ------------------| ------- | ------------ | ------- |
| `eventType`       | string  | **Required** | Either "Create" or "Update".
| `event`           | string  | **Required** | Always "EtimValue" for Etim Value events.
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.

# ETIM Value Created 

## Preconditions
- only sent once (any later update to a value triggers a "ETIM Value updated" event)

## Properties
### data

`object` with following properties:


| Property                 | Type    | Required     | Description |
| ------------------------ | ------- | ------------ | -------     |
| `id`                     | string  | **Required** | GUID (must be generated and can't be changed)                |
| `code`                   | string  | **Required** | ETIM Value code. Eg. "EV000007" 
| `description`            | string  | **Required** | ETIM Value description 




## Sample json
```json
{
	"metadata": {
		"eventType": "Create", // string
		"event": "EtimValue", // string
		"date": "2019-09-30T16:34:56",
		"author": "Byggtjeneste" // string
	},
	
	"data": {
		// identifiers
		"id": "4f214662-ba42-491c-b230-37b1420a4db9", 
		"code": "EV000005",
		
		// other data fields
		"description": "Gjengefatning"
	}
}

```


# ETIM Value Updated 

## Preconditions
- the "ETIM Value created" event has already been sent

## Dependencies
- ETIMValueCreated

## Note	
The identifiers must be part of the event data.	Otherwise, only changed fields can be part of the event data. 

## Properties
### data

`object` with following properties:


| Property                 | Type    | Required     | Description |
| ------------------------ | ------- | ------------ | -------     |
| `id`                     | string  | **Required** | GUID (must be generated and can't be changed)                |
| `code`                   | string  | **Required** | ETIM Value code. Eg. "EV000007" 
| `description`            | string  | **Optional** | ETIM Value description 


## Sample json
```json
{
	"metadata": {
		"eventType": "Update", // string
		"event": "EtimValue", // string
		"date": "2019-09-30T16:34:56",
		"author": "Byggtjeneste" // string
	},
	
	"data": {
		// identifiers
		"id": "4f214662-ba42-491c-b230-37b1420a4db9", 
		"code": "EV000008",
		
		// example of changed description
		"description": "Brun"
	}
}
```
