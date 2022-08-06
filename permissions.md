# Design document - Permissions

Blorum has some built-in roles of permissions, for atomized control, you need to change Flags of existing roles or create a new user role.

Roles could assign users permissions and rate limits, an user could have different user roles. Permissions could be assigned to roles.

If an user has multiple roles that has conflict permissions / role rate limits, the highest permission / rate limits will be left.

An user must have at least one user role.

Since JSON does not support Infinity, +Infinity will be stored as -1.


Roles' permissions could be as specific as certain forums, category, etc..., but an arbitrary permission to one type could also be applied. Such permission were stored in the key "any"(See default_permission.sql).

### Omni
**This role is code-builtin, and could not be removed with tweaks in database.**

Only the owner of the server should be, and by default, will be granted with this permission.

This permission allowed its user to bypass all verfications and rate limits, no restrictions will be applied.

Extensions developers should also implement so.

### Admin

### Moderator

