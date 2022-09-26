er# Design document - Permissions

Blorum has some built-in roles of permissions, for atomized control, you need to change Flags of existing roles or create a new user role.

Roles could assign users permissions and rate limits, an user could have different user roles. Permissions could be assigned to roles.

If an user has multiple roles that has conflict permissions / role rate limits, the highest permission / rate limits will be left, so does the max_session limit. _cookie_expire_after_ is different, for the cookie expire time, only the smallest value will be applied.

**An user must have at least one user role.**

User does not necessarily need a role with rate limits defined, if user has no roles defining rate limits, they are still confined by IP-based rate limits. Unless they have a role that has flag *override_ip_rate_limits*. However, a warning log will be generated if Blorum detected that an user don't have a role with rate limits or flag *override_ip_rate_limits*.

Since JSON does not support Infinity, +Infinity will be stored as -1.


Roles' permissions could be as specific as certain forums, category, etc..., but an arbitrary permission to one type could also be applied. Such permission were stored in the key "any"(See default_permission.sql).

## Permission Data Structure for Roles
Take a possible permisson of a site's moderator as an example
```
{
	
	"with_rate_limit": 1, //Indicate that this roles has rate limits with it

	"permissions": {
	
		"flags": ["override_ip_ratelimts"],
		
		"max_session": 10, 
		
		"cookie_expire_after": 13150000000
		
		"forums":{
			"0":{
				"flags": ["edit_info"]
				"remove_post": 5
			} //This moderator could edit the info and delete everyone's posts in 
                the forum with fid=0
		}
		"article": {
			"create": 5,
			"edit": 4,
			"remove": 2
		} //See what does those numbers indicate in flags.md
		
	},
	
	"rate_limits": {
		
		"edit": {
			
			"post": 60,
			
			"react": 120,
			
			"article": 60,
			
			"comment": 120
		
		},
		
		"login": 20,
		
		"create": {
		
			"post": 60,
			
			"react": 120,
			
			"article": 60,
			
			"comment": 120
		
		},
		
		"remove": {
		
			"post": 60,
			
			"react": 120,
			
			"article": 60,
			
			"comment": 120
		
		}
		
	}

}

```

## Builtin Roles

### Omni
**This role is code-builtin, and could not be removed with tweaks in database.**

Only the owner of the server should be, and by default, will be granted with this permission.

This permission allowed its user to bypass all verfications and rate limits, no restrictions will be applied.

Extensions developers should also implement so.

### Admin

### Moderator

