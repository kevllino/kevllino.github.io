---
layout: post
title: Reactive Data Pipelines
published: true
---

This blog post was written to prepare my lightning talk for [Kats Conf](http://www.katsconf.com/) and exposes how to build flexible data pipelines by leveraging the AWS stack and the Serverless framework.

## Context 

At Nitro, our main software Nitro PDF, generates billions of user events. In fact, everytime that a user view, edit, share or sign a PDF document, an event is generated. An event contains in JSON format a unique identifier, a timestamp, an action name (start, edit, view, convert, ...), a machine id and other important fields. And you are going to ask me: "_kevllino! Why are we capturing some much data?! What is the point of all that, maybe to contribute to the hype of Big Data_?". And to that I will answer: "_Data alone is not relevant, we indeed, need to trasnform, manipulate it to be able to do something relevant with it!_"

Hence, the goal was to build sessions out of those events. This data engineering project is the basis to any future Machine Learning tasks on customer behaviour. Knowing how users utilize our software will drive predictive user behavior and will allow us to improve user experience. Moreover, an important information to know is that we define a session as: 

- unique per machine 
- a session can be created by timeout given a specific threshold between 2 consecutive events. 
- a session can be created by an opening event

## Technology stack 

### Overview of the pipeline 

In this post, we conly cover the non-grey component of the following infrastructure: 

![Overview of the pipeline]({{site.baseurl}}https://github.com/kevllino/kevllino.github.io/blob/master/images/Screen%20Shot%202017-02-05%20at%2012.24.38.png?raw=true)

In order to sessionize events, there were three main steps to execute:

1. **Ingesting** the events in the pipeline by filtering out invalid events.

2. **Processing** valid events. In this context, the processing included a technical cleaning (fixing some erroneous value fields for some builds) and enriching the event schema, e.g. by deriving a timestamp into a date and creating new features from existing ones. 

3. **Sessionizing**: creating the sessions by tagging events with a uuid.

### Leveraging AWS components 

In order to implement the business logic of those steps, we used **AWS Lambda** in Python 2.7 (as AWS Lambda does not support 3+ versions). To transfer the data across the pipeline, we used Kinesis components, **Kinesis Streams** are used to move data between Lambdas and **Kinesis Firehose** to deliver data to S3 buckets through a _delivery stream_. Lambda is [great](https://aws.amazon.com/lambda/faqs/) in that, you don't have to manage the back-end of your application and you only pay for the compute time, i.e. every time your Lambda code gets triggered by external events such as events coming from a stream. Besides using Lambda for processing events, it also serves as a bridge to connect different pipeline components. In fact to transfer data coming from a Kinesis Stream to an S3 bucket, one needs a lambda to connect them. 

The following schema defines a generic Lambda which will retrieve data triggered by a Kinesis Stream and post it into a Firehose pipe which will deliver the data to the desired S3 bucket.

![Generic Lambda]({{site.baseurl}}https://github.com/kevllino/kevllino.github.io/blob/master/images/Screen%20Shot%202017-02-05%20at%2013.01.53.png?raw=true)


### Why Reactive?

To establish a reactive data pipeline, we took into account the  characteristics of a reactive system as defined by the [Reactive Manifesto](http://www.reactivemanifesto.org/), i.e.:

- **Responsive**: latency of message processing and ability to quickly responds to failures. 
- **Resilient**: provides high availability, failure recovery. 
- **Elastic**: scale up capacity when the limit capacity of these queues is reached. 
- **Message Driven**: as processes are triggered by events and work in a non-blocking style. 

As a matter of fact, all of the above are ensured thanks to the AWS components; for example,  Kinesis pipes can dynamically scale from megabytes to terabytes per hour and are reliable as data is replicated accross 3 other AWS regions. Lambdas ensure that computation is done in parallel and is a message-driven componen. Overall, the advantages of this architecture is that it is flexible, we can plug and unplug components quite easily and it is quick to get started with, [in comparison](https://blog.insightdatascience.com/ingestion-comparison-kafka-vs-kinesis-4c7f5193a7cd#.kq2nef9la) to using Kafka and one of the famous streaming engine. 

## Serverless Framework 

Now for those who are new to the Lambda service, if you start using it, you'll end up having to code in the AWS console like this: 

![Lambda Console]({{site.baseurl}}https://github.com/kevllino/kevllino.github.io/blob/master/images/Screen%20Shot%202017-02-06%20at%2021.24.34.png?raw=true)

"Aouch!" Yes there's no code completion or error checking, and imagine yourself having to implement and maintain many Lambda functions. This is just not viable! Thus, [Serverless framework](https://serverless.com/), an open-source web framework written in Node.js was developped to support the development and deployment of applications using AWS lambda. This framework helps managing the lifecycle of your serverless architecture (build, deploy, update, delete) by safely deploying functions, events and their required resources together via provider resource managers (e.g., AWS CloudFormation). 
Here are some useful commands to get started writing your own serverless services: 

```
# serverless installation 
npm install serverless -g

# service creation 
sls create -t aws-python -p ./data-service
```
![Serverless Service Creation]({{site.baseurl}}https://github.com/kevllino/kevllino.github.io/blob/master/images/Screen%20Shot%202017-02-11%20at%2014.49.04.png?raw=true)

Below are other command to allow you to deploy, update and test your functions:

```
# deploy the service 
sls deploy

# deploy one function 
sls deploy function -f function1 

# invoke a function against sample data 
sls invoke -f function1 -p event.json

#  invoke a function triggered by an event from a Kinesis Stream for example 
sls invoke -f hello -t Event

# get the CloudWatch logs 
sls logs -f hello
```

Now the question that everyone will ask is: "_Why is it called serverless? Does it mean that there's no servers at all?!_". No there're still some servers somewhere, serverless refers to serverless computing, which means that you don't need to provision and maintain servers, as AWS or IBM OpenWhisk do it for you. You only need to focus on your code.


## Conclusion

To put it in a nutschell, if you want to build event-driven, dynamic applications like data pipelines, then using Lambda and Kinesis is convenient as their instrumentation is flexible and easy to start with. The Serverless framework will allow you to develop your applications in a smoother wasy by using your favorite IDE and pushing your code to AWS, in a similar fashion as you do with Git. If you want a concrete example of how to structure such a project, check out [this](https://github.com/kevllino/reactive-data-pipeline).