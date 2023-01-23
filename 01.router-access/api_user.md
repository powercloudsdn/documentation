---
title: MikroCloud API User
description: The MikroCloud API user is mainly used by our synchronous API to perform tasks on your router in realtime.
---

## Why does MikroCloud need to put a user account on my router?

MikroCloud needs an API user on each router that it manages in order to perform synchronous tasks. These are some examples of tasks that would require API access:

* Read operations to obtain current configuration on the router
* Tasks that cannot be fulfilled by the asynchronous API (periodic backups, reboots, torch etc.)

## Security considerations and restrictions

### Layer 3 security
API access to your router is typically facilitated through the use of an OpenVPN management tunnel. The source IP address of this management tunnel (i.e. the remote address from the perspective of your router) will always be __154.66.115.255__. API access as well as the API user is restricted to this IP address. This will automatically prevent connections from other networks or hosts. __154.66.115.255__ is part of public IP address space under the control of MikroCloud and can be considered a trusted source.

### Encryption at rest
Passwords are stored in our environment with OpenSSL using AES-256 encryption. These passwords are also signed using a message authentication code (MAC) so that their underlying value can not be modified or tampered with once encrypted.

## Configuration script

The following script is used to create the MikroCloud API user:

```shell
:local password "?";

# Find and remove the existing MikroCloud API user
:if ([:len [/user find name="mikrocloud-api"]] = 1) do={
    /log info "MikroCloud: Removing mikrocloud-api user";
    /user remove [find name="mikrocloud-api"];
};

# Create a new API user
:do {
    /log info "MikroCloud: Creating mikrocloud-api user";
    /user add name="mikrocloud-api" group=full disabled=no password="$password" address="154.66.115.255" comment="MikroCloud automation API user - Do not edit or remove";
} on-error={
    /log error "MikroCloud: Failed to create mikrocloud-api user";
};

# Restrict the API service to a trusted IP address
:do {
    /log info "MikroCloud: Updating API service setting";
    /ip service set [find where name="api"] port=8728 disabled=no address="154.66.115.255";
} on-error={
    /log error "MikroCloud: Failed to update API service setting";
};
```
