---
layout: default
parent: English Documents
---
# APIs

Those APIs are meant to be designed in RESTful.

### Universal failbacks
If server-side unknown error happened: 500

If rate limit reached: 429

If session invalid: 412

If you ask Blorum to brew coffee: 418

# Site APIs
## /site/info
#### GET
##### _Take params_
> {
> 	"require": ["logo", "description" ......]
> }

##### _Return_
___If success___:

status 200

> {
>	"logo": "http://..."
>	......
> }

### Requirable list
(result are examples)
#### logo

#### title

#### description

#### owner
{
	"avatar": ...
	"name"
}


# User APIs
## /user/login
#### POST
##### _Take params_

> username - String

> password - String

##### _Return_
 
___If success___:

status: 200

>header:
>Set-Cookie: "uid=xxx; token=xxx...(same as token)"

body:

>{
>	"token": "xxx....",
>	"expire_after": "[UTC Timestamp]"
>}

___if failed___:

status: 401

## /user/register
#### POST
##### ___Take params___

> username - String

> password - String

##### ___Return___
___if success___:

>header:
>Set-Cookie: "uid=xxx; token=xxx...(same as token)"

body:

>{
>	"token": "xxx....",
>	"expire_after": "[UTC Timestamp]"
>}

## /user/logout
##### POST
_Take params_

A valid Blorum Session Header

_Return_

If success: 200

If error happened: 500


## /user/update

## /user/permissions
##### GET
_Take params_

> {
> 	"uid": [number]
> }

User could only get their own permissions, if they don't have the permission
defined with user.permissions.read.

_Return_

If success: 200

If error happened: 500