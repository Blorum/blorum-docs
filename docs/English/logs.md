---
layout: default
parent: English Documents
---
# Logging Function

## CLI-side logging
This logger is more for debugging purposes, by default level(warning) it normally won't trigger.

## Site-side Logging.
### Level
By default, blorum use 4 logging level

- Log (level = 1)
Like "uid=2" created post pid=2... It logs normal behaviors.
- Warning (level = 2)
Like "uid=2" triggered rate limit... 
- Error (level = 3)
Like "uid=2" has invalid permission of "user.permission.read"