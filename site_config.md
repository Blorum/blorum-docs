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
This is a Blorum site, where you could publish blogs and chat.

#### site_logo
##### default
/favicon.ico

#### ip_detect_method
This is neccessary to edit if you are running Blorum behind a nginx reverse proxy!
If so, you need to transfer the real client IP via header.
##### default
raw_ip
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

#### user_rate_limit_posts
##### default
12

#### user_rate_limit_react
##### default
64

#### user_rate_limit_comment
##### default
60

#### user_rate_limit_remove
##### default
{
	"article": 12,
	"posts": 12,
	"react": 128,
	"comment": 60
}

#### user_rate_limit_articles
##### default
12

#### user_rate_limit_login
##### default
30