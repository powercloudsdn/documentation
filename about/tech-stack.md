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

It's really obvious that Lambda was the way to go. These are some of the things we benefit from because of our choice to run serverless code:

* The billing for serverless infrastructure is calculated based on execution time * memory (RAM) - this means that we only pay while the code runs.
* Because of the billing model of Lambda, we are able to understand our unit economics down to a single MikroTik device.
* We don't need to worry about operating systems and patching them.
* There is no need to setup auto scaling groups.
* It's highly reliable. We aren't very concerned with Availability Zones, Lambda spans AZ's out of the box.
* It scales without scaling operations, what does that mean? When using something like EC2 instances that are part of an autoscaling group you need to define some sort of a metric that triggers a scaling operation. Example: If the CPU reaches 80% then scale up. Scaling up in this context means launching a new EC2 instance, and routing some of the load to it. It takes time to scale up, and what happens is this: You now have 2 EC2 instances, each sitting at 40% CPU load. You pay for the idle capacity and it hurts your pocket. Lambda on the other hand can execute at a concurrency of 1000 out of the box. Increasing this limit is a support request away, and if you ask them to set it to 20,000 and you run at a concurrency of 500, you still only pay for execution time and memory per invocation.

We make use of [Laravel Vapor](https://vapor.laravel.com/) to simplify the management of our ever growing number of microservices.

## Databases with scale
In the winter of 2021 we signed up our first enterprise user. While bootstrapping hundreds of MikroTik routers over a period 48 hours with virtually no sleep we noticed something alarming, we started maxing out our concurrent database connections. At this point we had a MVP (minimally viable product) that used CodeIgniter as a framework, and while considerations were made to database design it became really clear what we had was not going to scale. At this point we were using AWS Aurora Serverless. The issue (I should say one of the issues) was that this product was limited to 90 concurrent connections per ACU. To accommodate this immediate growth pain we switched to a provisioned Aurora database and turned op the maximum concurrent connections to 16,000. It got us out of a pickle for a few months.

In the six months that followed we tried virtually every cloud-native database on offer, our requirements were simple: It needs to handle load well, it needs to scale easily, it needs to cost reasonable money, and we want it to be fast. A clear winner emerged from the tests. [SingleStore](https://www.singlestore.com/).

Today our database does more that it ever has, and we're not worried about what will happen if we get 10,000 more devices over night. SingleStore must be hands down the fastest database available today. So much so that most of our dashboards can now execute queries in real time, we don't need service workers in the background that perform calculations.

How fast is it? Here's an example on a table with 50 million rows:

| Query | Result | Execution Time |
|-------|--------|----------------|
| SELECT COUNT(id) FROM site_checkins; | 50900951 | 530 ms |
| SELECT sum(cpu_count) FROM site_checkins; | 121662433 | 950 ms |

The results don't change much either for complex queries with joins. There's a great [article](https://usefathom.com/blog/worlds-fastest-analytics) that Jack Ellis wrote about databases if you are interested in what can be achieved with SingleStore.

