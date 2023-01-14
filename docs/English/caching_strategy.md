---
layout: default
parent: English Documents
---
# Caching strategy

Blorum has a default caching strategy, which you could tweak or just disable it.

As Blorum is a young project, I can't do any large-scale statistic tests to find the most optimized default config.


Blorum use **Redis** to cache and store thread-shared states.

Blorum was not designed to be running on multiple machines, therefore Blorum will not adapt **Redis Cluster**. However, after Blorum is finished, a new project called Blorux will be started in the future. Blorux will bring Blorum with multi-thread capacity since blorum is designed to be prepared for multi-threading.

## Overall

Blorum assumes that there are three circumstances that maintaining a cache, would be the best.

- When content is initially created, and does not show a statistically important decline compared to average viewer stats.

- When content maintained statistically important higher-than-average viewer stats.

- When content suddenly encountered higher-than-average accesses.

## How Blorum does statistics

Overall statistics, like total views, replies... were easy to statistic, but Blorum need to stat 3day, 7day, and 30day data, thus, Blorum used a [hacky move](https://levelup.gitconnected.com/create-redis-sets-with-member-expiration-6471e560f89f). 

This hacky move, in default, will cause a bug in Blorum: An attacker could create a large number of visits on content that, actually, has no viewers, and when they stop accessing, those memories will hardly ever be freed.

Thus, Blorum will maintain a cached content set, and **scheduled.mjs** will automatically check if there is a cache to be free every 3 days.

## Default strategy for Circumstances #1
When a new post is created, Blorum will cache it for 3 days, if its viewer count & participant count is lower than the 7-day average, its cache got removed.

## Default strategy for Circumstances #2
If content maintained a viewer count that is higher than (Average + 0.3\*IQR), Blorum will maintain its cache.

If a content maintained a viewer count that is higher than (Average + 0.4\*IQR), Blorum will maintain its, and also its comments & notes' cache.

## Default strategy for Circumstances #3
If a content's one-hour viewer is 50%IQR higher than the 7-day hour average or 50%IQR higher than the overall hour average, it got cached for 1 day.