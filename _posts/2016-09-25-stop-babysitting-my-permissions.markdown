---
layout: post
title: "Stop Babysitting my Permissions"
date: "2016-09-25 15:53:00 -0400"
author: mieko
categories: infra
---
The traditional Unix permission bits aren't that complicated:

User, Group, Other.
Read, Write, eXecute.[^1]

They've been successful because they allow you to express a lot of scenarios
without getting crazy-complicated.  It's hard to create an incomprehensible
situation with them.

You do hit the limitations of the scheme, though: There are times where you
think: *"Man, I wish this group could write, but this other group could only
read"*.  But those don't pop up as often as I would've guessed.  I can live
with it, I guess.[^2]

Anyway, these permissions are there so we can control access to files in a
more granular way than yes/no or "is this my file?" way.  The group ids and
group permission sets are a useful tool for a lot of models.

But even with the limited permissions Unix allows me to express, my biggest
permission headache has nothing to do with it.  It's shit like this:

    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    @         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    Permissions 0640 for 'nodes/postgres-client-b8792tmG/ssh.key' are too open.
    It is required that your private key files are NOT accessible by others.
    This private key will be ignored.
    Load key "nodes/postgres-client-b8792tmG/ssh.key": bad permissions
    Permission denied (publickey).

And then this:

    PGSSLKEY=/etc/ssl/metermd/postgres-client.key
    cult@postgres-client-b8792tmG:~$ psql -h postgres-server-Qy6Jnwj7
    psql: private key file "/etc/ssl/metermd/postgres-client.key" has group or
          world access; permissions should be u=rw (0600) or less

The group in question that has a `+r` is `postgres-client`.  I may want to
add users to that group to give them access to the keys.  You know, like what
Unix permissions were designed to do.  

Like the examples *exactly like this* in textbooks that explain users and
groups and permissions.

Invariably, these are fatal, with no switch like
`--im-using-permissions-as-designed-not-because-im-stupid` override switch.  Or
you get a blunt "just turn off all security for everything forever" flag.

Instead, I now have to script brainless tools like this, and decide where to
put them in the provision process:

```bash
if [ ! -f "$USER_KEY" ] || ! cmp -s "$USER_KEY" "$GLOBAL_KEY" ; then
  mkdir -p -m 0700 "$USER_KEY_DIR"

  cp "$GLOBAL_KEY" "$USER_KEY"
  chown "$USER" "$USER_KEY"
  chmod u=rw,g=,o= "$USER_KEY"
fi
```

All of this is to basically convince OpenSSH and PostgreSQL that I'm not
fucking up.

This also means I end up with a handful of copies of keys scattered around with
different owners that I have to chase during key rotation.[^3]

I get what the motivation is, and a warn-by-default would be OK.   But stop it.

Unix permissions don't give us lot of options here: don't take away the few it
*does* give us.

^D

[^1]:
    We'll ignore sticky/set[gu]id/search to keep this on-point.

[^2]
    I know there are a lot of Sheldons screaming about ACLs, and that you need
    a Turing-complete algebraic expression engine to properly express effective
    permissions in the real world.

[^3]:
    Soft link permissions are generally ignored in normal situations.
    Hard links cannot cross file system boundaries.
