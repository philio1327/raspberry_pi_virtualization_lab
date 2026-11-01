# USB Storage Preparation for ESXi

## Overview

This guide explains how to prepare a USB storage device for use with the VMware ESXi by creating two separate partitions. 

The purpose of this configuration is to separate the ESXi boot and host files from the remaining storage capacity, allowing the rest of the drive to be used for virtual machine storage.

<strong>Important:</strong> This guide prepares the USB storage device in Windows. It does not install ESXi or configure the VMFS datastore. Those steps are covered separately. 

Also, this document will be referenced in later guides.

## Prerequisites

Before beginning, ensure you have: 

- A USB storage device with sufficient capacity for both partitions (64 GB or more).
- A Windows computer.
- Access to Windows Disk Management.
- Any important files on the USB device backed up elsewhere.

For this configuration, a larger USB drive is preferable because the second partition will use the remaining capacity after reserving 8 GB for ESXi.

## 1. Open Windows Disk Management

1. Connect the USB storage device to your computer.
2. Open the Windows taskbar search (usually bottom left of Windows desktop).

<details>
  <summary>View screenshot</summary>

  ![Windows Taskbar](./doc_image/usb_storage_taskbar.png)
</details>

3. Type <code>Disk Management </code>
4. Select <strong>Create and format hard disk partitions</strong>

<details>
  <summary>View screenshot</summary>

  ![Create partitions](./doc_images/usb_storage_create_partitions.png)
</details>

Windows Disk Management displays the connected storage devices, their partitions, and available space.

