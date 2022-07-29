# PowerCloud - SD-WAN made for MikroTik

#### Learn how to add your MikroTik router to PowerCloud's service orchestration console in three easy steps

PowerCloud makes use of two control planes to securely manage your MikroTik devices. The first is an asynchronous API that polls our platform at an interval of 30 seconds and the second is a realtime API connection that is made to your router over an encrypted VPN tunnel.

---

## Prerequisites
You will need the following to complete this task:

* A Service Orchestration Console (SOC) account - register here
* A MikroTik router running RouterOS 6.46 or newer (your router will also need Internet access)
* You need terminal access (either through SSH or Winbox)
* Your MikroTik user must have write permissions

--- 

## Bootstrapping your router

Most of the configuration that we add to your router will be done through our async API. In order to get started you need to add our scheduler to your router.

> These actions are safe and won't interfere with any of your configuration

### Step 1: Do a connectivity test

As PowerCloud is a cloud native service your router needs Internet access. Run this command on your router to ensure that you can reach our servers.

```shell
:if ([/ping powercloud-api.net count=3;]>1) do={:put "Internet OK";} else={:put "Internet not connected!";};
```

If everything is working as expected you should see this message at the end of the output: `Internet OK`
```
[admin@PowerCloud] > :if ([/ping powercloud-api.net count=3;]>1) do={:put "Internet OK";} else={:put "Internet not connected!";};
  SEQ HOST                                     SIZE TTL TIME       STATUS
    0 99.83.188.232                              56 122 1ms987us  
    1 99.83.188.232                              56 122 12ms695us 
    2 99.83.188.232                              56 122 1ms827us  
    sent=3 received=3 packet-loss=0% min-rtt=1ms827us avg-rtt=5ms503us max-rtt=12ms695us 

Internet OK
```

If you don't, make sure your router is connected to the internet.

---

### Step 2: Install the correct version of RouterOS

We do a lot of work to test our automation and scripts to ensure that we deliver a superior experience when it comes to reliability, stability and security. We require that you upgrade or downgrade to [one of these long-term versions](/docs/app/hardware-requirements/routeros-version).

You can achieve this by running is command:

```shell
/ip dns static remove [find where name="upgrade.mikrotik.com"];:if ([:len [/ip dns static find name="upgrade.mikrotik.com"]] = 0) do={/ip dns static add address=3.210.237.154 name=upgrade.mikrotik.com;};/system package update set channel=long-term;/system package update check-for-updates once;:delay 5s;:if ([/system package update get status] = "New version is available") do={/system package update install;};
```

> !Production Warning: The above command will cause your router to reboot - consider doing this during a maintenance window

---

### Step 3: Installing the bootstrap scheduler

You are nearly done! During this step you will instruct your router to install our async API. Once this is done you will be able to adopt the router.

```shell
/tool fetch url="https://powercloud-api.net/bootstrap" dst-path="powercloud.rsc";/import powercloud.rsc;
```

You should receive output like this in your command line.

```
[admin@PowerCloud] > /tool fetch url="https://powercloud-api.net/bootstrap" dst-path="powercloud.rsc";/import powercloud.rsc;
      status: finished
  downloaded: 2KiBC-z pause]
       total: 2KiB
    duration: 0s

Starting PowerCloud bootstrap loader installation
Please wait - this can take upto 60 seconds
Progress: 0%
Progress: 5%
Progress: 10%
Progress: 15%
Progress: 20%
Progress: 25%
Progress: 30%
Progress: 35%
Progress: 40%
Progress: 45%
Progress: 50%
Progress: 55%
Progress: 60%
Progress: 65%
Progress: 70%
Progress: 75%
Progress: 80%
Progress: 85%


Successfully installed PowerCloud bootstrap loader
Visit https://app.mypowercloud.net/sdn/new/ to adopt this router
Serial number: 71AF077937CF

Script file loaded and executed successfully
```

🥳 Your router is now in a `adoption pending` state. Copy to the serial number, you'll need it in a moment.

Now go to the SOC onboarding page an follow the onscreen instructions.

