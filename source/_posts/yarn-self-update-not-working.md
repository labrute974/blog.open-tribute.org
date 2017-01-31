---
title: yarn self-update not working ?!
author: Karel Malbroukou
tags: [ node, javascript, bash, yarn ]
date: 2016-11-02 23:06:18
---

Hey y'all,

After a week of using YARN, I have to say I'm still happy with it.
A few things that I still can't get around/or fully happy with are the **yarn self-update **and the **yarn global add** that doesn't actually globally add binaries, but adds into the users yarn-cache folder.

But focusing on the problem at hand with the self update feature.

If you are using **yarn, version < 0.16.x**, you might receive the following error when trying to update it:

``` bash
10:25 $ yarn self-update
yarn self-update v0.15.1
error OAuth2 authentication requires a token or key & secret to be set
[...]
```

If you do encounter this error, just use **npm** to upgrade it to the latest version, then following updates of yarn has a fix for the self-update...
Yes, I know .. what an irony.. Anyway:

``` bash
10:25 $ npm -g install yarn
/usr/local/bin/yarnpkg -> /usr/local/lib/node_modules/yarn/bin/yarn.js
/usr/local/bin/yarn -> /usr/local/lib/node_modules/yarn/bin/yarn.js
10:29 $ yarn version
yarn version v0.16.1
```

Now you can enjoy the **yarn self-update**!!!
