---
layout: post
title: GCP Data Engineer certification
date: 2019-10-02 13:32:20 +0300
description: In this blogpost, I'd like to describe my specific experience taking the exam, from studying to the exam date. I hope to give as much tips and knowledge for future exam takers. 
img: gcp.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [GCP, Data Engineer]
---

In this blogpost, I'd like to describe my specific experience taking the exam, from studying to the exam date. I hope to give as much tips and knowledge for future exam takers.

## Background
Google’s recommended experience for taking the exam is 3 years in industry with 1 year using GCP. From my point of view, I’ve been working 3+ years as a data/software engineer. Regarding the use of GCP, I worked on migrating on-premises Spark jobs at Vodafone to the cloud for around a year. Hence, I had comprehensive working experience with a number of GCP services, such as Dataproc, Composer, Storage. I also had some experience with Big Query and Dataflow. This is really when you realise that when you master the technologies from a practical point of view, you don’t really need to study for them. To give you an idea, I was very familiar with things like GCP Airflow operators or building custom Dataproc images via a daisy workflow and sometimes, questions related to these elements are asked.

## Why should you get certified?
Most of us won’t have the opportunity to work on a daily basis with all of the services across the GCP spectrum. The certification offers a great way for engineers and architects to know all of the GCP solutions available to solve a specific problem. Therefore, for services that you haven’t used yet, you need to study, read and practice labs. For example, when designing row keys in BigTable, you should know the design patterns leading to hotspotting, i.e. those not to follow or how to avoid exploding indexes in Datastore.

## How did I get started?
I was already registered for several months in the Coursera class, I kept learning and forgetting concepts, until one day, I decided to register for the exam before even knowing the exam guide or taking any practice tests! I thought that putting some pressure on me will give me a boost to get it done and it worked. I gave myself 2 weeks to study and I registered on the 11th September 2019 for the exam on Saturday 28th. You’ll have to pay upfront 200$ + (tax 40$ in the UK).

## Study materials and study plan
I used 2 main sources to study for the exam:
- Cloudera - [Data Engineering on GCP](https://www.coursera.org/specializations/gcp-data-machine-learning) and [Preparing Cloud Professional Data Engineer Exam](https://www.coursera.org/learn/preparing-cloud-professional-data-engineer-exam)
- Linux Academy - [Google Cloud Certified Professional Data Engineer](https://linuxacademy.com/course/google-cloud-data-engineer/)

For more resources, please check [this list](https://github.com/ddneves/awesome-gcp-certifications#Google-Cloud---Professional-Data-Engineer). I divided the above study resources in pomodori sessions of 30 minutes each. I then used Trello to schedule them using days for the columns. I was able to achieve from 4 to 12 pomo per day. Basically, I was studying from 7pm to midnight after my day job and most of the weekend for 2 weekends in a row.

## Tips for you and things I should have done better
- Case studies (Flowlogistic and MJTelco) are not part of the exam anymore, as indicated by the exam guide, but some study resources still include them in their syllabus, hence it’s a bit confusing.
- A lot of questions deal with Big query, best choice of a storage option, best strategy to migrate from on-premises to cloud, design of IAM policies, …
- Know your Big Data/open source technologies, some questions won’t only relate to GCP services but existing technologies, e.g. Kafka.
- You should know Google Cloud essential services and some parts of the Architect specialisation are also useful, as you’re expected to know about compute instances, security, compliance, hybrid networking... They’re not part of the previously quoted resources, but questions will come on IAM, StackDriver, etc…
- Start practising with sample exams as early and regularly as you can, as they’ll give you an idea about the knowledge gaps you have and from there you can do extra reading/practice labs to clear out any confusion/doubt you have about a technology.

## D-DAY - Be Ready
I chose Saturday morning - 11a.m. to pass the exam. One advice, is go there early, plan to arrive at least 30 minutes in advance. Those examination centers are sometimes not easy to find. Get some earplugs and crack on. I did a first pass and answered all questions with 20 to be reviewed in about 50 minutes. Then, a second pass, removing most of the uncertain questions and adding new ones, resulting in 6 questions to be reviewed. I had to read many times, questions and answers to remove uncertainty. Don’t be tempted to skip reading some options because they look familiar! Then, for my third pass, I had 3 questions left, I removed the review check marks and submitted the test one minute before the end (for 200+ dollars, I’m using the 2 full hours :D). Here comes the most expected part, your heart starts beating very heavily, while you are trying to find the result, and here it comes, but you don’t see it at first because it’s written in tiny letters and it says PASS. Then Google will run a final compliance check before confirming your results.

## Conclusion
I hope that the above will help you in passing that certification. A mix of practice, thorough study and good organisation should get you this certification. In the end, I’m very happy that I took it, I learnt a lot and I would have felt more comfortable on my day to day job with all of this knowledge.

![Data Engineer Certification]({{site.baseurl}}/assets/img/gcp/data_engineer_certification.png)