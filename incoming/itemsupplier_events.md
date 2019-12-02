# Item supplier events


 These events relates to what is known as alternative suppliers within Riversand. The events related to item suppliers will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

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
| `event`           | string  | **Required** | Always "ItemSupplier" for item supplier events.
| `date`            | string  | **Required** | Date and time for the action that triggered the event. In format yyyy-MM-dd hh:mm:ss.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.



# Item supplier created

## Preconditions
- the item has been published/approved
- only sent once (any later update to an item triggers an "Item updated" event)

## Dependencies
- ItemCreated
- SupplierCreated (BT internal)


## Properties

	

### data


`object` with following properties:

| Property                | Type    | Required     | Description |
| ----------------------- | ------- | ------------ | ------- |
| `id`                    | string  | **Required** | GUID. Must be generated and can't be changed
| `expiryDate`            | string  | **Optional** | yyyy-MM-dd
| `freightGroup`          | string  | **Optional** | 
| `marketingText`         | string  | **Optional** | 
| `nobbNumber`            | integer | **Required** | 
| `participantNumber`     | integer | **Required** | Alternative suppliers participant number
| `supplierItemNumber` | string  | **Required** | 
| `uniqueSellingPoints`   | array of string   | **Optional** | 




## Sample json

```json
{
	"metadata": {
		"eventType": "Create", // string
		"event": "ItemSupplier", // string
		"date": "2019-09-30 12:34:56", // datetime in yyyy-MM-dd hh:mm:ss
		"author": "Icopal AS" // string
	},
	
	"data": {
		// identifiers
		"id": "4f214662-ba42-491c-b230-37b1420a4db9", // GUID (must be generated and can't be changed)
		"nobbNumber": 55556666, // integer
		"participantNumber": 207168, // integer
		
		// other data fields
		"supplierItemNumber": "ABC123", // string
		"freightGroup": "100", // string, optional
		"expiryDate": "2020-01-01", // date in yyyy-MM-dd
		"marketingText": "Lorem ipsum", // string, optional
		"uniqueSellingPoints": [ // list of strings, optional
			"Slipper inn mye lys",
			"For store glassfasader"
		]
	}
}
```

# Item supplier updated

## Dependencies
- ItemSupplierCreated

## Properties

### data

`object` with following properties:

| Property                | Type    | Required     | Description |
| ----------------------- | ------- | ------------ | ------- |
| `id`                    | string  | **Required** | GUID. Must always be part of the event and can't change value
| `expiryDate`            | string  | **Optional** | 
| `freightGroup`          | string  | **Optional** | 
| `marketingText`         | string  | **Optional** | 
| `nobbNumber`            | integer | **Required** | 
| `participantNumber`     | integer | **Required** | 
| `supplierItemNumber` | string  | **Optional** | 
| `uniqueSellingPoints`   | array of string   | **Optional** | If any item within the array has changed, the whole list must be supplied




## Sample json

```json
{
	"metadata": {
		"eventType": "Update", // string
		"event": "ItemSupplier", // string
		"date": "2019-09-30 12:34:56", // datetime in yyyy-MM-dd hh:mm:ss
		"author": "Icopal AS" // string
	},
	
	"data": {
		// identifiers must always be part of the event and can't change value
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"nobbNumber": 55556666,
		"participantNumber": 207168,
		
		// an example of removing expiryDate
		"expiryDate": null,
		
		// an example of updating uniqueSellingPoints
		"uniqueSellingPoints": ["Slipper inn mye lys"]
	}
}
```

