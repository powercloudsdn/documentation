---
title: Bootstrap your MikroTik
description: Bootstrapping your MikroTik router is the first step towards SDN enablement.
---

## Required Steps

1. Create a Service Orchestration Portal account. [Register Here](https://app.mikrocloud.com/authentication/register)
2. Perform an [Internet reachability test](#internet-reachability-test)
3. Install a [supported version](/documentation/router-onboarding/supported-routeros-versions) of RouterOS.
4. Run the [bootstrap script](#bootstrap-script).
5. Adopt your router in the Service Orchestration Portal.

---
## Internet Reachability Test

Copy this script and paste it into the terminal of your MikroTik router.
::rsc
```
:if ([/ping api.mikrocloud.com count=3;] > 1) do={
	:put "Internet OK";
} else={
	:put "Internet not connected!";
};
```

#copyable
```
:if ([/ping api.mikrocloud.com count=3;]>1) do={:put "Internet OK";} else={:put "Internet not connected!";};
```
::

If your router is connected to the Internet you should see a response like this:
```
  SEQ HOST                                     SIZE TTL TIME  STATUS                                                                               
    0 75.2.35.65                                 56 120 3ms  
    1 75.2.35.65                                 56 120 4ms  
    2 75.2.35.65                                 56 120 12ms 
    sent=3 received=3 packet-loss=0% min-rtt=3ms avg-rtt=6ms max-rtt=12ms 

Internet OK
```

If you cannot reach api.mikrocloud.com - please follow this troubleshooting guide.

## Bootstrap Script

In order to adopt your router we need to install an asynchronous API service. Copy the script below and paste it in your terminal:

::rsc
```
/tool fetch url="https://api.mikrocloud.com/bootstrap" dst-path="mikrocloud.rsc";
/import mikrocloud.rsc;
```

#copyable
```
/tool fetch url="https://api.mikrocloud.com/bootstrap" dst-path="mikrocloud.rsc";/import mikrocloud.rsc;
```
::

You should see the following output in your terminal:

```
[admin@MikroCloud - D9700EE2A596] > /tool fetch url="https://api.mikrocloud.com/bootstrap" dst-path="mikrocloud.rsc";/import mikrocloud.rsc;
      status: finished
  downloaded: 5KiBC-z pause]
       total: 5KiB
    duration: 1s

      status: finished
  downloaded: 5KiBC-z pause]
       total: 5KiB
    duration: 0s


Script file loaded and executed successfully
Awaiting scheduler creation process...
0%
20%
40%
60%
80%
100%
Dispatching initial heartbeat...
Done!


=========================================
      status: finished
  downloaded: 0KiBC-z pause]
        data: Bootstrap completed successfully | Activation Code: ******** | Visit https://app.mikrocloud.com/new?code=******** to activate this device.

=========================================

Script file loaded and executed successfully
[admin@MikroCloud - D9700EE2A596] >
```
