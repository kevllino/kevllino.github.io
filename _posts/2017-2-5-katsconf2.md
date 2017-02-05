---
layout: post
title: Reactive Data Pipeline
published: true
---

This blog post was written for [Kats Conf](http://www.katsconf.com/) as a lightning talk and exposes how to build flexible data pipelines by leveraging the AWS stack and the Serverless framework.

## Context 

At Nitro, our main software Nitro PDF, generates billions of user events. In fact, everytime that a user view, edit, share or sign a PDF document, an event is generated. And you are going to ask me:  

> kevllino! Why are we capturing some nuch data?! What is the point of all that, maybe to contribute to the hype of Big Data?

And to that I will answer: 

> Data alone is not relevant, we in fact, need to trasnform, manipulate it to be able to do something relevant with it!

Hence, the goal was to build sessions out of those events. This data engineering project is the basis to any future Machine Learning task on customer behaviour. Knowing how the user makes use of our software will drive predictive user behavior and will allow us to improve user experience. FIY a session in our context is defined as: 

- unique per machine 
- a session can be created by timeout given a specific threshold between 2 consecutive events. 
- a session can be created by an opening event

## Technology stack 

### Overview of the pipeline 

In this post, we conly cover the non-grey component of the follwoing infrastructure: 

![Overview of the pipeline]({{site.baseurl}}https://github.com/kevllino/kevllino.github.io/blob/master/images/Screen%20Shot%202017-02-05%20at%2012.24.38.png?raw=true)

In order to sessionize events, there are straightforward main steps to follow: 

1.Ingest the events in the pipeline
2.Filter our invalid events and process valid events
3.Create the sessions by tagging events with a uuid

- AWS for implementation 
- why Reactive? 

## Serverless Framework 

- convenience 
- structure 

## Conclusion
