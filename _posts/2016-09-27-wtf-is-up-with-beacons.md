---
layout: post
title: "WTF is up with Beacons?"
date: "2016-09-27 15:16:00 -0400"
categories: iot
author: mieko
---

I just pre-ordered a handful of the new [C.H.I.P.](https://getchip.com/pages/chip)
single board computers to experiment with.

I've played-with and prototyped-on different single-board Linux computers
for years, but the game changers here are:

  1. $9USD/ea in quantities of **ONE**, with:
  2. 4GB on-board flash
  3. WiFi and Bluetooth 4.0/BLE.  This is huge.

These things aren't shipping in huge numbers yet (although they've delivered
thousands), but it looks promising.  And now, I know that this price-point, with
that hardware, is *possible*.  So I think *somebody* will be successful with it
soon.

We jumped on iBeacon/Eddystone early on, because it directly solved a problem
we were working on.  We bought dozens of beacons to develop against from 4-5
manufacturers at $20/$30 each, betting that the price would end up in the $5-$10
range soon enough, through adoption and competition.

Two years later, the beacons that work reliably, are field programmable, have a
useful battery life, and are ready-to-use out-of-the-box still cost around
$20-$30USD/each[^1].  One of our customer installations can have over a hundred
beacons.

We don't make money on beacons: we just want our customers to have them.  Our
software can do all sorts of cool stuff if they're installed.  I've spent hours
on the phone with sales people trying to see where the price breaks would be:
They're not significant at anything quoted to me, even at volumes twice what
we'd need.

Luckily, our use case is semi-permanent installations, where AC power is
easily available and convenient.  The battery thing is considered a maintenance
burden for our customers anyway, but the ability to place them *anywhere* is
really nice.  The AC vs. battery thing is a tradeoff that can go either way.
Lucky me.

BLE Beacon manufacturers are really pricing themselves out of the game.  The
features I could add with a single-board Linux computer "beacon" still make my
head spin.  (And Linux has the awesome [bluez](http://www.bluez.org) stack!  So
they can still speak iBeacon and EddyStone and be a strict functionality
superset.)

The only advantage of BLE Beacons for us would've been unit price.  The
C.H.I.P. has low-end-smartphone class hardware and capabilities at about a
third of the cost of a UUID-broadcasting "dumb" device.  This is a pretty
stupid reversal of how I guessed it'd turn out.

Even after I factor in suppliers for a case and AC adapter, if C.H.I.P., or
something similar, plays out, I think I could get an *actually smart* "beacon"
into production at two-thirds-is the cost of what's available on the turn-key
BLE Beacon market.

(Everybody calls their beacons smart.  They're not.  A "smart" beacon could do
at least part of what's possible with these SBCs with a BLE chip: *notice the
phone and take action in response to it*.  Make HTTP requests.  Allow
establishing a richer streaming Bluetooth channel to the device.  Play Quake.
Whatever.  I mean: *smart* to me in this sector means "runs my code".  I'd
settle for straight C. The C.H.I.P. offers the entire Debian ecosystem.)

Pinched between NFC and SBCs, I'm not seeing too much room left for Beacons at
their current price point.  For the functionality they provide, I feel like
they should cost, with a plastic case and battery, less than $5 in quantities
of a few hundred.  Apple opening iOS to NFC would kill the entire industry now,
before they've had a chance to become entrenched.  I'd say good riddance.

What's sad about the whole thing: If iOS allowed NFC access to applications, and
the Android landscape were a little more consistent, we could've solved our
problem with dime-a-dozen RFID/NFC transmitters from the word go.  But I've
already got a few *really* interesting ideas about what we could do with a
*truly smart* beacon.

[^1]:
    Trust me, we've done everything from high-dollar "fashion" manufacturers,
    to weird naked grab-bag PCB stuff on Alibaba.  We need a reliable vendor
    with some sort of stable, consistent supply chain, and something that we
    can feel good about developing against and having in the field for a few
    years.
