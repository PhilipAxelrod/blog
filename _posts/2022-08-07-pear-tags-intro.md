---
theme: jekyll-theme-cayman
layout: post
title: "P-air tags: Exploring ideas for building an AirTag alternative"
date: 2022-08-07 21:00:00 -0000
categories: P-AirTags Pear-tags
---

## The need for AirTag alternatives
When Apple released their AirTag trackers in 2021, I was initially quite excited for many reasons. For example, I could 
track my bike more easily, (see this blog post about 
the app I wrote to automatically note down my bike parking location) and cease worrying about losing jackets.

My excitement was quickly crushed after a Google search
revealed that AirTags were not compatible with Android phones.
I wasn't about to sacrifice my sparkling, brand-new Pixel phone, and while there are existing AirTag equivalents such as Tile,
they didn't work nearly as well. 
There's a simple reason for this, and it's not 
because AirTags are engineered better. 

Rather, it has to do with the fact that everyone with an iPhone is forced to participate in the process of locating a lost AirTag. You see, trackers like AirTags and Tiles work by pinging nearby smartphones.
These phones then phone home to Apple's and Tile's respective servers, who in turn make this location data
available to the owner of the tracker. 
Thus, as long as a participating phone is physically near the tracker, the tracker's owner can infer the location of the tracker from the location of the nearby phone.
Anyone with an iPhone participates in this system, whether they want to or not. AirTags work so well
because there are just so many iPhones scattered 
around the world that there will almost always be at 
least one iPhone near one's AirTag. 

In contrast,
the only participating phones in Tile's similar network are those of other Tile owner. This network
is much smaller: In the entire Santa Barbara area,
there are only about 1,500 Tile owners.

So I did what any reasonable, self-respecting, and normal person would do in my situation: 
I set out to design and build my own physical location tracker that would work with Android phones and avoid the issues
associated with Tile. Come join me on this adventure!