# OpenWRT DNS Configuration

## Overview

This guide explains how to configure local DNS records and static DHCP leases using OpenWRT.

In this project, OpenWRT acts as the DHCP and DNS server for the isolated Raspberry Pi virtualization network. Configuring local DNS records allows each ESXi host to be accessed using a memorable hostname instead of requiring its IP address to be entered manually. 

For example, instead of accessing an ESXi host using an address such as: 

https://192.168.1.105

You can access it using a hostname:

https://raspberrypi4_1.lan

The guide also explains how to configure static DHCP leases to ensure that each host consistently receives the same IP address. 

## Prerequisites


