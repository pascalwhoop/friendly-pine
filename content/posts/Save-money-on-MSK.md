---
title: Save money on MSK
date: '2020-06-28T09:45:30.672Z'
excerpt: >-
  Let’s be blunt here for a second: MSK is not a mature managed service. The
  author of that post may have changed his mind in the meantime…
thumb_img_path: images/Save-money-on-MSK/1*AFw_z6xm8FnyF0vrl-lDig.jpeg
layout: post
---
![](/images/Save-money-on-MSK/1*AFw_z6xm8FnyF0vrl-lDig.jpeg)

<figcaption><a href="https://unsplash.com/photos/CeW1CS6d08E" data-href="https://unsplash.com/photos/CeW1CS6d08E" class="markup--anchor markup--figure-anchor" rel="noopener" target="_blank">Franz Kafka statue in&nbsp;Prague</a></figcaption>

Let’s be blunt here for a second: [MSK is not a mature managed service](https://medium.com/@stephane.maarek/an-honest-review-of-aws-managed-apache-kafka-amazon-msk-94b1ff9459d8). The author of that post may have changed his mind in the meantime, but I have not. A simple kafka cluster on AWS runs you ~500 $. Do you want SSL with that? Well, better factor in another 500 $ for a private certificate authority because that’s the only supported authentication mechanism on MSK. If you enabled the detailed metrics, well, you are in for a surprise. We are currently facing almost 10k metrics on our 2 clusters (6 nodes total), running us another 3.000 $ on the cloudwatch bill. So here are 3 tips to save on MSK:

### 1\. Private CA

If your org has several AWS accounts, make sure you only register one PCA for the whole organization. We automated the creation of a CA with terraform, one per environment. That ran us almost 2.000 $ just to be able to create certificates. Probably not the best idea.

### 2\. Disable detailed metrics on AWS console

![](/images/Save-money-on-MSK/1*qLGyu1pxii-xXQ8kjLQrOA.png)

If you happened to enable enhanced topic-level logging (like we did) and then never disabled it, you probably saw your cloudwatch costs rise nicely. We had around 8000 metrics on DEV and 2000 on the ACC(eptance) environment. As a frame of reference, each topic creates between 5 and 15 metrics. So each topic cost us ~ 1.5$- 4.5$ a month. Thinking we pay for the cluster already, we didn’t think much of creating topics and leaving them on the cluster. It’s a shame you cannot enable the metrics only for specific topics on MSK.

    # fetch the metrics from the API and create a newline delimited json  
    aws cloudwatch list-metrics | jq '.Metrics[]' -c > /tmp/metrics.json
    # load into pandas and use script from [https://stackoverflow.com/a/57334325/1170940](https://stackoverflow.com/a/57334325/1170940) to flatten the json

![](/images/Save-money-on-MSK/1*M5qLe_RPMAuz0j45JZgpcA.png)

Alright, so disabling detailed metrics on DEV alone saved over 2.300 $. We will use Prometheus instead which we already have deployed on our K8S clusters.

If you can’t give up the metrics per topic, consider only adding metrics to the prod cluster. If you can, try switching to prometheus. Grafana works great with it and only needs a small instance (or pod).

### 3\. Don’t scale up unless you absolutely have to

![](/images/Save-money-on-MSK/1*gfg6nnSofuXBgjGi8B200w.jpeg)

The minimum setup for AWS MSK is 3 nodes, one per AZ. It’s a cluster technology, so naturally you want to scale up under load. Well, you’ll have to go for 6 nodes now, you can only scale up for all AZ at the same time, so that is 3,6,9,12,… nodes.

Here’s the fun part: Kafka is a storage so don’t think this works like lambda functions or K8S. Once you scale up, you won’t scale down again.

### Hindsight tip: Consider not using kafka

This may be the GCP favoring engineer talking in me but consider not using kafka. **Kafka links compute nodes and storage together**. Technologies like [BigTable](https://cloud.google.com/bigtable/docs/overview#architecture) or [BigQuery](https://cloud.google.com/files/BigQueryTechnicalWP.pdf) disconnect compute from storage. When doing so, you can scale compute independently from storage. MSK however is basically a set of nodes running on EC2 instances, storing kafka’s partition data on disk storage. If you want to go for an event-driven organization and maybe even integrate many databases through kafka connect, you may need a lot of storage. You may increase the volume storage per node but once you hit a CPU bottleneck, you have to choose: Wait and suffer the performance or upscale and pay for it from that day forth.

Kafka has an active community and offers lots of tools like [kafka connect](https://docs.confluent.io/current/connect/index.html) or [Debezium](https://debezium.io/) which allow you to generically stream events from many RDS or BI tools. But the bottom line to me is: Kafka is an enterprise technology. If you don’t waste a second thought on spending another 1.000$/month on infra, kafka is an option. But if these sort of numbers make you think if you’re doing the right thing for even a few seconds, kafka may be too big for you. Consider cloud native alternatives instead!
