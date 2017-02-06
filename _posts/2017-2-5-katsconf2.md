---
layout: post
title: Reactive Data Pipelines
published: true
---

This blog post was written for [Kats Conf](http://www.katsconf.com/) as a lightning talk and exposes how to build flexible data pipelines by leveraging the AWS stack and the Serverless framework.

## Context 

At Nitro, our main software Nitro PDF, generates billions of user events. In fact, everytime that a user view, edit, share or sign a PDF document, an event is generated. And you are going to ask me: "_kevllino! Why are we capturing some much data?! What is the point of all that, maybe to contribute to the hype of Big Data_?". And to that I will answer: "_Data alone is not relevant, we in fact, need to trasnform, manipulate it to be able to do something relevant with it!_"

Hence, the goal was to build sessions out of those events. This data engineering project is the basis to any future Machine Learning task on customer behaviour. Knowing how the user makes use of our software will drive predictive user behavior and will allow us to improve user experience. FIY a session in our context is defined as: 

- unique per machine 
- a session can be created by timeout given a specific threshold between 2 consecutive events. 
- a session can be created by an opening event

## Technology stack 

### Overview of the pipeline 

In this post, we conly cover the non-grey component of the following infrastructure: 

![Overview of the pipeline]({{site.baseurl}}https://github.com/kevllino/kevllino.github.io/blob/master/images/Screen%20Shot%202017-02-05%20at%2012.24.38.png?raw=true)

In order to sessionize events, there were three main steps to execute:

1. Ingest the events in the pipeline

2. Filter our invalid events and process valid events. In this context, the processing includes a technical cleaning (fixing some erroneous value fields for some builds) and enriching the event schema, e.g. by deriving a timestamp into date and creating new features from existing ones. 

3. Create the sessions by tagging events with a uuid

### AWS for implementation 

In order to implement the business logic of those steps, we used **AWS Lambda** in Python. To transfer the data across the pipeline, we used Kinesis components, **Kinesis Streams** are used to move data between Lambdas and **Kinesis Firehose** to deliver data to an S3 bucket through a _delivery stream_. Lambda is great in that, you don't have to manage the back-end og your applicaiton and you only pay for the compute time, i.e. every time your Lambda code gets triggered by external events such as events coming from a stream.(https://aws.amazon.com/lambda/faqs/). Besides using Lambda for processing events, it also serves as a bridge to connect different pipeline components. In fact to transfer data coming from a Kinesis Stream to an S3 bucket, one needs a lambda to connect them. 

![Generic Lambda]({{site.baseurl}}https://github.com/kevllino/kevllino.github.io/blob/master/images/Screen%20Shot%202017-02-05%20at%2013.01.53.png?raw=true)

The above defines a generic Lambda which will retrieve data triggered by a Kinesis Stream and post it into a Firehose which will deliver the data to the desired S3 bucket.

### Why Reactive?

To establish a reactive data pipeline, we took into account the  characteristics of a reactive system as defined by the [Reactive Manifesto](http://www.reactivemanifesto.org/), i.e.:

- **Responsive**: latency of message processing and ability to quickly responds to failures. 
- **Resilient**: provides high availability, failure recovery 
- **Elastic**: scale up capacity when the limit capacity of these queues is reached. 
- **Message Driven**: as processes are triggered by events and work in a non-blocking style. 

As a matter of fact, all of the above are ensured thanks to the AWS components: Kinesis pipes can dynamically scale from megabytes to terabytes per hour and are reliable as data is replicated accross 3 other AWS regions. Lambdas ensure that computation is done in parallel and is a message-driven component. Overall, the advantages of this architecture is that it is flexible, we can plug and unplug components quite easily and it is quick to get started with [compared to](https://blog.insightdatascience.com/ingestion-comparison-kafka-vs-kinesis-4c7f5193a7cd#.kq2nef9la) for example using Kafka and one of the famous streaming engine. It really depends on the amount of data one's system is ingesting.  

## Serverless Framework 

- convenience 
- structure 

## Conclusion
