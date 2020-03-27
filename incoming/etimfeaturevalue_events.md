# ETIM Feature value events

The events related to ETIM Feature values will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

## Message properties

### SessionID: 	<data.nobbNumber>

## Properties

| Property              | Type     | Required     | Nullable |
| --------------------- | -------- | ------------ | -------- |
| `data`                | `object` | **Required** | No       |
| `metadata`            | `object` | **Required** | No       |

### metadata

`object` with the following properties:

| Property          | Type    | Required     | Description |
| ------------------| ------- | ------------ | ------- |
| `eventType`       | string  | **Required** | Either "Create", "Update" or "Delete".
| `event`           | string  | **Required** | Always "EtimFeatureValue" for ETIM Feature value events.
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.


# ETIM Feature value created

## Preconditions
- the item the ETIM Feature value belongs to must have been published/approved
- the referenced ETIM Feature must have been published
- only sent once (any later update to an ETIM Feature value triggers an "ETIM Feature value updated" event)

## Dependencies
- Item
- ETIM feature

## Properties
### data

`object` with the following properties:

| Property                 | Type         | Required     | Description |
| ------------------------ | ------------ | ------------ | ----------- |
| `nobbNumber`             | integer      | **Required** | NOBB number of the item.
| `etimFeatureCode`        | string       | **Required** | ETIM Feature code.
| `value`                  | string       | **Required** | ETIM Feature value for the item. See below for more description.
| `secondaryValue`         | string       | **Optional** | Secondary ETIM Feature value for the item. Only used when ETIM Feature datatype is "R" (Range). See below for more description.


The ETIM Feature value depends on the datatype of the ETIM Feature:

| ETIM Feature datatype | Allowed value in `value` field    | Allowed value in `secondaryValue` field |
| --------------------- | --------------------------------- | --------------------------------------- |
| "A" (Alphanumeric)    | ETIM Value code, e.g. `"EV000007"`. | N/A
| "L" (Logical)         | String representation of boolean value, either `"true"`, or `"false"`. | N/A
| "N" (Numeric)         | String representation of numeric value, with "." as decimal separator. E.g. `"50"`, `"-20"`, or `"4.5"`. | N/A
| "R" (Range)           | Same as for Numeric, with the lower value of the range. | Same as for Numeric, with the upper value of the range.


## Sample JSON
```json
{
    "metadata": {
        "eventType": "Create",
        "event": "EtimFeatureValue",
        "date": "2019-09-30T16:34:56",
        "author": "Glava AS"
    },

    "data": {
        "nobbNumber": 44445555,
        "etimFeatureCode": "EF000008",
        "value": "true"
    }
}
```

# ETIM Feature value updated

## Preconditions
- the "ETIM Feature value created" event has already been sent

## Note
NOBB number and ETIM Feature code must be part of the event data. Otherwise, only changed fields can be part of the event data.

## Properties
### data

`object` with the following properties:

| Property                 | Type         | Required     | Description |
| ------------------------ | ------------ | ------------ | ----------- |
| `nobbNumber`             | integer      | **Required** | NOBB number of the item.
| `etimFeatureCode`        | string       | **Required** | ETIM Feature code.
| `value`                  | string       | **Optional** | Same rules as in create events, see above for more description.
| `secondaryValue`         | string       | **Optional** | Same rules as in create events, see above for more description.


## Sample JSON
Example of changing the ETIM Feature value for an ETIM Feature of type "N" (Numeric):
```json
{
    "metadata": {
        "eventType": "Update",
        "event": "EtimFeatureValue",
        "date": "2019-09-30T16:34:56",
        "author": "Glava AS"
    },

    "data": {
        "nobbNumber": 44445555,
        "etimFeatureCode": "EF550005",
        "value": "1.25"
    }
}
```

# ETIM Feature value deleted

## Preconditions
- the "ETIM Feature value created" event has already been sent

## Note
NOBB number and ETIM Feature code must be part of the event data.

## Properties
### data

`object` with the following properties:

| Property                 | Type         | Required     | Description |
| ------------------------ | ------------ | ------------ | ----------- |
| `nobbNumber`             | integer      | **Required** | NOBB number of the item.
| `etimFeatureCode`        | string       | **Required** | ETIM Feature code.


## Sample JSON
```json
{
    "metadata": {
        "eventType": "Delete",
        "event": "EtimFeatureValue",
        "date": "2019-09-30T16:34:56",
        "author": "Glava AS"
    },

    "data": {
        "nobbNumber": 44445555,
        "etimFeatureCode": "EF550005"
    }
}
```