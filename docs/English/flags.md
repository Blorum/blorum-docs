---
layout: default
parent: English Documents
---
# Flags

Flags are the atom controller of Blorum's permissions.

Flags were used to assign permissions to user roles.

## True / False Flags

Those flags stored in permissons.flags

### override_ip_rate_limits
Allow an user to override IP based rate limits.

### administrative
For detail please read below, it cooperate with other flags to limit permissions.

Those flags stored in permissions with corresponding keys.

## Categorical Flags

Type 1:
- 0: Unallowed.
- 1: Allowed to perform action to themselves' contents.(For notes, it is the ability to create note with public visibility.)
- 2: Allowed to perform action to themselves' contents, even if it is "protected".
- 3: Allowed to perform action to everyones' contents, except those with an author that has the role of admin or moderator, and articles/posts with "protected".
- 4: Allowed to perform action to everyones' contents, except those with an author that has administrative flag
- 5: Allowed to perform action to everyones' contents.

## Fallback permissions
Fallback permissions stored in "default", those 0-5 permission levels could be assigned to user's permission of specific category, tag... And could also be assigned to default fallback permissions when a resource is not protected by laws based on things like category.


#### article.create
##### description
0: disallowed to create articles.
1,2,3: allowed to create articles.
4: allowed to create articles, even in protected tags.
5: allowed to create articles, even in protected tags and categories.

#### article.anonymous

#### article.edit

#### article.remove

#### article.pin

#### article.protect

#### article.react

#### article.react.remove

#### article.pin

#### article.report


#### note.create

#### note.anonymous

#### note.edit

#### note.remove

#### note.report


#### post.create

#### post.anonymous

#### post.edit

#### post.remove

#### post.pin.forum

#### post.pin.global

#### post.react

#### post.react.remove

#### post.report


#### comment.anonymous

#### comment.pin.article

#### comment.pin.post

#### comment.pin.user

#### comment.create

#### comment.edit

#### comment.remove

#### comment.vote.up

#### comment.vote.down

#### comment.report


#### user.create

#### user.modify.regular

#### user.modify.sensitive

#### user.report

#### user.ban.post

#### user.ban.article

#### user.ban.react

#### user.ban.note

#### user.remove

#### user.statistics.read.regular

#### user.statistics.read.sensitive

#### user.permissions.read
##### description
0 will all result in users only has the permission to read their own permissions, 1 will allow them to read other user's permissions(except those with *administrative* flag), 2 will allow them to read anyone's permissions

#### user.session_list.read
##### description
0 will all result in users only has the permission to read their own session_list, 1 will allow them to read other user's session_list(except those with *administrative* flag), 2 will allow them to read anyone's session_list.

#### site.config.modify.regular

#### user.permission.modify

#### site.config.modify.sensitive




### Varible Flags

