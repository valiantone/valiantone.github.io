---
title: A Whirlwind Intro to Servers in 2024
pubDatetime: 2024-09-19T15:30:00Z
postSlug: serving-primer
featured: true
draft: false
tags:
  - cs-fundamentals
  - networking
  - serving
  - backends
description: Get a contextual intro to web servers and start hands-on with a minimal example in your preferred language to see servers in action.
---

> This article is part of our **CS101** series, designed for new coders and learners who want to intuitively understand the computer science theory behind the applications they use. In this post, we explore web servers — what they are, why they matter, and how they revolutionized software delivery.

Servers are everywhere—powering everything from the websites we browse, to the apps we use, and the data we rely on daily. They’re so deeply integrated into our lives that many of us intuitively know they are the backbone of our digital world. But how much do we really understand about the inner workings of these systems?

So, let’s take a step back and explore the roots of server technologies. Ready to dig in?

### How Web Servers Changed the Game

**For years, software had been a tangible product** — packaged in CDs or floppy disks, shipped off to users in boxes, and installed directly onto their machines. Whether it was a classic video game, office software, or early computer programs, one thing remained constant: the code ran **client-side**, meaning everything happened on the user’s machine. Software delivery was static, confined to what was contained in those hardware boxes, and updates or changes required entirely new shipments.

This world of static, local software was **revolutionized in 1990** when **Tim Berners-Lee** ran the first **web server** at CERN. With the simple invention of the **World Wide Web**, we changed the landscape of how we think about software. For the first time, **code could live elsewhere**—on remote servers—allowing clients (in this case, web browsers) to send requests and retrieve fully functioning pages over the internet.

**Here’s how it worked**: a browser would send an IP address or domain name (DNS) request, and somewhere, through the magic of the internet, that request would land on a **web server**. The server’s job was to respond with the desired page, often generated dynamically on the fly. This was groundbreaking because, unlike client-side software that had to be installed on each machine, servers could now serve **up-to-date content to countless users** simultaneously, without being burdened by shipping or constant reinstallation.

The big deal about this was that **software no longer had to live on your computer**. Now, it could live on the web server, and anyone with a browser could just “request” what they needed from that server. This meant that the software could be updated or changed whenever the server wanted, and everyone using it would get the new version instantly—**no more CDs or downloads needed**. This was the birth of **dynamic web applications**, marking a shift in how we interact with software.

## Putting Theory to Practice
