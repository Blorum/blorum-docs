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
	"permissions": {
	}
	"statistics": {
		"date": [UTC Timestamp],
		"userAgent": "xxx",
		"ip": "xxx.xxx.xxx.xxx"
	}
}

### User rate limits
##### key:
user_rate_limit_[item]:[uid]
##### value:
[number]

### IP rate limits
##### key:
ip_rate_limit_[item]:x.y.z.w
##### value:
[number]

