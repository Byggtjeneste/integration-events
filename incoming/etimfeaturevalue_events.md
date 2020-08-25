# ETIM Feature value events

The events related to ETIM Feature values will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

## Message properties

### SessionID: 	<data.nobbNumber>

## Payload properties

| Property              | Type     | Required     | Nullable |
| --------------------- | -------- | ------------ | -------- |
| `data`                | `object` | **Required** | No       |
| `metadata`            | `object` | **Required** | No       |

### metadata

`object` with the following properties:

| Property          | Type    | Required     | Description |
| ------------------| ------- | ------------ | ------- |
| `eventType`       | string  | **Required** | Either "Create" or "Update".
| `event`           | string  | **Required** | Always "EtimFeatureValue" for ETIM Feature value events.
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.


# ETIM Feature value created

## Preconditions
- the item the ETIM Feature value(s) belong to must have been published/approved
- the referenced ETIM Feature(s) must have been published
- only sent once (any later update to an item's ETIM Feature values triggers an "ETIM Feature value updated" event)

## Dependencies
- Item
- ETIM feature(s)

## Properties
### data

`object` with the following properties:

| Property                 | Type             | Required     | Description |
| ------------------------ | ---------------- | ------------ | ----------- |
| `nobbNumber`             | integer          | **Required** | NOBB number of the item.
| `etimFeatures`           | array of objects | **Required** | Array of all ETIM Feature values for the item. Note that this array can be empty, like in update events when all ETIM feature values have been removed from an item.


#### etimFeatures Type

Array type: `object[]`

All items must be of the type: `object` with following properties:

| Property                 | Type         | Required     | Description |
| ------------------------ | ------------ | ------------ | ----------- |
| `code`                   | string       | **Required** | ETIM Feature code.
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
In this sample:
- ETIM Feature EF000008 is of type Logical
- ETIM Feature EF111111 is of type Alphanumeric
- ETIM Feature EF222222 is of type Range

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
        "etimFeatures": [
            {
                "code": "EF000008",
                "value": "true"
            },
            {
                "code": "EF111111",
                "value": "EV333333"
            },
            {
                "code": "EF222222",
                "value": "-10",
                "secondaryValue": "15.2"
            }
        ]
    }
}
```

# ETIM Feature value updated

## Preconditions
- the "ETIM Feature value created" event has already been sent

## Note
The NOBB number and the ETIM features array must be part of the event data. The ETIM features array must include all ETIM feature values for the item, even those that didn't change since the last event was sent.

## Properties
Identical as for create events, see above for more description.


## Sample JSON
Assuming that the sample create event above was sent, then this sample will:
- Remove the value for ETIM Feature EF111111
- Update the values for ETIM Feature EF222222

Note that existing ETIM Feature EF000008 is also part of the event even though it was not changed:

```json
{
    "metadata": {
        "eventType": "Update",
        "event": "EtimFeatureValue",
        "date": "2019-09-30T22:34:56",
        "author": "Glava AS"
    },

    "data": {
        "nobbNumber": 44445555,
        "etimFeatures": [
            {
                "code": "EF000008",
                "value": "true"
            },
            {
                "code": "EF222222",
                "value": "-12",
                "secondaryValue": "24.8"
            }
        ]
    }
}
```
