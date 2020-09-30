---
layout: post
title: "Web Things You Might Still Be Doing But Can Stop Doing Now"
date: "2020-09-30 15:10:00 -0500"
categories: web
author: mieko
---

These are things you might be doing just because of habit.

I'm sure you'll find at least one issue that is *absolute garbage advice* because you have to make
JSP pages for the US Navy on Internet Explorer 6.  This isn't for you then.  I work on sites for the
public.

If you've been developing for a while, you've probably picked up some habits, and uh, just keep
doing them forever until someone points them out.  I'm here to point them out.  These are things
that made sense years ago, but now don't.

So stop doing them.

## The CSRF double-submit trick

The thing where you have a hidden form field, and check that it's also in a cookie when you process 
the request. Or you generate a signed message and include it in a hidden form field and verify the 
signature on the server.

This is called `protect_from_forgery` in Rails.

You know.

This adds boilerplate to both client-side code and server-side code the second you step out of the
framework at all:

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

It's "partially supported" in Safari, but the way it's weird doesn't effect `Lax`.

If you're still supporting IE11, most of that 1.03% population also has `SameSite=Lax`.

CSRF protection stops third-party sites from forcing your logged-in users to interact with your
site in unexpected ways. Cookies marked `SameSite=Lax` are only sent with requests from your Origin.

So mark your session/auth cookies `Lax` and your CSRF problems go away, because the request will be
unauthenticated because you never get the cookie. Neat.

Most modern browsers already treat cookies without a `SameSite` value as `Lax`, so you're
already getting this protection. Make it explicit, then rip out all the CSRF double-submit stuff
from your code.

If you're writing an SPA do the token-passing thing, but please don't reinvent CSRF protection. It's
fixed.  Something finally got fixed on the web platform and we can move on and forget it was ever a
thing.

Old browsers not supporting this have much bigger security issues than CSRF. We can now consider
this a client security issue.

## "Remember Me?" Checkboxes

This is something that's still being cargo-culted since I started webdev in the late '90s.

Back then, shared devices where a common thing. A family would have a "family computer" that
everyone took turns using. Hotels had &ldquo;business centers&rdquo; that were a few nasty HP 
Pavilions sitting on desks near the lobby.

(Sidenote: If they still exist, you should never use a &ldquo;public computer&rdquo;, especially one 
you intend to enter a password into. **PEOPLE USED TO BANK ON THESE IN HOTELS**.)

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
is regardless of any office rules or a service's ToS.

If you know your users are in this scenario, a reality-based UI improvement you can make for
them is the opposite of short-lived session cookies: allow multiple accounts to be logged in
simultaneously and include a fast-account-switcher.  This is in contrast to a theoretical security
improvement that's no longer applicable.

In 2020, the browser itself is a security boundary. Anyone that sits at/handles another person's
browser typically has saved passwords, saved credit cards, browser history, etc. And the browser
relies on a combination of the operating system's user accounts, and integrated cloud sync services.
Let that be the level of protection for long-lived session cookies.

### Oh yeah, and they don't work anyway and might be a security problem

Here's something that might have changed out from under you: 

> When do session cookies (like those set when you log in with an option like "Remember Me?" 
> unchecked) expire?

As an old dude, I'd want to still naturally assume "when I close the tab" or "when I close the 
window".  The idea being that, as a user, if I uncheck "Remember Me" and log-in, I'm logged-out 
when I "leave the site" by closing it.  This is no longer true.

In at least Chrome, Firefox, and Safari, the session cookie is actually removed "some time after
closing the tab, maybe".  This is to support features like re-opening closed tabs and windows, or 
"Continue where you left off" when restarting the browser.

You may consider this a misfeature (I don't, really), but "Remember Me" authentication features
probably don't work how users or developers think they do, and in a dangerous way.  Session cookies
no longer expire at any point that a normal human can predict or reason about.

Treat your log-in forms as if "Remember Me" is what everyone wants, and just make sure "Log Out"
easy to find.

Sam Dutton, Developer Advocate on the Chrome team has a great 20 minute video covering
[Sign-in form best practices](https://www.youtube.com/watch?v=alGcULGtiv8).  "Remember Me?" is not
shown in any examples or even mentioned once.

## TLS &lt; 1.2

Hopefully in a year, this will read "TLS &lt; 1.3".  But even if you've dropped IE11, there's a
weird long-tail of iOS devices stuck on iOS 12 and obscure Android mobiles that, in aggregate,
are just at 10% of users

You are wasting configuration brain-space and increasing vulnerability surface area by supporting
anything older than TLS 1.2.  So pull those old versions out of `ssl_protocols`.

## EV Certificates

This is sort of a joke, because hardly anyone got tricked into buying these.  If you need data
backing how useless these are, here's
[a TLS certificate scrape of I did of the Alexa Top 2000](https://github.com/mieko/greenbar).

If your boss wants to waste money, try to convince them to [donate half the cost of an EV cert to
LetsEncrypt](https://letsencrypt.org/donate/) instead.

## Cosmetic Validations

You have to sanitize inputs to your webapps.  But how much time should you spend on those error
messages for invalid form fields?  There's a chance you're doing fancy JS client-side validation,
and then falling back to the same logic on the backend, and reporting errors in a little pretty
error message box, etc.

[HTML5 client-side form validation](https://html.spec.whatwg.org/multipage/forms.html#client-side-form-validation)
can probably handle most of the "superficial UI" part of this problem, and they're supported pretty
much everywhere.

```html
  <input type=text required>
  <input type=email required>
  <input type=text minlength=8 pattern="\ANCC-\d+\z">
```

You still have to check for sanity on the backend, but letting browsers handle these ultra-common
cases means, in some apps, you don't even have to worry about how to *present* these errors to the
user.  [MDN has some great examples, too.](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation)

Don't control-freak over not being able to style the browser's validation UI.  That's the kind of
thinking that got us "Smooth scrolling" JS.

## Smooth-Scrolling JS

Just kidding.  You never did that, right?

## CSS Sprites

That thing where you just send one `.png` with a bunch of images crammed in, then specify the
bounding boxes in CSS.  This was to get around connection limits and head-of-line blocking behaviour
in HTTP/1.1.  You should be using HTTP/2 and that stuff matters hardly at all there.

## Things that may be on this list depending on how adventurous you are

  - Compiling ES6+ down to 1996 Javascript.  At least check the browsers you want to support and see
    what preset tweaks you can make.  I could save approximately 40% of the filesize of assets I'm
    working on if I could just target fully-up-to-date JS engines.

## Things that aren't on this list but should be

  - Language-choosers.  We should be able to respond to `Accept-Language` and be done with it.
    Users are weird, though, and browsers don't offer them anything quick, so you might still need
    the menu or lame little flag icons.
  - [Listening on port 80 at all.](/web/2019/10/24/https-by-default.html)

I'm gladly accepting additions to this list at [@leetliekmiek on Twitter](https://twitter.com/leetliekmiek)
