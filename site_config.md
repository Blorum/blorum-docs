# Document - config

Those configs will be loaded into Redis database in Blorum's initialization process.

## True / False flags
##### Example
> true
> false

#### allowed_register
##### default
true
#### allowed_login
##### default
true
#### enable_email_challenge
##### default
false

## Varible flags
##### Example
> {"value" : "something"}
> {"value" : {
>   "a": 1,
>   "test": "a"
> }}

#### site_url
##### default
127.0.0.1

#### site_title
##### default
Yet another Blorum site.

#### site_excerpt
##### default
This is a Blorum site, where you could publish blogs and talk.

#### site_logo
##### default
/favicon.ico

#### default_avatar
##### default
/statics/avatar.png

#### ip_detect_method
This is neccessary to edit if you are running Blorum behind a nginx reverse proxy!
If so, you need to transfer the real client IP via header.
##### default
connection
##### selectable
header

#### ip_detect_header
##### default
X-Forwarded-From


* Time were based on seconds.
#### user_cookie_expire_after
##### default
2630000 

(One month)

#### admin_cookie_expire_after
##### default
1315000

(Half month)

#### mod_cookie_expire_after
##### default
1315000

(Half month)

#### max_user_sessions
##### default
4

#### max_admin_sessions
##### default
1

#### max_mod_sessions
##### default
2


*Rate limits are based on requests per hour.

#### ip_rate_limit_posts
##### default
12

#### ip_rate_limit_register
##### default
1

#### ip_rate_limit_react
##### default
64

#### ip_rate_limit_comment
##### default
60

#### ip_rate_limit_remove
##### default
128

#### ip_rate_limit_articles
##### default
12

#### ip_rate_limit_login
##### default
30

#### ip_rate_limit_bypass_whitelist
##### default
[127.0.0.1]


#### user_roles
##### default
["omni", "admin", "moderator", "forum_admin", "auditor", "writer", “user”]

#### roles_permissions
##### default
{
	"admin": {
		"flags": {
			[]
		},
		"rate_limits":{
			"create":{
				"article": Infinity,
				"post": Infinity,
				"react": Infinity,
				"comment": Infinity
			},
			"remove": {
				"article": Infinity,
				"post": Infinity,
				"react": Infinity,
				"comment": Infinity
			}
			"login": Infinity
		}
	},
	"moderator": {
		"flags": {
			[]
		},
		"rate_limits":{
			"create":{
				"article": 60,
				"post": 60,
				"react": 120,
				"comment": 120
			},
			"remove": {
				"article": 60,
				"post": 60,
				"react": 120,
				"comment": 120
			}
			"login": 20
		}
	},
	"forum_admin": {
		"flags": {
			[]
		},
		"rate_limits":{
			"create":{
				"article": 0,
				"post": 60,
				"react": 120,
				"comment": 120
			},
			"remove": {
				"article": 0,
				"post": 100,
				"react": 240,
				"comment": 240
			}
			"login": 20
		}
	},
	"auditor": {
		"flags": {
			[]
		},
		"rate_limits":{
			"create":{
				"article": 0,
				"post": 10,
				"react": 60,
				"comment": 60
			},
			"remove": {
				"article": 1,
				"post": 15,
				"react": 240,
				"comment": 360
			}
			"login": 20
		}
	},
	"writer": {
		"flags": {
			[]
		},
		"rate_limits":{
			"create":{
				"article": 5,
				"post": 10,
				"react": 30,
				"comment": 30
			},
			"remove": {
				"article": 5,
				"post": 10,
				"react": 30,
				"comment": 30
			}
			"login": 20
		}
	},
	"user": {
		"flags": {
			[]
		},
		"rate_limits":{
			"create":{
				"article": 0,
				"post": 6,
				"react": 30,
				"comment": 24
			},
			"remove": {
				"article": 0,
				"post": 10,
				"react": 30,
				"comment": 24
			}
			"login": 20
		}
	}
}

#### register_default_role
##### default
user


#### default_email_protocol
##### default
smtp
##### selectable
sendmail

SES

#### smtp_config

#### ses_config

