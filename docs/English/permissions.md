
---
layout: default
parent: English Documents
---
# Permissions

Blorum has some built-in roles of permissions, for atomized control, you need to change the Flags of existing roles or create a new user role.

Roles could assign users permissions and rate limits, or deprive them of permissions, users could have different user roles. Permission could be assigned to roles.

## Type of user roles
There are two types of user roles: **grantive** and **limitive** 
### Multiple grantive roles
If a user has multiple roles that have conflict permissions / role rate limits, the highest permission / rate limits will be left, and so does the max_session limit.

limitive is considered before grantive.

## How user's permission being determined by roles
The permission sum of a user could be summarized into this formula:
#### permSum = max(grantive_role) - max(limitive_role)

A limitive role has higher priority than a grantive role. The maximum sum of limitation is checked first, and if the action of the user is not limited by it, Blorum will check the user's grantive role's permission sum to see if the user was allowed to perform the action. 
 
## Rate limits
Rate limits also follow the same judgment: the minimum rate limit values (so it's the maximum limitation) given by limitive role sum are considered first, then the maximum rate limit values (so it's the minimum limitation) given by grantive roles were considered.

Blorum only provides rate limits for those APIs that have an explicit side-effect, thus, APIs like fetching the content of a post will not be able to be rate-limited with Blorum Permission System, and such limitation should be deployed on a higher level, like nginx.

## Other rules
**An user must have at least one grantive user role.**

The user does not necessarily need a role with rate limits defined, if a user has no roles defining rate limits, they are still confined by IP-based rate limits. Unless they have a role that has flag *override_ip_rate_limits*. However, a warning log will be generated if Blorum detected that a user doesn't have a role with rate limits or flag *override_ip_rate_limits*.

Since JSON does not support Infinity, +Infinity will be stored as -1.


Roles' permissions could be as specific as certain forums, categories, etc..., but arbitrary permission to one type could also be applied. Such permission was stored in the key "any"(See default_permission.sql).

## Permission Data Structure for Roles
Take the possible permission of a site's moderator as an example
**This is a grantive role**

```
{
	"with_rate_limit": 1, //Indicate that these roles have rate limits with it
	"permissions": {
		"flags": ["override_ip_ratelimits"],
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
		} //See what those numbers indicate in flags.md
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

Take a possible limitation-permission of an untrusted user as another example
**This is a limitive role**
```
{	
	"with_rate_limit": 0,
	"permissions": {
		"flags": [],
	},
	"rate_limits": {
		"login": 10,
		"create": {
			"post": 10,
			"react": 12,
			"article": 0,			
			"comment": 10
		}
	}
}

