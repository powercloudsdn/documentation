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

