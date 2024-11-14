---
author: Minh PN
pubDatetime: 2024-09-18T13:20:53Z
title: "[K8s] How to install ubuntu server in VMWare Part 1"
slug: "how-to-install-ubuntu-server-in-vmware-part-1"
featured: true
draft: false
tags:
  - ubuntu
  - vmware
description: "This article provides a detailed guide on installing Ubuntu Server in VMware with a Bridge network configuration. This setup allows the virtual machine (VM) to share the local area network (LAN) connection with the host machine, making it suitable for deploying servers or services on the same network as other devices."
---

## Table of contents

## Prerequisites

- [Latest version of VMware](https://www.vmware.com/)
- [Ubuntu Server 20.04 LTS ISO image](https://ubuntu.com/download/server)

## Step 1: Install VMWare

1. Go to VMware's website and download the latest version of VMware.
2. Follow the instructions provided on the website or in the documentation to install VMware.

## Step 2: Create and Install Ubuntu Server in VMware

1. Open VMware and select "Create a New Virtual Machine."
2. Choose the Ubuntu Server 20.04 LTS ISO image you downloaded.
3. Configure the VM with the following settings:

   - CPU: 2 cores
   - RAM: 3GB
   - Disk space: 20GB
   - Network Adapter: Set to Bridge mode so that the VM shares the LAN connection with the host machine.

### Notes:

- Bridge: The VM will share the LAN connection with the host, making it part of the same network as other devices.
- NAT: The VM uses a private IP and shares the host’s internet connection, but its IP won’t be visible on the local network.

4. Proceed with the Ubuntu Server installation as instructed on the screen.

## Step 3: Configure Networking with Netplan

After successfully installing Ubuntu, you need to configure the network with Netplan to assign a static IP to the VM.

1. Open the Netplan configuration file:

```
sudo nano /etc/netplan/01-netcfg.yaml
```

2. Add the following configuration to the file (adjust the IP address to match your network):

```
. . .
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: false
      addresses:
        - 192.168.1.110/24
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
. . .
```

- **addresses**: The static IP address for the VM (adjust 192.168.1.110 to suit your LAN's IP range).
- **gateway4**: The gateway address of your LAN.
- **nameservers**: The DNS servers (you can use Google's DNS as shown in the example).

3. Save the file and apply the new configuration:

```
sudo netplan apply
```

## Step 4: Verify IP Address

To verify that the IP address has been configured correctly, run the following command:

```
ip a
```

If you see the IP address `192.168.1.110` listed in the output, congratulations! You have successfully configured the network for Ubuntu Server.
