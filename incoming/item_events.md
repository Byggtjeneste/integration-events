# Item events

The events related to items will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

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

## Properties



### data



`object` with following properties:

| Property                    | Type                | Required when `mainSupplier` is `true` | Required when `mainSupplier` is `false` | Description                                                    | Riversand Comment                                 |
| --------------------------- | ------------------- | ------------ | ------------ | -------------------------------------------------------------- | ------------------------------------------------- |
| `id`                        | string              | **Required** | **Required** | GUID (must be generated and can't be changed)                  | This will be generated and stored in Middleware.  |
| `nobbNumber`                | integer             | **Required** | **Required** | Must be generated and can't be changed. 8 digits.              | thgnobbno |
| `participantNumber`         | integer             | **Required** | **Required** | Participant number for the supplier. The supplier is either a main supplier or an alternative supplier. |
| `mainSupplier`              | boolean             | **true**     | **false** | `true` when the participant number belongs to the main supplier, `false` otherwise. |
| `accessories`               | array of integers   | **Optional** | **N/A** | Array of NOBB numbers.                                         | __relationships__ [ serviceaccessories, standardaccessories ]  |
| `customsEuCode`             | string              | **Optional** | **N/A** |                                                                | thgeucustomcode |
| `customsNoCode`             | string              | **Optional** | **N/A** |                                                                | thgnocustomcode |
| `dangerousGoods`            | object              | **Optional** | **N/A** |                                                                | see `dangerousGoods Type` |
| `description`               | string              | **Optional** | **N/A** |                                                                | thgdescription |
| `environmentLabels`         | array of string     | **Optional** | **N/A** |                                                                | thgenvironmentlabels |
| `expiryDate`                | string              | **Optional** | **Optional** | In format `yyyy-MM-dd`. Different suppliers can have different expiry dates. | thgexpiredate |
| `finfoNumber`               | string              | **Optional** | **N/A** |                                                                | thgfinfono |
| `freightGroup`              | string              | **Optional** | **Optional** | Different suppliers can have different freight groups.         | thgfreightgroup |
| `hasDurabilityDate`         | boolean             | **Required** | **N/A** |                                                                | thghasdurabilitydate |
| `hazardLabels`              | array of string     | **Optional** | **N/A** |                                                                | thgpackaginglabels |
| `launchDate`                | string              | **Optional** | **N/A** | yyyy-MM-dd                                                     | thglaunchdate |
| `manufacturerItemNumber`    | string              | **Required** | **N/A** |                                                                | thgmanufactureritemno |
| `marketingText`             | string              | **Optional** | **Optional** | Different suppliers can have different marketing texts.        | **TBD** |
| `modelName`                 | string              | **Optional** | **N/A** |                                                                | thgmodelname |
| `moduleNumber`              | integer             | **Required** | **N/A** |                                                                | thgmoduleno |
| `nrfInfo`                   | object              | **Optional** | **N/A** |                                                                | __NRF Taxonomy__ => see `nrfInfo Type` |
| `priceUnit`                 | string              | **Required** | **N/A** |                                                                | thgpriceunit |
| `primaryText`               | string              | **Required** | **N/A** | "Varetekst 1"                                                  | thgtext1 |
| `productNumber`             | integer             | **Optional** | **N/A** | Reference to Product                                           | **TBD** |
| `replacesNobbNumbers`       | array of integers   | **Optional** | **N/A** |                                                                | thgreplacesnobbnumber |
| `secondaryText`             | string              | **Optional** | **N/A** | "Varetekst 2" in NOBB domain language.                         | thgtext2 |
| `seriesName`                | string              | **Optional** | **N/A** |                                                                | thgserialname |
| `stocked`                   | boolean             | **Required** | **N/A** |                                                                | thgstocked |
| `supplierItemNumber`        | string              | **Required** | **Required** | Different suppliers can have different supplier item numbers.  | thgsupplieritemno | 
| `tax`                       | string              | **Optional** | **N/A** |                                                                | thgtax |
| `toleratesFrost`            | boolean             | **Optional** | **N/A** |                                                                | thgtoleratesfrost |
| `tunNumber`                 | string              | **Optional** | **N/A** |                                                                | thgtunno |
| `type`                      | string              | **Required** | **N/A** | Type of item. One of "Standard", "Display", "Composite", "Special", or "Service" | Set by Middleware based on RS entity type. |
| `uniqueSellingPoints`       | array of string     | **Optional** | **N/A** |                                                                | [ thgusp1, thgusp2, thgusp3, thgusp4, thgusp5 ] |
| `videos`                    | array of objects    | **Optional** | **N/A** |                                                                | see `videos Type` |
| `gwpData`                   | object              | **Optional** | **N/A** |                                                                | New attributes to be established in Riversand |



### Sub-types (Only relevant when `mainSupplier` is `true`)

##### dangerousGoods Type

`object` with following properties:

| Property       | Type    | Required     |	Description                     | Riversand Comment                                 |
| -------------- | ------- | ------------ | ------------------------------- | ------------------------------------------------- |
| `adrName`      | string  | **Optional** |                                 | thgadritemname                                    |
| `class`        | string  | **Optional** | Eg "5.1"                        | thgdangerclass => rddangerclass::refcode          |
| `className`    | string  | **Optional** | Eg "Oxidizing substances"       | thgdangerclass => rddangerclass::refvalue         |
| `number`       | integer | **Required** | 4 digit number                  | thgunno                                           |
| `packingGroup` | integer | **Optional** | Emballasjegruppe, ex 1, 2 or 3  | thgpackaginggroup => rdpackaginggroup::refcode    |


##### videos Type

`object` with following properties:

| Property       | Type    | Required     |	Description                     | Riversand Comment                                 |
| -------------- | ------- | ------------ | ------------------------------- | ------------------------------------------------- |
| `type`      | string  | **Required** |    "Vimeo" or "Youtube"                             |                                     |
| `description`        | string  | **Optional** |                        | thgtyoutube=>thgyoutubedescription / thgvimeo=>thgvimedescription |
| `code`    | string  | **Optional** |Vimeo/Youtube ID for the video      | thgtyoutube=>thgyoutubecode / thgvimeo=>thgvimecode |




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
| `supplierNumber`     | integer | **Optional** | NRF Supplier number   | txnrfsuppliernr           |


##### gwpData Type

`object` with following properties:

| Property       | Type    | Required     |	Description                     | Riversand Comment                                 |
| -------------- | ------- | ------------ | ------------------------------- | ------------------------------------------------- |
| `epdId`      | string  | **Optional** |    ID (registration number) of the EPD                             |                                     |
| `A1`        | decimal  | **Optional** |                        |  |
| `A2`        | decimal  | **Optional** |                        |  |
| `A3`        | decimal  | **Optional** |                        |  |
| `A1toA3`        | decimal  | **Optional** |                        |  |
| `validFrom`        | string  | **Optional** |  In format `yyyy-MM-dd`                      |  |
| `validTo`        | string  | **Optional** |   In format `yyyy-MM-dd`                         |  |
| `calculationFactor`        | decimal  | **Optional** |                        |  |




## Sample json (main supplier)

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
        "participantNumber": 51128,
        "mainSupplier": true,

        "moduleNumber": 22223333,
        "productNumber": 77778888,
        "type": "Standard",
        "primaryText": "VINDU FK U12 TRE 13X24",
        "secondaryText": "2-LAGS GLASS, U-VERDI 1,2",
        "marketingText": "Vindu Fastkarm U12 Tre 13X24",
        "seriesName": null,
        "modelName": null,
        "description": "Natre Fastkarm - isolerer bedre...",
        "supplierItemNumber": "ABC123",
        "manufacturerItemNumber": "DEF456",
        "priceUnit": "STK",
        "stocked": true,
        "launchDate": "2019-12-01",
        "expiryDate": "2020-01-01",
        "hasDurabilityDate": false,
        "toleratesFrost": true,
        "replacesNobbNumbers": [44553344, 55667123],
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
        "uniqueSellingPoints": [
            "Kan leveres med funksjonsglass",
            "Samme profil som øvrige produkter"
        ],
        "videos": [
            {
                "type":"Vimeo",
                "description":"A Vimeo video",
                "code": "vimeoId"
            },
            {
                "type":"Youtube",
                "description":"A Youtube video",
                "code": "youtubeId"
            },
        ],
		"gwpData" : {
			"epdId":"NEPD-001-NO",
			"A1":0.0034,
			"A2":1.02345,
			"A3":0.0004,
            "validFrom":"2021-05-01",
            "validTo":"2025-04-30",
			"calculationFactor": 65.1234
		}
    }
}
```

## Sample json (alternative supplier)

```json
{
    "metadata": {
        "eventType": "Create",
        "event": "Item",
        "date": "2019-09-30T12:34:56",
        "author": "Icopal AS"
    },
    
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9",
        "nobbNumber": 55556666,
        "participantNumber": 207168,
        "mainSupplier": false,

        "supplierItemNumber": "ABC123",
        "freightGroup": "100",
        "expiryDate": "2020-01-01",
        "marketingText": "Lorem ipsum"
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
The identifiers must be part of the event data. Otherwise, only changed fields can be part of the event data. If there is a change inside a list field (like "hazardLabels"), the whole list must be part of the event data.


## Properties


### data



`object` with following properties:

| Property                    | Type                | Required when `mainSupplier` is `true` | Required when `mainSupplier` is `false` | Description                                                    | Riversand Comment |
| --------------------------- | ------------------- | ------------ | ------------ | -------------------------------------------------------------- | ----------------- |
| `id`                        | string              | **Required** | **Required** | GUID (must be generated and can't be changed)                  | This will be generated and stored in Middleware. | 
| `nobbNumber`                | integer             | **Required** | **Required** | Must be generated and can't be changed. 8 digits.              | thgnobbno |
| `participantNumber`         | integer             | **Required** | **Required** | Participant number for the supplier. The supplier is either a main supplier or an alternative supplier. |
| `mainSupplier`              | boolean             | **true**     | **false** | `true` when the participant number belongs to the main supplier, `false` otherwise. |
| `accessories`               | array of integers   | **Optional** | **N/A** | Array of NOBB numbers.                                         | __relationships__  [ serviceaccessories, standardaccessories ] |
| `customsEuCode`             | string              | **Optional** | **N/A** |                                                                | thgeucustomcode      |
| `customsNoCode`             | string              | **Optional** | **N/A** |                                                                | thgnocustomcode      |]
| `dangerousGoods`            | object              | **Optional** | **N/A** |                                                                | see `dangerousGoods Type` |
| `description`               | string              | **Optional** | **N/A** |                                                                | thgdescription |
| `environmentLabels`         | array of string     | **Optional** | **N/A** |                                                                | thgenvironmentlabels |
| `expiryDate`                | string              | **Optional** | **Optional** |                                                                | thgexpiredate |
| `finfoNumber`               | string              | **Optional** | **N/A** |                                                                | thgfinfono |
| `freightGroup`              | string              | **Optional** | **Optional** |                                                                | thgfreightgroup |
| `hasDurabilityDate`         | boolean             | **Optional** | **N/A** |                                                                | thghasdurabilitydate |
| `hazardLabels`              | array of string     | **Optional** | **N/A** |                                                                | thgpackaginglabels |
| `launchDate`                | string              | **Optional** | **N/A** | yyyy-MM-dd                                                     | thglaunchdate |
| `manufacturerItemNumber`    | string              | **Optional** | **N/A** |                                                                | thgmanufactureritemno |
| `marketingText`             | string              | **Optional** | **Optional** |                                                                | **TBD**           |
| `modelName`                 | string              | **Optional** | **N/A** |                                                                | thgmodelname |
| `moduleNumber`              | integer             | **Optional** | **N/A** |                                                                | thgmoduleno |
| `nrfInfo`                   | object              | **Optional** | **N/A** |                                                                | __NRF Taxonomy__ => see `nrfInfo Type` |
| `priceUnit`                 | string              | **Optional** | **N/A** |                                                                | thgpriceunit |
| `primaryText`               | string              | **Optional** | **N/A** |                                                                | thgtext1 |
| `productNumber`             | integer             | **Optional** | **N/A** |                                                                | **TBD**           |
| `replacesNobbNumbers`       | array of integers   | **Optional** | **N/A** |                                                                | thgreplacesnobbnumber |
| `secondaryText`             | string              | **Optional** | **N/A** |                                                                | thgtext2 |
| `seriesName`                | string              | **Optional** | **N/A** |                                                                | thgserialname |
| `stocked`                   | boolean             | **Optional** | **N/A** |                                                                | thgstocked |
| `supplierItemNumber`        | string              | **Optional** | **Optional** |                                                                | thgsupplieritemno |
| `tax`                       | string              | **Optional** | **N/A** |                                                                | thgtax |       
| `toleratesFrost`            | boolean             | **Optional** | **N/A** |                                                                | thgtoleratesfrost |  
| `tunNumber`                 | string              | **Optional** | **N/A** |                                                                | thgtunno |
| `type`                      | string              | **Optional** | **N/A** | Type of item. One of "Standard", "Display", "Composite", "Special", or "Service" | Set by Middleware based on RS entity type.|
| `uniqueSellingPoints`       | array of string     | **Optional** | **N/A** |                                                                | [ thgusp1, thgusp2, thgusp3, thgusp4, thgusp5 ] |
| `videos`                    | array of objects    | **Optional** | **N/A** |                                                                | see `videos Type` |
| `gwpData`                   | object              | **Optional** | **N/A** |                                                                | New attributes to be established in Riversand |

### Sub-types (Only relevant when `mainSupplier` is `true`)

##### dangerousGoods Type

`object` with following properties:

| Property       | Type    | Required     | Description                     | Riversand Comment                                 | 
| -------------- | ------- | ------------ | ------------------------------- | ------------------------------------------------- |
| `adrName`      | string  | **Optional** |                                 | thgadritemname                                    |
| `class`        | string  | **Optional** | Eg "5.1"                        | thgdangerclass => rddangerclass::refcode          |
| `className`    | string  | **Optional** | Eg "Oxidizing substances"       | thgdangerclass => rddangerclass::refvalue         |
| `number`       | integer | **Required** | 4 digit number                  | thgunno                                           |
| `packingGroup` | integer | **Optional** | Emballasjegruppe, ex 1, 2 or 3  | thgpackaginggroup => rdpackaginggroup::refcode    |

##### videos Type

`object` with following properties:

| Property       | Type    | Required     |	Description                     | Riversand Comment                                 |
| -------------- | ------- | ------------ | ------------------------------- | ------------------------------------------------- |
| `type`      | string  | **Required** |    "Vimeo" or "Youtube"                             |                                     |
| `description`        | string  | **Optional** |                        | thgtyoutube=>thgyoutubedescription / thgvimeo=>thgvimedescription |
| `code`    | string  | **Optional** |Vimeo/Youtube ID for the video      | thgtyoutube=>thgyoutubecode / thgvimeo=>thgvimecode |


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
| `supplierNumber`     | integer | **Optional** | NRF Supplier number   | txnrfsuppliernr           |


##### gwpData Type

`object` with following properties:

| Property       | Type    | Required     |	Description                     | Riversand Comment                                 |
| -------------- | ------- | ------------ | ------------------------------- | ------------------------------------------------- |
| `epdId`      | string  | **Optional** |    ID (registration number) of the EPD                             |                                     |
| `A1`        | decimal  | **Optional** |                        |  |
| `A2`        | decimal  | **Optional** |                        |  |
| `A3`        | decimal  | **Optional** |                        |  |
| `A1toA3`        | decimal  | **Optional** |                        |  |
| `validFrom`        | string  | **Optional** |  In format `yyyy-MM-dd`                      |  |
| `validTo`        | string  | **Optional** |   In format `yyyy-MM-dd`                         |  |
| `calculationFactor`        | decimal  | **Optional** |                        |  |



## Sample json (main supplier)
Example which does the following:
- Removes expiry date
- Updates description
- Updates dangerous goods class

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
        "participantNumber": 51128,
        "mainSupplier": true,

        "expiryDate": null,
        "description": "Natre Fastkarm - isolerer best...",
        "dangerousGoods": {
            "class": "5.2"
        }
    }
}
```

## Sample json (alternative supplier)
Example of removing expiry date:
```json
{
    "metadata": {
        "eventType": "Update",
        "event": "Item",
        "date": "2019-09-30T12:34:56",
        "author": "Icopal AS"
    },
    
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9",
        "nobbNumber": 55556666,
        "participantNumber": 207168,
        "mainSupplier": false,

        "expiryDate": null
    }
}
```
