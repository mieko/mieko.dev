---
layout: post
title: "systemd: I'm Totally Sold"
date: "2016-09-25 10:07:00 -0400"
categories: infra
author: mieko
---

I've been out of the "devops" game for a while, since it was just called
"admin".  So while I had read about the systemd/upstart wars, everything just
kept working for me, so I didn't pay too much attention.

When I had to take up parts of the ops role again at meter.md, it seemed like a
lot had changed: sysvinit was dead, upstart had came and gone, systemd won, but
it seemed like everyone hated it.

Circa 2000 or so, I wrote, but never released, a dependency-based half-assed
init called finitd (in C, totally unrelated to the Python utility that a web
search will find you).  It did the notify-pid-0-via-a-socket-thing that was
managed by client programs talking to the daemon.  I had utilities called cute
things like "wants" and "needs" and "provides" that were placed at the
beginning of the init script's `$PATH` before it was executed.  They'd notify
and and block as required.  The scripts ended up about as readable as a
script-based solution could be, a typical service like Apache being less than
a screen-full of code.

I didn't have cgroups or dbus, and I had the notion that it'd be Unix-
portable, so I got to avoid those decisions.  I had converted my SuSE machine
to launch with it, writing startup scripts that'd bring up system services all
the way through XFree86 and KDM/KDE2.  Spending two weeks sitting in hotel
rooms from 7PM to 2AM with gcc-2.95 and W. Richard Stevens' _Advanced
Programming in the Unix Environment_[^apue] pretty much solidified my alliance with
Unix: writing finitd basically just re-wired me to think in terms of Unix, and
I haven't been able to shake it since.

[^apue]:
    APUE is vastly underrated, even today.  I consider it the closest thing to
    a Unix programing bible as has ever been written.  Regardless of your
    experience level, reviewing APUE every other year will remind you of
    *something* that'll end up applicable soon.  The later editions,
    co-authored by Stephen A. Rago after Mr. Steven's passing, are equally
    excellent.  Keep it next to Knuth and the Dragon Book on the shortlist.
    My attachment to this book and era makes it the only book that just feels
    wrong if it's not a physical copy.


What sparked the ~6K lines that became finitd was basically being totally
pissed at sysvinit.  I'd do anything to avoid having to write an init script
for an application I was writing.  It was a pain in the ass.  They were hard
to properly test.  They were inherently non-portable.  And above all, it just
seemed like busywork.  

It seems like similar approaches were explored around the same time by other
people, and never quite caught on.  I didn't bother with releasing or promoting
finitd because I really had nothing to gain from it: and it'd just get picked
apart and I'd spend all day arguing on Slashdot about it, much the way Lennart
and crew had to on Hacker News through the eventual semi-acceptance of systemd.

Anyway, the point of this story is: I had a huge tribal need to defend "the
Unix way", and I think finitd exemplified that.  And it worked.  But even with
years of polish, it wouldn't have touched half the surface area of solved
problems systemd does with its approach.

There are a few things in Unix that hung around just way too long.  The tty/pty/
terminal abstraction needs to be stripped down to the parts that are still
useful.  crond should've been replaced long before systemd had a chance to take
it on.  The `fork(), fork(), setsid()` dance and `daemon()` should've been long
dead, because we should've had supervisors as smart as systemd years 20 years
ago.

A few days ago, during the middle of task, I naturally just had the thought:
"...and now I'll just write a quick systemd unit for that."

That's when I realized systemd had really become a great thing for
administrators.  I first looked into systemd *at all* by necessity just a few
weeks ago, and I had already assimilated the idea that "a quick systemd unit"
is a Real Thing in my toolbox, and had become part of my mindset.

I write systemd units like I edit config files.  I write scripts that generate
systemd units.  These are things I avoided with sysvinit and /etc/rc for years.

On the other side, these days I write my daemons like a normal program without
any gymnastics.  I run in the foreground.  I only fork when I *need* to fork.  
I don't have flags like `--detach` or `--no-daemon` or `--foreground`.  
I don't `syslog()`, I log messages to STDOUT and STDERR.  I don't create a
pidfile.  I exit with a return code from main like the prophets Ken and Dennis
intended.  I don't `chroot` or `setuid` for security.  I don't load a
.conf file if all I need are a few values that can be expressed in environment
variables.

The first few lines of running code are on their way to completing its task,
not dragging around 30 years of Unix daemon boilerplate.  That stuff is handled
in like 10 lines of systemd, and if they disagree with the user's use-case,
they're editable without learning another config file format.

I don't get the argument that systemd is desktop-centric.  On Linux desktops,
I could literally never touch the init system and get by fine not caring.  On
servers, I'm using for really useful stuff constantly.

I don't give a shit about what it does on a desktop. I do know that I'd trade
a decent amount of niceties on macOS to get systemd instead of the "can't force
myself to clutter my thoughts with this" launchd[^launchd].

[^launchd]:
    Please don't bring launchd into the BSDs.  There, we need something
    declarative and more modern, but not launchd as-is.  If I were tasked with
    solving this, I'd start from the systemd Unit file format and work myself
    backward.  The language has the right balance of simplicity, and enough outs
    and options to actually express the zoo of services you'd actually need to
    handle.

    Of course, because surely someone will come up with *obviously superior*
    nouns, verbs, and comma placements, this will never happen.


systemd is bigger than some people like, and I haven't even touched on timers
or socket activation or disks or the other stuff systemd subsumed.  

The difference is: If nothing else, systemd  made large parts of this stuff
actually useable by people other than package-maintainers and OS integrators.  
And it'll continue to take logic out of init scripts and eventually *even the
running daemons themselves* if the model continues to hold.

But the real bullshit argument here is the "Unix Way" one.  The "my init system
shouldn't care about my mountpoint and cdroms and network events" one.  

We're using a distributed filesystem holding user data, and we have daemons
that operate on this data.

The systemd way of handling this turns into a task that depends on the network,
and then depends on a service (glusterfs) to launch, and then ensures that the
network filesystem is mounted, before launching our tool at specified intervals
throughout the day.  This isn't stuff we can just leave to the default packaged
configs.  It's like two-dozen declarative lines because systemd knows about all
this stuff, and how they relate to each other: The network, launching daemons,
mount events, timers.  

What's the alternative "Unix philosophy" way?  We have perfect single-purpose
tools for each of these.  We'll have sysvinit or rc.d, and then set up ifupdown
hooks if we wan't to be informed when the network starts and stops, and  we'll
have scripts calling out to mount when we need to, and use cron for scheduling.

Everything perfectly handling its one piece of the problem.

So how do we orchestrate all this stuff together, to actually solve our
particulars?  Simple: a fucking 500 line init script that ends up looking
a *lot* like sysvinit.

The complexity systemd's "overreaching bloat" handles is our complexity now.  

And our implementation won't be as battle-tested.  

And it'll be written in bash.

Good luck with that.

If that's the Unix Way people want, I've lived it, and they can keep it.

*(You'll notice we have no comments here: This is a one-way trash-talking
soapbox, and I can never be wrong if I don't see rebuttals.)*

^D
