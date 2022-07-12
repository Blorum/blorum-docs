# Document - Redis

Blorum use Redis to store user tokens, permissions and rate limits.

### User sessions

Stored in 

#### List:
##### key:
user_session:[uid]
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
user permissions cache will be set to expire as same as the last key's expire date, so technically it is impossible for a cache-miss to take place.

##### key:
user_permissions:[uid]
##### value:
["p1", "p2"......]

### User rate limits
##### key:
user_rate_limit_[item]:[uid]
##### value:
[number]