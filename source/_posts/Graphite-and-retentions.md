---
title: Graphite and retentions
author: Karel Malbroukou
tags:
  - Linux
  - monitoring
  - graphite
date: 2013-01-28 22:22:25
---

I'm Backkkk ... again!!!

So here, we'll talk about graphite woo!
If you don't know what is graphite, have a look at: http://graphite.wikidot.com/

Data retention can be configured in Graphite.

The way graphite does it, is by using a pair: **TimePerPoint:timeToStore**.

**TimePerPoint** refers to the interval between each stored datapoint (it's then also a precision metric -- average over the period specified).
**TimeToStore** refers to the number of datapoints we would keep.

The retention configuration is done through _/opt/graphite/conf/storage-schemas.conf_.

You can specified different retention settings for the same data by separating them with commas.
Example:
```
[mysql]
priority = 50
pattern = ^mysql\.
retentions = 60:43200,300:350400
```

This translates to: for any metrics under mysql, keep 43200 datapoints at 1-minute precision, and 350400 datapoints at 5-minutes precision which is:

*   43200 / 60 / 24 ==> keep 1-minute precision datapoints for 30 days
*   350400 / 12 / 24 / 365 ==> keep 5-minutes precision datapoints for 3 years
Also, be aware that retention are set in the file when first metrics is received.
This means that if you decide to change retention on a metric that you already have data for, you would have to either:
Delete the metric file, and leave graphite recreating it
Resize the file with the whisper command

For example, here's a file that doesn't have the retention we would want to, which would be 1-minute precision for 30 days:
``` bash
 ~ $ whisper-info.py /opt/graphite/storage/whisper/servers/host/network/if_eth0/down.wsp
maxRetention: 33955200
xFilesFactor: 0.5
aggregationMethod: average
fileSize: 6791068

Archive 0
retention: 33955200
secondsPerPoint: 60
points: 565920
size: 6791040
offset: 28
```

To change retention use:
``` bash
 ~ $ whisper-resize.py /opt/graphite/storage/whisper/servers/host/network/if_eth0/down.wsp 60:43200
Retrieving all data from the archives
Creating new whisper database: /opt/graphite/storage/whisper/servers/host/network/if_eth0/down.wsp.tmp
Created: /opt/graphite/storage/whisper/servers/host/network/if_eth0/down.wsp.tmp (518428 bytes)
Migrating data...
Renaming old database to: /opt/graphite/storage/whisper/servers/host/network/if_eth0/down.wsp.bak
Renaming new database to: /opt/graphite/storage/whisper/servers/host/network/if_eth0/down.wsp

 ~ $ whisper-info.py /opt/graphite/storage/whisper/servers/host/network/if_eth0/down.wsp
maxRetention: 2592000
xFilesFactor: 0.5
aggregationMethod: average
fileSize: 518428

Archive 0
retention: 2592000
secondsPerPoint: 60
points: 43200
size: 518400
offset: 28
```

Also ... what if you did resize but you can't see any more disk space in return ... well the resize command move the old data into a new file: .bak
You can either delete the bak file or use the command flag: --nobackup

That's it!!
I hope this will help you manage your graphite data better!
