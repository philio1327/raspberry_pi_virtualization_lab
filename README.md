# Raspberry Pi Virtualization Lab

## Description

Multi-host Raspberry Pi virtualization lab using VMware ESXi, vCenter, OpenWRT, and OpenMediaVault, developed in preparation for malware analysis.

## Overview

This academic project involved building a multi-host virtualization environment using Raspberry Pi hardware and VMware ESXi. The lab combined virtualized guest operating systems, centralized host management, network services, and shared storage.
The environment was designed as a foundation for future malware-analysis work. Virtual machine provisioning, network connectivity restrictions, and snapshot-based recovery were tested. Live malware execution and behavioural analysis were not performed.

## Objectives

- Build a low-cost, multi-host virtualization environment.
- Configure local network services and shared storage.
- Provision and manage multiple guest operating systems.
- Explore centralized management using vCenter Server.
- Test guest connectivity restrictions and snapshot recovery.
- Identify technical limitations and future security improvements.

## Technologies 

- Virtualization: VMware ESXi, vCenter Server, VMware Workstation Pro
- Hardware: Raspberry Pi 3, four Raspberry Pi 4 hosts, Raspberry Pi 5
- Networking: OpenWRT, DHCP, local DNS, network switch
- Storage: OpenMediaVault, NFS, VMFS datastores
- Guest operating systems: Windows, Kali Linux, Fedora, CentOS, Ubuntu, and other Linux distributions

## Implemented and Tested

- Registered four ESXi hosts in vCenter Server for centralized management.
- Configured OpenWRT network services and local hostname resolution.
- Configured NAS-backed shared storage using OpenMediaVault and NFS.
- Provisioned Windows and multiple Linux guest operating systems.
- Tested guest connectivity restrictions to selected lab resources.
- Disabled shared folders and USB passthrough for the intended analysis workflow.
- Tested snapshot creation, persistence across reboot, and rollback on Windows and Kali Linux.

## Limitations

- vSphere clustering was attempted but not completed.
- Live malware execution and behavioural analysis were not performed.
- Snapshot rollback was tested using file changes, not malware samples.
- No screenshots or contemporaneous logs of the original snapshot rollback tests were retained.

## Future Work 

- Complete the architecture diagram and detailed implementation documentation.
- Organize available screenshots and configuration evidence.
- Further assess network isolation and guest-to-host attack paths.
- Investigate clustering requirements and shared-storage limitations.
- Define and validate additional containment controls before considering live malware analysis.

## Project Status

This repository documents a completed hands-on infrastructure project and its tested features, alongside incomplete objectives and potential future improvements. It does not represent a validated malware-analysis sandbox.
