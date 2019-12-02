# Certificate events

These events relates to supplier certificates that will be maintained in the DAM domain within Riversand. Events will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

[Certificate Created](#Certificate-Created)

[Certificate Updated](#Certificate-Updated)

[Certificate Deleted](#Certificate-Deleted)

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
| `event`           | string  | **Required** | Always "Certificate" for certificate events.
| `date`            | string  | **Required** | Date and time for the action that triggered the event. In format yyyy-MM-dd HH:mm:ss.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.


# Certificate Created 

## Preconditions
- The supplier has uploaded a valid certificate in DAM, and selected a valid certificate type


## Note
The identifier must be part of the event data.

## Properties

### data

`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | -------     |
| `id`                        | string  | **Required** |  GUID. Must be generated and can't be changed.
| `type`                      | string | **Required** | Type of certificate, from reference data. Eg. "FSC"
| `participantNumber`    | integer | **Required** | Participant number for certificate owner
| `url`                  	  | string  | **Required** | URL to certificate blob
| `expiryDate`                    | string  | **Optional** | Certificate expiration date, yyyy-MM-dd

## Sample json

```json
{
	"metadata": {
		"eventType": "Create",
		"event": "Certificate",
		"date": "2019-09-30 12:34:56",
		"author": "Glava AS"
	},	
	"data": {
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",		
		"type": "FSC",
		"participantNumber": 51128,
		"url": "https://blobs.riversand.com/certificates/4f214662-ba42-491c-b230-37b1420a4db9",
		"expiryDate": null		
	}
}
```

# Certificate Updated 

## Preconditions
- the "Certificate created" event has already been sent

## Note
The identifier must be part of the event data. Otherwise, only changed fields can be part of the event data.

## Properties

### data

`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | -------     |
| `id`                        | string  | **Required** |  GUID. Must be generated and can't be changed.
| `type`                      | string | **Required** | Type of certificate, from reference data. Eg. "FSC"
| `participantNumber`    	  | integer | **Required** | Participant number for certificate owner
| `url`                  	  | string  | **Required** | URL to certificate blob
| `expiryDate`                    | string  | **Optional** | yyyy-MM-dd


## Sample json

Example of setting "expiryDate" for a certificate

```json
{
	"metadata": {
		"eventType": "Update",
		"event": "Certificate",
		"date": "2019-09-30 12:34:56",
		"author": "Glava AS"
	},
	
	"data": {
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",		
		"expiryDate": "2020-01-01"
	}
}
```

# Certificate Deleted 

## Preconditions
- the "Certificate created" event has already been sent

## Note
The identifier must be part of the event data. 

## Properties

### data

`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | -------     |
| `id`                        | string  | **Required** |  


## Sample json

```json
{
	"metadata": {
		"eventType": "Delete",
		"event": "Certificate",
		"date": "2019-09-30 12:34:56",
		"author": "Glava AS"
	},
	"data": {
		"id": "4f214662-ba42-491c-b230-37b1420a4db9"
	}
}
```