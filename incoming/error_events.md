# Error type events

Error events are sent from Middleware to BT, to notify error scenarios.

## Payload properties


| Property              | Type     | Required     | Nullable |
| --------------------- | -------- | ------------ | -------- |
| `data`                | `object` | **Required** | No       |
| `metadata`            | `object` | **Required** | No       |

### metadata

`object` with the following properties:

| Property          | Type    | Required     | Description |
| ------------------| ------- | ------------ | ------- |
| `event`           | string  | **Required** | Either "Error" or "Warning".
| `date`            | string  | **Required** | Date and time in UTC for the action that triggered the event. In format `yyyy'-'MM'-'dd'T'HH':'mm':'ss`. Example value: `2020-02-27T23:39:46`.


### data

`object` with following properties:

| Property                | Type    | Required     | Description |
| ----------------------- | ------- | ------------ | ------- |
| `entityReference`                    | string  | **Optional** | Identifier for the entity involved in the error. Eg. NOBB number for Item entities.
| `entityType`			      | string | **Optional**  | Type of entity involved in the error.
| `message`           		  | string  | **Required** | Descriptive message of the error.


## Sample json

```json
{
	"metadata": {
		"event": "Error",
		"date": "2019-09-30T18:34:56"
	},	
	"data": {
		"entityReference": "54130625",
                "entityType": "Item",
		"message": "Missing price for entity '54130625'"
	}
}

```
