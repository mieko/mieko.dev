---
layout: post
title: "It's 2020: It's Time for HTTPS by Default"
date: "2019-10-24 15:10:00 -0500"
categories: web
author: mieko
---
Hey, good people!

It's damn-near 2020, and it's time for us to convince browsers to go HTTPS by default.

I started an experiment a month ago: I configured [asuswrt-merlin](https://www.asuswrt-merlin.net/),
to completely black-hole outgoing port 80 on the network.

Over that time, over 40 clients have connected to my network: my friends and family.  iPhones,
TracFones, Apple TVs, 200+ Ubuntu VMs, Kindles, MacBooks, everything.  Dozens of people did their
normal non-nerdy online things with no HTTP port available.  I was paying attention and asking to
make sure.

It's crazy how much stuff worked.  We should do more of that.

Here's the one thing that did break for me though: my old portfolio site that I just moved to
[mikeowens.us](https://mikeowens.us/).  My stupid firewall config doesn't refuse a connection, it
just drops them.  I just got the domain recently, and it was still in the queue to be added to HSTS
preload.  So, when I typed the domain by hand, like it'd appear on a business card or written in the
sky by a plane, my browser would try to connect to HTTP/80 and just spin.  In a normal browser,
it'd just say `NET::ERR_CONNECTION_REFUSED`.

It's probably better that it did, because otherwise, it'd let ISPs and other assorted Eves rewrite
it.  The browser is just waiting for the MITM.

Every site I have control over is [in the HSTS Preload List](https://hstspreload.org/).  That always
seemed like a hack to me.  I mean, it's awesome that the functionality is there, but browsers
shipping a big-ass JSON file of domain names just seems janky.

I develop web applications all day, and have for years.  These days, if you want to do anything
half-interesting, even your development environment has to be HTTPS: Service workers, WebRTC, and
a lot of cool new APIs are gated behind a secure connection, for good reason.
[mkcert](https://github.com/FiloSottile/mkcert) makes that really easy to deal with (and opinionated
web frameworks like Rails should start doing that out of the box.  But that's another argument.)

## Fixing It

If I type `example.com` into my omnipresent omnibar, browsers should try `https://example.com/`
before it tries `http://example.com/`.  Because that's what's on the side of my truck.

I'd like to close my port 80 listeners that just redirect with HSTS headers to https.  I want to
close that MITM gap by default.  I don't want browsers trying plaintext before a secured
connection.  How can we do that?  Here's my best effort:

  1. Given a typed URL into a browser URL box, with no protocol given, assume HTTPS before HTTP.
     This kills a MITM vector, and will be more and more likely to be correct moving forward if you
     believe an encrypted web is becoming more prevalent.

     The HTTPS connection should respond with an HSTS header that confirms that was the right
     decision anyway.  As a nod to backwards compatibility, if it doesn't, killing the connection
     and re-requesting over HTTP is acceptable for now, because there's a chance different content
     is served over HTTP and HTTPS endpoints.  *For now.*

  2. *And if you have any yarbles:* treat URL-bar HTTP urls, and `http` links on the web as
     HTTPS transparently (like a more-chill `Upgrade-Insecure-Hosts`), with HTTP considered a legacy
     fallback.  In 2022, I want 'https' to be effectively  retired, and 'http' links to my site to
     be assumed to mean port 443+TLS by my browser.  If that fails, attempt 80+plaintext, with
     appropriate scary warnings in the UI.

HTTP has vulnerable spots already.  Assuming HTTPS-first closes one of the the last few encrypted
gaps we have that matters (*encrypted SNI rant here*).  Right now, sites trying to do the
*right thing* get an extra round-trip and MITM vector unless they're in HSTS preload.  Let's fix
this.

The world is getting scarier but better-equipped every day.  We're getting better tools, and it's
time to take advantage of them.
