# Document - Redis

Blorum use Redis to store user tokens, permissions and rate limits.

### User sessions

Stored in 

#### List:
##### key:
user_session@[uid]
##### value:
{
	"token": "xxxxxxxx",
	"statistics": {
		"date": [UTC Timestamp],
		"userAgent": "xxx",
		"ip": "xxx.xxx.xxx.xxx"
	}
}

### User permissions
user permissions cache will be set to expire in the same time as the last key.

##### key:
user_permissions@[uid]
##### value:
["p1", "p2"......]