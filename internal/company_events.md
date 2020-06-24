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
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.

# Company created

## Preconditions
- The company has a NOBB participant number.

## Properties

### data

`object` with the following properties:

| Property                | Type    		| Required     | Description 															
| ----------------------- | ------------------- | ------------ | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ 
| `id`                    | string  		| **Required** |  Unique identifier for the NOBB Supplier generated by the Subscription System											
| `legalName`             | string 		| **Required** | 																
| `name`                  | string 		| **Required** | 
| `logoUrl`               | string  		| **Optional** | 																			
| `isVvsCompany`          | boolean 		| **Required** | Internal usage for Nobb.no						
| `efoParticipantNumbers`  | string 	| **Optional** | Comma separated list of Participant number(s) for company in EFO database											
| `nrfParticipantNumbers`  | string 	| **Optional** | Comma separated list of Participant number(s) for company in EFO database											
| `glnNumber`            | string 	| **Optional** | Global Location Number						
| `organizationNumber`               | object  		| **Required** | 
| `address`               | object  		| **Optional** | 							
| `billingAddress`        | object  		| **Optional** | 
| `parentCompanyId`       | integer  		| **Optional** | ID of parent company. Default is null.												
| `subscriptions`         | array of objects 	| **Required** |	
| `participants`         | array of objects 	| **Required** |	


#### subscription

`object` with the following properties:

| Property     	| Type   | Required     |  Description 
| ------------ 	| ------ | ------------ | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| `product`    	| string | **Required** |
| `level`    	| string | **Required** | 
| `active`    	| boolean | **Required** | Indicates whether the subscription is active or not. 

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

| Property     | Type   | Required     |  Description 		
| ------------ | ------ | ------------ |  --------------------
| `addressLine`    | string | **Optional** | 
| `countryCode`    | string | **Optional** | 2 character ISO code
| `postalArea` | string | **Optional** | 
| `postalCode` | string | **Optional** | 
| `email` | string | **Optional** | 



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
    "GlnNumber": "0",
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
        "CustomerNumber": 0,
        "Subscriptions": null
      }
    ]
  }
}
```

# Company updated

## Note
The identifiers will be part of the event data.

## Properties

### data

`object` with the following properties:

| Property                | Type    		| Required     | Description 															
| ----------------------- | ------------------- | ------------ | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ 
| `id`                    | string  		| **Required** |  Unique identifier for the NOBB Supplier generated by the Subscription System											
| `legalName`             | string 		| **Optional** | 																
| `name`                  | string 		| **Optional** | 
| `logoUrl`               | string  		| **Optional** | 																			
| `isVvsCompany`          | boolean 		| **Required** | Internal usage for Nobb.no						
| `efoParticipantNumbers`  | string 	| **Optional** | Comma separated list of Participant number(s) for company in EFO database											
| `nrfParticipantNumbers`  | string 	| **Optional** | Comma separated list of Participant number(s) for company in EFO database											
| `glnNumber`            | string 	| **Optional** | Global Location Number						
| `organizationNumber`               | object  		| **Required** | 
| `address`               | object  		| **Optional** | 							
| `billingAddress`        | object  		| **Optional** | 
| `parentCompanyId`       | integer  		| **Optional** | ID of parent company. Default is null.												
| `subscriptions`         | array of objects 	| **Optional** |	
| `participants`         | array of objects 	| **Required** |	


#### subscription

`object` with the following properties:

| Property     	| Type   | Required     |  Description 
| ------------ 	| ------ | ------------ | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| `product`    	| string | **Required** |
| `level`    	| string | **Required** | 
| `active`    	| boolean | **Required** | Indicates whether the subscription is active or not. 

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

| Property     | Type   | Required     |  Description 		
| ------------ | ------ | ------------ |  --------------------
| `addressLine`    | string | **Optional** | 
| `countryCode`    | string | **Optional** | 2 character ISO code
| `postalArea` | string | **Optional** | 
| `postalCode` | string | **Optional** | 
| `email` | string | **Optional** | 
### Sample JSON

Example of updating marketing name and adding an NRF participant number:

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
    "LogoUrl": "../assets/img/defaultCompanyLogo.svg",
    "IsVvsCompany": false,
    "EfoParticipantNumbers": "1234",
    "GlnNumber": "0",
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
    "Participants": []
  }
}

```