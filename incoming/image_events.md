# Image events

These events relates to images that will be maintained in the DAM domain within Riversand. Events will be sent from Avensia Middleware to an Azure Service Bus queue for consumption by Byggtjeneste.

[Image Created](#Image-Created)

[Image Updated](#Image-Updated)

[Image Deleted](#Image-Deleted)

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
| `event`           | string  | **Required** | Always "Image" for image events.
| `date`            | string  | **Required** | Date and time in UTC for the action in Riversand that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.
| `author`          | string  | **Required** | Author of the action that triggered the event.

### data
The data model depends on the event type, see below.


# Image Created 

## Preconditions
- The supplier has uploaded a valid image in DAM, and selected a valid image type.
- The image has been scaled to the different sizes.

## Dependencies
- Item Created (for `referencedByItems`)
- Module Created (for `referencedByModules`)
- Product Group Created (for `referencedByProductGroups`)
- MediaType Created (for `type`)
- Supplier Created (for `participantNumber`)


## Properties

### data

`object` with following properties:

| Property            | Type    | Required     | Description |
| ------------------- | ------- | ------------ | ----------- |
| `id`                | string  | **Required** | GUID. Must be generated and can't be changed.
| `angle`             | string  | **Optional** |
| `copyright`         | string  | **Optional** |
| `copyrightOwner`    | string  | **Optional** |
| `description`       | string  | **Optional** |
| `dimensionX`        | integer | **Optional** | Image width.
| `dimensionY`        | integer | **Optional** | Image height.
| `fileName`          | string  | **Required** | Image file name.
| `fileType`          | string  | **Optional** |
| `imageTakenDate`    | string  | **Optional** | Date for when the image was taken in format yyyy-MM-dd.
| `participantNumber` | integer | **Required** | Participant number of image owner.
| `referencedByItems` | array of integers | **Optional** | NOBB numbers of items that references this image.
| `referencedByModules` | array of integers | **Optional** | Module numbers of modules that references this image.
| `referencedByProductGroups` | array of strings | **Optional** | Product group numbers of product groups that references this image.
| `resolutionX`       | decimal | **Optional** | From EXIF data.
| `resolutionY`       | decimal | **Optional** | From EXIF data.
| `side`              | string  | **Optional** |
| `size`              | string  | **Optional** |
| `title`             | string  | **Optional** |
| `type`              | string  | **Required** | Type of image, from reference data. E.g. "PB".
| `urls`              | array of objects | **Required** | URLs to the original and the scaled versions of the image. 
| `validFor`          | string  | **Optional** | Use case for image based on image quality.
| `validFromDate`     | string  | **Optional** | Date in format yyyy-MM-dd.
| `validToDate`       | string  | **Optional** | Date in format yyyy-MM-dd.



#### urls

Array type: `object[]`

All items must be of the type: `object` with following properties:

| Property    | Type    | Required     |	Description |
| ----------- | ------- | ------------ | ---------------
| `scaledSize`| string  | **Required** | "Original" or a lower scale.
| `url`       | string  | **Required** | Full URL to image file.




## Sample JSON

```json
{
	"metadata": {
		"eventType": "Create",
		"event": "Image",
		"date": "2019-09-30T16:34:56",
		"author": "Glava AS"
	},	
	"data": {
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"fileName": "rormansjett.jpg",
		"participantNumber": 51128,
		"referencedByItems": [ 49831963, 49831952 ],
		"referencedByProductGroups": [ "0930400" ],
		"type": "PB",
		"urls": [
			{
				"scaledSize": "Original",
				"url": "https://blobs.riversand.com/images/4f214662-ba42-491c-b230-37b1420a4db9"
			},
			{
				"scaledSize": "Thumbnail",
				"url": "https://blobs.riversand.com/images/e7befba3-0f92-44c6-9b5a-1e88b33b83f3"
			}
		]
	}
}
```

# Image Updated 

## Preconditions
- the "Image created" event has already been sent

## Dependencies
- Image Created
- Other dependencies depends on the data being updated (see dependencies for Image Created event)

## Note
The identifier must be part of the event data. Otherwise, only changed fields can be part of the event data. If there is a change inside a list field (like `urls`), the whole list must be part of the event data.

## Properties

### data

`object` with following properties:

| Property            | Type    | Required     | Description |
| ------------------- | ------- | ------------ | ----------- |
| `id`                | string  | **Required** | GUID. Must be generated and can't be changed.
| `angle`             | string  | **Optional** |
| `copyright`         | string  | **Optional** |
| `copyrightOwner`    | string  | **Optional** |
| `description`       | string  | **Optional** |
| `dimensionX`        | integer | **Optional** | Image width.
| `dimensionY`        | integer | **Optional** | Image height.
| `fileName`          | string  | **Optional** | Image file name.
| `fileType`          | string  | **Optional** |
| `imageTakenDate`    | string  | **Optional** | Date for when the image was taken in format yyyy-MM-dd.
| `participantNumber` | integer | **Optional** | Participant number of image owner.
| `referencedByItems` | array of integers | **Optional** | NOBB numbers of items that references this image.
| `referencedByModules` | array of integers | **Optional** | Module numbers of modules that references this image.
| `referencedByProductGroups` | array of strings | **Optional** | Product group numbers of product groups that references this image.
| `resolutionX`       | decimal | **Optional** | From EXIF data.
| `resolutionY`       | decimal | **Optional** | From EXIF data.
| `side`              | string  | **Optional** |
| `size`              | string  | **Optional** |
| `title`             | string  | **Optional** |
| `type`              | string  | **Optional** | Type of image, from reference data. E.g. "PB".
| `urls`              | array of objects | **Optional** | URLs to the original and the scaled versions of the image. 
| `validFor`          | string  | **Optional** | Use case for image based on image quality.
| `validFromDate`     | string  | **Optional** | Date in format yyyy-MM-dd.
| `validToDate`       | string  | **Optional** | Date in format yyyy-MM-dd.


## Sample JSON

Example of when the image has been referenced from one more item:

```json
{
	"metadata": {
		"eventType": "Update",
		"event": "Image",
		"date": "2019-09-30T17:34:56",
		"author": "Glava AS"
	},	
	"data": {
		"id": "4f214662-ba42-491c-b230-37b1420a4db9",
		"referencedByItems": [ 49831963, 49831952, 49831944 ]
	}
}
```

# Image Deleted 

## Preconditions
- the "Image created" event has already been sent

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
		"event": "Image",
		"date": "2019-09-30T19:34:56",
		"author": "Glava AS"
	},
	"data": {
		"id": "4f214662-ba42-491c-b230-37b1420a4db9"
	}
}
```