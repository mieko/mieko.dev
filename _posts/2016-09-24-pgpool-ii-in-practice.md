---
layout: post
title:  "pgpool-ii in Practice"
date:   2016-09-24 15:52:14 -0400
categories: postgres infrastructure mieko
---

pgpool-ii is awesome.  We don't have it in production yet, but have a staging
environment and have run it through production-scale loads, and are getting
ready for it.  Here's a few tips:

  1. *Pretend it doesn't support SSL at all.*  On the frontend-side, it has no
     hostname mapping (e.g., `hostssl ... cert clientcert=1`, etc).  On the
     backend side, it has no way to specify a separate SSL certificate/key than
     the front-end, has no way to specify something like `sslmode=verify`, and
     doesn't use libpq, so it doesn't support configured conninfo strings or
     environment variables.  Just wrap it in stunnel.  On both sides.  Yeah.

  2. Since you've now got pgpool operating on local sockets, you'd think you
     can set `listen_addresses = ''` as the docs state.  Unless you're using
     3.5.4, which was just released and isn't readily packaged yet, there's a
     bug that'll stop you from launching with that setting.  So let it listen
     on localhost on a bullshit port and firewall it for now.  Note: Your port
     setting will effect the name of the socket files it generates.

  3. stunnel has a `protocol = pgsql`, which is exactly what you want for the
     local socket to postgres backend connection.

  4. `protocol = pgsql` doesn't imply `options = NO_TICKET`, which is required,
     because the postgresql server doesn't support session tickets, and will
     promptly kick you after the first connection.

  5. Item #4 is hard to debug when your path is: client → stunnel → pgpool →
     stunnel → postgres.

  6. [Just work off of these hard-fought stunnel configs.](https://gist.github.com/mieko/a075f9ce8cb8fd5c68fed310acebe449)
