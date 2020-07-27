---
title: This is not a huge improvement though.
date: '2020-05-22T14:35:04.572Z'
excerpt: >-
  Requiring a 500Euro/month private CA to get kafka security makes MSK a
  1000e/month service out of the box. No business features created…
layout: post
---
This is not a huge improvement though. Talking from experience, we killed our UAT environment at a client (aviation) to save some costs and just brought it back up. Bringing all networking, K8S, all applications on K8S etc back up: 4h with terraform. Finding all the bugs in the kafka certificate hell across all applications: 3w and counting.

Requiring a 500Euro/month private CA to get kafka security makes MSK a 1000e/month service out of the box. No business features created yet. This is not the purpose of cloud. This is a massive price bump for what should be free: SSL
