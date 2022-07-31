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

## Good Ecosystems
At the time of writing this (July 2022) I think that our technology journey in many respects have evolved at a better pace than our actual product. Saying that is a bit frustrating, but if I look back to where we were two years ago then all the long hours seem worth it. When we started out we crafted API's in Php using CodeIgniter as a framework. It seemed like a good choice at the time as we had experience with it. I also had hundreds of hours of experience with Bootstrap and jQuery. So we set off to build a jQuery / NodeJS powered frontend that uses JWT stateless authentication. We got it working despite it being an unimaginable amount of work (writing an SPA using jQuery is not a good call!). We had some 50 MikroTik routers on our system, albeit far from perfect, we got some critical customer validation of the idea. About six months in to what is now known as the jQuery saga I met this guy [Nico](https://nicovanzyl.com/), I will forever be thankful that he came onboard for 6 months and helped us put together a better stack for the frontend app. We switched the frontend to Nuxt / VueJS / Vuetify. Personally I've never been very focussed on frontend, but once I got the opportunity to work on a modern PWA framework, and when I compared it to our experience with jQuery I suddenly questioned all my life choices!

It was not long after this that we decided it's time to dump CodeIgniter 3 (again a choice that was made to get going as fast as possible) and move to a much more modern and maintainable framework. Today our tech stack looks as follow:

#### Frontend
* [VueJS](https://vuejs.org/) - The worlds most loved JavaScript framework.
* [NuxtJS](https://nuxtjs.org/) - This is a great framework to streamline VueJS development.
* [Vuetify](https://vuetifyjs.com/en/) - A [Material Design](https://material.io/) framework for VueJS.
* [TailwindCSS](https://tailwindcss.com/) - A really great utility-first CSS framework.

#### Backend
* [Laravel](https://laravel.com/) - This needs about as much introduction as Elvis Presley and operates with similar levels of swag.
* [RoadRunner](https://roadrunner.dev/) - A high-performance PHP application server, load-balancer, and process manager.
* [SingleStore]() - The fastest relational database in the known universe.
* [DynamoDB]() - NoSQL database for single-digit millisecond performance at any scale, we mainly use it as a cache layer.
* [FluentBit](https://fluentbit.io/) - This is a really great system that helps us collect logs and traffic flow data at a massive scale.

#### Infrastructure
* [Lambda](https://aws.amazon.com/lambda/) - Run serverless code at any scale.
* [SQS](https://aws.amazon.com/sqs/) - Fault tolerant and durable message queuing system.
* [Kinesis](https://aws.amazon.com/kinesis/) - Great to ingest and enrich data through a pipeline approach.
* [S3](https://aws.amazon.com/s3/) - Object storage with 99.999999999 (11 9's) data durability.
* [ELB](https://aws.amazon.com/elasticloadbalancing/) - Highly available application load balancer with a [WAF](https://aws.amazon.com/waf/).

Of course we've left out a few things as we operate a large ecosystem but our approach in selecting technologies is centered around architectures that are modern, cloud native and battle tested by the largest Internet businesses on the planet.

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

## A Fast Database
In the winter of 2021 we signed up our first enterprise user. While bootstrapping hundreds of MikroTik routers over a period 48 hours with virtually no sleep we noticed something alarming, we started maxing out our concurrent database connections. At this point we had a MVP (minimally viable product) that used CodeIgniter as a framework, and while considerations were made to database design it became really clear what we had was not going to scale. At this point we were using AWS Aurora Serverless. The issue (I should say one of the issues) was that this product was limited to 90 concurrent connections per ACU. To accommodate this immediate growth pain we switched to a provisioned Aurora database and turned op the maximum concurrent connections to 16,000. It got us out of a pickle for a few months.

In the six months that followed we tried virtually every cloud-native database on offer, our requirements were simple: It needs to handle load well, it needs to scale easily, it needs to cost reasonable money, and we want it to be fast. A clear winner emerged from the tests. [SingleStore](https://www.singlestore.com/).

Today our database does more that it ever has, and we're not worried about what will happen if we get 10,000 more devices over night. SingleStore must be hands down the fastest database available today. So much so that most of our dashboards can now execute queries in real time, we don't need service workers in the background that perform calculations.

How fast is it? Here's an example on a table with 50 million rows:

| Query | Result | Execution Time |
|-------|--------|----------------|
| SELECT COUNT(id) FROM site_checkins; | 50900951 | 530 ms |
| SELECT sum(cpu_count) FROM site_checkins; | 121662433 | 950 ms |

The results don't change much either for complex queries with joins. There's a great [article](https://usefathom.com/blog/worlds-fastest-analytics) that Jack Ellis wrote about databases if you are interested in what can be achieved with SingleStore.

## Coffee
It wouldn't have been right to leave this out. Coffee is an undeniable part of our tech stack.

---
Written by [Hannes Kruger](https://twitter.com/HannesKruger_)