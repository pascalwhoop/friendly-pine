---
title: Dig into API calls on AWS with cloudtrail and jq
date: '2019-08-30T12:22:26.439Z'
excerpt: >-
  While trying to deploy a dashboard for our kafka cluster, I ran into an issue
  where the dashboard won’t show data as expected. All the…
thumb_img_path: >-
  images/Dig-into-API-calls-on-AWS-with-cloudtrail-and-jq/1*8y77C-dCQ3XXj4LNMpgriw.jpeg
layout: post
---
![](/images/Dig-into-API-calls-on-AWS-with-cloudtrail-and-jq/1*8y77C-dCQ3XXj4LNMpgriw.jpeg)

While trying to deploy a dashboard for our kafka cluster, I ran into an issue where the dashboard won’t show data as expected. All the dashboard data seems OK but I keep getting a *There was an error while trying to get graph data.* The dashboard is made up of widgets looking as such:

    {  
          "type": "metric",  
          "x": 0,  
          "y": 18,  
          "width": 6,  
          "height": 6,  
          "properties": {  
            "metrics": [  
              [ "AWS/Kafka", "MessagesInPerSec", "Cluster Name", "abc-kafka-dev", "Broker ID", "3", "Topic", "flights-v0", { "period": 60 } ],  
              [ "...", "1", ".", ".", { "period": 60 } ]  
            ],  
            "view": "timeSeries",  
            "stacked": true,  
            "region": "eu-central-1",  
            "title": "flights"  
          }  
        },

![](/images/Dig-into-API-calls-on-AWS-with-cloudtrail-and-jq/1*CJ5os6XCqvnj-XTauYToRg.jpeg)

To debug this, I decided to make use of our cloudtrail logs.

1.  Download the logs:

     aws s3 sync "s3://abc-dev-account-qiovox/cloudtrail/AWSLogs/123222934032/CloudTrail/eu-central-1/2019/08/30" ./08-30 --profile abc-dev

#### 2\. Interpret the logs

Some examples to check what’s happening

1.  Show all the event sources

    zcat ./*.json.gz | jq '.Records[] | .eventSource' | sort -u                              
    "acm.amazonaws.com"  
    "autoscaling.amazonaws.com"  
    "cloudtrail.amazonaws.com"  
    "config.amazonaws.com"  
    "dynamodb.amazonaws.com"  
    "ec2.amazonaws.com"  
    "ecr.amazonaws.com"  
    "eks.amazonaws.com"  
    "elasticloadbalancing.amazonaws.com"  
    "kafka.amazonaws.com"  
    "lambda.amazonaws.com"  
    "logs.amazonaws.com"  
    "monitoring.amazonaws.com"  
    "rds.amazonaws.com"  
    "redshift.amazonaws.com"  
    "resource-groups.amazonaws.com"  
    "s3.amazonaws.com"  
    "sns.amazonaws.com"  
    "sts.amazonaws.com"  
      
    

OK. Our Dashboard accesses the `monitoring.amazonaws.com` resources so let’s check with

2\. show errors on monitoring

    zcat ./413522934032_CloudTrail_eu-central-1_* | jq '.Records[] | select(.eventSource == "monitoring.amazonaws.com") | select(.errorCode != null)' -C | less -r
    ...  
    {  
      "eventVersion": "1.05",  
      "userIdentity": {  
        ...  
      },  
      "eventTime": "2019-08-30T10:10:07Z",  
      "eventSource": "monitoring.amazonaws.com",  
      "eventName": "DescribeAlarms",  
      "awsRegion": "eu-central-1",  
      "sourceIPAddress": "...",  
      "userAgent": "AWS CloudWatch Console",  
      "errorCode": "ValidationException",  
      "errorMessage": "1 validation error detected: Value 'INVALID_FOR_SUMMARY' at 'stateValue' failed to satisfy constraint: Member must satisfy enum value set: [INSUFFICIENT_DATA, ALARM, OK]",  
      "requestParameters": {  
        "stateValue": "INVALID_FOR_SUMMARY"  
      },  
      "responseElements": null,  
      "requestID": "97d63d57-e819-4010-a7a1-5fede146d102",  
      "eventID": "549091b7-45cf-4209-b593-790b97197b22",  
      "eventType": "AwsApiCall",  
      "recipientAccountId": "..."  
    }  
    ...

Alright. Looks like that isn’t an issue with the dashboard definition. What types of errors are we getting anyways?

    zcat ./413522934032_CloudTrail_eu-central-1_* | jq '.Records[] | select(.eventSource == "monitoring.amazonaws.com") | select(.errorCode != null) | .eventName' | sort -u                                                                        
    "DescribeAlarms"  
    "GetDashboard"

Ah, maybe it’s the `GetDashboard` that I need to look at ?

     zcat ./413522934032_CloudTrail_eu-central-1_* | jq '.Records[] | select(.eventSource == "monitoring.amazonaws.com") | select(.errorCode != null) | select(.eventName == "GetDashboard")'                                                      
    {  
      "eventVersion": "1.05",  
      "userIdentity": {...},  
      "eventTime": "2019-08-30T10:09:07Z",  
      "eventSource": "monitoring.amazonaws.com",  
      "eventName": "GetDashboard",  
      "awsRegion": "eu-central-1",  
      "sourceIPAddress": "141.135.118.103",  
      "userAgent": "AWS CloudWatch Console",  
      "errorCode": "DashboardNotFoundError",  
      "errorMessage": "Dashboard CloudWatch-Default does not exist",  
      "requestParameters": null,  
      "responseElements": null,  
      "requestID": "859babd9-89ae-4a95-b874-cecb13f0faa9",  
      "eventID": "20992809-da32-422f-8870-8ff2162d436a",  
      "eventType": "AwsApiCall",  
      "recipientAccountId": "..."  
    }

Nope. OK that’s odd. No error on the cloudtrail logs warrants anything wrong with the permissions.

* * *

Not very rewarding, but ultimately, I couldn’t open the dashboard with firefox. For some reason, it was a CORS error which I found only when opening the dashboard in Chrome. Still, I learned how to quickly scan CloudTrail for errors from a variety of services. Surely helpful!

![](/images/Dig-into-API-calls-on-AWS-with-cloudtrail-and-jq/1*j2yrx1v1uxCQcQUhSxD8pw.jpeg)

Some last TL;DR (generated with [tldr.sh](https://tldr.sh/))

![](/images/Dig-into-API-calls-on-AWS-with-cloudtrail-and-jq/1*rCatUtkTv6z8PeExRzCQqw.jpeg)
