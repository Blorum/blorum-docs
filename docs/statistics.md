# Documents - Statistics

## User Statistics Prototype
{
	"invited_by": [uid, if not invited - 0],
	"registered_at": [UTC Timestamp],
	"create":{
		"comment": [number],
		"like": [number],
		"react": [number],
		"article": [number],
		"post": [number],
		"note": [number],	
	},
	"received": {
		"comment": {
			"profile": [number],
			"article": [number],
			"post": [number],
			"comment": [number]
		},
		"like": [number],
		"react": [number]
	},
	"read": {
		"day_visited": [number],
		"article": {
			"count": [number],
			"total_time": [number(approx. minutes)]
		},
		"post": {
			"count": [number],
			"total_time": [number(approx. minutes)]
		},
	},
	"rank":{
		"like": [
			{
				"uid": [number]
			}
		],
		"liked_by": [],
		"reply": []
	},
	"invite": [uid1, uid2.....]
}

## User Session Statistics Prototype
> "statistics": {
>	"date": [timestamp],
>	"userAgent": "xxx",
>	"ip": "xxx"
> }