---
title: 'Hi Bart,'
date: '2020-03-02T10:48:40.067Z'
excerpt: 'indeed, the two points you raised are valid.'
layout: post
---
Hi Bart,

indeed, the two points you raised are valid.

*   If you have a better idea on how to evaluate OOO messages in a stateless receiver (well, short of what state I keep in redis), I’d be happy to rerun the experiments with the new way of measuring OOO. What I do is simply to check if `previous_message+1 == current_message` . So I only need to keep the latest seen message sequence number in the memory store. I wanted to avoid doing complex “sequence analysis”. For human, it’s easy to see that the sequence is shuffled around because msg 2 is late by 2. But it felt harder doing this in a stateless environment. But maybe I was just missing the right idea.
*   I numbered the messages instead of stamping them with a datetime at the producer. That was a mistake I made and as you say, any business usually cares about “30 seconds late” not “3 messages late”. However, the concepts translate easily, just integer math is easier. So in a real world scenario, messages have a `creation_timestamp` value and one would keep a running average of how many seconds to wait to capture 99.9% (or whatever the SLA) of late messages within a window.

The Kafka vs. Pub/Sub discussion is indeed flaring up more and more. It is an interesting discussion to have and IMHO it is **important to understand both technologies to make educated decisions what to use**. I’ve seen people deploy batch processes in kafka simply because kafka was hip and I’ve seen people loose hundreds of hours on setting up SSL/authentication on a MSK cluster. Both those (auth/encryption) came out of the box in my experiment with simple service accounts and GCP taking care of all the boilerplate for us. So I would say tools like Pub/Sub reduce the degrees of freedom for you but in exchange, you give up ecosystem size and customizability. I would ask this question:

Is building a streaming architecture your core business? Or do you use the streaming tools simply as such: Tools to achieve a different goal. If the latter is the case, I’d go for a higher level service such as GCP. If your core business however revolves around handling streaming data, then Kafka is probably a good bet.

Thanks for the feedback and the discussion!
