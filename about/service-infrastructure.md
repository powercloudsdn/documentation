---
title: Infrastructure IP Addresses, Ports and FQDN's
description: This is a list of resources that we make use of to securely deliver our services to thousands of MikroTik routers and users over the internet.
---

## Anycast Addresses

The IP addresses below are anycast IP's that are mainly used to facilitate ingress connections from users and MikroTik devices on the internet.


### JSON and RouterOS Services

The IP addresses below service our system API, RouterOS API, Management VPN and SFTP Backup servers

| IP Address | TCP Ports | Services |
|------------|------|-------------|
| 75.2.35.65 | 443, 1194, 2022 | HTTPS API, Management VPN, SFTP (backups) |
| 99.83.143.188 | 443, 1194, 2022 | HTTPS API, Management VPN, SFTP (backups) |
| 75.2.118.244 | 443 | HTTPS API |
| 99.83.188.232 | 443 | HTTPS API |


### Traffic Steering Tunnels

Traffic steering tunnels are seen on managed WAN failover configurations. There is a tunnel created on the router for every WAN, each tunnel connects to a unique IP address selected from the list below:

| External IP | Internal IP | TCP Port |
|------------|------|------|
| 52.223.15.65 | 154.66.115.3 | 443 |
| 35.71.131.150 | 154.66.115.4 | 443 |
| 15.197.134.118 | 154.66.115.5 | 443 |
| 3.33.233.165 | 154.66.115.6 | 443 |


## Egress IP Addresses

Whenever an outbound connection is made from MikroCloud's production environment to a resource on the internet it will be originated from one these IP's

| IP Address |
|------------|
| 15.197.134.118 |
| 75.2.35.65 |

## Domains

MikroCloud makes use of these domains to deliver SD-WAN for MikroTik.

| FQDN | Remark |
|------------|------|
| pwrcld.net | Used for short-links |
| mypowercloud.net | Web apps live here |
| powercloud-api.net | Mainly used for API services
| mikrotik-sdwan.com | Dynamic DNS - subdomains point to user routers

## Autonomous System Number

It is also safe to trust traffic originated from [AS328400](https://www.peeringdb.com/net/10680)


