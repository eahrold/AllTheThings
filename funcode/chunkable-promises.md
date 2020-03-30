---
layout: layout.njk
title: Chunkable Promises
date: 2020-03-26T17:15:32.261Z
---

I've worked on a number of integration scripts in the past, and a common problem I've run up on is the executable has a limited time the script is allowed to run, and the API has a fixed number of requests it can handle per second.

So on one hand, you can work to not slam the api by executing an HTTP requests sequentially, or on the other hand you do Promise.all(), and most certainly infuriate the API owner.

Well the following bits were developed to help smooth over to contradictory requirements, and help find a workable middle ground.

One of the things to keep in mind is that you actually pass in an array of functions to the `chunkRequest()`, in order for the resolution timing to be correct.

<script src="https://gist.github.com/eahrold/42944ea60f7afe98f988f0ed2a9d3a4e.js"></script>