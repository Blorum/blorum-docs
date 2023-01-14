---
layout: default
parent: English Documents
---
# Reserved data in database

Blorum often uses id=0 columns as reserved data, for example, a comment in reply to cid 0 will be considered as a first-level comment, for another example, a user invited by uid=0 user is considered to have their account created by themselves.

## Roles determined by UID

The uid=1 will by default the guest user. This role is used for those requests without a login status.

The uid=2 will by default the omni user. Omni is not a role that could be given. 