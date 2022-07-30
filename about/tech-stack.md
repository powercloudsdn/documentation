---
title: Technology Stack
description: What goes in to building the worlds largest MikroTik API?
---

What goes in to building the worlds largest MikroTik API? {% .lead %}

We are constantly questioning our choices, iterating our code and testing new technologies with the aim of bettering our product. All our API's are built from the ground up around three pillar requirements:

1. Reliability
2. Security
3. Scalability

And here's how we achieve it:

---


## Serverless Computing
From February 2022 we no longer deploy code to servers, virtual machines or docker containers. All our API traffic is routed to AWS Lambda functions that require no scaling operations during peak traffic situations. 

AWS Lambda was a real game changer for PowerCloud, we frequently faced complex challenges in the past when it came to building things that can scale dynamically, have reliability and doesn't weigh us down with server management. The last point is really where we've seen a massive improvement. Managing load balancers, servers, and planning auto scale systems is a discipline on its own. It came down to a really simple choice, do we;

1. Do everything including the management of servers and operating systems? Or
2. Focus on writing good code and put our energy into solving things for MikroTik networks?

It's really obvious that Lambda was the way to go, and it really was that binary, not a lot of gray area. These are some of the things we benefit from because of our choice to run serverless code:

* The billing is calculated based on execution time * memory (RAM) - this means that we only pay while the code runs.
* Because of the billing model of Lambda, we are able to understand our unit economics down to a single MikroTik device.
* We don't need to worry about operating systems and patching them.
* There is no need to setup auto scaling groups.
* It's highly reliable. We aren't very concerned with Availability Zones, Lambda spans AZ's out of the box.
* It scales without scaling operations, what does that mean? When using something like EC2 instances that are part of a autoscaling group you need to define some sort of a metric that triggers a scaling operation. Example: If the CPU reaches 80% then scale up. Scaling up in this context means we launch a new EC2 instance, and start routing some of the load to it. This sucks. It takes time to scale up, and what happens is this: You now have 2 EC2 instances, each sitting at 40% CPU load. You pay for the idle capacity and it hurts your pocket. Lambda on the other hand can execute at a concurrency of 1000 out of the box. Increasing this limit is a support request away, and if you ask them to set it to 20,000 and you run at a concurrency of 500, you still only pay for execution time and memory per invocation.
