# Video events

These events relates to video links that can be added to items in Riversand. Events will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

[Video Created](#Video-Created)

[Video Updated](#Video-Updated)

[Video Deleted](#Video-Deleted)

## Message properties

### SessionID: 	<data.id>

## Payload properties

| Property              | Type     | Required     | Nullable | Description                                |
| --------------------- | -------- | ------------ | -------- | ------------------------------------------ |
| [data](#data)         | `object` | **Required** | No       |         |
| [metadata](#metadata) | `object` | **Required** | No       |         |

### metadata

`object` with the following properties:

| Property          | Type    | Required     | Description |
| ------------------| ------- | ------------ | ------- |
| `eventType`       | string  | **Required** | Either "Create", "Update" or "Delete".
| `event`           | string  | **Required** | Always "Video" for video events.
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.


# Video Created 

## Preconditions
- the item the video link belongs to must have been published/approved

## Dependencies
- Item Created (for `referencedByItems`)
- Supplier Created (for `participantNumber`)

## Properties

### data

`object` with following properties:

| Property            | Type    | Required     | Description |
| ------------------- | ------- | ------------ | ----------- |
| `id`                | string  | **Required** | GUID. Must be generated and can't be changed.
| `description`       | string  | **Optional** |
| `participantNumber` | integer | **Required** | Participant number of video link owner.
| `referencedByItems` | array of integers | **Required** | NOBB numbers of items that references this video link.
| `url`               | string  | **Required** | Full URL to video, either YouTube URL or Vimeo URL.



## Sample JSON

```json
{
    "metadata": {
        "eventType": "Create",
        "event": "Video",
        "date": "2019-09-30T16:34:56",
        "author": "Glava AS"
    },	
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9",
        "description": "Instruction video",
        "participantNumber": 51128,
        "referencedByItems": [ 49831963 ],
        "url": "https://www.youtube.com/watch?v=pLrqYpti3-U"
    }
}
```

# Video Updated 

## Preconditions
- the "Video created" event has already been sent

## Dependencies
- Video Created

## Note
The identifier must be part of the event data. Otherwise, only changed fields can be part of the event data. If there is a change inside a list field (like "referencedByItems"), the whole list must be part of the event data.

## Properties

### data

`object` with following properties:

| Property            | Type    | Required     | Description |
| ------------------- | ------- | ------------ | ----------- |
| `id`                | string  | **Required** | GUID. Must be generated and can't be changed.
| `description`       | string  | **Optional** |
| `participantNumber` | integer | **Optional** | Participant number of video link owner.
| `referencedByItems` | array of integers | **Optional** | NOBB numbers of items that references this video link.
| `url`               | string  | **Optional** | Full URL to video, either YouTube URL or Vimeo URL.


## Sample JSON

Example of when the video link description has been changed:

```json
{
    "metadata": {
        "eventType": "Update",
        "event": "Video",
        "date": "2019-09-30T17:34:56",
        "author": "Glava AS"
    },	
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9",
        "description": "Instruction video for DIY",
    }
}
```

# Video Deleted 

## Preconditions
- the "Video created" event has already been sent

## Note
The identifier must be part of the event data. 

## Properties

### data

`object` with following properties:

| Property                    | Type    | Required     | Description |
| --------------------------- | ------- | ------------ | -------     |
| `id`                        | string  | **Required** |  


## Sample json

```json
{
    "metadata": {
        "eventType": "Delete",
        "event": "Video",
        "date": "2019-09-30T19:34:56",
        "author": "Glava AS"
    },
    "data": {
        "id": "4f214662-ba42-491c-b230-37b1420a4db9"
    }
}
```