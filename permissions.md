# Design document - Permissions

Blorum has some built-in roles of permissions, for atomized control, you need to change Flags of existing roles or create a new user role.

Roles could assign users permissions and rate limits, or deprive them with permissions, an user could have different user roles. Permissions could be assigned to roles.

## Type of user roles
There are two types of user roles: **grantive** and **limitive** 
### Multiple grantive role
If an user has multiple roles that has conflict permissions / role rate limits, the highest permission / rate limits will be left, so does the max_session limit.

limitive is considered before grantive.

## How user's permission being determined by roles
The permission sum of a user could be summarized into this formula:
#### permSum = max(grantive_role) - max(limitive_role)

Limitive role has higher priority than grantive role. The maximum sum of limitation is checked first, if the action of user is not limited by it, Blorum will check user's grantive role's permission sum to see if user were allowed to perform the action. 
 
## Rate limits
Rate limits also follow the same judgment: the minimum rate limit values (so it's the maximum limitation) given by limitive role sum is considered first, then the maximum rate limit values (so it's the minimum limitation) given by grantive roles was considered.

## Other rules
**An user must have at least one grantive user role.**

User does not necessarily need a role with rate limits defined, if user has no roles defining rate limits, they are still confined by IP-based rate limits. Unless they have a role that has flag *override_ip_rate_limits*. However, a warning log will be generated if Blorum detected that an user don't have a role with rate limits or flag *override_ip_rate_limits*.

Since JSON does not support Infinity, +Infinity will be stored as -1.


Roles' permissions could be as specific as certain forums, category, etc..., but an arbitrary permission to one type could also be applied. Such permission were stored in the key "any"(See default_permission.sql).

## Permission Data Structure for Roles
Take a possible permission of a site's moderator as an example
**This is a grantive role**

```
{
	
	"with_rate_limit": 1, //Indicate that this roles has rate limits with it

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

Take a possible limitation-permission of a untrusted user as another example
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

### Guest (pseudo)
User with uid = 1 automatically has this role.

This role is reserved for guests with no login status.

### Omni (pseudo)
**It is not an actual role, and could not be removed with tweaks in database.**

User with uid = 2 automatically has this role. This role cannot be granted to other users.

This permission allowed its user to bypass all verfications and rate limits, no restrictions will be applied.

Extensions developers should also implement so.

### Admin

### Moderator

## Actual complete grantive role permission JSON structure

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
                    "allow": []
                }
            },
            "role": {
                "read": {
                    "default": 0,
                    "allow": []
                }
            }
        },
        "role": {
            "read": {
                "default": 0,
                "allow": []
            },
            "grant": {
                "default": 0,
                "allow": []
            },
            "remove": {
                "default": 0,
                "allow": []
            },
        },
        "article": {
            "read": {
                "default": 0,
                //With those
                "category": {
                    "allow": []
                },
                "tag": {
                    "allow": []
                }
            },
            "create": {
                "default": 0,
                //With those
                "category": {
                    "allow": []
                },
                "tag": {
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
        "category": {
            "edit": {
                "allow": []
            },
            "remove": {
                "allow": []
            }
        },
        "tag": {
            "edit": {
                "allow": []
            },
            "remove": {
                "allow": []
            }
        },
        "forum": {
            "default": {
                "read": {
                    "category": {
                        "post_list": {
                            "allow": []
                        },
                        "self": { //Description and name
                            "allow": []
                        }
                    },
                    "tag": {
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
                            "allow": []
                        },
                        "tag": {
                            "allow": []
                        }
                    }
                },
                "edit":{
                    "post": {
                        "default": 0,
                        "constraint": {
                            "category": {
                                "all": 0, //Ignore category restriction when removing post
                                "allow": []
                            },
                            "tag": {
                                "all": 0, //Ignore tag restriction when removing post
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
                            "all": 0, //Ignore category restriction when removing post
                            "allow": []
                        },
                        "tag": {
                            "all": 0, //Ignore tag restriction when removing post
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

`with_rate_limit` : If this role comes with a rate limit constraint.

`*.edit_list` :  The permission to edit `allow_list`. 0 = disallow, 1 = allow

`permissions.flags` : Store flags. See flags.md

`permissions.max_session` : Maximum sessions(login status) a user could have.

`permissions.cookie_expire_after` : Time(seconds) for a session to expire.

`permissions.user.permission.read.default` : The ability to read an user's permission(sum), see footnote[^1]

`permissions.user.permission.read.all`: Ignore allow list. Only consider read.default

`permissions.user.permission.read.allow`: Specific users' permission that this role could read.

`permissions.user.role.read.default` : See footnote[^3]

`permissions.user.role.read.all` : See footnote[^2]

`permissions.user.role.read.allow`: Allow list for user reading the role of other users (Only the list of roles). The list it self is a list of role, and user with roles defined in the list could be readed (for their list of roles).

`permissions.user.role.grant.allow.\*` / `permissions.user.role.remove.\*`: Same as the definition in `permissions.user.role.read`, those are the restrictants of what users could this user perform action on, the specific roles that this user are allowed to grant/remove on others are defined in `permission.role.read`.

`permission.role.read.default`: See footnote[^4]

`permission.role.grant.default`: 0 = Disallow any role granting, 1 = allow role granting (Only the roles in the allow list). 2 = allow granting any roles (**Caution!** This means that users could grant any permission that they want as long as role with such permission exists.)

`permission.role.remove.default`, `permission.role.edit.default`: Same as above, but action is remove/edit.

`permission.role.edit.permissions.default`: 0 = You cannot edit any permissions. 1 = You can edit permissions as allow list has defined. 2 = You can edit any permissions (**Caution!** This means that users could grant any permission that they want by granting their user role with the desired permission.)

permission.role.edit.permissions.allow: This is a special allow list. The objects stored in it are formatted as follow
{
	"lookup": [Permission Lookup Object],
	"numeric_edit": {
		"max": [Numeric Value],
		"min": [Numeric Value]
	}
}

Permission Lookup Object is basically an sorted array, it indicate the keyname of a permission, for example, permission.role.edit are stored as
["permission","role","edit"]

Wildcard is supported, for example, if you want this role to be able to grant others any permissions under user.role category, the Permission Lookup Object might look like:
["permission","user","role","\*"]

If the value of a permission key shoule be numeric, the values inside `numeric_edit` could limit the max(increasing) and the min(decreasing) value that the role can set. This value will only exists if the permission is numeric.

permission.article.read

---

[^1]: 0 = can only read self's {{something}}, 1 = able to read normal user's {{something}}, 2 = able to read everyone's {{something}}.

[^2]: For "all", value 1 = ignore allow list (Bypass, allow everything) , 0 = consider allow list.

[^3]: 0 = cannot read anyone, include the user itself's {{something}}. 1 = only allow reading self {{something}}. 2 = allow reading non-administrative (flag:administrative) users' {{something}}. 3 = Allow reading everyone's {{something}}. {{something}} refer to  what is the permission actually controlling. 

[^4]: 0 = Disallow any such operation, 1 = allow such operation on targets defined in the allowlist, 2 = allow operation on all targets (Bypassing allow list)

