---
layout: post
title: "It's all connected"
date:   2015-06-19 20:00:54
tags: [the wire, pcap, graph, clustering]
excerpt: clustering the wire
---
TLDR: [I made a graph of _The Wire_ Season One.](/pages/wire/)

I have to imagine that most github.io blogs get started the same way: work is unfulfilling, and something is needed to quell the hungry software engineer's desire to create.  Today that happened to me, and I can tell you that having spent around 20 minutes setting up this blog (15 of which was ruby gem related), I feel better already.

For my innaugural post, I would like to show you how I found social clusters in my all time favorite show, _The Wire_.  I had the idea of visualizing dependencies around my 3rd viewing of the series.  For example, consider the case of [Brandon](http://ia.media-imdb.com/images/I/71wiha6D6SL.png) mentioning [Omar](http://ia.media-imdb.com/images/M/MV5BMTY2MzcyNTg2MF5BMl5BanBnXkFtZTgwMzkwOTM0MjE@._V1_.jpg)'s name during their first stick up.  This leads to [Avon](http://ia.media-imdb.com/images/M/MV5BMTY4NzExNTM4MV5BMl5BanBnXkFtZTgwODk4MjM0MjE@._V1_.jpg) putting a bounty on Brandon's head, [Wallace](http://ia.media-imdb.com/images/I/71UZcJWV7gL.png) collecting a piece of the bounty, Wallace feeling guilty and talking to the cops, [Stringer](http://ia.media-imdb.com/images/M/MV5BMjE1ODE0NDkyOF5BMl5BanBnXkFtZTgwNzM5MjM0MjE@._V1_.jpg) putting a hit out on Wallace, [D'Angelo](http://ia.media-imdb.com/images/M/MV5BMjY5OTM0MzY5MF5BMl5BanBnXkFtZTgwMTY4MjM0MjE@._V1_.jpg) being standoffish in prison (_Where the fuck is Wallace, String?_), Stringer putting a hit out on D'Angelo, and finally Avon allowing a hit on Stringer.

A few months ago, I finally was able to watch Amazon prime video on an android tablet.  I noticed the imdb x-ray data that pops up on the side:

<img src="/assets/images/wire/jameson14.png" width="640">

_My favorite scene: Jimmy celebrating sticking Rawls with 14 bodies_

It didn't click at first, but eventually it occured to me that I could build up a graph of character to character interactions based on what scenes they share together.  It's not perfect (thanks to scenes like the sentencing hearing where most of the Barksdale crew are all present), but it was enough for me to want to graph it.

###Step 1: Get the data
My first thought was "ok, lets firebug this and watch the requests go out to imdb".  Unfortunately, amazon xray is only available on certain devices, and the browser is not one of them.  If you are interested though, you can capture all the subtitles that way.  They are loaded on start, so you don't need to do anything fancy.

Second thought: let's set up a proxy server and send all my android traffic through it and log it.  The problem with that approach was that android only lets you proxy browser traffic, and not app traffic.

Third thought: let's man-in-the-middle it.  I don't remember the exact google searching that lead me this way, but I found [tPacketCapture](https://play.google.com/store/apps/details?id=jp.co.taosoftware.android.packetcapture&hl=en), an app that acts as a VPN and logs all traffic to a pcap file.  Perfect!

###Step 2: Look at the data

Looking through the session, one of the packets looked promising (first one in green):

<img src="/assets/images/wire/pcap.png" width="640">

Ok, export an http object (Wireshark -> file -> export objects -> http):

<img src="/assets/images/wire/export.png" width="640">

Note the one that says imdb.  This is looking golden.  Let's see what's in there:

<img src="/assets/images/wire/json.png" width="640">

Hot damn!  Not only does it have exactly what I want in clearly labeled json, but it also has scene information.  I thought I was going to have to figure out what constituted a scene based on character changes, but they have done that for me and given me a caption for error checking (_poor snot boogie_).

###Step 3: Graph it

You can play around with it [here](/pages/wire).  I blatantly stole an example from the d3.js website.  For clustering, I tried a few things, but the easiest was using [this matlab code](http://www.ece.ucsb.edu/~hespanha/software/grPartition.html).  I would have prefered to do this either server side or in browser, but that was way more effort than I was willing to make.  I think I went with 15 clusters as the winner.

<img src="/assets/images/wire/graph.png" width="640"/>

###What's next

I doubt I'll do the last 4 seasons for a few reasons:<br>
- the tediousness of capuring pcap<br>
- the matrix for all 5 seasons would be bigger than a standard broswer window<br>
- I'm ready for a new side project<br><br>

I didn't get what I originally wanted, but I did get to play with some new stuff along the way.  And my thirst to do something fun with code was quenched, at least temporarily.  Time to step it up, work.
