# Package events

These events are triggered by the owner of the related item. Changes made by alternative suppliers are separate events. 

The events related to packages will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

[Package Created](#Package-Created)

[Package Updated](#Package-Updated)

[Package Deleted](#Package-Deleted)


## Properties

| Property              | Type     | Required     | Nullable | Description                                |
| --------------------- | -------- | ------------ | -------- | ------------------------------------------ |
| [data](#data)         | `object` | **Required** | No       |         |
| [metadata](#metadata) | `object` | **Required** | No       |         |

### metadata

`object` with the following properties:

| Property          | Type    | Required     | Description |
| ------------------| ------- | ------------ | ------- |
| `eventType`       | string  | **Required** | Either "Create", "Update" or "Delete".
| `event`           | string  | **Required** | Always "Package" for package events.
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.


# Package Created

## Preconditions
- the item the package belongs to must have been published/approved
- only sent once (any later update to a package triggers a "Package updated" event)

## Dependencies
- ItemCreated
- UnitCreated (unit, consistsOfUnit)

## Properties

### data 

`object` with following properties:

| Property            | Type    | Required     | Description |
| ------------------- | ------- | ------------ | ------- |
| `id`                | string  | **Required** | GUID (must be generated and can't be changed)
| `nobbNumber`        | integer | **Required** |
| `availableFrom`     | string  | **Optional** | yyyy-MM-dd
| `availableTo`       | string  | **Optional** | yyyy-MM-dd
| `calculatedCount`   | integer | **Required** | 
| `consistsOfCount`   | integer | **Required** | 
| `consistsOfUnit`    | string  | **Required** | From reference data, eg. "STK"
| `dPakLayerCount`    | integer | **Optional** |
| `deliverable`       | boolean | **Required** |
| `gtin`              | string  | **Optional** |
| `height`            | decimal | **Optional** | millimeters
| `length`            | decimal | **Optional** | millimeters
| `maxStackingWeight` | decimal | **Optional** | 
| `minOrderQuantity`  | integer | **Optional** |
| `packageNumber`     | integer | **Required** |
| `stocked`           | boolean | **Required** |
| `type`              | string  | **Optional** | Either "F-PAK", "D-PAK", "T-PAK", "PSE-PAK", or `null`. `null` is used for packages without a defined type (a.k.a. "UDEF").
| `unit`              | string  | **Required** | From reference data, eg. "STK"
| `volume`            | decimal | **Optional** | dm3
| `weight`            | decimal  | **Optional** | kg
| `width`             | decimal | **Optional** | millimeters


## Sample json

```json
{
	"metadata": {
		"eventType": "Create", // string
		"event": "Package", // string
		"date": "2019-09-30T12:34:56",
		"author": "Glava AS" // string
	},
	
	"data": {
		// identifier
		"id": "b7c6081c-7b8e-47fd-8294-b195fe05ae63", // GUID

		// other data fields
		"nobbNumber": 44445555, // integer
		"type": "F-PAK", // string
		"packageNumber": 1, // integer
		"gtin": "022266667777", // string, optional
		"stocked": true, // boolean
		"deliverable": false, // boolean
		"unit": "PAK", // string
		"consistsOfUnit": "STK", // string
		"consistsOfCount": 5.0, // decimal
		"calculatedCount": 10.0, // decimal
		"weight": 2.2, // decimal (kg), optional
		"height": 100.0, // decimal (millimeters), optional
		"length": 200.0, // decimal (millimeters), optional
		"width": 300.0, // decimal (millimeters), optional
		"volume": 60.0, // decimal (dm^3), optional
		"dPakLayerCount": 4, // integer, optional
		"maxStackingWeight": 100.0, // decimal (kg), optional
		"minOrderQuantity": 6, // integer, optional
		"availableFrom": "2019-05-01", // date in yyyy-MM-dd, optional
		"availableTo": "2020-05-01", // date in yyyy-MM-dd, optional
	}
}
```

# Package Updated

## Preconditions
- the "Package created" event has already been sent
- the item the package belongs to has been published/approved after the package was updated

## Dependencies
- PackageCreated

## Note
The identifier must be part of the event data. Otherwise, only changed fields can be part of the event data.


## Properties


### data

`object` with following properties:

| Property            | Type    | Required     | Description |
| ------------------- | ------- | ------------ | ------- |
| `id`                | string  | **Required** | GUID (must be generated and can't be changed)
| `availableFrom`     | string  | **Optional** | yyyy-MM-dd
| `availableTo`       | string  | **Optional** | yyyy-MM-dd
| `calculatedCount`   | integer | **Optional** | 
| `consistsOfCount`   | integer | **Optional** | 
| `consistsOfUnit`    | string  | **Optional** | From reference data, eg. "STK"
| `dPakLayerCount`    | integer | **Optional** |
| `deliverable`       | boolean | **Optional** |
| `gtin`              | string  | **Optional** |
| `height`            | decimal | **Optional** | millimeters
| `length`            | decimal | **Optional** | millimeters
| `maxStackingWeight` | decimal | **Optional** | 
| `minOrderQuantity`  | integer | **Optional** |
| `packageNumber`     | integer | **Optional** |
| `stocked`           | boolean | **Optional** |
| `type`              | string  | **Optional** | Either "F-PAK", "D-PAK", "T-PAK", "PSE-PAK", or `null`. `null` is used for packages without a defined type (a.k.a. "UDEF").
| `unit`              | string  | **Optional** | From reference data, eg. "STK"
| `volume`            | decimal | **Optional** | dm3
| `weight`            | decimal  | **Optional** | kg
| `width`             | decimal | **Optional** | millimeters




## Sample json

```json
{
	"metadata": {
		"eventType": "Update", // string
		"event": "Package", // string
		"date": "2019-09-30T13:34:56",
		"author": "Glava AS" // string
	},
	
	"data": {
		// the identifier must always be part of the event and can't change value
		"id": "b7c6081c-7b8e-47fd-8294-b195fe05ae63",
		
		// an example of updating weight
		"weight": 3.2,
		
		// an example of removing availableTo
		"availableTo": null
	}
}
```
# Package Deleted

## Preconditions
- the "Package created" event has already been sent
- the item the package belongs to has been published/approved after the package was deleted

## Dependencies
- PackageCreated

## Note
The identifier must be part of the event data.

##  Properties


### data

`object` with following properties:

| Property            | Type    | Required     | Description |
| ------------------- | ------- | ------------ | ------- |
| `id`                | string  | **Required** |



## Sample json

```json
{
	"metadata": {
		"eventType": "Delete", // string
		"event": "Package", // string
		"date": "2019-09-30T13:34:56",
		"author": "Glava AS" // string
	},
	
	"data": {
		// the identifier to the package that was deleted
		"id": "b7c6081c-7b8e-47fd-8294-b195fe05ae63"
	}
}
```
