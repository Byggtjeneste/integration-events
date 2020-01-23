# Item events

The events related to items will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

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
| `event`           | string  | **Required** | Always "Item" for item events.
| `date`            | string  | **Required** | Date and time for the action that triggered the event. In format yyyy-MM-dd hh:mm:ss.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.

# Item Created

## Preconditions
- the item has been published/approved
- only sent once (any later update to an item triggers an "Item updated" event)

## Dependencies
- SupplierCreated (BT internal)
- ModuleCreated
- ManufacturerCreated
- UnitCreated (for priceunit)
- PackageCreated (for bundle items)

## Properties



### data



`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | ------------
| `id`                        | string  | **Required** | GUID (must be generated and can't be changed)
| `nobbNumber`                | integer | **Required** | Must be generated and can't be changed. 8 digits.
| `bundleItems`               | array of objects  | **Optional** | Only used for Display and Composite items 
| `customsEuCode`             | string  | **Optional** | 
| `customsNoCode`             | string  | **Optional** | 
| `dangerousGoods`            | object  | **Optional** |
| `description`               | string  | **Optional** |
| `environmentLabels`         | array of string   | **Optional** |
| `expiryDate`                | string  | **Optional** | yyyy-MM-dd
| `finfoNumber`               | string  | **Optional** |
| `freightGroup`              | string  | **Optional** |
| `hasDurabilityDate`         | boolean | **Required** |
| `hazardLabels`              | array of string   | **Optional** |
| `launchDate`                | string  | **Optional** | yyyy-MM-dd
| `manufacturerItemNumber`    | string  | **Required** |
| `marketingText`             | string  | **Optional** |
| `modelName`                 | string  | **Optional** |
| `moduleNumber`              | integer | **Required** |
| `nrfInfo`                   | object  | **Optional** |
| `priceUnit`                 | string  | **Required** |
| `primaryText`               | string  | **Required** | "Varetekst 1"
| `productNumber`             | integer | **Optional** | Reference to Product
| `itemOwnerItemNumber`       | string  | **Required** | 
| `replacesNobbNumber`        | integer | **Optional** |
| `secondaryText`             | string  | **Optional** | "Varetekst 2" in NOBB domain language.
| `seriesName`                | string  | **Optional** |
| `stocked`                   | boolean | **Required** |
| `tax`                       | string  | **Optional** |
| `toleratesFrost`            | boolean | **Optional** |
| `tunNumber`                 | string  | **Optional** |
| `type`                      | string  | **Required** | Type of item. One of "Standard, Display, Composite, Service"
| `uniqueSellingPoints`       | array of string   | **Optional** |

#### bundleItems



Array type: `object[]`

All items must be of the type: `object` with following properties:

| Property    | Type    | Required     |	Description |
| ----------- | ------- | ------------ | ---------------
| `packageId` | string  | **Required** | GUID of package entity
| `quantity`  | integer | **Required** | 



##### dangerousGoods Type

`object` with following properties:

| Property       | Type    | Required     |	Description |
| -------------- | ------- | ------------ | -------------
| `adrName`      | string    | **Optional** |
| `class`        | string  | **Optional** | Eg "5.1"
| `className`    | string  | **Optional** | Eg "Oxidizing substances"
| `number`       | integer | **Required** |
| `packingGroup` | integer | **Optional** |




##### nrfInfo Type

`object` with following properties:

| Property             | Type    | Required     |	Description |
| -------------------- | ------- | ------------ | --------------
| `additionalText`     | string  | **Optional** |
| `dimension`          | string  | **Optional** |
| `name`               | string  | **Required** |
| `number`             | integer | **Optional** |
| `productGroupNumber` | string  | **Required** | NRF item group number
| `supplierNumber`     | integer | **Required** | NRF Supplier number




## Sample json

```json
{
    "metadata": {
        "eventType": "Create",
        "event": "Item",
        "date": "2019-09-30 12:34:56",
        "author": "Glava AS"
    },
    
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9",
        "nobbNumber": 55556666,

        "moduleNumber": 22223333,
        "productNumber": 77778888,
        "type": "Standard",
        "primaryText": "VINDU FK U12 TRE 13X24",
        "secondaryText": "2-LAGS GLASS, U-VERDI 1,2",
        "marketingText": "Vindu Fastkarm U12 Tre 13X24",
        "seriesName": null,
        "modelName": null,
        "description": "Natre Fastkarm - isolerer bedre...",
        "itemOwnerItemNumber": "ABC123",
        "manufacturerItemNumber": "DEF456",
        "priceUnit": "STK",
        "stocked": true,
        "launchDate": "2019-12-01",
        "expiryDate": "2020-01-01",
        "hasDurabilityDate": false,
        "toleratesFrost": true,
        "replacesNobbNumber": 77778888,
        "freightGroup": "T500",
        "hazardLabels": ["Very Flammable", "Poisonous"],
        "customsNoCode": "1999",
        "customsEuCode": "2999",
        "tax": null,
        "environmentLabels": ["NAAF", "FSC"],
        "dangerousGoods": {
            "number": 1001,
            "class": "5.1",
            "className": "Oxidizing substances",
            "adrName": null,
            "packingGroup": 3,
        },
        "nrfInfo": {
            "number": 5110033,
            "supplierNumber": 273782,
            "productGroupNumber": "5110000",
            "name": "Uponor Teck Endetetning",
            "dimension": "25-54/34",
            "additionalText": "L=117mm",
        },
        "tunNumber": "00001748433",
        "finfoNumber": "004101343",
        "bundleItems": [
            {
                "packageId": "a279846f-6654-4394-84f8-4f99b170fad9",
                "quantity": 2,
            }
        ],
        "uniqueSellingPoints": [
            "Kan leveres med funksjonsglass",
            "Samme profil som øvrige produkter"
        ]
    }
}
```

# Item updated

## Preconditions
- the "Item created" event has already been sent
- the item has been published/approved after the item was updated, or fields that doesn't require approval has been updated. 

## Dependencies
- ItemCreated
- ModuleCreated
- SupplierCreated (BT internal)
- Other dependencies depends on the data being updated (see dependencies for ItemCreated event)

## Note
The identifiers must be part of the event data. Otherwise, only changed fields can be part of the event data. If there is a change inside a list field (like "bundleItems"), the whole list must be part of the event data.


## Properties


### data



`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | ------------
| `id`                        | string  | **Required** | GUID (must be generated and can't be changed)
| `nobbNumber`                | integer | **Required** | Must be generated and can't be changed. 8 digits.
| `bundleItems`               | array of objects  | **Optional** | Only used for Display and Composite items 
| `customsEuCode`             | string  | **Optional** |
| `customsNoCode`             | string  | **Optional** |
| `dangerousGoods`            | object  | **Optional** |
| `description`               | string  | **Optional** |
| `environmentLabels`         | array of string   | **Optional** |
| `expiryDate`                | string  | **Optional** |
| `finfoNumber`               | string  | **Optional** |
| `freightGroup`              | string  | **Optional** |
| `hasDurabilityDate`         | boolean | **Optional** |
| `hazardLabels`              | array of string   | **Optional** |
| `launchDate`                | string  | **Optional** | yyyy-MM-dd
| `manufacturerItemNumber`    | string  | **Optional** |
| `marketingText`             | string  | **Optional** |
| `modelName`                 | string  | **Optional** |
| `moduleNumber`              | integer | **Optional** |
| `nrfInfo`                   | object  | **Optional** |
| `priceUnit`                 | string  | **Optional** |
| `primaryText`               | string  | **Optional** |
| `productNumber`             | integer | **Optional** |
| `itemOwnerItemNumber`       | string  | **Optional** |
| `replacesNobbNumber`        | integer | **Optional** |
| `secondaryText`             | string  | **Optional** |
| `seriesName`                | string  | **Optional** |
| `stocked`                   | boolean | **Optional** |
| `tax`                       | string  | **Optional** |
| `toleratesFrost`            | boolean | **Optional** |
| `tunNumber`                 | string  | **Optional** |
| `type`                      | string  | **Optional** | Type of item. One of "Standard, Display, Composite, Service"
| `uniqueSellingPoints`       | array of string   | **Optional** |

#### bundleItems



Array type: `object[]`

All items must be of the type: `object` with following properties:

| Property    | Type    | Required     | Description
| ----------- | ------- | ------------ | ------------
| `packageId` | string  | **Required** | GUID of package entity
| `quantity`  | integer | **Required** |



##### dangerousGoods Type

`object` with following properties:

| Property       | Type    | Required     | Description
| -------------- | ------- | ------------ | ------------
| `adrName`      | string    **Optional** |
| `class`        | string  | **Optional** |
| `className`    | string  | **Optional** |
| `number`       | integer | **Required** |
| `packingGroup` | integer | **Optional** |




##### nrfInfo Type

`object` with following properties:

| Property             | Type    | Required     | Description
| -------------------- | ------- | ------------ | -----------
| `additionalText`     | string  | **Optional** |
| `dimension`          | string  | **Optional** |
| `name`               | string  | **Required** |
| `number`             | integer | **Optional** |
| `productGroupNumber` | string  | **Required** |
| `supplierNumber`     | integer | **Required** | NRF Supplier number



## Sample json
Example which does the following:
- Removes expiry date
- Updates description
- Updates dangerous goods class
- Updates quantity for a bundled item

```json
{
    "metadata": {
        "eventType": "Update",
        "event": "Item",
        "date": "2019-09-30 12:34:56",
        "author": "Glava AS"
    },
    
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9",
        "nobbNumber": 55556666,

        "expiryDate": null,
        "description": "Natre Fastkarm - isolerer best...",
        "dangerousGoods": {
            "class": "5.2"
        },
        "bundleItems": [
            {
                "packageId": "a279846f-6654-4394-84f8-4f99b170fad9",
                "quantity": 3
            }
        ]
    }
}
```
