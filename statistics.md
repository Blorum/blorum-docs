# Documents - Statistics

## User Statistics Prototype
{
	"invited_by": [uid, if not invited - 0]
	"registered_at": [UTC Timestamp],
	"create":{
		"comment": [number],
		"react": [number],
		"article": [number],
		"note": [number],	
	},
	"received": {
		"comment": {
			"profile": [number],
			"article": [number],
			"post": [number],
			"comment": [number]
		}
	},
	"read": {
		"time": [number(approx. minutes)],
		"article": [number],
		"post": [number]
	},
	"invite": [uid1, uid2.....]
}

