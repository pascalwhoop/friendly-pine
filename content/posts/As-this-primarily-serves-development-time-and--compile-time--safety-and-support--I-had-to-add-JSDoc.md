---
title: >-
  As this primarily serves development time and “compile time” safety and
  support, I had to add JSDoc…
date: '2018-12-17T08:31:59.504Z'
layout: post
---
As this primarily serves development time and “compile time” safety and support, I had to add JSDoc to each `with` method to get my WebStorm to autocomplete my builder calls after the first `withX` call. Apparently the IDE didn’t pick up the returned type inside of the method body and generating JSDoc with a `@returns {Builder}` assured that this is the case.
