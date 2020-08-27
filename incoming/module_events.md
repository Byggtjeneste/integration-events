# Module events

The events related to modules will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

## Message properties

### SessionID: 	<data.id>

## Payload properties

| Property              | Type     | Required     | Nullable |
| --------------------- | -------- | ------------ | -------- |
| `data`                | `object` | **Required** | No       |
| `metadata`            | `object` | **Required** | No       |

### metadata

`object` with the following properties:

| Property          | Type    | Required     | Description                                                                                                                                                  |
| ------------------| ------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `eventType`       | string  | **Required** | Either "Create" or "Update".                                                                                                                                 |
| `event`           | string  | **Required** | Always "Module" for module events.                                                                                                                           |
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.  |
| `author`          | string  | **Required** | Author of the action that triggered the event.                                                                                                               |

### data
The data model depends on the event type, see below.

# Module Created 

## Preconditions
-  the module has been published/approved
- only sent once (any later update to a module triggers a "Module updated" event)

## Dependencies
- ManufacturerCreated
- ProductGroupCreated
- SupplierCreated (BT internal)

## Properties
### data

`object` with following properties:


| Property                 | Type    | Required     | Description                                                                       | Riversand Comment                                 |
| ------------------------ | ------- | ------------ | --------------------------------------------------------------------------------- | ------------------------------------------------- |
| `id`                     | string  | **Required** | GUID (must be generated and can't be changed)                                     | This will be generated and stored in Middleware.  |
| `number`                 | integer | **Required** | This is the NOBB module number. Must be generated and can't be changed. 8 digits. | thgmoduleno                                       |
| `brand`                  | string  | **Required** | Brand is a new required field in Riversand based on reference data. New modules and changes to existing modules must contain a Brand. However, during data migration the Brand field will have no value. Existing brand names will be migrated to a different read-only field in Riversand. In module events `brand` should contain the value from Brand in Riversand if it exists, otherwise it should contain the value from the read-only field. | thgbrandname |
| `corporateBrand`         | string  | **Optional** |                                                                                   | thgcorporatebrand                                 |
| `description`            | string  | **Optional** |                                                                                   | thgmoduledescription                              |
| `etimClass`              | string  | **Optional** |                                                                                   | thgetimclass                                      |
| `expiryDate`             | string  | **Optional** | yyyy-MM-dd                                                                        | thgmoduleexpiredate                               |
| `internalNumber`         | string  | **Required** |                                                                                   | thginternalmoduleno                               |
| `manufacturerNumber`     | integer | **Required** | Reference to manufacturer (ref.data)                                              | thgmanufacturernumber                             |
| `ownerParticipantNumber` | integer | **Required** | Reference to company                                                              | thgsupplierno                                     |
| `productGroupNumber`     | integer | **Required** | Reference to productgroup (taxonomy)                                              |  **TBD**                                          |
| `text`                   | string  | **Required** | Referred to as Moduletext 1 in NOBB                                               | thgmoduletext                                     |



## Sample json
```json
{
    "metadata": {
        "eventType": "Create",
        "event": "Module",
        "date": "2019-09-30T12:34:56",
        "author": "Glava AS"
    },
    
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9",
        "number": 44445555,

        "productGroupNumber": 1234567,
        "ownerParticipantNumber": 51128,
        "manufacturerNumber": 2002,
        "internalNumber": "ABC123",
        "corporateBrand": "WEBER",
        "brand": "SYS 840",
        "text": "WEBER SYS 840 KLINKEROLJE",
        "description": "En klar luktfri olje for behandling av uglaserte fliser og klinker.",
        "etimClass": "EC000149",
        "expiryDate": "2019-09-30"
    }
}

```


# Module Updated 

## Preconditions
- the "Module created" event has already been sent
- the module has been published/approved after the module was updated

## Dependencies
- ModuleCreated

## Note	
The identifiers must be part of the event data.	Otherwise, only changed fields can be part of the event data.

## Properties
### data

`object` with following properties:


| Property                 | Type    | Required     | Description                                                               | Riversand Comment                                 |
| ------------------------ | ------- | ------------ | ------------------------------------------------------------------------- | ------------------------------------------------- | 
| `id`                     | string  | **Required** | GUID. Must be generated and can't be changed.                             | This will be generated and stored in Middleware.  | 
| `number`                 | integer | **Required** | This is the NOBB module number. Must be generated and can't be changed    | thgmoduleno                                       |
| `brand`                  | string  | **Optional** |                                                                           | thgbrandname                                      |
| `corporateBrand`         | string  | **Optional** |                                                                           | thgcorporatebrand                                 |
| `description`            | string  | **Optional** |                                                                           | thgmoduledescription                              |
| `etimClass`              | string  | **Optional** |                                                                           | thgetimclass                                      |
| `expiryDate`             | string  | **Optional** | yyyy-MM-dd                                                                | thgmoduleexpiredate                               |
| `internalNumber`         | string  | **Optional** |                                                                           | thginternalmoduleno                               |
| `manufacturerNumber`     | integer | **Optional** | Reference to manufacturer (ref.data)                                      | thgmanufacturernumber                             |
| `ownerParticipantNumber` | integer | **Optional** | Reference to company                                                      | thgsupplierno                                     |
| `productGroupNumber`     | integer | **Optional** | Reference to productgroup (taxonomy)                                      | **TBD**                                           |
| `text`                   | string  | **Optional** | Referred to as Moduletext 1 in NOBB                                       | thgmoduletext                                     |



## Sample json
Example of removing expiry date:
```json
{
    "metadata": {
        "eventType": "Update",
        "event": "Module",
        "date": "2019-09-30T12:34:56",
        "author": "Glava AS"
    },
    
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9",
        "number": 44445555,

        "expiryDate": null,
    }
}

```
