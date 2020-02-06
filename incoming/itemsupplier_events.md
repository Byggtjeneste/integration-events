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
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
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
| `supplierItemNumber`    | string  | **Required** | 



## Sample json

```json
{
	"metadata": {
		"eventType": "Create",
		"event": "ItemSupplier",
		"date": "2019-09-30T12:34:56",
		"author": "Icopal AS"
	},
	
	"data": {
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"nobbNumber": 55556666,
		"participantNumber": 207168,

		"supplierItemNumber": "ABC123",
		"freightGroup": "100",
		"expiryDate": "2020-01-01",
		"marketingText": "Lorem ipsum"
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
| `supplierItemNumber`    | string  | **Optional** | 



## Sample json
Example of removing expiry date:
```json
{
	"metadata": {
		"eventType": "Update",
		"event": "ItemSupplier",
		"date": "2019-09-30T12:34:56",
		"author": "Icopal AS"
	},
	
	"data": {
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"nobbNumber": 55556666,
		"participantNumber": 207168,

		"expiryDate": null
	}
}
```
