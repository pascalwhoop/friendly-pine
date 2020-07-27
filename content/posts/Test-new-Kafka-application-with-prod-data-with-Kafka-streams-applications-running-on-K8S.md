---
title: >-
  Test new Kafka application with prod data with Kafka streams applications
  running on K8S
date: '2019-08-27T07:34:12.050Z'
subtitle: "Or how to pipe messages from one kafka cluster to another cluster through kubectl and your local\_machine"
excerpt: >-
  Or how to pipe messages from one kafka cluster to another cluster through
  kubectl and your local machine
thumb_img_path: >-
  images/Test-new-Kafka-application-with-prod-data-with-Kafka-streams-applications-running-on-K8S/1*7ceKO4NvKLgYlbQqJfgRQg.jpeg
layout: post
---
#### Context

Here is a typical situation in enterprises that recently took on streaming and kubernetes in their Big Data infrastructure: **Two environments for production and testing,** each with their own **K8S cluster** and their own **MSK (kafka)** cluster **and a firewall in between.** In our case, it’s all on AWS and you cannot reach the kafka clusters directly. Now the testing environment has a new version of an application installed but the prod data doesn’t get routed to that environment. **How do you test the application with prod data?**

We have 3 raw incoming streams from an on-premise environment that feeds our production kafka. I want to feed 10k messages of each stream into the test environment to see if everything works out as expected. We have a dashboard that shows the message streams of each of the three streams in prod and if everything looks clean, raw messages on the left lead to a lot of traffic in our egress. I’m aiming for the same case with our testing environment

![](/images/Test-new-Kafka-application-with-prod-data-with-Kafka-streams-applications-running-on-K8S/1*7ceKO4NvKLgYlbQqJfgRQg.jpeg)

<figcaption>plenty of traffic in&nbsp;prod</figcaption>

![](/images/Test-new-Kafka-application-with-prod-data-with-Kafka-streams-applications-running-on-K8S/1*0ErEpqQp8yqQaTJi3AzCFA.jpeg)

<figcaption>nothing yet in&nbsp;testing</figcaption>

Now some shell magic with `kubectl` `kafka-console-consumer` and `kafka-console-producer` . The core idea: `tail -n 10000 kafka1:/topic{1,2,3} kafka2:/topic{1,2,3}`

![](/images/Test-new-Kafka-application-with-prod-data-with-Kafka-streams-applications-running-on-K8S/1*Hf-_AZ3OyYAj94cJhxQOrw.png)

#### Pull data from kafka prod

For this, I deploy a small helper container

    kubectl run -i --rm --tty kafka-helper --image="confluentinc/cp-kafka" --command "/bin/bash"

Now inside of this container, I execute some commands to collect messages.

    mkdir messages  
    cd messages  
    SERVER="<your-kafka-bootstrap-urls>"  
    kafka-console-consumer --bootstrap-server "$SERVER" --topic people-raw-v1 --from-beginning | head -n 10000 > people-raw-v1  
    kafka-console-consumer --bootstrap-server "$SERVER" --topic cars-raw-v1 --from-beginning | head -n 10000 > cars-raw-v1  
    kafka-console-consumer --bootstrap-server "$SERVER" --topic clicks-raw-v1 --from-beginning | head -n 10000 > clicks-raw-v1  
      
    

This creates three files in `/messages` which we can now grab. **In a new terminal window** run the copy command

`kubectl cp kafka-helper-977c4988d-p78gb:/messages ./messages`

You will have to change the container name here. Check it with `kubectl get pods` .

#### Push data to kafka test

Now, connect to the different cluster. In my case, I have to update my kubeconfig for this so I run

    aws eks update-kubeconfig --name clientx-datalake-eks-cluster-dev --region eu-central-1 --profile clientX-dev

Now my `kubectl` is linked to the new cluster. Same procedure as above, I

1.  create a new kafka-helper with an interactive shell
2.  cp the `./messages` folder into that helper (inverting the cp command parameters)
3.  pipe the messages into `kafka-console-producer`

    cd /messages  
    SERVER="<your-kafka-bootstrap-urls>"  
    cat people-raw-1  | kafka-console-producer --broker-list "$SERVER" --topic people-raw-v1  
    cat cars-raw-1    | kafka-console-producer --broker-list "$SERVER" --topic cars-raw-v1  
    cat clicks-raw-v1 | kafka-console-producer --broker-list "$SERVER" --topic clicks-raw-v1

Et voilà:

![](/images/Test-new-Kafka-application-with-prod-data-with-Kafka-streams-applications-running-on-K8S/1*MevCcBh1aqU4v5DMt-L9bw.jpeg)

All messages went through our flow and the results look good in our egress.

#### Homework

Run all of this in a single script from your machine. For this, you can use something like

    kubectl run tail -i --rm --restart=Never --image=confluentinc/cp-kafka:5.1.2 -- <COMMAND_ON_POD> > file
