# Package events

These events are triggered by the owner of the related item. Changes made by alternative suppliers are separate events. 

The events related to packages will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

[Package Created](#Package-Created)

[Package Updated](#Package-Updated)

[Package Deleted](#Package-Deleted)

## Message properties

### SessionID: 	<data.nobbNumber>

## Properties

| Property              | Type     | Required     | Nullable | Description                            	|
| --------------------- | -------- | ------------ | -------- | ---------------------------------------- |
| [data](#data)         | `object` | **Required** | No       |         					|
| [metadata](#metadata) | `object` | **Required** | No       |         					|

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

| Property            | Type    | Required     | Description 					| Riversand Comment                                 	|
| ------------------- | ------- | ------------ | ---------------------------------------------- | ----------------------------------------------------- |
| `id`                | string  | **Required** | GUID (must be generated and can't be changed)	| This will be generated and stored in Middleware. 	|
| `nobbNumber`        | integer | **Required** |						| thgnobbno 						|
| `availableFrom`     | string  | **Optional** | yyyy-MM-dd					| thgpackageavailablefrom 				|
| `availableTo`       | string  | **Optional** | yyyy-MM-dd					| thgexpiredate 					|
| `calculatedCount`   | integer | **Required** | 						| thgcalculatedcount 					|
| `consistsOfCount`   | integer | **Required** | 						| thgconsistsofcount 					|
| `consistsOfUnit`    | string  | **Required** | From reference data, eg. "STK"			| thgconsistsofunit (rdunit) 				|
| `dPakLayerCount`    | integer | **Optional** | Only "T-PAK"					| thgdpaklayercount 					|	
| `deliverable`       | boolean | **Required** |						| thgcanbeordered 					|
| `gtin`              | string  | **Optional** |						| thggtin 						|	
| `height`            | decimal | **Optional** | millimeters					| thgheight 						|	
| `length`            | decimal | **Optional** | millimeters					| thglength 						|
| `maxStackingWeight` | decimal | **Optional** | Only "T-PAK"					| thgmaxstackingweight 					|
| `minOrderQuantity`  | integer | **Optional** | Only "F-PAK"					| thgminorderquantity 					|
| `packageNumber`     | integer | **Required** |						| **TBD**						|
| `stocked`           | boolean | **Required** |						| thgstocked 						|
| `type`              | string  | **Optional** | Either "F-PAK", "D-PAK", "T-PAK", "PSE-PAK", or `null`. `null` is used for packages without a defined type (a.k.a. "UDEF").| Set by Middleware based on RS entity type.		|
| `unit`              | string  | **Required** | From reference data, eg. "STK"			| thgpackageunit (rdunit) 				|
| `volume`            | decimal | **Optional** | dm3						| thgvolume 						|
| `weight`            | decimal	| **Optional** | kg						| thgweight 						|
| `width`             | decimal | **Optional** | millimeters					| thgwidth 						|


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

| Property            | Type    | Required     | Description 					| Riversand Comment                                 	|
| ------------------- | ------- | ------------ | ---------------------------------------------- | ----------------------------------------------------- |
| `id`                | string  | **Required** | GUID (must be generated and can't be changed)	| This will be generated and stored in Middleware. 	|
| `availableFrom`     | string  | **Optional** | yyyy-MM-dd					| thgpackageavailablefrom 				|
| `availableTo`       | string  | **Optional** | yyyy-MM-dd					| thgexpiredate						|
| `calculatedCount`   | integer | **Optional** | 						| thgcalculatedcount 					|
| `consistsOfCount`   | integer | **Optional** | 						| thgconsistsofcount 					|
| `consistsOfUnit`    | string  | **Optional** | From reference data, eg. "STK"			| thgconsistsofunit (rdunit) 				|
| `dPakLayerCount`    | integer | **Optional** | Only "T-PAK"					| thgdpaklayercount 					|
| `deliverable`       | boolean | **Optional** |						| thgcanbeordered 					|
| `gtin`              | string  | **Optional** |						| thggtin 						|	
| `height`            | decimal | **Optional** | millimeters					| thgheight 						|	
| `length`            | decimal | **Optional** | millimeters					| thglength 						|
| `maxStackingWeight` | decimal | **Optional** | Only "T-PAK"					| thgmaxstackingweight 					|
| `minOrderQuantity`  | integer | **Optional** | Only "F-PAK"					| thgminorderquantity 					|	
| `packageNumber`     | integer | **Optional** |						| **TBD**						|
| `stocked`           | boolean | **Optional** |						| thgstocked 						|
| `type`              | string  | **Optional** | Either "F-PAK", "D-PAK", "T-PAK", "PSE-PAK", or `null`. `null` is used for packages without a defined type (a.k.a. "UDEF"). | Set by Middleware based on RS entity type. 		|
| `unit`              | string  | **Optional** | From reference data, eg. "STK"			| thgpackageunit (rdunit)				|
| `volume`            | decimal | **Optional** | dm3						| thgvolume						|
| `weight`            | decimal | **Optional** | kg						| thgweight 						|
| `width`             | decimal | **Optional** | millimeters					| thgwidth 						|




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

| Property            | Type    | Required     | Description	| Riversand Comment                                |
| ------------------- | ------- | ------------ | -------------- | ------------------------------------------------ |
| `id`                | string  | **Required** |		| This will be generated and stored in Middleware. |



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
