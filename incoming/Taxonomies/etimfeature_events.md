# ETIM Feature events

The events related to ETIM Features will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

### Note: Only "Create" events will be published in the intial release

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
| `eventType`       | string  | **Required** | Either "Create" or "Update". Only "Create" events will be published in the intial release
| `event`           | string  | **Required** | Always "EtimFeature" for Etim Feature events.
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.

# ETIM Feature Created 

## Preconditions
- only sent once (any later update to a feature triggers a "Feature updated" event)


## Properties
### data

`object` with following properties:


| Property                 | Type    | Required     | Description |
| ------------------------ | ------- | ------------ | -------     |
| `id`                     | string  | **Required** | GUID (must be generated and can't be changed)                |
| `code`                   | string  | **Required** | ETIM Feature code. Eg. "EF000007" 
| `description`            | string  | **Required** | ETIM Feature description 
| `type`            	   | string  | **Required** | ETIM Feature datatype. One of "N" (Numeric), "R" (Range), "L" (Logical), "A" (Alphanumeric)       |


## Sample json
```json
{
	"metadata": {
		"eventType": "Create", // string
		"event": "EtimFeature", // string
		"date": "2019-09-30T16:34:56",
		"author": "Byggtjeneste" // string
	},
	
	"data": {
		// identifiers
		"id": "4f214662-ba42-491c-b230-37b1420a4db9", 
		"code": "EF000008",
		
		// other data fields
		"description": "Bredde",
		"type": "N",
	}
}

```


# ETIM Feature Updated 

## Preconditions
- the "ETIM Feature created" event has already been sent

## Dependencies
- ETIMFeatureCreated

## Note	
The identifiers must be part of the event data.	Otherwise, only changed fields can be part of the event data. 

## Properties
### data

`object` with following properties:



| Property                 | Type    | Required     | Description |
| ------------------------ | ------- | ------------ | -------     |
| `id`                     | string  | **Required** | GUID (must be generated and can't be changed)                |
| `code`                   | string  | **Required** | ETIM Feature code. Eg. "EF000007". This can't change.  
| `description`            | string  | **Optional** | ETIM Feature description 
| `type`            	   | string  | **Optional** | ETIM Feature datatype. One of "N" (Numeric), "R" (Range), "L" (Logical), "A" (Alphanumeric)       |



## Sample json
```json
{
	"metadata": {
		"eventType": "Update", // string
		"event": "EtimFeature", // string
		"date": "2019-09-30T16:34:56",
		"author": "Byggtjeneste" // string
	},
	
	"data": {
		// identifiers
		"id": "4f214662-ba42-491c-b230-37b1420a4db9", 
		"code": "EF000008",
		
		// example of changed description
		"description": "Bredde",
	}
}
```
