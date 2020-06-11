# Response events

TODO: Description


## Properties

`object` with the following properties:

| Property                | Type    | Required     | Description |
| ----------------------- | ------- | ------------ | ------- |
| `correlationId`         | string  | **Required** | 
| `enqueuedDateTimeUtc`   | string  | **Optional** |
| `processedDateTimeUtc`             | string  | **Required** |
| `source`               | string  | **Optional** |
| `status`             | string | **Required** | One of "Received", "Processed", "Failed"
| `error`                 | object | **Optional** | 
| `originalPayload`       | object | **Optional** | 


### error

`object` with the following properties:

| Property          | Type    | Required     | Description |
| ------------------| ------- | ------------ | ------- |
| `reason`       | string  | **Required** | 
| `stackTrace`           | string  | **Required** | 



## Sample JSON for message received

```json
{
  "correlationId": "a5af7ff7-1cce-4410-a575-cacea22709ad",
  "enqueuedDateTimeUtc": "2019-09-30T12:34:56",
  "processedDateTimeUtc": "2019-09-30T13:34:56",
  "source": "NobbInternal.API",
  "status": "Received"
}

```

## Sample JSON for message processed successfully

```json
{
  "correlationId": "a5af7ff7-1cce-4410-a575-cacea22709ad",
  "enqueuedDateTimeUtc": "2019-09-30T12:34:56",
  "processedDateTimeUtc": "2019-09-30T13:34:56",
  "source": "NobbInternal.API",
  "status": "Processed"
}

```

## Sample JSON for message failed

```json
{
  "correlationId": "a5af7ff7-1cce-4410-a575-cacea22709ad",
  "enqueuedDateTimeUtc": "2019-09-30T12:34:56",
  "processedDateTimeUtc": "2019-09-30T13:34:56",
  "source": "NobbInternal.API",
  "status": "failed",
  "error": {
    "reason": "Missing required field.",
    "stackTrace": "..."
  },
  "originalPayload": {
    "data": {
      "id": "e6aaaec6-d542-4234-96c4-2b95fa514b8e",
      "number": 25909524,
      "productGroupNumber": 1965100,
      "ownerParticipantNumber": 209328,
      "manufacturerNumber": 2346,
      "internalNumber": "M100",
      "text": "KJERNEBORMASKIN WEKA",
      "etimClass": "EC003138"
    },
    "metadata": {
      "eventType": "Create",
      "event": "Module",
      "date": "2020-06-10T11:42:23",
      "author": "system"
    }
  }
}

```


