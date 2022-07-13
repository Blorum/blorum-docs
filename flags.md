# Document - Flags

Flags are the atom controller of Blorum's permissions.

They are just the prerequisite for a user to perform certain actions, specific 

contents could set different policies, allowing a user to act without those 

flags, or disable them with corresponding flags.

## True / False Flags

### bypass_all_rate_limits

## Categorical Flags

articles, posts, notes, comments:

- 0: Unallowed.
- 1: Allowed to perform action to themselves' contents.(For notes, it is the ability to create note with public visibility.)
- 2: Allowed to perform action to themselves' contents, even if it is "protected".
- 3: Allowed to perform action to everyones' contents, except those with an author that has the role of admin or moderator, and articles/posts with "protected".
- 4: Allowed to perform action to everyones' contents, except those with an author that has the role of admin or moderator.
- 5: Allowed to perform action to everyones' contents.

#### article.create

#### article.anonymous

#### article.edit

#### article.remove

#### article.pin

#### article.protect

#### article.react

#### article.react.remove

#### article.pin

#### article.report


#### post.create

#### post.anonymous

#### post.edit

#### post.remove

#### post.pin.forum

#### post.pin.global

#### post.react

#### post.react.remove

#### post.report


#### note.create

#### note.anonymous

#### note.edit

#### note.remove

#### note.report


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


### Varible Flags
