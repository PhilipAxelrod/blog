---
layout: post
title: "PearTags: Building a Better AirTag™"
date: 2022-08-07 21:00:00 -0000
categories: P-AirTags Pear-tags
---

## The need for AirTag alternatives
My excitement for Apple's AirTags was summarily crushed upon realizing AirTags are not Android compatible.
While there are AirTag equivalents for non-iPhones such as Tile,
they don't work nearly as well. 
There's a simple reason for this.

Trackers like AirTags and Tiles work by pinging nearby smartphones over bluetooth.
These phones then send their location data to Apple's and Tile's respective servers, who in turn make this location data
available to the owner of the tracker. 
Thus, as long as a participating phone is physically near the tracker, the tracker's owner can infer the location of the
tracker from the location of the nearby phone.
AirTags work so well because everyone with an iPhone is forced to participate in this system.
Thus, an iPhone will almost always be near your AirTag, so you can almost always know your AirTag's location.

In contrast,
the only participating phones in Tile's similar network are those of other Tile owner. This network
is much smaller: In the entire Santa Barbara area,
there are only about 1,500 Tile owners.

So I did what any reasonable, self-respecting, and normal person would do in my situation: 
I set out to design and build my own location tracker that will work with Android phones, avoid
Tile's mistakes, and be cleverly named. 
Join me on this adventure to create PearTags!

## PearTags Will Use Wi-Fi Signals
Unlike AirTags, my tracker will not have a large network of GPS enabled iPhones to rely on for location measurements. 
Instead, PearTags will have to take their own measurements with their own sensors and calculate their locations
using their own processors.
From my literature review (insert citation here), there are three main options for measurements that PearTags could make
while maintaining a small form factor:

* Satellite GPS data
* Cell tower signal strengths
* Wi-Fi router measurements

While GPS can be fairly accurate in outdoor locations, it wouldn't work well for PearTags.
In indoor locations, GPS devices can't connect to as many GPS satellites as they can outdoors, greatly reducing their
accuracy indoors.

I also looked into using cell tower data because cell signal functions consistently in most outdoor *and* locations.
By measuring the strength of a cell signal from a particular tower, one can measure the distance from that tower.
Repeat that process three times, and you get three circles whose intersection reveals the tracker's location.
This is known as "triangulation," or "cell signal triangulation" in this case.
![](/assets/images/cell%20triangulation.jpg)

*Image courtesy of O'Reily, [source](https://www.oreilly.com/library/view/windows-phone-8/9780133383959/ch17lev2sec2.html)*

Unfortunately, this method tends to have an accuracy in the tens of meters.
This might help me find which building I left my jacket in, but nothing more accurate than that.

Finally, PearTags could measure Wi-Fi signals.
Unfortunately, Wi-Fi signals use on a lower wavelength than cell signal, which means that
physical objects like walls weaken and reflect Wi-Fi signals.
Thus, you can't accurately calculate the distance to a router based on Wi-Fi signal strength,
and therefore cannot use triangulation.

A strategy known as Wi-Fi fingerprinting overcomes this obstacle by 
not considering distance in the first place.
With Wi-Fi fingerprinting, "fingerprints" of Wi-Fi signal strengths (aka, RSSIs or Received Signal Strength Intensity) are recorded
onto a map.
When a device wants to find its location, it measures its current Wi-Fi RSSIs and compares them to the 
map of fingerprints.
The fingerprint closest to its current measurement is the devices' location.
Fancier Wi-Fi algorithms can get more precise location estimates by interpolating between nearby fingerprints. 

Wi-Fi fingerprinting can achieve an accuracy in single digit meters and functions well indoors, which is why I 
ultimately decided to use it for PearTags.

## Creating a simple proof of concept
Before rushing along to create a complicated algorithm that may ultimately fail, I wanted to 
first create a proof of concept. 
In particular, I wanted to be able to measure RSSI's in several location in my home,
and see there is enough data to clearly differentiate one location from another.
After all, in order for this adventure to succeed, it must be able to differentiate between my living room
and my kitchen.

Thus, I set about placing my laptop in various parts of my house, collecting 2 minutes worth of RSSI data
to account for RSSI noise.
Blog post about the scripting necessary to achieve this task coming soon. 
Below is a crude drawing I made of several rooms in my house:

![](/assets/images/crude-home-data/floor%20plan.PNG)

And here are my measurements of RSSIs from my neighbor's Wi-Fi
at various locations! 

![All-Data](/assets/images/crude-home-data/all-neighbor-distributions.png)

Tada!
Let's select just a few locations to compare at the same time, so we're not so overwhelmed with data.
Here is the distribution of RSSI's at the bookshelf, the piano, and location 9 (the kitchen)
![](/assets/images/crude-home-data/piano-kitchen-bookshelf.png)

From this we have some good news and some bad news.
For the good news, the RSSI distribution is clearly
different near the piano than in the kitchen and 
near the bookshelf. 
Thus, just by looking at my neighbor's Wi-Fi's
RSSI distribution, I can differentiate between
the piano location and the kitchen

For the bad news, we see that the kitchen and
bookshelf RSSI distribution are almost identical,
despite being several meters away from each other
and in different rooms. Using my neighbor's Wi-Fi
is not enough to differentiate between the bookshelf
location and the oven location.
This is also the case for the location of
my home router:
![](/assets/images/crude-home-data/router-book-kitchen.png)

The RSSI distributions are slightly different 
for location 5 and 6, but not enough to inspire
confidence:
![](/assets/images/crude-home-data/router-book-kitchen-5-6.png)

There are really only 3 clearly distinct RSSI distributions: the router distribution groups, the piano group,
and the area 5/border area distribution:

![](/assets/images/crude-home-data/neighbor-3-group.png)

Let's see if we can differentiate within the router group using my own network,
which is cleverly named PBIB10112408:
![](/assets/images/crude-home-data/pb%205-ghz/router%20group.png)

Wow! we have perfect separation! 
Now let's include the distributions in the router group that were partially distinct
from the router distribution:
