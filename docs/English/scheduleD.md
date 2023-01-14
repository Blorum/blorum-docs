---
layout: default
parent: English Documents
---
# ScheduleDaemon
scheduled.mjs is one of the core components of Blorum. It handles...

- User-defined scheduled tasks.
- Statistics lazy write.

## User-defined scheduled tasks

ScheduleD will handle user tasks like "publish after".

## Statistics lazy write

When the statistics of a resource are updated, Blorum will sync it to Redis. However, syncing every change in statistics would cause great stress on the MySQL server. So Blorum will sync those changes to ScheduleD first. ScheduleD will merge all changes and write them to the MySQL database periodically.