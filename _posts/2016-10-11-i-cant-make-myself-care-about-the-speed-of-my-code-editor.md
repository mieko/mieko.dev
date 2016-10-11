---
layout: post
title: "I Can't Make Myself Care about the Speed of my Code Editor"
date: "2016-10-11 05:50:00 -0400"
categories: tools
author: mieko
---

Above a certain minimum usability threshold, I don't care how fast my code
editor performs.  I don't care if it can open a 2GB SQL dump.  I care about
features that make me more effective while writing *code*.[^1]

And you get those features through breadth.  And you get breadth through
ecosystem.  And you get ecosystem via programmer accessibility.  And that
accessibility has a good chance of making you slow.  I'm fine with that.

To me, a "code editor" lives in that spot above a text editor, which has no
conceptual understanding of the language you're writing in, and a full-blown
IDE, which does. A code editor has a *shallow understanding* of your language.
Enough to syntax highlight, draw red squiggles under words, maybe ctags-style
rudimentary navigation.  You know.

Now, people like to whine about it, but an IDE has a total out for being slow.
It's doing a *lot* of work.  A lot of them literally run a compiler front-end
over your buffer every few key-strokes.  They constantly re-index a huge amount
of open buffers and closed files to resolve references, so you can click on
`UIViewController` and bring yourself to the documentation or header file.

The engineering effort required to do all of that in real-time is huge.  Which
is why most IDEs focus on *a* language, or *a few related* languages.  It's a
depth vs. breadth thing.

In my day to day work, I need a *lot* of breadth.  That's sort of the nature/
nightmare of modern web and mobile development.  And you get breadth by making
your editor easy to extend.

A while after Chrome came out, it got a lot of flak when they revealed its
extension system.  It was *really* simple compared to Firefox's, and offered
nowhere near the same capability.  Around the same time, they decided to
remove the "View Background Image" context menu item.  I thought this was a
good excuse to play with the extension API.  After about an hour, I had
[this](https://gist.github.com/mieko/a6a16d239d8179fd8b771322dfa4706f).

Having written Firefox add-ons (And previously, the worst experience this side
of autotools: *Mozilla Browser extensions*), I knew immediately that this was
the way to go.  What it lacked in capability, it made up for in approachability.

When a programmer doesn't face a huge uphill battle to scratch their itch, they
actually will.  And, being the helpful (AKA vain) species we are, we'll
publish it.

I moved to Atom a few years ago, when it *barely* hit the minimum bar for
speed.  Hell, it still lacked a lot of features I really wanted.  But it had
the few things I knew it *had to have* to end up useful to me:

  1. It was (eventually) open source.  I change code editors about as often as
     I plan on changing spouses, so I wasn't going to risk having my muscle
     memory re-trained on something that would end up abandoned.  Enough people
     outside of GitHub care about Atom that I know it won't end up useless if
     the original authors call it a day.
  2. It had a straightforward plugin API that people were actually using
     en-masse.  It did this by shoving a browser and JS runtime into a binary
     and developing on top of that.  Crazy times.

As a programmer, I'm not a fan of Javascript.  I'm not a fan of "Classic
Javascript".  I'm not a fan of "Modern Javascript".  I think The Good Parts are
half-assed.  But you can't avoid it, and there's a huge mass of people with
much worse taste than myself eager to write Javascript.

And if you want an editor with breadth, you need an army of people contributing
to the ecosystem.  And you do that by making it easy for them to scratch their
itch by using technologies that they're familiar with.  Even if that means
benchmarks put you at a quarter the rendering speed of SublimeText.

Even if that means "it gets unusably slow when I open `/dev/hda`".

Even if that means *I* have to write Javascript to extend it.

This perspective doesn't apply to all fields.  Postgres or Linux would be a
disaster prioritizing like this.  But weirdly enough, **a code editor needs
network effects**.  Really bad.

If you felt burnt by the SublimeText thing (I was), or the TextMate thing
(skipped that one), consider this point.

An editor thats an order of magnitude faster than Atom means nothing to me if
it's not at least *approaching* feature parity, ecosystem included.

## Bonus: A Mini-Rant about Hardware and Tooling

VSCode is pretty awesome.  I've played with it a lot over the last two months.
I need multiple project folders, blah, blah..., but it'll get there.

People tell me it's faster than Atom.  I've read it enough to believe it, but
I can't tell.  And I spend half my time on a 2011 mid-range iMac.  And before
that, when Atom was slower, Ubuntu + an Acer laptop that was so old that the
battery was more likely to catch fire than hold a charge.

What are you people coding on?

  > Well, every morning I have to manipulate a 3GB nginx log file as an
  > integral part of my job.  Atom and VSCode are useless because they can't
  > handle this.

I understand that not everyone "thinks in Unix", but when I was using Vim for
*everything* (for over a decade), which can handle files that size just fine,
it still didn't feel like a problem with "an editor solution".  It felt like a
problem for `grep` or `sed` or `awk`.

I wouldn't get too attached to a hex editor unable to deal with large/huge
files.  I expect it to handle an edit to a BluRay ISO file's header or
whatever.

But I want my code editor to provide the best experience it can for editing
code.  You know: Text files, typically a few hundred lines, maybe a few thousand
lines if you're on a shitty project or hand-editing generated code like a shitty
programmer.  A tradeoff made that sacrifices *anything*  for a dealing MB+
log files was chosen poorly.

Yeah, we'd all like a Wrangler that can climb over a building, and then do 0-60
in 4 seconds from stoplight.

In reality, the things that make a Wrangler good at what it does are directly at
odds with what makes a Model S awesome.  And in software, it doesn't even have
to be an engineering trade-off: it can be man-power, feature priorities, or
choosing an architecture that sucks for edge cases but optimizes for ecosystem.

[^1]:
  This is inspired by a recent event where Atom got ultra slow: I pasted a 15KB
  single-line of Base64-encoded PNG into a vCard for compatibility testing.

  I wouldn't want Atom to be optimized for stupid cases like this at the
  expense of *anything* else.  There are maybe a dozen times in the last decade
  where handling that better would've been useful.  Keep improving highlighting
  and tree-views and linters and panes and the extension APIs.  You know, code
  stuff that brings benefit every day.
