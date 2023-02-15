---
title: Bootstrap your MikroTik
description: Bootstrapping your MikroTik router is the first step towards SDN enablement.
---

## Required Steps

1. Create a Service Orchestration Portal account.
2. Perform an [Internet reachability test](#internet-reachability-test)
3. Install a [supported version](/documentation/router-onboarding/supported-routeros-versions) of RouterOS.
4. Run the bootstrap script.
5. Adopt your router in the Service Orchestration Portal.

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
:if ([/ping api.mikrocloud.com count=3;]>1) do={:put "Internet OK";} else={:put "Internet not connected!";};
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

> If you cannot reach api.mikrocloud.com - please follow this troubleshooting guide.

