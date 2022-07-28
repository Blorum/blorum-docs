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

### Rate Limits

Blorum used an algorithm called "Token Bucket" to do traffic controls, the only difference is that key were based on UID/IP instead of tokens

