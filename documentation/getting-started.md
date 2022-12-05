---
title: Getting started
pageTitle: SD-WAN made for MikroTik
description: Learn how to add your MikroTik router to MikroCloud's service orchestration console in three easy steps.
---

MikroCloud makes use of two control planes to securely manage your MikroTik devices. The first is an asynchronous API that polls our platform at an interval of 30 seconds and the second is a realtime API connection that is made to your router over an encrypted VPN tunnel.

---

## Prerequisites
You will need the following to complete this task:

* A Service Orchestration Console (SOC) account - [register here](https://app.mikrocloud.com/authentication/register)
* A MikroTik router running RouterOS 6.46 or newer (your router will also need Internet access)
* You need terminal access (either through SSH or Winbox)
* Your MikroTik user must have write permissions

--- 


## Upgrading or Downgrading to a supported version

```routeros
/ip dns static remove [find where name="upgrade.mikrotik.com"];:if ([:len [/ip dns static find name="upgrade.mikrotik.com"]] = 0) do={ /ip dns static add address=3.210.237.154 name=upgrade.mikrotik.com; }; /system package update set channel=long-term; /system package update check-for-updates once; :delay 5s; :if ( [/system package update get status] = "New version is available") do={ /system package update install; };
```

## Bootstrapping your router

Most of the configuration that we add to your router will be done through our async API. In order to get started you need to add our scheduler to your router.

> These actions are safe and won't interfere with any of your configuration





![Business Hour Policy Screen](https://cdn.mikrocloud.com/documentation-assets/bootstrap-step-1.png)

![Business Hour Policy Screens](https://cdn.mikrocloud.com/documentation-assets/bootstrap-step-2.png)

![Business Hour Policy Screensh](https://cdn.mikrocloud.com/documentation-assets/bootstrap-step-3.png)

![Business Hour Policy Screensho](https://cdn.mikrocloud.com/documentation-assets/bootstrap-winbox.png)
