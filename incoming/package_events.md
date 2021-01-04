# Package events

The events related to packages will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

[Package Created](#Package-Created)

[Package Updated](#Package-Updated)

[Package Deleted](#Package-Deleted)

## Message properties

### SessionID: 	<data.nobbNumber>

## Payload properties

| Property              | Type     | Required     | Nullable | Description |
| --------------------- | -------- | ------------ | -------- | ------------|
| [data](#data)         | `object` | **Required** | No       |             |
| [metadata](#metadata) | `object` | **Required** | No       |             |

### metadata

`object` with the following properties:

| Property          | Type    | Required     | Description |
| ------------------| ------- | ------------ | ----------- |
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
- for packages that belongs to an item of type Display or Composite, the PackageCreated event must have been sent for packages referred within the "bundleItems" object.

## Dependencies
- ItemCreated
- UnitCreated (unit, consistsOfUnit)
- PackageCreated (bundleItems)

## Properties

### data 

`object` with following properties:

| Property            | Type    | Required when `mainSupplier` is `true` | Required when `mainSupplier` is `false` | Description              | Riversand Comment |
| ------------------- | ------- | ------------ | ------------ | ---------------------------------------------- | ------------------------------------------------ |
| `id`                | string  | **Required** | **Required** | GUID (must be generated and can't be changed)  | This will be generated and stored in Middleware. |
| `nobbNumber`        | integer | **Required** | **Required** | Always main supplier's NOBB number.            | thgnobbno                                        |
| `participantNumber` | integer | **Required** | **Required** | Participant number for the supplier. The supplier is either a main supplier or an alternative supplier. | |
| `mainSupplier`      | boolean | **true**     | **false**    | `true` when the participant number belongs to the main supplier, `false` otherwise. |             |
| `availableFrom`     | string  | **Optional** | **N/A**      | yyyy-MM-dd                                     | thgpackageavailablefrom                          |
| `availableTo`       | string  | **Optional** | **N/A**      | yyyy-MM-dd                                     | thgexpiredate                                    |
| `calculatedCount`   | decimal | **Required** | **N/A**      |                                                | thgcalculatedcount                               |
| `consistsOfCount`   | decimal | **Required** | **N/A**      |                                                | thgconsistsofcount                               |
| `consistsOfUnit`    | string  | **Required** | **N/A**      | From reference data, eg. "STK"                 | thgconsistsofunit (rdunit)                       |
| `dPakLayerCount`    | integer | **Optional** | **N/A**      | Only "T-PAK"                                   | thgdpaklayercount                                |
| `deliverable`       | boolean | **Required** | **Required** |                                                | thgcanbeordered                                  |
| `gtin`              | string  | **Optional** | **N/A**      |                                                | thggtin                                          |
| `height`            | decimal | **Optional** | **N/A**      | millimeters                                    | thgheight                                        |
| `length`            | decimal | **Optional** | **N/A**      | millimeters                                    | thglength                                        |
| `maxStackingWeight` | decimal | **Optional** | **N/A**      | Only "T-PAK"                                   | thgmaxstackingweight                             |
| `minOrderQuantity`  | integer | **Optional** | **N/A**      | Only "F-PAK"                                   | thgminorderquantity                              |
| `packageNumber`     | integer | **Required** | **N/A**      |                                                | **TBD**                                          |
| `stocked`           | boolean | **Required** | **Required** |                                                | thgstocked                                       |
| `type`              | string  | **Optional** | **N/A**      | Either "F-PAK", "D-PAK", "T-PAK", "PSE-PAK", "UDEF", or `null`. `null` is used for packages without a defined type (a.k.a. "UDEF").| Set by Middleware based on RS entity type. | |
| `unit`              | string  | **Required** | **N/A**      | From reference data, eg. "STK"                 | thgpackageunit (rdunit)                          |
| `volume`            | decimal | **Optional** | **N/A**      | dm3                                            | thgvolume                                        |
| `weight`            | decimal | **Optional** | **N/A**      | kg                                             | thgweight                                        |
| `width`             | decimal | **Optional** | **N/A**      | millimeters                                    | thgwidth                                         |
| `bundleItems`       | array of objects | **Optional** | **N/A** | Only used for Display and Composite items  | __relationship__ see `bundleItems Type`          |

### Sub-types (Only relevant when `mainSupplier` is `true`)

#### bundleItems Type

Array type: `object[]`

All items must be of the type: `object` with following properties:

| Property    | Type    | Required     |	Description         | Riversand Comment                                |
| ----------- | ------- | ------------ | ---------------------- | ------------------------------------------------ |
| `packageId` | string  | **Required** | GUID of package entity | This will be generated and stored in Middleware. |
| `quantity`  | integer | **Required** |                        | **TBD**                                          |

Note: 
1. For package events for **F-Pack** that belongs to a **Composite**, if the Composite itself has any standard items and/or special items, retrieve the packageId from the matching **F-pack** of the items.
2. For package events for **D-Pack** that belongs to a **Display**, if the Display itself has any standard items and/or special items, retrieve the packageId from the matching **F-pack** of the items.

## Sample json (main supplier)

```json
{
    "metadata": {
        "eventType": "Create",
        "event": "Package",
        "date": "2019-09-30T12:34:56",
        "author": "Glava AS"
    },
    
    "data": {
        "id": "b7c6081c-7b8e-47fd-8294-b195fe05ae63",
        "nobbNumber": 44445555,
        "participantNumber": 51128,
        "mainSupplier": true,
        "type": "F-PAK",
        "packageNumber": 1,
        "gtin": "022266667777",
        "stocked": true,
        "deliverable": false,
        "unit": "PAK",
        "consistsOfUnit": "STK",
        "consistsOfCount": 5.0,
        "calculatedCount": 10.0,
        "weight": 2.2,
        "height": 100.0,
        "length": 200.0,
        "width": 300.0,
        "volume": 60.0,
        "dPakLayerCount": 4,
        "maxStackingWeight": 100.0,
        "minOrderQuantity": 6,
        "availableFrom": "2019-05-01",
        "availableTo": "2020-05-01",
        "bundleItems": [
            {
                "packageId": "a279846f-6654-4394-84f8-4f99b170fad9",
                "quantity": 2,
            }
        ]
    }
}
```

## Sample json (alternative supplier)
```json
{
    "metadata": {
        "eventType": "Create",
        "event": "Package",
        "date": "2019-09-30T12:34:56",
        "author": "Glava AS"
    },

    "data": {
        "id": "b7c6081c-7b8e-47fd-8294-b195fe05ae63",
        "nobbNumber": 44445555,
        "participantNumber": 201768,
        "mainSupplier": false,

        "stocked": true,
        "deliverable": false
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

| Property            | Type    | Required when `mainSupplier` is `true` | Required when `mainSupplier` is `false` | Description             | Riversand Comment |
| ------------------- | ------- | ------------ | ------------ |---------------------------------------------- | ------------------------------------------------ |
| `id`                | string  | **Required** | **Required** | GUID (must be generated and can't be changed) | This will be generated and stored in Middleware. |
| `participantNumber` | integer | **Required** | **Required** | Participant number for the supplier. The supplier is either a main supplier or an alternative supplier. |
| `mainSupplier`      | boolean | **true**     | **false**    | `true` when the participant number belongs to the main supplier, `false` otherwise. |            |
| `availableFrom`     | string  | **Optional** | **N/A**      | yyyy-MM-dd                                    | thgpackageavailablefrom                          |
| `availableTo`       | string  | **Optional** | **N/A**      | yyyy-MM-dd                                    | thgexpiredate                                    |
| `calculatedCount`   | decimal | **Optional** | **N/A**      |                                               | thgcalculatedcount                               |
| `consistsOfCount`   | decimal | **Optional** | **N/A**      |                                               | thgconsistsofcount                               |
| `consistsOfUnit`    | string  | **Optional** | **N/A**      | From reference data, eg. "STK"                | thgconsistsofunit (rdunit)                       |
| `dPakLayerCount`    | integer | **Optional** | **N/A**      | Only "T-PAK"                                  | thgdpaklayercount                                |
| `deliverable`       | boolean | **Optional** | **Optional** |                                               | thgcanbeordered                                  |
| `gtin`              | string  | **Optional** | **N/A**      |                                               | thggtin                                          |
| `height`            | decimal | **Optional** | **N/A**      | millimeters                                   | thgheight                                        |
| `length`            | decimal | **Optional** | **N/A**      | millimeters                                   | thglength                                        |
| `maxStackingWeight` | decimal | **Optional** | **N/A**      | Only "T-PAK"                                  | thgmaxstackingweight                             |
| `minOrderQuantity`  | integer | **Optional** | **N/A**      | Only "F-PAK"                                  | thgminorderquantity                              |
| `packageNumber`     | integer | **Optional** | **N/A**      |                                               | **TBD**                                          |
| `stocked`           | boolean | **Optional** | **Optional** |                                               | thgstocked                                       |
| `type`              | string  | **Optional** | **N/A**      | Either "F-PAK", "D-PAK", "T-PAK", "PSE-PAK", "UDEF" or `null`. `null` is used for packages without a defined type (a.k.a. "UDEF"). | Set by Middleware based on RS entity type. |
| `unit`              | string  | **Optional** | **N/A**      | From reference data, eg. "STK"                | thgpackageunit (rdunit)                          |
| `volume`            | decimal | **Optional** | **N/A**      | dm3                                           | thgvolume                                        |
| `weight`            | decimal | **Optional** | **N/A**      | kg                                            | thgweight                                        |
| `width`             | decimal | **Optional** | **N/A**      | millimeters                                   | thgwidth                                         |
| `bundleItems`       | array of objects | **Optional** | **N/A** | Only used for Display and Composite items | __relationship__ see `bundleItems Type`          |

### Sub-types (Only relevant when `mainSupplier` is `true`)

#### bundleItems Type

Array type: `object[]`

All items must be of the type: `object` with following properties:

| Property    | Type    | Required     | Description            | Riversand Comment                                |
| ----------- | ------- | ------------ | ---------------------- | ------------------------------------------------ |
| `packageId` | string  | **Required** | GUID of package entity | This will be generated and stored in Middleware. |
| `quantity`  | integer | **Required** |                        | **TBD**                                          |

Note: See rules for setting packageId in the create section

## Sample json (main supplier)
Example which does the following:
- Updates `weight`
- Removes `availableTo`

```json
{
    "metadata": {
        "eventType": "Update",
        "event": "Package",
        "date": "2019-09-30T13:34:56",
        "author": "Glava AS"
    },
    
    "data": {
        "id": "b7c6081c-7b8e-47fd-8294-b195fe05ae63",
        "participantNumber": 51128,
        "mainSupplier": true,

        "weight": 3.2,
        "availableTo": null
    }
}
```

## Sample json (alternative supplier)
Example of updating stocked:
```json
{
    "metadata": {
        "eventType": "Update",
        "event": "Package",
        "date": "2019-09-30T12:34:56",
        "author": "Glava AS"
    },

    "data": {
        "id": "b7c6081c-7b8e-47fd-8294-b195fe05ae63",
        "participantNumber": 201768,
        "mainSupplier": false,

        "stocked": false
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

| Property            | Type    | Required     | Description | Riversand Comment                                |
| ------------------- | ------- | ------------ | ----------- | ------------------------------------------------ |
| `id`                | string  | **Required** |             | This will be generated and stored in Middleware. |
| `participantNumber` | integer | **Required** | Participant number for the supplier. The supplier is either a main supplier or an alternative supplier. | |
| `mainSupplier`      | boolean | **Required** | `true` when the participant number belongs to the main supplier, `false` otherwise. | |



## Sample json

```json
{
    "metadata": {
        "eventType": "Delete",
        "event": "Package",
        "date": "2019-09-30T13:34:56",
        "author": "Glava AS"
    },
    
    "data": {
        "id": "b7c6081c-7b8e-47fd-8294-b195fe05ae63",
        "participantNumber": 51128,
        "mainSupplier": true
    }
}
```
