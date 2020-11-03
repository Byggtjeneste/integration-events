# EcoProduct Documentation events

These events relates to media documentation that will be maintained in the EcoProduct. Events will be sent from EcoProduct Admin to an Azure Service Bus Topic '**ecoproduct**'.

[EcoProductDocumentation Created](#EcoProductDocumentation-Created)

[EcoProductDocumentation Updated](#EcoProductDocumentation-Updated)

[EcoProductDocumentation Deleted](#EcoProductDocumentation-Deleted)

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
| `event`           | string  | **Required** | Always "EcoProductDocumentation".
| `date`            | string  | **Required** | Date and time in UTC for the action in EcoProduct Admin that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.

### data
The data model depends on the event type, see below.


# EcoProductDocumentation Created 

## Properties

### data

`object` with following properties:

| Property            | Type    | Required     | Description |
| ------------------- | ------- | ------------ | ----------- |
| `id`                | string  | **Required** | GUID base on assessmentId.
| `referencedByItems` | array of integers | **Required** | NOBB numbers of items that references this documentation.
| `url`               | string  | **Required** | Full URL to EcoProductDocumentation file.


## Sample JSON

```json
{
    "metadata": {
        "eventType": "Create",
        "event": "EcoProductDocumentation",
        "date": "2019-09-30T16:34:56"
    },	
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9",
        "referencedByItems": [ 49831963, 49831952 ],
        "url": "https://ecoproduct.com/docs/177"
    }
}
```

# EcoProductDocumentation Updated 

## Preconditions
- the "EcoProductDocumentation created" event has already been sent

## Dependencies
- EcoProductDocumentation Created
- Other dependencies depends on the data being updated (see dependencies for EcoProductDocumentation Created event)

## Note
The identifier must be part of the event data. If there is a change inside a list field "referencedByItems", the whole list must be part of the event data.

## Properties

### data

`object` with following properties:

| Property            | Type    | Required     | Description |
| ------------------- | ------- | ------------ | ----------- |
| `id`                | string  | **Required** | GUID base on assessmentId.
| `referencedByItems` | array of integers | **Required** | NOBB numbers of items that references this documentation.
| `url`               | string  | **Required** | Full URL to EcoProductDocumentation file.


## Sample JSON

Example of when the document has been referenced from one more modules:

```json
{
    "metadata": {
        "eventType": "Update",
        "event": "EcoProductDocumentation",
        "date": "2019-09-30T17:34:56",
    },	
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9",
        "referencedByItems": [ 49831963],
        "url": "https://ecoproduct.com/docs/177"
    }
}
```

# EcoProductDocumentation Deleted 

## Note
The identifier must be part of the event data. 

## Properties

### data

`object` with following properties:

| Property            | Type    | Required     | Description |
| ------------------- | ------- | ------------ | ----------- |
| `id`                | string  | **Required** | GUID base on assessmentId.


## Sample json

```json
{
    "metadata": {
        "eventType": "Delete",
        "event": "EcoProductDocumentation",
        "date": "2019-09-30T19:34:56"
    },
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9"
    }
}
```