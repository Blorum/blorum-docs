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
#### allowed_comment_to_article
##### default
true
#### allowed_comment_to_post
##### default
true
#### allowed_comment_to_user
##### default
true
#### enable_email_register_challenge
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
X-Forwarded-For


* Time were based on seconds.

*Rate limits are based on requests per hour.

#### ip_rate_limit_get
##### default
2400

#### ip_rate_limit_create
##### default
{

	"article": 12,
	
	"posts": 12,

	"notes": 30,
	
	"react": 64,
	
	"comment": 60
	
}

#### ip_rate_limit_register
##### default
1

#### ip_rate_limit_remove
##### default
{

	"article": 12,
	
	"posts": 20,

	"notes": 30,
	
	"react": 128,
	
	"comment": 60
	
}

#### ip_rate_limit_edit
##### default
{

	"article": 12,
	
	"posts": 20,

	"notes": 30,
	
	"react": 128,
	
	"comment": 60
	
}

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
		"permissions": {
			"flags": []
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
			},
			"edit": {
				"article": Infinity,
				"post": Infinity,
				"react": Infinity,
				"comment": Infinity
			},
			"login": Infinity
		},
		"cookie_expire_after": 13150000000,
		"max_session": 10
	},
	"moderator": {
		"permissions": {
			"flags": []
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
			},
			"edit": {
				"article": 60,
				"post": 60,
				"react": 120,
				"comment": 120
			},
			"login": 20
		},
		"cookie_expire_after":  13150000000,
		"max_session": 10
	},
	"forum_admin": {
		"permissions": {
			"flags": []
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
			},
			"edit": {
				"article": 0,
				"post": 100,
				"react": 240,
				"comment": 240
			},
			"login": 20
		},
		"cookie_expire_after": 13150000000,
		"max_session": 10
	},
	"auditor": {
		"permissions": {
			"flags": []
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
			},
			"edit": {
				"article": 1,
				"post": 15,
				"react": 240,
				"comment": 360
			},
			"login": 20
		},
		"cookie_expire_after": 13150000000,
		"max_session": 10
	},
	"writer": {
		"permissions": {
			"flags": []
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
			},
			"edit": {
				"article": 5,
				"post": 10,
				"react": 30,
				"comment": 30
			},
			"login": 20
		},
		"cookie_expire_after": 2630000000,
		"max_session": 10
	},
	"user": {
		"permissions": {
			"flags": []
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
			},
			"edit": {
				"article": 0,
				"post": 10,
				"react": 30,
				"comment": 24
			},
			"login": 20
		},
		"cookie_expire_after": 2630000000,
		"max_session": 8
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

#### sendmail_config
##### default
{}

#### smtp_config
##### default
{}
#### ses_config
##### default
{}
