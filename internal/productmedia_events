# Product Media events

These events relates to product media (documentations and Images) that will be maintained in the MediaApi. 
Events will be sent from MediaApi to an Azure Service Bus Topic '**productmedia**'.

[ProductMedia Created](#ProductMedia-Created)

[ProductMedia Updated](#ProductMedia-Updated)

[ProductMedia Deleted](#ProductMedia-Deleted)

## Message properties

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
| `event`           | string  | **Required** | Always "ProductMedia".
| `date`            | string  | **Required** | Date and time in UTC for the action in MediaApi that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.

### data
The data model depends on the event type, see below.


# ProductMedia Created 

## Properties

### data

`object` with following properties:

| Property            | Type    | Required     | Description |
| ------------------- | ------- | ------------ | ----------- |
| `id`                | string  | **Required** | Media GUID.
| `type`              | string  | **Required** | Type of Image or Documentation, from reference data. E.g. "PB", "FDV".
| `fileName`          | string  | **Required** | Media file name.
| `participantNumber` | integer | **Required** | Participant number of media owner.
| `nobbNumber`        | integer | **Required** | NOBB number of item that references this media.
| `url`               | string  | **Required** | Full URL to Media file.


## Sample JSON

```json
{
    "metadata": {
        "eventType": "Create",
        "event": "ProductMedia",
        "date": "2019-09-30T16:34:56"
    },	
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9",
        "fileName": "rormansjett.jpg",
        "type": "PB",
        "participantNumber": 51128,
        "nobbNumber": 49831963,
        "url": "https://cdn.byggtjeneste.no/nobb/9397f64b-98ae-48ac-a0d3-10701089fe53/Original"
    }
}
```

# ProductMedia Updated 

## Properties

### data

`object` with following properties:

| Property            | Type    | Required     | Description |
| ------------------- | ------- | ------------ | ----------- |
| `id`                | string  | **Required** | Media GUID.
| `type`              | string  | **Required** | Type of Image or Documentation, from reference data. E.g. "PB", "FDV".
| `fileName`          | string  | **Optional** | Media file name.
| `participantNumber` | integer | **Required** | Participant number of media owner.
| `nobbNumber`        | integer | **Required** | NOBB number of item that references this media.
| `url`               | string  | **Optional** | Full URL to media file.


## Sample JSON

Example of when the document has been referenced from one more modules:

```json
{
    "metadata": {
        "eventType": "Update",
        "event": "ProductMedia",
        "date": "2019-09-30T17:34:56",
    },	
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9",
        "type": "PB",
        "participantNumber": 51128,
        "nobbNumber": 49831963
    }
}
```

# ProductMedia Deleted 

## Properties

### data

`object` with following properties:

| Property            | Type    | Required     | Description |
| ------------------- | ------- | ------------ | ----------- |
| `id`                | string  | **Required** | Media GUID.
| `type`              | string  | **Required** | Type of Image or Documentation, from reference data. E.g. "PB", "FDV".
| `fileName`          | string  | **Optional** | Media file name.
| `participantNumber` | integer | **Required** | Participant number of image owner.
| `nobbNumber`        | integer | **Required** | NOBB number of item that references this media.


## Sample json

```json
{
    "metadata": {
        "eventType": "Delete",
        "event": "ProductMedia",
        "date": "2019-09-30T19:34:56"
    },
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9",
        "type": "PB",
        "participantNumber": 51128,
        "nobbNumber": 49831963
    }
}
```
