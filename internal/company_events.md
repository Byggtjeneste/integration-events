# Company events

The events related to companies will be sent from Byggtjeneste's Subscription System to an Azure Service Bus queue for consumption by internal subscribers. 

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
| `event`           | string  | **Required** | Always "Company" for company events.
| `date`            | string  | **Required** | Date and time in UTC for the action that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.

# Company created

## Properties

### data

`object` with the following properties:

| Property                | Type    		| Required     | Description 															
| ----------------------- | ------------| ------------ | -------------------------------------------------------------- 
| `id`                    | integer  		| **Required** |  Internal ID for Company generated by Subscription System.											
| `legalName`             | string 		| **Required** | 																
| `name`                  | string 		| **Required** | 
| `logoUrl`               | string  		| **Optional** | 																			
| `isVvsCompany`          | boolean 		| **Required** | Internal usage for Nobb.no						
| `efoParticipantNumbers`  | string 	| **Optional** | Comma separated list of Participant number(s) for company in EFO database											
| `nrfParticipantNumbers`  | string 	| **Optional** | Comma separated list of Participant number(s) for company in NRF database											
| `glnNumber`            | string 	| **Optional** | Global Location Number						
| `organizationNumber`               | object  		| **Required** | 
| `address`               | object  		| **Optional** | 							
| `billingAddress`        | object  		| **Optional** | 
| `parentCompanyId`       | integer  		| **Optional** | ID of parent company. Default is null.												
| `participants`         | array of objects 	| **Required** |	

#### address

`object` with the following properties:

| Property     | Type   | Required     |  Description 		
| ------------ | ------ | ------------ |  --------------------
| `addressLine`    | string | **Optional** | 
| `countryCode`    | string | **Optional** | 2 character ISO code
| `postalArea` | string | **Optional** | 
| `postalCode` | string | **Optional** | 
| `email` | string | **Optional** | 


#### billingAddress

`object` with the following properties:

| Property     | Type   | Required     |  Description 		
| ------------ | ------ | ------------ |  --------------------
| `addressLine`    | string | **Optional** | 
| `countryCode`    | string | **Optional** | 2 character ISO code
| `postalArea` | string | **Optional** | 
| `postalCode` | string | **Optional** | 
| `email` | string | **Optional** | 

#### organizationNumber

`object` with the following properties:

| Property     | Type   | Required     |  Description
| ------------ | ------ | ------------ |  --------------------
| `number`    | string | **Required** | 
| `countryCode`    | string | **Required** | 2 character ISO code	

#### participant

`object` with the following properties:

| Property                | Type    		| Required     | Description 															
| ----------------------- | ------------| ------------ | -------------------------------------------------------------- 
| `number`                    | integer  		| **Required** |  Participant number for created participant. 											
| `companyId`             | integer 		| **Required** | Internal ID of participants company.																
| `name`                  | string 		| **Required** | Participant name
| `customerNumber`               | integer  		| **Required** | Participant Visma customer number																			




### Sample JSON

```json
{
  "metadata": {
    "eventType": "Create",
    "event": "Company",
    "origin": "SubscriptionApi",
    "author": "Norsk Byggtjeneste AS",
    "date": "2020-06-24T07:14:11.609Z"
  },
  "Data": {
    "Id": 1098,
    "LegalName": "AS Rockwool",
    "Name": "Rockwool",
    "LogoUrl": null,
    "IsVvsCompany": false,
    "EfoParticipantNumbers": "1234, 5678",
    "GlnNumber": "5236524853215",
    "NrfParticipantNumbers": "4321, 1234",
    "OrganizationNumber": {
      "Number": "923828583",
      "CountryCode": "NO"
    },
    "ParentId": null,
    "AddressId": 1191,
    "Address": {
      "Id": 1191,
      "Email": null,
      "AddressLine": null,
      "PostalCode": null,
      "PostalArea": null,
      "CountryCode": null
    },
    "BillingAddressId": 1192,
    "BillingAddress": {
      "Id": 1192,
      "Email": "test@nbt.no",
      "AddressLine": "Postboks 4215 Nydalen",
      "PostalCode": null,
      "PostalArea": null,
      "CountryCode": "NO"
    },
    "Participants": [
      {
        "Number": 75,
        "CompanyId": 1098,
        "Name": "Rockwool",
        "CustomerNumber": 12354
      }
    ]
  }
}
```