```


## Builtin Roles

### guest (pseudo)
A user with uid = 1 automatically has this role.

This role is reserved for guests with no login status.

### omni (pseudo)
**It is not an actual role, and could not be removed with tweaks in the database.**

The user with uid = 2 automatically has this role. This role cannot be granted to other users.

This permission allowed its user to bypass all verifications and rate limits, no restrictions will be applied.

Extension developers should also implement so.

### admin

### moderator

### forum_admin

### writer


## Actual complete grantive role permission JSON structure

A user's grantive roles will be finally summed, and this is the structure of a finally computed grantive role. You can also understand it as the fallback if any permission is not defined by any roles that the user has.

A grantive role does not necessarily need to have all those fields below, the field that it has will be counted in the summation. For example, a user only has role `a` and role `b`, `a` only defined `user.permission.read.default` permission as `2` and `b` only defined `user.permission.read.default` as `3`, eventually, in the summation of this user's grantive role, field `user.permission.read.default` will be `3` and all other fields will remain the same as below.


```
let permSum = {
    "with_rate_limit": 0,
    "permissions": {
        "flags": [],
        "max_session": 10,
        "cookie_expire_after": 13150000000,
        "user": {
            "permission": {
                "read": {
                    "default": 0,
                    "all":0,
                    "allow": []
                }
            },
            "role": {
                "read": {
                    "default": 0,
                    "all":0,
                    "allow": []
                }
            }
        },
        "role": {
            "read": {
                "default": 0,
                "all":0,
                "allow": []
            },
            "grant": {
                "default": 0,
                "all":0,
                "allow": [{Permission Editing Object(See below for detail)}]
            },
            "remove": {
                "default": 0,
                "all":0,
                "allow": []
            },
            "edit": {
	            "all": 0,
	            "allow": [
		            {Edit Permission Object(See details below)}
	            ]
            }
        },
        "article": {
            "read": {
                "default": 0,
                //With those
                "category": {
	                "all":0,
                    "allow": []
                },
                "tag": {
	                "all":0,
                    "allow": []
                }
            },
            "create": {
                "default": 0,
                //With those
                "category": {
	                "all":0,
                    "allow": []
                },
                "tag": {
	                "all":0,
                    "allow": []
                }
            },
            "edit": {
                "author": 0,
                "category": 0,
                "tag": 0,
                "self": 0
            },
        },
        "category": { //Global categories
	        "create": {
		        "default": 0
	        },
            "edit": {
	            "all":0,
                "allow": []
            },
            "remove": {
	            "all":0,
                "allow": []
            }
        },
        "tag": { //Global tags
	        "create": {
		        "default": 0
	        },
            "edit": {
	            "all":0,
                "allow": []
            },
            "remove": {
	            "all":0,
                "allow": []
            }
        },
        "forum": {
	        "other": {
		        "{{forum_id}}": {{Forum Permission Object}}
	        },
            "default": {
                "read": {
                    "category": {
                        "post_list": {
	                        "all":0,
                            "allow": []
                        },
                        "self": { //Description and name
	                        "all":0,
                            "allow": []
                        }
                    },
                    "tag": {
	                    "all":0,
                        "allow": []
                    }
                },
                "create": {
                    "category": {
                        "default": 0
                    },
                    "post": {
                        //With those
                        "category": {
							"all":0,   
                            "allow": []
                        },
                        "tag": {
	                        "all":0,
                            "allow": []
                        }
                    }
                },
                "edit":{
                    "post": {
                        "default": 0,
                        "constraint": {
                            "category": {
                                "all": 0, //Ignore category restriction when removing a post
                                "allow": []
                            },
                            "tag": {
                                "all": 0, //Ignore tag restriction when removing a post
                                "allow": []
                            }
                        },
                        "author": 0,
                        "category": 0,
                        "tag": 0,
                        "self": 0
                    }
                },
                "remove": {
                    "comment": {
                        "default": 0,
                        //Under a post with
                        "category": {
                            "all":0,
                            "allow": []
                        },
                        "tag": {
                            "all":0,
                            "allow": []
                        }
                    },
                    "post": {
                        "default": 0,
                        "category": {
                            "all": 0, //Ignore category restriction when removing a post
                            "allow": []
                        },
                        "tag": {
                            "all": 0, //Ignore tag restriction when removing a post
                            "allow": []
                        }
                    }
                },
                "config": {
                    "all": 0,
                    "allow": []
                }
            },
            "others": {}
        },
        "comment": {
            "user": {
                "default": 0,
                "all": 0,
                "allow": []
            },
            "article": {
                "tag": {
                    "all": 0,
                    "allow": []
                },
                "category": {
                    "all": 0,
                    "allow": []
                }
            }
        },
        "report": {
            "create": 0,
            "read": 0,
            "process": 0
        },
        "log": {
            "read": 0,
            "delete": 0
        }
    },
    "rate_limits": {
        "login": 1,
        "invite": 0,
        "report": 0,
        "edit": {
            "post": {
                "self": 0,
                "tag": 0,
                "category": 0,
                "forum": 0
            },
            "article": {
                "self": 0,
                "tag": 0,
                "category": 0
            },
            "comment": 0,
            "note": 0,
            "user": 0,
            "category": 0,
            "forum": 0,
        },
        "create": {
            "category": 0,
            "post": 0,
            "react": 0,
            "article": 0,
            "comment": 0,
            "note": 0,
            "forum": 0,
            "report": 0,
            "user": 0
        },
        "remove": {
            "category": 0,
            "post": 0,
            "react": 0,
            "article": 0,
            "comment": 0,
            "note": 0,
            "forum": 0,
            "report": 0,
            "user": 0
        },
        "site": {
            "change_config": 0
        }
    }
};
```

# Grantive role fields explained

## Special fields

#### with_rate_limit 
If this role comes with a rate limit constraint.

#### \*.all
For "all", value 1 = ignore allow list (Bypass, allow everything) , 0 = consider allow list.

#### flags
Store flags. See flags.md

#### max_session
Maximum sessions(login status) a user could have.

#### cookie_expire_after
Time(seconds) for a session to expire.

## User related permission fields

#### user.permission.read.default
The ability to read a user's permission(sum), see footnote[^1]

#### user.permission.read.all
Ignore allow list. Only consider read.default

#### user.permission.read.allow
Specific users' permission that this role could read.

#### user.role.read.default
See footnote[^3]

#### user.role.read.allow
Allow list for user reading the role of other users (Only the list of roles). The list itself is a list of roles, and users with roles defined in the list could be read (for their list of roles).

#### user.role.grant.allow.\* or user.role.remove.\*
Same as the definition in  `permissions.user.role.read`, those are the restrictions of what users could this user perform an action on, the specific roles that this user is allowed to grant/remove on others are defined in  `permission.role.read`.

## Permission(administration) related permission fields

#### role.read.default
See footnote[^4]

#### role.grant.default
0 = Disallow any role granting, 1 = allow role granting (Only the roles in the allow list). 2 = allow granting any roles (**Caution!** This means that users could grant any permission that they want as long as a role with such permission exists.)

#### role.remove.default or permission.role.edit.default
Same as above, but the action is remove/edit.

#### role.edit.all
This is a special "all" property. When it equals 1, the role will be able to edit permission as it wishes with no limitation.

#### role.edit.allow
This is a special allow list. It could be said that this is the most complex structure in the Blorum permission system. The objects stored in it are formatted as follows

{
	"on": [List of Roles],
	"allow": [
		{
			"lookup": [Permission Lookup Object],
			"value": {
				"any": 0,
				"numeric": {
					"max": [Numeric Value],
					"min": [Numeric Value]
				},
				"enum": [List of  String],
				"list": [List of value]
			}
		}
}

The `on` attribute is a list of roles that this object is able to change.

The `allow` contained a list of Objects that determined the roles' permission to edit permission, the structure of this object is explained as follows.


The `lookup` attribute indicates the specific permission that could be changed.

Permission Lookup Object is basically an ordered array, it indicates the keyname of permission, for example, permission.role.edit are stored as
["permission","role","edit"]

Wildcard is supported, for example, if you want this role to be able to grant others any permissions under user.role category, the Permission Lookup Object might look like this:
["permission","user","role","\*"]

The `value` attribute indicated what could the value be edited to.
If the value of a permission key should be numeric, the values inside `numeric` could limit the max(increasing) and the min(decreasing) value that the role can set. This value will only exist if the permission is numeric. The same for `enum`, and `list` (for a list, it is the value that the role could add/remove with)

`any` still works the same way - if declared `1`, any value could be assigned to the permission field(Of course, the range will still be checked).

Permission Lookup Object can point to `permission.role.edit.permission.[something]`. Blorum will parse it correctly. This could be confusing, still. Since technically it could form a permission-granting chain. This makes sense for webmasters who want atomic permission control, but for most scenarios, you might want to just use one-depth for permission allocation.

Permission Lookup Object can points to `allow_list`, as `list` in `value` will be used as constraints.


When roles are computed into the final sum, this field will be compiled, and eventually generate a Set, the Set will have permission's keyname as the key, and allowed values as the value. This is also one of the few fields that is stored differently inside Blorum.

## Article related permission fields

#### article.read.default

#### article.read.category.allow

#### article.read.tag.allow

#### article.create.default

#### article.create.category.allow

#### article.read.category.allow

#### article.edit.author

#### article.edit.category

#### article.edit.tag

#### article.edit.self

## Global category / tag related permission fields

#### category.edit.allow

#### category.remove.allow

#### tag.edit.allow

#### tag.remove.allow

## Forum related permission fields
### forum.other
This field contained a set (in user defined structure, a JSON key-value storage) that use a forum's ID as the key and Forum Permission Object for the role's permission on that forum as the value.
This will also be eventually compiled into a set inside Blorum.
### forum.default
This field also contained a Forum Permission Object, it is used as the fallback permission if the permission of a forum is undefined for the role.

However, the fallback will not be able to limit on the categories and tags that is forum-exclusive, it could only cast such limitation on global (shared between all forums) categories and tags.

### forum.default




---

[^1]: 0 = can only read self's {{something}}, 1 = able to read normal user's {{something}}, 2 = able to read everyone's {{something}}.

[^2]:

[^3]: 0 = cannot read anyone, include the user itself's {{something}}. 1 = only allow reading self {{something}}. 2 = allow reading non-administrative (flag:administrative) users' {{something}}. 3 = Allow reading everyone's {{something}}. {{something}} refers to what is the permission actually controls. 

[^4]: 0 = Disallow any such operation, 1 = allow such operation on targets defined in the allowlist, 2 = allow operation on all targets (Bypassing allow list)

