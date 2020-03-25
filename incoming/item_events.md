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
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
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

| Property                    | Type                | Required     | Description                                                    | Riversand Comment                                 |
| --------------------------- | ------------------- | ------------ | -------------------------------------------------------------- | ------------------------------------------------- |
| `id`                        | string              | **Required** | GUID (must be generated and can't be changed)                  | This will be generated and stored in Middleware.  |
| `nobbNumber`                | integer             | **Required** | Must be generated and can't be changed. 8 digits.              | thgnobbno |
| `accessories`               | array of integers   | **Optional** | Array of NOBB numbers.                                         | **TBD** |
| `bundleItems`               | array of objects    | **Optional** | Only used for Display and Composite items                      | __relationship__ see `bundleItems Type` |
| `customsEuCode`             | string              | **Optional** |                                                                | thgeucustomcode |
| `customsNoCode`             | string              | **Optional** |                                                                | thgnocustomcode |
| `dangerousGoods`            | object              | **Optional** |                                                                | see `dangerousGoods Type` |
| `description`               | string              | **Optional** |                                                                | thgdescription |
| `environmentLabels`         | array of string     | **Optional** |                                                                | thgenvironmentlabels |
| `expiryDate`                | string              | **Optional** | yyyy-MM-dd                                                     | thgexpiredate |
| `finfoNumber`               | string              | **Optional** |                                                                | thgfinfono |
| `freightGroup`              | string              | **Optional** |                                                                | thgfreightgroup |
| `hasDurabilityDate`         | boolean             | **Required** |                                                                | thghasdurabilitydate |
| `hazardLabels`              | array of string     | **Optional** |                                                                | thgpackaginglabels |
| `launchDate`                | string              | **Optional** | yyyy-MM-dd                                                     | thglaunchdate |
| `manufacturerItemNumber`    | string              | **Required** |                                                                | thgmanufactureritemno |
| `marketingText`             | string              | **Optional** |                                                                | **TBD** |
| `modelName`                 | string              | **Optional** |                                                                | thgmodelname |
| `moduleNumber`              | integer             | **Required** |                                                                | thgmoduleno |
| `nrfInfo`                   | object              | **Optional** |                                                                | __NRF Taxonomy__ => see `nrfInfo Type` |
| `priceUnit`                 | string              | **Required** |                                                                | thgpriceunit |
| `primaryText`               | string              | **Required** | "Varetekst 1"                                                  | thgtext1 |
| `productNumber`             | integer             | **Optional** | Reference to Product                                           | **TBD** |
| `itemOwnerItemNumber`       | string              | **Required** |                                                                |  thgsupplieritemno | 
| `replacesNobbNumber`        | integer             | **Optional** |                                                                | thgreplacesnobbnumber |
| `secondaryText`             | string              | **Optional** | "Varetekst 2" in NOBB domain language.                         | thgtext2 |
| `seriesName`                | string              | **Optional** |                                                                | thgserialname |
| `stocked`                   | boolean             | **Required** |                                                                | thgstocked |
| `tax`                       | string              | **Optional** |                                                                | thgtax |
| `toleratesFrost`            | boolean             | **Optional** |                                                                | thgtoleratesfrost |
| `tunNumber`                 | string              | **Optional** |                                                                | thgtunno |
| `type`                      | string              | **Required** | Type of item. One of "Standard, Display, Composite, Service"   | Set by Middleware based on RS entity type. |
| `uniqueSellingPoints`       | array of string     | **Optional** |                                                                | **TBD** |

#### bundleItems Type

Array type: `object[]`

All items must be of the type: `object` with following properties:

| Property    | Type    | Required     |	Description         | Riversand Comment                                 |
| ----------- | ------- | ------------ | ---------------------- | ------------------------------------------------- |
| `packageId` | string  | **Required** | GUID of package entity | This will be generated and stored in Middleware.  |
| `quantity`  | integer | **Required** |                        | **TBD**                                           |



##### dangerousGoods Type

`object` with following properties:

| Property       | Type    | Required     |	Description                     | Riversand Comment                                 |
| -------------- | ------- | ------------ | ------------------------------- | ------------------------------------------------- |
| `adrName`      | string  | **Optional** |                                 | thgadritemname                                    |
| `class`        | string  | **Optional** | Eg "5.1"                        | thgdangerclass => rddangerclass::refcode          |
| `className`    | string  | **Optional** | Eg "Oxidizing substances"       | thgdangerclass => rddangerclass::refvalue         |
| `number`       | integer | **Required** | 4 digit number                  | thgunno                                           |
| `packingGroup` | integer | **Optional** | Emballasjegruppe, ex 1, 2 or 3  | thgpackaginggroup => rdpackaginggroup::refcode    |




##### nrfInfo Type

__Comment:__ All NRF attributes are taxonomy attributes from the NRF taxonomy.

`object` with following properties:

| Property             | Type    | Required     |	Description         | Riversand Comment         |
| -------------------- | ------- | ------------ | --------------------- | ------------------------- |
| `additionalText`     | string  | **Optional** |                       | txnrftilleggsopplysninger |
| `dimension`          | string  | **Optional** |                       | txnrfdimension            |
| `name`               | string  | **Required** |                       | txnrfname                 |
| `number`             | integer | **Optional** |                       | txnrfnumber               | 
| `productGroupNumber` | string  | **Required** | NRF item group number | txnrfvaregruppenr         |
| `supplierNumber`     | integer | **Required** | NRF Supplier number   | txnrfsuppliernr           |



## Sample json

```json
{
    "metadata": {
        "eventType": "Create",
        "event": "Item",
        "date": "2019-09-30T12:34:56",
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
        "accessories": [11223344, 55667788],
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

| Property                    | Type                | Required     | Description                                                    | Riversand Comment |
| --------------------------- | ------------------- | ------------ | -------------------------------------------------------------- | ----------------- |
| `id`                        | string              | **Required** | GUID (must be generated and can't be changed)                  | This will be generated and stored in Middleware. | 
| `nobbNumber`                | integer             | **Required** | Must be generated and can't be changed. 8 digits.              | thgnobbno |
| `accessories`               | array of integers   | **Optional** | Array of NOBB numbers.                                         | **TBD**           |
| `bundleItems`               | array of objects    | **Optional** | Only used for Display and Composite items                      | __Relationship__ see `bundleItems Type`|
| `customsEuCode`             | string              | **Optional** |                                                                | thgeucustomcode      |
| `customsNoCode`             | string              | **Optional** |                                                                | thgnocustomcode      |
| `dangerousGoods`            | object              | **Optional** |                                                                | see `dangerousGoods Type` |
| `description`               | string              | **Optional** |                                                                | thgdescription |
| `environmentLabels`         | array of string     | **Optional** |                                                                | thgenvironmentlabels |
| `expiryDate`                | string              | **Optional** |                                                                | thgexpiredate |
| `finfoNumber`               | string              | **Optional** |                                                                | thgfinfono |
| `freightGroup`              | string              | **Optional** |                                                                | thgfreightgroup |
| `hasDurabilityDate`         | boolean             | **Optional** |                                                                | thghasdurabilitydate |
| `hazardLabels`              | array of string     | **Optional** |                                                                | thgpackaginglabels |
| `launchDate`                | string              | **Optional** | yyyy-MM-dd                                                     | thglaunchdate |
| `manufacturerItemNumber`    | string              | **Optional** |                                                                | thgmanufactureritemno |
| `marketingText`             | string              | **Optional** |                                                                | **TBD**           |
| `modelName`                 | string              | **Optional** |                                                                | thgmodelname |
| `moduleNumber`              | integer             | **Optional** |                                                                | thgmoduleno |
| `nrfInfo`                   | object              | **Optional** |                                                                | __NRF Taxonomy__ => see `nrfInfo Type` |
| `priceUnit`                 | string              | **Optional** |                                                                | thgpriceunit |
| `primaryText`               | string              | **Optional** |                                                                | thgtext1 |
| `productNumber`             | integer             | **Optional** |                                                                | **TBD**           |
| `itemOwnerItemNumber`       | string              | **Optional** |                                                                | thgsupplieritemno |
| `replacesNobbNumber`        | integer             | **Optional** |                                                                | thgreplacesnobbnumber |
| `secondaryText`             | string              | **Optional** |                                                                | thgtext2 |
| `seriesName`                | string              | **Optional** |                                                                | thgserialname |
| `stocked`                   | boolean             | **Optional** |                                                                | thgstocked |
| `tax`                       | string              | **Optional** |                                                                | thgtax |       
| `toleratesFrost`            | boolean             | **Optional** |                                                                | thgtoleratesfrost |  
| `tunNumber`                 | string              | **Optional** |                                                                | thgtunno |
| `type`                      | string              | **Optional** | Type of item. One of "Standard, Display, Composite, Service"   | Set by Middleware based on RS entity type.|
| `uniqueSellingPoints`       | array of string     | **Optional** |                                                                | **TBD** |

#### bundleItems

Array type: `object[]`

All items must be of the type: `object` with following properties:

| Property    | Type    | Required     | Description            | Riversand Comment                                 |
| ----------- | ------- | ------------ | ---------------------- | ------------------------------------------------- |
| `packageId` | string  | **Required** | GUID of package entity | This will be generated and stored in Middleware.  |
| `quantity`  | integer | **Required** |                        | **TBD**                                           |



##### dangerousGoods Type

`object` with following properties:

| Property       | Type    | Required     | Description                     | Riversand Comment                                 | 
| -------------- | ------- | ------------ | ------------------------------- | ------------------------------------------------- |
| `adrName`      | string  | **Optional** |                                 | thgadritemname                                    |
| `class`        | string  | **Optional** | Eg "5.1"                        | thgdangerclass => rddangerclass::refcode          |
| `className`    | string  | **Optional** | Eg "Oxidizing substances"       | thgdangerclass => rddangerclass::refvalue         |
| `number`       | integer | **Required** | 4 digit number                  | thgunno                                           |
| `packingGroup` | integer | **Optional** | Emballasjegruppe, ex 1, 2 or 3  | thgpackaginggroup => rdpackaginggroup::refcode    |



##### nrfInfo Type

__Comment:__ All NRF attributes are taxonomy attributes from the NRF taxonomy.

`object` with following properties:

| Property             | Type    | Required     | Description           | Riversand Comment         |
| -------------------- | ------- | ------------ | --------------------- | ------------------------- |
| `additionalText`     | string  | **Optional** |                       | txnrftilleggsopplysninger |
| `dimension`          | string  | **Optional** |                       | txnrfdimension            |
| `name`               | string  | **Required** |                       | txnrfname                 |
| `number`             | integer | **Optional** |                       | txnrfnumber               |
| `productGroupNumber` | string  | **Required** |                       | txnrfvaregruppenr         |
| `supplierNumber`     | integer | **Required** | NRF Supplier number   | txnrfsuppliernr           |



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
        "date": "2019-09-30T12:34:56",
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
