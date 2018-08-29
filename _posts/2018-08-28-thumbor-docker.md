---
title: 'Thumbor Docker image for web performance'
date: 2018-08-28
permalink: /posts/2018/08/thumbor-docker-image-for-web-performance
tags:
  - Development
  - Web Performance
  - Docker
---

In the last few days I was looking for a service, which should speed up my company's website performance. One thing I needed, was a image optimization tool, which reduces image size based on various factors (jpeg/png, size, quality, ...). While reading Google's Web Fundamentals, I stumbled upon [Thumbor](http://thumbor.org). Such a nice little service which can be used as a proxy (like varnish) to optimize images on the run.

After reading the documentation, I was quickly looking for a Docker image to play around. Sadly, there is no official one.
I found two others: APSL and MinimalCompact which did a great job providing a Docker image and fullfilling my needs. Indeed, both images just work fine. But there is more.


Docker should be small
======================

As for all in Docker, every image should one run _one process_ (not [running circus](https://github.com/MinimalCompact/thumbor/blob/master/thumbor/Dockerfile#L60), which can be configured to run multiple instances of a process). Again, it just works, but it's bloated.

After looking at the dependencies of both images, I made a decision: both images are good to go, but for my opinion, quiet to large (in size and dependencies).
So I decided to build my own.

Finally, my first public Docker image arrived: [mvhirsch/thumbor](https://hub.docker.com/r/mvhirsch/thumbor/)

It comes with the smallest footprint of dependencies I was able to manage (650 MiB instead 1.7 GiB). Still, there is room for improvement (like a planned `alpine` version of it). But I'm happy to announce a fully working, just scaleable Docker image. Feel free to contribute: https://github.com/mvhirsch/thumbor-docker

