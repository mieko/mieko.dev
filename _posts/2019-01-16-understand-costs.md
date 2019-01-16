---
layout: post
title:  "Understand the Actual Costs of Building (or defer to someone who does)"
date:   2019-01-16 09:25:00 -0500
categories: software
author: mieko
---

I've got experience in a lot of totally different industries, so my anecdotes and analogies are all
over the map.

This is a housekeeping parable, but relates to software:  A lesson built around one of the
smartest, most well-intentioned, and hardworking people I've ever met.  A facilities manager at my
commercial housekeeping laundry.  But she made a mistake by not fully understanding costs.

My housekeeping operation has to provide those little foam dish scrubbing pads when we clean a room.
They're ultra-boring scouring pads.  They're included in the fixed cost we charge after cleaning
the room, so we have an incentive to keep the costs down on these things, within reason of
suitability.

We had a vendor for them, and they came in once a week. Each one was tossed into a bag with all of
the other, similarly boring items (toilet paper, soap, etc) and went out into rooms.

One day, while ordering, she noticed we could get scouring pads that were twice as wide and twice
as tall as what we needed, for only double our cost.  But we could get four out of each of them if
we just cut them down.  Basically cutting our scouring pad costs in half.

She ordered a pretty hefty cloth slicer for the job, only $40 or so, and a one-time purchase.
The new, over-sized scouring pads started coming in.  Boxes at a time.  During workday lulls, she'd
go to our prep room and slice them into quarters.  They were indistinguishable from what they
replaced.

Our laundry staff normally prepares these bags that go out to rooms, so she trained them to slice up
the pads, too.

A couple of things have already gone wrong: she failed to take into account her hourly rate.  She
had a good attention to detail, and her partitioned pads came out great.  And she was as fast at
it as a human could be expected to be.  

When *she* was quartering the pads, she didn't realize that the actual cost to the company had
tripled vs. the ones we had been using previously.  Her laundry subordinates didn't make
as much as she did.  When *they* were quartering the pads, the company cost had merely doubled.

Except sometimes, the pads would come out wrong.  They'd be cut misaligned, and we cannot send a
trapezoid out onto property.  This came up a few times, so after a while, she had to inspect the
output of her staff.  Sometimes she'd be too busy, and these weird trapezoid pads would find their
way into bags.  That means the housekeeper had to call up an inspector to get another one if she
didn't have a spare.

Sometimes the inspector would have to drive a few miles across the property to fetch more if they,
too, had exhausted their supply.  And sometimes the housekeeper wasn't paying attention, and placed
a weirdly cut scouring pad on the sink.  So the inspectors had to check every room they inspected
with special detail on the scouring pad.

Sometimes, a shipment would be late.  Our delivery driver would show up, heroically, just at the
last minute.  Right when we needed more scouring pads, like *now*.  But we couldn't just throw them
into bags, we'd have to appropriate extra people, some of which were never actually trained in the
fine art of scouring pad slicing, onto the project.  Thus generating more trapezoids.

With all of these cascading effects, scouring pads, one of the simplest parts of what we do,
probably reached 4-5% of the cost of cleaning a room.  It was in the tenth-of-a-percent range
before this cost optimization.

Developers are not asked to pay for the services we rely upon to make our products work.  Developer
time is the largest expense we have.  It may feel "free" to a developer, or even personally
profitable, but most of the time, if we can pay for a service that specializes in what we need it
to do, competently, it's probably our best choice to pay, unless a good argument can be made
otherwise.

Sometimes the cost is so low, just the few-hour debate on a ticket would've already bought us
months of service.

We don't set up our own mail servers and IRC channels and SCM hosting anymore for a reason.  It is
dramatically cheaper to hire it out if you're not doing it on your own time, as a hobby, for free.  

And the companies we pay to handle this stuff are experts: way better than the half-assed efforts
we could afford to put toward it, as it's not our core competency.  And what we get isn't some
shitty misaligned trapezoid version, either.

Don't write what ends up being thousands of dollars worth of code (from a company cost perspective)
to save $7 or $50 or probably even $1000/m.  I'm open about costs.  Sometimes paying $1000/m is
placed head-to-head against company costs many times over in terms of maintenance and development.

If my facilities supervisor had asked first, I would've told her it's not even worth optimizing,
but there are other things we could address that *would* have a real effect on the bottom line.  

Things that if she attacked, would put thousands of dollars a month back into the budget.  And
that's where raises and holiday bonuses and all that come from.

We went back to the normal scouring pads.  We even gave away a dozen boxes of the oversized ones to
rid ourselves of that problem.
