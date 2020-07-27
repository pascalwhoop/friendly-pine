---
title: 'Google Pub/Sub: Putting a number on the lack of order'
date: '2020-02-28T16:01:27.450Z'
subtitle: Evaluating Pub/Sub’s lack of order. Making good decisions about watermarks
excerpt: Evaluating Pub/Sub’s lack of order. Making good decisions about watermarks
thumb_img_path: >-
  images/Google-Pub-Sub--Putting-a-number-on-the-lack-of-order/1*cSz3k_8liL_Lg34nRNliDw.jpeg
layout: post
---
![](/images/Google-Pub-Sub--Putting-a-number-on-the-lack-of-order/1*cSz3k_8liL_Lg34nRNliDw.jpeg)

The topic of message ordering often becomes a discussion point for us when talking about GCP Pub/Sub or alternative products and systems. When looking at the architecture, it becomes clear, Pub/Sub can have ordering issues, since it contains N publishing forwarders and M subscribing forwarders, depending on the ingress and egress load.

![](/images/Google-Pub-Sub--Putting-a-number-on-the-lack-of-order/1*tdvoRLSKpguNQvahU7uXaQ.png)

But how out of order will they be? Often, messages may come in at a rate as low as 1 msg per second. Or maybe at a rate of 10 msg/sec? Or 50?

**In short, how does *out of orderness* increase with increasing velocity of incoming messages ceteris paribus?**

Of course this depends on many factors (such as message size and sender count / geographical location). But assuming we have 1 sender and 1 receiver, when will PubSub start showing out of order messages?

To me, this sounds like a perfect question to run a little experiment.

#### Experimental setup

1.  Every hour, a cloud scheduler event triggers a cloud function
2.  the function sends messages to pubsub for 10 minutes sequentially with a pause interval of `[1sec;1/2th sec; 1/10th sec;... 1/100 sec]` . Each message contains a continuous counter and a total of about up to 9000 msgs is sent per run. Each message also contains the current “stage” `[0,1,2,3,4,5]`
3.  Pub Sub push forwards these messages to an AppEngine deployment
4.  AppEngine keeps track of the messages coming in and compares the previous message counter and the current messages’ counter. **If it isn’t continuous, it logs an out-of-order message. If the message isn’t in “it’s right place**” (meaning message with counter 100 is the 100th message received), **an out-of-index is logged**. Finally, **for each message, the offset compared to the latest message (it’s lateness) is logged**
5.  Because AppEngine is stateless, all **results are stored in a basic memorystore instance**

![](/images/Google-Pub-Sub--Putting-a-number-on-the-lack-of-order/1*C2rboO_fJCPi52Vs9B0fPw.png)

**All code is available on** [**Github**](https://github.com/pascalwhoop/pubsub-order-test)**. The infra is defined through Terraform, the generator and reader are both written in python and I also added a Notebook**

#### Results

Each run logs a number of metrics such as:

*   **total**: total number of messages received
*   **last:** last message counter received
*   **hist**: a single string that contains a `./O/I` per message
*   **ooo**: The number of out of order messages
*   **ooi**: the number of out of index messages
*   **max**: the highest received counter
*   **offs**: an array of int values that list the offset of each message

To see how these values are built or used, I really suggest looking at the [code](https://github.com/pascalwhoop/pubsub-order-test/blob/master/reader/main.py#L76).

Looking at all runs, the following distribution of out of order (OOO) and out of index (OOI) messages can be observed

![](/images/Google-Pub-Sub--Putting-a-number-on-the-lack-of-order/1*xTXIGmEtg_ANBmsJGGt_Ww.png)

The way to interpret this is that there seems to be an unexpected amount of OOI messages starting at 0.5 msg/sec already and then an expected rise in OOO messages with increasing rate of messages. Messages that are OOO cannot be OOI, which is why the chart is stacked. **A single message being dropped and redelivered after a wait timeout makes all messages in between out of index**. Hence, the out of index values are very sensible to a single message missing or being late. Out of order on the other hand is “forgiving”. As long as the previous message’s counter has `-1` of the current message, it is considered in order.

What is more important to businesses is how many messages arrive late. Given my example, I am looking at a simple counter, hence a message is “5 late” if 5 other messages have “passed” it. So message `5` arriving in spot `10` is “late 5 messages”. Looking at the worst case of 100msg per second, messages arrive as such:

<script src="https://gist.github.com/pascalwhoop/043a967e6f4520c9719a91596a023c0e.js"></script>

When plotting these values on a logarithmic scale, the following plot shows the distribution. **Note this is a log scale on the Y axis so each marking on the left is an order of magnitude.**

![](/images/Google-Pub-Sub--Putting-a-number-on-the-lack-of-order/1*hdV59UHXaOxQYxwkgASKLQ.png)

* * *

### Synthesis of things learned

Order becomes increasingly hard to trust, the faster the messages come in. If the consumer is too slow in handling messages, timeouts and retries make it even harder.

10 messages per second are still *mostly* in order. 50+ messages per sec on the other hand show plenty of out of ordering events. But most can be caught by waiting 2–3 messages.

100 msg/sec have ~10% messages late by 1 and 1% messages late by 2. There is a long tail of messages late by n.

One has to evaluate the individual use-case to decide how long to wait for missing messages before going ahead with the calculation with missing values. It really depends on the use-case but it’s nice to see that the extend of the lack of order can be evaluated.

PubSub does some awesome auto-scaling magic behind the scenes. The fact that I could be served by 100s of forwarders without knowing or worrying about it is pretty nice.

Terraform for GCP allows me to build what is essentially enterprise grade cloud native infrastructure in a weekend. I’ve used it plenty on AWS infrastructure but the components on GCP just feel “higher level” than AWS. But 400 errors from the Google API in Terraform are sometimes hard to debug without `TF_LOG=DEBUG` set and looking at the HTTP responses / Docs

GCP API enabling as a first step is annoying and could(should) be made transparent. Ideally, an organization would take care of this at a project start level and this would be handled centrally. The GCP manager would enable the new project, assign the owner and enable all needed APIs for the team.

Redis is a pretty cool technology, given that you cannot have state. However, the performance is still network I/O bound. Things like the atomic `getset` operator remind me of the computer science 1 & 2 lectures where [one tries to really optimize for performance](https://github.com/pascalwhoop/pubsub-order-test/blob/master/reader/main.py#L107).
