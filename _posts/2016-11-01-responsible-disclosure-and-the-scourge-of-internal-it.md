---
layout: post
title: "Responsible Disclosure, and the Scourge of Internal IT"
date: "2016-11-01 05:05:00 -0400"
categories: security
author: mieko
---

Google's recent disclosure of a bug in Windows after a seven day hold has caused some complaining.
I'm not going to repeat the story other than to provide a little context for this rant, but it
boils down to:

  * There is an exploit in the wild that is actually being used to compromise Windows machines.
    Google pinned it down and gave Microsoft only seven days to respond before disclosing.
  * Microsoft isn't too happy about the duration of this window, and asked for an extension
    because they have an established workflow in place to handle things like this.
  * Microsoft's process ("Patch Tuesday") was, as far as I can tell, brought about because IT
    departments inside of many businesses wanted ("needed") a predictable update schedule.
  * None of this shit works.

This is only peripherally related, but I'll take any excuse I can to run off at the mouth about
how terrible present-day corporate IT is.

Shitty IT departments have ruined software and the internet for too long.  It seems like they spend
the majority of their salaried hours figuring out how not to do their job.

Here's how IT should break down:

  1. Employees need to use computers and networks to be effective at their jobs, many of which are
     actually revenue-generating, instead of the cost center that is IT.
  2. IT should be there to enable and support these operations, and the proper functioning of the
     overall company infrastructure.  A lot of times that means protecting employees from
     themselves, and saying no and locking down stuff.

Instead, it's turned into:

  1. Employees need to be able to use up-to-date browsers and software to be able to accomplish
     their job more effectively than they did last year.  Some of their competitors have newer,
     smarter, systems, and management expects more per week than last year.
  2. IT finds a way to excuse *doing fucking nothing*.  They literally have to be drug into and
     usually *past* end-of-life notices from software vendors, and then whine about having to take
     action.

So software providers caught on to the fact that your average IT department is either grossly
incompetent, or have the work ethic of the DMV.  They started providing solutions: packaged-in,
on-by-default anti-malware.  Auto-updating "evergreen" browsers.  

IT whines because they "can't allow software to update itself without our knowledge".  But then
they do nothing.  Except try to stretch another few years out of IE6.[^upgrade]

[^upgrade]:
    I've virtualized ancient business-critical Unix systems when the company could no longer buy
    hardware replacements.  I've set up access to an otherwise network-isolated Windows server over
    VNC because it couldn't run a secure browser (or a modern RDP).  In either case, I knew these
    problems had answers beyond "everyone runs IE6 forever", or "we can't do anything about that."  
    If you're in IT, and you're getting paid to solve problems like that, it's your *job* to figure
    it out.  "Do nothing" isn't a solution.  I've started calling these types of IT departments
    "printer driver specialists".

A lot of software companies figured this stuff out a while time ago.  Hence the rise of "devops".
It's a way of saying "doing nothing forever is not doing your job, and be on-point, or the
developers will *drag* you along."  A SaaS provider is not going to be hamstrung because an IT
department refuses to add a firewall rule, or update PostgreSQL after 2004: their productivity
depends on it.

The client-side corporate end doesn't have such internal advocates, so they languish in insecurity
and workarounds for long-fixed bugs forever.

I'd wager that the average consumer-purchased and maintained desktop is more secure *and*
featureful than one that has a team of "professionals" "supporting"[^scare] it in a business
context.  Because the every day consumer has no policies that force them to turn off automatic
security updates, use old software that has known issues, or disable modern security features.

[^scare]: I'm going to break my keyboard scare-quoting so hard.

I've never seen a field where my expectations of the level of competency and the actual level of
competency are so wildly mismatched.

Microsoft caters to this because it's their bread and butter.  They've built processes around
dealing with organizations like that. I get it.  But that Microsoft is beholden to shitty IT
departments does not place Google at fault for not playing along.  It's a corner that part of the
industry has painted themselves into, and the rest of the world shouldn't have to unknowingly
suffer the repercussions of not having timely disclosure.
