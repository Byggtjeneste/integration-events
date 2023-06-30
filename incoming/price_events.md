# Price events

The events related to prices will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

[Price Created](#Price-Created)

[Price Updated](#Price-Updated)

[Price Deleted](#Price-Deleted)

## Message properties

### SessionID: 	<data.nobbNumber>

## Payload properties

| Property              | Type     | Required     | Nullable | Description                                |
| --------------------- | -------- | ------------ | -------- | ------------------------------------------ |
| [data](#data)         | `object` | **Required** | No       |         |
| [metadata](#metadata) | `object` | **Required** | No       |         |

### metadata

`object` with the following properties:

| Property          | Type    | Required     | Description |
| ------------------| ------- | ------------ | ------- |
| `eventType`       | string  | **Required** | Either "Create", "Update" or "Delete".
| `event`           | string  | **Required** | Always "Price" for price events.
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.


# Price Created 

## Preconditions
- the item the price belongs to must have been published/approved
- the price must have been published/approved
- only sent once (any later update to a price triggers a "Price updated" event)

## Dependencies
- ItemCreated
- SupplierCreated (BT internal)

## Note
The identifier must be part of the event data.

## Properties

### data

`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | -------     |
| `id`                        | string  | **Required** |  GUID. Must be generated and can't be changed.
| `nobbNumber`                | integer | **Required** | 
| `currency`                  | string  | **Required** | Rererence data, eg. "NOK"
| `fromDate`                  | string  | **Required** | yyyy-MM-dd
| `price`                     | decimal | **Required** | 
| `supplierParticipantNumber` | integer | **Required** | 
| `toDate`                    | string    | **Optional** | yyyy-MM-dd


## Sample json

```json
{
	"metadata": {
		"eventType": "Create", // string
		"event": "Price", // string
		"date": "2019-09-30T12:34:56",
		"author": "Glava AS" // string
	},
	
	"data": {
		// identifier
		"id": "4f214662-ba42-491c-b230-37b1420a4db9", // GUID (must be generated and can't be changed)
		
		// other data fields
		"fromDate": "2019-10-01", // date in yyyy-MM-dd
		"toDate": null, // date in yyyy-MM-dd, or null
		"price": 199.0, // decimal
		"currency": "NOK", // string
		"nobbNumber": 12345678, // integer
		"supplierParticipantNumber": 51128 // integer
	}
}
```

# Price Updated 

## Preconditions
- the "Price created" event has already been sent

## Dependencies
- PriceCreated

## Note
The identifier must be part of the event data. Otherwise, only changed fields can be part of the event data.

## Properties

### data

`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | -------     |
| `id`                        | string  | **Required** |  GUID. Must be generated and can't be changed.
| `currency`                  | string  | **Optional** | Rererence data, eg. "NOK"
| `fromDate`                  | string  | **Optional** | yyyy-MM-dd
| `price`                     | decimal | **Optional** | 
| `supplierParticipantNumber` | integer | **Optional** | 
| `toDate`                    | string    | **Optional** | yyyy-MM-dd


## Sample json

```json
{
	"metadata": {
		"eventType": "Update", // string
		"event": "Price", // string
		"date": "2019-10-10T12:34:56",
		"author": "Glava AS" // string
	},

	"data": {
		// the identifier must always be part of the event and can't change value
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		
		// an example of updating toDate
		"toDate": "2019-12-31"
	}
}
```

# Price Deleted 

## Preconditions
- the "Price created" event has already been sent


## Dependencies
- PriceCreated

## Note
The identifier must be part of the event data. 

## Properties

### data

`object` with following properties:

| Property     | Type    | Required     | Description |
| ------------ | ------- | ------------ | -------     |
| `id`         | string  | **Required** | The identifier to the price that was deleted.
| `nobbNumber` | integer | **Required** | 


## Sample json

```json
{
    "metadata": {
        "eventType": "Delete",
        "event": "Price",
        "date": "2019-09-30T12:34:56",
        "author": "Glava AS"
    },
    
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9",
        "nobbNumber": 44445555
    }
}
```