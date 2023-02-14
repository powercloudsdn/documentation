---
title: Supported RouterOS Versions
description: All our scripts and automation procedures for RouterOS is thoroughly tested and production ready. In order to onboard your router you need to use one of the supported versions of RouterOS.
---

## 6.47.10 (long-term)
::rsc
/ip dns static remove [find where name="upgrade.mikrotik.com"];
:if ([:len [/ip dns static find name="upgrade.mikrotik.com"]] = 0) do={
	/ip dns static add address=3.210.237.154 name=upgrade.mikrotik.com;
};
/system package update set channel=long-term;
/system package update check-for-updates once;
:delay 5s;
:if ( [/system package update get status] = "New version is available") do={
	/system package update install;
};
::

## 7.5 stable

