# ETIM Class events

The events related to ETIM classes will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

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
| `eventType`       | string  | **Required** | Either "Create", "Update" or "Delete". Only "Create" events will be published in the intial release
| `event`           | string  | **Required** | Always "EtimClass" for ETIM class events.
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.

# ETIM Class Created

## Preconditions
- only sent once (any later update to an ETIM class triggers an "ETIM Class updated" event.
- any ETIM feature/unit/value referenced in the model must have been created beforehand, and the events for creating those must have been sent.


## Properties

### data

`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | ------------
| `id`                        | string  | **Required** | GUID. Must be generated and can't be changed.
| `classCode`                 | string  | **Required** | ETIM class code.
| `classVersion`              | integer | **Required** | ETIM class version.
| `description`               | string  | **Required** | Description in Norwegian.
| `features`                  | array of objects | **Optional** | List of ETIM features in this ETIM class.
| `groupCode`                 | string  | **Required** | ETIM group code.
| `status`                    | string  | **Required** | Either "Published" or "Ready for Publication"

#### features

Array type: `object[]`

All items must be of the type: `object` with following properties:

| Property    | Type    | Required     |	Description |
| ----------- | ------- | ------------ | ---------------
| `id`        | string  | **Required** | GUID of ETIM feature.
| `code`      | string  | **Required** | ETIM feature code.
| `sortOrder` | integer | **Required** |
| `unit`      | object  | **Optional** | ETIM unit.
| `values`    | array of objects | **Optional** | ETIM values.


#### feature.unit

`object` with following properties:

| Property    | Type    | Required     |	Description |
| ----------- | ------- | ------------ | ---------------
| `id`        | string  | **Required** | GUID of ETIM unit.
| `code`      | string  | **Required** | ETIM unit code.


#### feature.values

Array type: `object[]`

All items must be of the type: `object` with following properties:

| Property    | Type    | Required     |	Description |
| ----------- | ------- | ------------ | ---------------
| `id`        | string  | **Required** | GUID of ETIM value.
| `code`      | string  | **Required** | ETIM value code.
| `sortOrder` | integer | **Required** |


## Sample JSON

```json
{
	"metadata": {
		"eventType": "Create",
		"event": "EtimClass",
		"date": "2019-09-30T16:34:56",
		"author": "Glava AS"
	},
	
	"data": {
		"id": "a0823b8b-f886-43e9-ab54-a67726cd0a8c",
		"classCode": "EC002860",

		"classVersion": 2,
		"description": "Trykkluftkompressor",
		"features": [
			{
				"id": "02cd081d-6abc-407e-aebb-2f35301aa817", 
				"code": "EF000008",
				"sortOrder": 1
			},
			{
				"id": "dc58bc43-0494-46fb-9e87-2c0e2e37e07a", 
				"code": "EF000009",
				"sortOrder": 2,
				"unit": {
					"id": "42b602ae-4af8-41f7-a2ae-ff3b9e9a18d4", 
					"code": "EU570399"
				}
			},
			{
				"id": "9c3c4e68-a192-4170-80ca-6339cafd7ba0", 
				"code": "EF002169",
				"sortOrder": 3,
				"values": [
					{
						"id": "a9b159af-a57b-4c86-8a30-7b53547dcf74", 
						"code": "EV000179",
						"sortOrder": 1
					},
					{
						"id": "2fadbb91-6854-414e-969b-abb4cbad7180", 
						"code": "EV000149",
						"sortOrder": 2
					}
				]
			}
		],
		"groupCode": "EG000051",
		"status": "Published"
	}
}
```

# ETIM Class updated

## Preconditions
- the "ETIM Class created" event has already been sent.
- any ETIM feature/unit/value referenced in the model must have been created beforehand, and the events for creating those must have been sent.

## Dependencies
- ETIMClassCreated
- ETIMFeatureCreated for all referenced features
- ETIMUnitCreated for all referenced units
- ETIMValueCreated for all referenced values

## Note
The identifiers must be part of the event data. Otherwise, only changed fields can be part of the event data. If there is a change inside a list field (like "features"), the whole list must be part of the event data.

## Properties

### data

`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | ------------
| `id`                        | string  | **Required** | GUID. Must be generated and can't be changed.
| `classCode`                 | string  | **Required** | ETIM class code.
| `classVersion`              | integer | **Optional** | ETIM class version.
| `description`               | string  | **Optional** | Description in Norwegian.
| `features`                  | array of objects | **Optional** | List of ETIM features in this ETIM class.
| `groupCode`                 | string  | **Optional** | ETIM group code.
| `status`                    | string  | **Optional** | Either "Published" or "Ready for Publication"


#### features

The `features` property is the same as for the "ETIM Class created" event, see above.


## Sample JSON

```json
{
	"metadata": {
		"eventType": "Update",
		"event": "EtimClass",
		"date": "2019-09-30T16:34:56",
		"author": "Glava AS"
	},
	
	"data": {
		// Identifiers must always be part of the event and can't change value.
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"classCode": "EC002860",

		// An example of updating status.
		"status": "Ready for Publication",

		// An example of removing feature with ETIM code EF002169 from the example above.
		// Note that all the other ETIM features are part of the event data.
		// Any missing ETIM feature will be considered as removed.
		"features": [
			{
				"id": "02cd081d-6abc-407e-aebb-2f35301aa817", 
				"code": "EF000008",
				"sortOrder": 1
			},
			{
				"id": "dc58bc43-0494-46fb-9e87-2c0e2e37e07a", 
				"code": "EF000009",
				"sortOrder": 2,
				"unit": {
					"id": "42b602ae-4af8-41f7-a2ae-ff3b9e9a18d4", 
					"code": "EU570399"
				}
			}
		],
	}
}
```
