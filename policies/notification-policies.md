---
title: Notification Policies
description: Alert automation for sites when fault conditions are created and updated.
---

[Notification policies](https://app.mikrocloud.com/policies/notifiable) allow you to receive automated WhatsApp notifications when routers have fault conditions. You can attach business hour policies to suppress unwanted notifications during certain hours. Notification policies use the timezone specified in the associated business hour policy.

---

## What are notification policies used for?

At the heart of our orchestration system is an asynchronous API services that securely connects your MikroTik routers to our system. Your MikroTik router is configured to send a JSON payload to our service bus at an interval of 30 seconds. This event is referred to as a **heartbeat**. We monitor the frequency and jitter of heartbeats received from each of your MikroTik devices. In the event that 10 consecutive heartbeats are missed it means the site has been unreachable for 5 minutes (30 seconds X 10 heartbeats). If we stop receiving heartbeats from a router, at the 5 minute mark a fault condition is registered and asynchronous API access is marked as offline. A notification policy will notify selected users of such fault conditions as well as the recovery of those fault conditions.

---

## Users

Notification policies make use of the global user object store. At the moment we only support notification delivery via WhatsApp in the form of a text message. We have some plans to include support for webhooks, Slack, MS Teams and telegram, but this is likely to only arrive somewhere in 2023. Any user type can be used in a notification policy as long as the following requirements are met:

- The user has a mobile number and an active WhatsApp account
- The user has verified their number

---

## Business Hour Policy

Business hour policies serve two purposes in the context of notification policies:

1. It describes the timezone in which the sites associated with the notification policy operate in.
2. If the policy is configured to suppress notifications outside of business hours, then the selected business hour policy is used to determine when to mute notifications.

---

## Mute Outside Business Hours

In some cases you might not care if branches or routers go offline outside of your operational hours. By enabling this setting we will suppress notifications during hours when the selected business hour policy is inactive.

---

## Mute During Maintenance Windows

Enable this if you want to suppress notifications during planned downtime events.

---

## Limits and Pricing

### Limits

Each MikroCloud account can create a maximum of 100 notification polices.
Each notification policy can have a maximum of 50 users and there is an upper limit of 300 sites per policy

### Pricing

Notification policies are free of charge, but there are charges for message delivery that exceed free tier thresholds.

---

## Validation and Constraints

| Field                           | Remarks                                                            |
| ------------------------------- | ------------------------------------------------------------------ |
| Name                            | String, required, max 100 characters                               |
| Users                           | Array, required, max 50 objects                                    |
| Business Hour Policy            | UUID, required, refers to [this](/policies/business-hour-policies) |
| Mute Outside of Business Hours  | Boolean, required, default false                                   |
| Sites                           | Array, required, max 300 objects                                   |
| Mute During Maintenance Windows | Boolean, required, default false                                   |
| Disable Policy                  | Boolean, required, default false                                   |

---

## Example Screenshot

![Notification Policy Screenshot](https://cdn.mikrocloud.com/documentation-assets/notification-policy.png)

---

_Last updated on 26 Oct 2022_
