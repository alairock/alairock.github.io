---
title: Aftertouch in Logic Pro X and 2015 Macbook Pro
date: 2016-02-24 00:00:00 -07:00
categories:
- Music
- Midi
- Hacking
tags:
- music
- logic
- midi
- macbook
- trackpad
layout: post
---

Just upgraded to a 2015 Macbook Pro and one of the primary hardware update is the trackpad. The trackpad is not a physical button, like previous models, but static, and has force tracking technology underneath.

This hasn't really changed the experience for me day-to-day but I did find a really cool trick with it while playing with a Midi Python library. When you open Logic and open up the virtual keyboard, you can use your keyboard or mouse to activate a note and play a sound. With the new trackpad, it adds another element to this keyboard, aftertouch.

Aftertouch is after the note has been turned on, but before it has been turned off. In midi, this can be used to control things like vibrato, volume, pitch, and any combination of things. Logic has integrated the force sensor of the trackpad into the virtual keyboard, allowing you to incorporate aftertouch activities in the software.  Pretty neat, if you ask me.
