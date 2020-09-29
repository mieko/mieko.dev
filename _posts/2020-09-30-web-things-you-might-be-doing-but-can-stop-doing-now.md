---
layout: post
title: "Web Things You Might Be Doing But Can Stop Doing Now"
date: "2020-09-30 15:10:00 -0500"
categories: web
author: mieko
---

# The CSRF double-submit trick

The thing where you have a hidden form field in forms, and check that it's also in a cookie when you
process the request. Or you generate a signed message and include it in a hidden form field your
and verify the signature on the server.

This is called `protect_from_forgery` in Rails.

You know.

This adds boilerplate to both client-side code and server-side code:

```javascript
/* WE CAN STOP DOING THIS NOW */
const csrf = document
  .querySelector("meta[name='csrf-token']")
  .getAttribute("content");
  
const jsonRequestHeaders = {
  "Content-Type": "application/json",
  "X-CSRF-Token": csrf /* <---- LOOK HOW TERRIBLE THIS IS */,
};
```

You can get rid of that in 2020, and replace it with `SameSite=Lax` cookies.

It's "partially supported" in Safari, but the way it's doesn't effect `Lax`.

If you're still supporting IE11 (don't), most of that 1.03% population also has `SameSite=Lax`.

CSRF stops third-party sites from forcing your logged-in users to interact with your site in
unexpected ways. Cookies marked `SameSite=Lax` are only sent with requests from your origin.

So mark your session/auth cookies `Lax` and your CSRF problems go away, because the request will be
unauthenticated because you never get the cookie. Neat.

Most modern browsers already treat cookies without a `SameSite` value as `Lax`, so you're
already getting this protection. Make it explicit, then rip out all the CSRF double-submit stuff
from your code.

If you're writing an SPA do the token-passing thing, but please don't reinvent CSRF protection.  
It's fixed.  Something finally got fixed on the web platform and we can move on and forget it was
ever a thing.

Old browsers not supporting this have much bigger security issues than CSRF. We can now consider
this a client security issue.

# "Remember Me?" Checkboxes

This is something that's still being cargo-culted since I started webdev in the late '90s.

Back then, shared devices where a common thing. A family would have a "family computer" that
everyone took turns using. Hotels had "business" centers that were a few nasty HP Pavilions
sitting on desks near the lobby.

(Sidenote: If they still exist, you should never use a "public computer", especially one you intend
to enter a password into. **PEOPLE USED TO BANK ON THESE IN HOTELS**.)

"Remember Me?" or "Keep Me Logged In" checkboxes were for those situations, and those situations
are vanishingly small.

I had to do a lot of actual field research on this a few years ago, and the only significant usage
of shared devices in 2018 was (_ahem_) kids on an iPad watching YouTube. And they're not going to
be logging in and out every time the device changes hands anyway.

We have another somewhat-common scenario in business: The shared "shop" or "front desk" computer.
If you're building an SaaS that will probably be on the other side of this, "Remember Me?" is still
not useful. Overwhelmingly, the _next user_ is more likely to _log out the previous
user_ before logging in, because people are forgetful and rarely anticipate having to log-out.

A lot of times, the norm in this situation is to just use a shared account anyway if possible. This
is regardless of any office rules or a services' ToS.

If you know your users are in this scenario, a reality-based UI improvement you can make for
them is the opposite of short-lived session cookies: allow multiple accounts to be logged in
simultaneously and include a fast-account-switcher.  This is in contrast to a theoretical security
improvement that's no longer applicable.

In 2020, the browser itself is a security boundary. Anyone that sits at/handles another person's 
browser typically has saved passwords, saved credit cards, browser history, etc. And the browser 
relies on a combination of the operating system's user accounts, and integrated cloud sync services. 
Let that be the level of protection for long-lived session cookies.

Treat your log-in forms as if "Remember Me" is what everyone wants, and just make sure "Log Out"
easy to find.

Sam Dutton, Developer Advocate on the Chrome team has a great 20 minute video covering 
[Sign-in form best practices](https://www.youtube.com/watch?v=alGcULGtiv8).  "Remember Me?" is not
shown in any examples or even mentioned once.

# TLS &lt; 1.2

Hopefully in a year, this will read "TLS &lt; 1.3".  But even if you've dropped IE11, there's a 
weird long-tail of iOS devices stuck on iOS 12 and obscure Android mobiles that, in aggregate, 
are just at 10% of users 

You are wasting configuration brain-space and increasing vulnerability surface area by supporting 
anything older than TLS 1.2.  So pull those old versions out of `ssl_protocols`.

# EV Certificates

This is sort of a joke, because hardly anyone got tricked into buying these.  If you need data 
backing how useless these are, here's 
[a TLS certificate scrape of I did of the Alexa Top 2000](https://github.com/mieko/greenbar).

If your boss wants to waste money, try to convince them to [donate half the cost of an EV cert to 
LetsEncrypt](https://letsencrypt.org/donate/) instead.

# Cosmetic Validations

