# Subscription events

The events related to subscriptions will be sent from Byggtjeneste's Subscription System to an Azure Service Bus queue for consumption by internal subscribers. 
A subscription will always relate to a Participant and not directly to the Company.

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
| `event`           | string  | **Required** | Always "Subscription" for subscription events.
| `date`            | string  | **Required** | Date and time in UTC for the action that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.

# Subscription created

## Properties

### data

`object` with the following properties:

| Property                | Type    		| Required     | Description 															
| ----------------------- | ------------| ------------ | -------------------------------------------------------------- 
| `id`                    | int     		| **Required** | Internal database id											
| `productId`             | string 		  | **Required** | Internal product id (GUID)																
| `product`               | object 		  | **Required** | The product subscribed to
| `productLevelId`        | string  		| **Optional** | 																			
| `productLevel`          | object 		  | **Optional** |  			
| `subscriptionProductUsage`  | string 	| **Optional** | 										
| `participantNumber`     | integer 	  | **Required** | 										
| `participant`           | object 	    | **Required** | 					
| `contactEmail`          | string  		| **Optional** | 
| `contactName`           | string  		| **Optional** | 							
| `billingReference`      | string  		| **Optional** | 
| `active`                | boolean  		| **Required** | Indicates whether the subscription is active or not.											


#### product

`object` with the following properties:

| Property     	| Type   | Required     |  Description 
| ------------ 	| ------ | ------------ | ----------------------------- 
| `id`    	| string | **Required** | Internal product id (GUID)	
| `name`    	| string | **Required** | Name of product
| `url`    	| string | **Optional** | Product URL 
| `isVisible`    	| boolean | **Required** | 

#### productLevel

`object` with the following properties:

| Property     | Type   | Required     |  Description 		
| ------------ | ------ | ------------ |  --------------------
| `id`    	| string | **Required** | Internal productlevel id (GUID)	
| `name`    | string | **Required** | Product level name
| `constraintMin` | integer | **Optional** | 
| `constraintMax` | integer | **Optional** | 



#### participant

`object` with the following properties:

| Property     | Type   | Required     |  Description 		
| ------------ | ------ | ------------ |  --------------------
| `number`    | integer | **Required** | Participant number
| `companyId`    | integer | **Required** | Internal ID of particpants company
| `name` | string | **Required** | Participant name
| `customerNumber` | integer | **Required** | Visma customer number



### Sample JSON

```json
{
  "metadata": {
    "eventType": "Create",
    "event": "Subscription",
    "origin": "SubscriptionApi",
    "author": "Norsk Byggtjeneste AS",
    "date": "2020-06-24T11:40:46.457Z"
  },
  "Data": {
    "Id": 1156,
    "ProductId": "c7c26b1e-bc75-42d4-e55c-08d63d85e9cc",
    "Product": {
      "Id": "c7c26b1e-bc75-42d4-e55c-08d63d85e9cc",
      "Name": "ProsjektDok",
      "Url": "https://prosjektdok.no/",
      "IsVisible": true
    },
    "ProductLevelId": null,
    "ProductLevel": null,
    "SubscriptionProductUsage": null,
    "ParticipantNumber": 83,
    "Participant": {
      "Number": 83,
      "CompanyId": 1100,
      "Name": "Marketing Name",
      "CustomerNumber": 36513
    },
    "ContactEmail": "",
    "ContactName": "",
    "BillingReference": "",
    "Active": false
  }
}
```

# Subscription updated

## Note
Any update on a subscription will publish this event, with all data and not only changed fields.

## Properties

### data

`object` with the following properties:

| Property                | Type    		| Required     | Description 															
| ----------------------- | ------------| ------------ | -------------------------------------------------------------- 
| `id`                    | int     		| **Required** | Internal database id											
| `productId`             | string 		  | **Required** | Internal product id (GUID)																
| `product`               | object 		  | **Required** | The product subscribed to
| `productLevelId`        | string  		| **Optional** | 																			
| `productLevel`          | object 		  | **Optional** |  			
| `subscriptionProductUsage`  | string 	| **Optional** | 										
| `participantNumber`     | integer 	  | **Required** | 										
| `participant`           | object 	    | **Required** | Participant subscribing to product					
| `contactEmail`          | string  		| **Optional** | 
| `contactName`           | string  		| **Optional** | 							
| `billingReference`      | string  		| **Optional** | 
| `active`                | boolean  		| **Required** | Indicates whether the subscription is active or not.											


#### product

`object` with the following properties:

| Property     	| Type   | Required     |  Description 
| ------------ 	| ------ | ------------ | ----------------------------- 
| `id`    	| string | **Required** | Internal product id (GUID)	
| `name`    	| string | **Required** | Name of product
| `url`    	| string | **Optional** | Product URL 
| `isVisible`    	| boolean | **Required** | 

#### productLevel

`object` with the following properties:

| Property     | Type   | Required     |  Description 		
| ------------ | ------ | ------------ |  --------------------
| `id`    	| string | **Required** | Internal productlevel id (GUID)	
| `name`    | string | **Required** | Product level name
| `constraintMin` | integer | **Optional** | 
| `constraintMax` | integer | **Optional** | 



#### participant

`object` with the following properties:

| Property     | Type   | Required     |  Description 		
| ------------ | ------ | ------------ |  --------------------
| `number`    | integer | **Required** | Participant number
| `companyId`    | integer | **Required** | Internal ID of particpants company
| `name` | string | **Required** | Participant name
| `customerNumber` | integer | **Required** | Visma customer number


### Sample JSON

```json
{
  "metadata": {
    "eventType": "Update",
    "event": "Subscription",
    "origin": "SubscriptionApi",
    "author": "Norsk Byggtjeneste AS",
    "date": "2020-06-24T11:40:51.759Z"
  },
  "Data": {
    "Id": 1156,
    "ProductId": "c7c26b1e-bc75-42d4-e55c-08d63d85e9cc",
    "Product": {
      "Id": "c7c26b1e-bc75-42d4-e55c-08d63d85e9cc",
      "Name": "ProsjektDok",
      "Url": "https://prosjektdok.no/",
      "IsVisible": true
    },
    "ProductLevelId": "81c3cf0d-1fcf-4a5c-88a2-cd9a18898039",
    "ProductLevel": {
      "Id": "81c3cf0d-1fcf-4a5c-88a2-cd9a18898039",
      "Name": "1-2 Prosjekter",
      "ConstraintMin": "0",
      "ConstraintMax": "2"
    },
    "SubscriptionProductUsage": null,
    "ParticipantNumber": 83,
    "Participant": {
      "Number": 83,
      "CompanyId": 1100,
      "Name": "Marketing Name",
      "CustomerNumber": 0
    },
    "ContactEmail": "",
    "ContactName": "",
    "BillingReference": "",
    "Active": true
  }
}

```
