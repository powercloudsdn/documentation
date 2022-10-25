---
title: Business Hour Policies
description: Define business hour policies that describe operational hours for branches and sites.
---

[Business hour policies](https://app.mikrocloud.com/policies/business-hours) can be used in firewall rules, content filtering policies, notification policies and SLA reports. They act as time constraints to toggle services or rules and to calculate downtime.

---

## Where can business hour policies be applied?

Currently you can apply business hour policies as time constraints to the following services:

* [Notification Policies](/policies/notification-policies)
* SLA Reports
* IP Block Lists

---

## Timezone selection

While setting up a business hour policy you can select a timezone that will be used to determine the UTC offset. Other services that use this policy will assume this timezone. Example: A notification policy or scheduled SLA report uses the timezone that is selected in its assigned business hour policy.

---

## All day policies

In order create a policy that is active for an entire day set the **Start Time** of the day to **00:00** and the **End Time** to **23:59** (*see Monday in the screenshot below*).

---

## Limits and Pricing

### Limits
Each MikroCloud account can create a maximum of 100 business hour policies.

### Pricing
Business hour policies are free of charge.

---

## Validation and Constraints

| Field | Remarks |
|-------|---------|
| Name | String, required, max 100 characters |
| Timezone | String, required, must be in [this list](https://www.php.net/manual/en/timezones.php) |
| Start Time | String, required, format HH:MM, must be before End Time |
| End Time | String, required, format HH:MM, must be after Start Time |

---

## Example Screenshot

![Business Hour Policy Screenshot](https://cdn.mikrocloud.com/documentation-assets/business-hour-policy.png)

---

*Last updated on 25 Oct 2022*
