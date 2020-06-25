# Participant events

The events related to participants will be sent from Byggtjeneste's Subscription System to an Azure Service Bus queue for consumption by internal subscribers. 

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
| `event`           | string  | **Required** | Always "Participant" for participant events.
| `date`            | string  | **Required** | Date and time in UTC for the action that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.

# Participant created

## Preconditions

## Properties

### data

`object` with the following properties:

| Property                | Type    		| Required     | Description 															
| ----------------------- | ------------| ------------ | -------------------------------------------------------------- 
| `number`                    | integer  		| **Required** |  Participant number for created participant 											
| `companyId`             | integer 		| **Required** | Internal ID of participants company																
| `name`                  | string 		| **Required** | Participant name
| `customerNumber`               | integer  		| **Required** | Participant Visma customer number																			



### Sample JSON

```json
{
  "metadata": {
    "eventType": "Create",
    "event": "Participant",
    "origin": "SubscriptionApi",
    "author": "Norsk Byggtjeneste AS",
    "date": "2020-06-25T06:40:01.137Z"
  },
  "Data": {
    "Number": 111,
    "CompanyId": 1098,
    "Name": "Rockwool participant #2",
    "CustomerNumber": 451215
  }
}
```

# Participant updated

## Note
Any update on a participant will publish this event, with all data and not only changed fields.

## Properties

### data

`object` with the following properties:

| Property                | Type    		| Required     | Description 															
| ----------------------- | ------------| ------------ | -------------------------------------------------------------- 
| `number`                    | integer  		| **Required** |  Participant number for updated participant. This cannot change. 											
| `companyId`             | integer 		| **Required** | Internal ID of participants company.																
| `name`                  | string 		| **Required** | Participant name
| `customerNumber`               | integer  		| **Required** | Participant Visma customer number																			


### Sample JSON

Example of updating marketing name and adding an NRF participant number:

```json
{
  "metadata": {
    "eventType": "Update",
    "event": "Participant",
    "origin": "SubscriptionApi",
    "author": "Norsk Byggtjeneste AS",
    "date": "2020-06-25T07:04:17.304Z"
  },
  "Data": {
    "Number": 111,
    "CompanyId": 1098,
    "Name": "Rockwool participant #2 updated",
    "CustomerNumber": 451215
  }
}

```
