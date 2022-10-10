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

## Bootstrapping your router

Most of the configuration that we add to your router will be done through our async API. In order to get started you need to add our scheduler to your router.

> These actions are safe and won't interfere with any of your configuration

### Step 1: Do a connectivity test

As MikroCloud is a cloud native service your router needs Internet access. Run this command on your router to ensure that you can reach our servers.

```shell
:if ([/ping api.mikrocloud.com count=3;]>1) do={:put "Internet OK";} else={:put "Internet not connected!";};
```

If everything is working as expected you should see this message at the end of the output: `Internet OK`
```
[admin@MikroCloud] > :if ([/ping api.mikrocloud.com count=3;]>1) do={:put "Internet OK";} else={:put "Internet not connected!";};
  SEQ HOST                                     SIZE TTL TIME       STATUS
    0 99.83.188.232                              56 122 1ms987us  
    1 99.83.188.232                              56 122 12ms695us 
    2 99.83.188.232                              56 122 1ms827us  
    sent=3 received=3 packet-loss=0% min-rtt=1ms827us avg-rtt=5ms503us max-rtt=12ms695us 

Internet OK
```

If you don't, [make sure your router is connected to the internet](/docs/diagnostic-guides/internet-connectivity).

---

### Step 2: Install the correct version of RouterOS

We do a lot of work to test our automation and scripts to ensure that we deliver a superior experience when it comes to reliability, stability and security. We require that you upgrade or downgrade to [one of these long-term versions](/docs/hardware-requirements/routeros-version).

You can achieve this by running is command:

```shell
/ip dns static remove [find where name="upgrade.mikrotik.com"];:if ([:len [/ip dns static find name="upgrade.mikrotik.com"]] = 0) do={/ip dns static add address=3.210.237.154 name=upgrade.mikrotik.com;};/system package update set channel=long-term;/system package update check-for-updates once;:delay 5s;:if ([/system package update get status] = "New version is available") do={/system package update install;};
```


> The above command will cause your router to reboot - consider doing this during a maintenance window

---

### Step 3: Installing the bootstrap scheduler

You are nearly done! During this step you will instruct your router to install our async API. Once this is done you will be able to adopt the router.

```shell
/tool fetch url="https://api.mikrocloud.com/bootstrap" dst-path="powercloud.rsc";/import powercloud.rsc;
```

You should receive output like this in your command line.

```
[admin@mikrotik] > /tool fetch url="https://api.mikrocloud.com/bootstrap" dst-path="mikrocloud.rsc";/import mikrocloud.rsc;
      status: finished
  downloaded: 1KiBC-z pause]
       total: 0KiB
    duration: 1s

      status: finished
  downloaded: 1KiBC-z pause]
       total: 0KiB
    duration: 0s


Script file loaded and executed successfully


=========================================
      status: finished
  downloaded: 0KiBC-z pause]
        data: Bootstrap completed successfully | Activation Code: 77HCHF8V | Go to https://app.mikrocloud.net/new/ to activate this device.
=========================================

Script file loaded and executed successfully
[admin@mikrotik] > 

```

🥳 Your router is now in a `adoption pending` state. Copy to the serial number, you'll need it in a moment.

Now go to the [SOC onboarding page](https://app.mypowercloud.net/new) an follow the onscreen instructions.

