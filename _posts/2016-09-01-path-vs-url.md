---
layout: post
title: Path vs URL
---

Today's post will be shorter than usual.

One day I started wondering what's the difference between Rails some_route_path and some_route_url. Because at first, it looked like both returned the same address. After some research and help from Arkency, I learned that the difference is in fact quite significant.

Path helper provides a path that is relative to the root of a site. And should always be used if you provide a link inside your application. In that case, domain part is redundant.

URL, on the other hand, provides an absolute path, including protocol and server name. And should be used when you create a link for use outside your application. For example, when you send an email to confirm password absolute path is required.

{% include twitter_plug.html %}
