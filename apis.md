# Documents - APIs

Those APIs are meant to be designed in RESTful.

### Universal failbacks
If server-side unknown error happened: 500

If rate limit reached: 429

If you ask Blorum to brew coffee: 418


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

- token - String

_Return_

If success: 200

If error happened: 500


## /user/update

## /user/suicide