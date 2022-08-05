# Design document - Permissions

Blorum has some built-in roles of permissions, for atomized control, Flags needed to be manipulated.

User roles defined permissions that an user have, an user could have different user roles. Permissions could be assigned to roles.

If an user has multiple roles that has conflict permissions, the highest permission will be left.

*Except for Omni*

### Omni

Only the owner of the server should be, and by default, will be granted with this permission.

This permission allowed its user to bypass all verfications.

Extensions developers should also implement so.

### Admin

### Moderator

