scheduled.mjs is one of the core component of Blorum. It handles...

- User defined scheduled tasks.
- Statistics lazy write.

## User defined scheduled tasks

ScheduleD will handle user tasks like "publish after".

## Statistics lazy write

When the statistics of a resource is updated, Blorum will sync it to Redis. However, sync every changes in statistics would cause great stress on MySQL server. So Blorum will sync those changes to ScheduleD first. ScheduleD will merge all changes and write them to MySQL database periodically.