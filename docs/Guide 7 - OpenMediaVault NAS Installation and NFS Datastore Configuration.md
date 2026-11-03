# Guide 7 - OpenMediaVault NAS Installation and NFS Datastore Configuration

## 1. Overview

This guide documents the installation and configuration of OpenMediaVault (OMV) on a Raspberry Pi 5, with an external 1 TB SSD drive configured as network-attached storage.

The NAS provides shared storage for the four Raspberry Pi ESXi hosts in the lab. An NFS share is created in OpenMediaVault and mounted as a datastore on each ESXi host.

The setup consists of:

- Raspberry Pi 5 (8 GB RAM) running Raspberry Pi OS Lite 64-Lite.
- 1 TB external storage drive (SSD)
- USB cable to connect SSD to Raspberry Pi Model 5's USB 3.0 port (blue port)
- OpenMediaVault for NAS Management
- NFS for sharing storage with the ESXi hosts.
- OpenWRT for DHCP and local network management.

<strong>Storage architecture</strong>:

- The physical 1 TB drive is formatted and mounted in OpenMediaVault.
- A shared folder is created on that filesystem.
- The folder is exported through NFS.
- Each ESXi host mounts the NFS export as a datastore.

## 2. Prerequisites

Before starting, ensure that you have:

- Raspberry Pi 5 with 8 GB RAM.
- microSD card for the Raspberry Pi OS.
- 1 TB external storage drive.
- USB 3.0 connection for the external drive.
- Ethernet connection to the lab network.
- Another computer to prepare the microSD card and access the management interfaces.

The Raspberry Pi should be connected to the network through Ethernet. Wi-Fi is not used in this setup.

## 3. Install Raspberry Pi OS Lite (64-bit)


