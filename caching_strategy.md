# Documents - Caching strategy

Blorum has a default caching strategy, which you could tweak or just disable it.

As a young project, I can't do any large-scale statistic tests to find the most optimized default config.


Blorum use **Redis** to cache.

Blorum were not designed to be running on multiple machine, therefore Blorum will not adapt **Redis Cluster**. if such demand do exists, a new project called Blorux will be started in the future.

## Overall

Blorum assume that there is three circumstances that a cache, would be the best.

- When a content is initially created, and does not show statistically important decline compared to average viewer stats.

- When a content suddenly encountered higher-than-average accesses.

- When a content maintained a statistically important higher-than-average viewer stats.


## How Blorum does statistics

Overall statistics, like total views, replies... were easy to stat, but Blorum need to stat 3day, 7day, 30day datas, thus, Blorum used a [hacky move](https://levelup.gitconnected.com/create-redis-sets-with-member-expiration-6471e560f89f). 

This hacky move, in default, will cause a bug in Blorum: An attacker could create a large amount of visits on a content that, actually, has no viewers, and when they stop accessing, those memory will hardly ever be freed.

Thus, Blorum will maintain a cached-content set, and **scheduled.mjs** will automatically check if there is access logs to be free every 3 days.

## When does those strategy started working?
Blorum caching strategy will not work until the corresponding content type has >10 numbers in total. Instead, if there is <10 contents for certain type, Blorum will always cache them.

## Default strategy for Circumstances #1
## Default strategy for Circumstances #2
## Default strategy for Circumstances #3