# Company updated

## Note
Any update an a company will publish this event, with all data and not only changed fields.

## Properties

### data

`object` with the following properties:

| Property                | Type    		| Required     | Description 															
| ----------------------- | ------------- | ----------- | -------------------------------------------------------------
| `id`                    | string  		| **Required** |  Unique identifier for the NOBB Supplier generated by the Subscription System											
| `legalName`             | string 		| **Optional** | 																
| `name`                  | string 		| **Optional** | 
| `logoUrl`               | string  		| **Optional** | 																			
| `isVvsCompany`          | boolean 		| **Required** | Internal usage for Nobb.no						
| `efoParticipantNumbers`  | string 	| **Optional** | Comma separated list of Participant number(s) for company in EFO database											
| `nrfParticipantNumbers`  | string 	| **Optional** | Comma separated list of Participant number(s) for company in NRF database											
| `glnNumber`            | string 	| **Optional** | Global Location Number						
| `organizationNumber`               | object  		| **Required** | 
| `address`               | object  		| **Optional** | 							
| `billingAddress`        | object  		| **Optional** | 
| `parentCompanyId`       | integer  		| **Optional** | ID of parent company. Default is null.												
| `participants`         | array of objects 	| **Required** |	

#### address

`object` with the following properties:

| Property     | Type   | Required     |  Description 		
| ------------ | ------ | ------------ |  --------------------
| `addressLine`    | string | **Optional** | 
| `countryCode`    | string | **Optional** | 2 character ISO code
| `postalArea` | string | **Optional** | 
| `postalCode` | string | **Optional** | 
| `email` | string | **Optional** | 



#### billingAddress

`object` with the following properties:

| Property     | Type   | Required     |  Description 		
| ------------ | ------ | ------------ |  --------------------
| `addressLine`    | string | **Optional** | 
| `countryCode`    | string | **Optional** | 2 character ISO code
| `postalArea` | string | **Optional** | 
| `postalCode` | string | **Optional** | 
| `email` | string | **Optional** | 

#### organizationNumber

`object` with the following properties:

| Property     | Type   | Required     |  Description
| ------------ | ------ | ------------ |  --------------------
| `number`    | string | **Required** | 
| `countryCode`    | string | **Required** | 2 character ISO code	

#### participant

`object` with the following properties:

| Property                | Type    		| Required     | Description 															
| ----------------------- | ------------| ------------ | -------------------------------------------------------------- 
| `number`                    | integer  		| **Required** |  Participant number for updated participant. This cannot change. 											
| `companyId`             | integer 		| **Required** | Internal ID of participants company.																
| `name`                  | string 		| **Required** | Participant name
| `customerNumber`               | integer  		| **Required** | Participant Visma customer number																			

### Sample JSON


```json
{
  "metadata": {
    "eventType": "Update",
    "event": "Company",
    "origin": "SubscriptionApi",
    "author": "Norsk Byggtjeneste AS",
    "date": "2020-06-24T12:03:22.015Z"
  },
  "Data": {
    "Id": 1098,
    "LegalName": "AS Rockwool",
    "Name": "Rockwool",
    "LogoUrl": null,
    "IsVvsCompany": false,
    "EfoParticipantNumbers": "1234",
    "GlnNumber": null,
    "NrfParticipantNumbers": "4321",
    "OrganizationNumber": {
      "Number": "923828583",
      "CountryCode": "NO"
    },
    "ParentId": null,
    "AddressId": 1191,
    "Address": {
      "Id": 1191,
      "Email": null,
      "AddressLine": null,
      "PostalCode": null,
      "PostalArea": null,
      "CountryCode": null
    },
    "BillingAddressId": 1192,
    "BillingAddress": {
      "Id": 1192,
      "Email": "test@nbt.no",
      "AddressLine": "Postboks 4215 Nydalen",
      "PostalCode": null,
      "PostalArea": null,
      "CountryCode": "NO"
    },
    "Participants": [
      {
        "Number": 75,
        "CompanyId": 1098,
        "Name": "Rockwool",
        "CustomerNumber": 123123
      }
    ]
  }
}

```
