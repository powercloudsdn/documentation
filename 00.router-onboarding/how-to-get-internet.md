---
title: Configuring Internet Access
description: This guide will help you configure your MikroTik router to establish an internet connection.
navigation: false
---

 We'll cover three common methods: Ethernet (with DHCP client), PPPoE, and LTE. Please note that this guide is focused on providing internet access to the router itself, and setting up the router to provide access for users is outside the scope of this document.

## Prerequisites
Before starting, ensure that you have performed a system reset on your MikroTik router without defaults. If you have not done this, refer to the [bootstrapping guide](/documentation/router-onboarding/bootstrap).

## Establishing an Internet Connection

### 1. Identifying the Ethernet Port with Internet Access
To determine which Ethernet port has internet access, connect your MikroTik router to your ISP's modem or gateway device using different Ethernet ports. You can use the router's LEDs to identify the port that has an active link.

### 2. Configuring the DHCP Client
Once you have identified the Ethernet port with internet access, follow these steps to configure the DHCP client:

1. Log in to your MikroTik router using Winbox.
2. Navigate to the "IP" menu and select "DHCP Client."
3. Click the "+" button to add a new DHCP client.
4. In the "Interface" dropdown menu, select the Ethernet port identified in the previous step.
5. Ensure that the "Add Default Route" checkbox is selected, then click "OK" to save the configuration.

### 3. Adding DNS Servers
Now, you'll configure the router to use Quad9 and Cloudflare DNS servers:

1. Navigate to the "IP" menu and select "DNS."
2. Click the "Settings" button (the gear icon) in the lower-right corner of the window.
3. In the "Servers" field, enter `9.9.9.9`, `149.112.112.112`, `1.1.1.1`, `1.0.0.1`.
4. Click "OK" to save the DNS configuration.

### 4. Configuring PPPoE (If Applicable)
If your ISP requires PPPoE authentication, follow these steps:

1. Navigate to the "Interfaces" menu.
2. Click the "+" button and select "PPPoE Client."
3. In the "Interface" dropdown menu, select the Ethernet port identified in step 1.
4. Enter the PPPoE username and password provided by your ISP.
5. Ensure that the "Add Default Route" checkbox is selected, then click "OK" to save the configuration.

### 5. Configuring LTE (If Applicable)
If you are using an LTE modem to connect your MikroTik router to the internet, follow these steps:

1. Insert your SIM card into the LTE modem.
2. Connect the modem to your MikroTik router's USB port.
3. In Winbox, navigate to the "Interfaces" menu.
4. Click the "+" button and select "LTE."
5. In the "Name" field, provide a name for your LTE interface.
6. In the "Modem Port" dropdown menu, select the appropriate USB port.
7. Enter the APN information provided by your ISP.
8. Click "OK" to save the LTE configuration.

## Testing the Internet Connection
Once you have configured the internet connection, test it by pinging `api.mikrocloud.com`:

1. In Winbox, navigate to the "Tools" menu and select "Ping."
2. In the "Address" field, enter api.mikrocloud.com.
3. Click "Start" to initiate the ping test.

If the ping test is successful, you should see replies from `api.mikrocloud.com`, indicating that your MikroTik router has established an internet connection.

## Troubleshooting and Tips
If you cannot connect to the internet, try the following troubleshooting steps:

1. Double-check your DHCP client, PPPoE, or LTE configuration settings. Ensure that you have entered the correct information provided by your ISP.
2. Verify that your DNS settings are correct, and that you have entered the correct Quad9 and Cloudflare DNS server addresses.
3. Restart your router and the modem or gateway device provided by your ISP.
4. Check the physical connections between your router and the modem or gateway device. Ensure that the Ethernet cables are securely plugged in.
5. Contact your ISP to confirm that there are no service outages or issues on their end.

Remember that this guide is focused on providing internet access to the router itself. Configuring the router to provide internet access to users is outside the scope of this document. Consult the MikroTik RouterOS documentation or the MikroTik community forums for further guidance on setting up your router for user access.

By following this guide, you should be able to establish an internet connection for your MikroTik router using Ethernet (with DHCP client), PPPoE, or LTE methods.