# Certification type events

Certification types are reference data that represents the available certifications a supplier can assign themselves by uploading corresponding certificate. 

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
| `event`           | string  | **Required** | Always "CertificationType" for certification type events.
| `date`            | string  | **Required** | Date and time for the action that triggered the event. In format yyyy-MM-dd HH:mm:ss.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.



# CertificationType Created 

## Preconditions
- only sent once (any later update to a CertificationType triggers a "CertificationType updated" event

## Properties

### data

`object` with following properties:

| Property                | Type    | Required     | Description |
| ----------------------- | ------- | ------------ | ------- |
| `id`                    | string  | **Required** | GUID. Must be generated and can't be changed.
| `code`			      | string | **Required**  | Certification type code. Eg. "FSC". This can't change.
| `name`           		  | string  | **Required** | Descriptive name eg. "FSC sertifikat"
| `description`           | string  | **Optional** | Complementary description 


## Sample json

```json
{
	"metadata": {
		"eventType": "Create", // string
		"event": "CertificationType", // string
		"date": "2019-09-30 18:34:56", // datetime in yyyy-MM-dd HH:mm:ss
		"author": "Byggtjeneste AS" // string
	},
	
	"data": {
		// identifiers
		"id": "4f214662-ba42-491c-b230-37b1420a4db9", 
		"code": "FSC",

		// other fields
		"name": "FSC sertifikat", 
		"description": "FSC (Forest Stewardship Council) er en sertifiseringsordning for skogbruk"
	}
}

```

# CertificationType Updated 

## Preconditions
- the "CertificationType created" event has already been sent

## Note
The identifiers must be part of the event data. Otherwise, only changed fields can be part of the event data. 

## Properties
### data

`object` with following properties:

| Property                | Type    | Required     | Description |
| ----------------------- | ------- | ------------ | ------- |
| `id`                    | string  | **Required** | GUID. Must be generated and can't be changed.
| `code`			      | string | **Required**  | Certification type code. Eg. "FSC". This can't change.
| `name`           		  | string  | **Optional** | Descriptive name eg. "FSC sertifikat"
| `description`           | string  | **Optional** | Complementary description 




# Sample json

```json
{
	"metadata": {
		"eventType": "Update", // string
		"event": "CertificationType", // string
		"date": "2019-09-30 16:34:56", // datetime in yyyy-MM-dd HH:mm:ss
		"author": "Byggtjeneste AS" // string
	},
	
	"data": {
		// identifiers
		"id": "4f214662-ba42-491c-b230-37b1420a4db9", 
		"code": "FSC",

		// Example of change of description
		"description": "FSC (Forest Stewardship Council) er en sertifiseringsordning for skogbruk"
	}
}

```
