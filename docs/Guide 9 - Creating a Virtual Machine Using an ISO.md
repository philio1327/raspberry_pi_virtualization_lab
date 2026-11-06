# Guide 9 - Creating a Virtual Machine Using an ISO

# 1. Overview

This guide describes how to create and configure a virtual machine (VM) on VMware ESXi using an uploaded operating-system ISO image. 

For this project, Windows 10 ARM64 was used as the primary installation example. The VM was hosted on a Raspberry Pi 4 running VMware ESXi ARM.

The guide covers:

- Creating a new Virtual Machine
- Selecting the guest operating system.
- Choosing a datastore for VM storage.
- Allocating CPU and memory resources.
- Mounting an ISO image
- Configuring the virtual network adapter.
- Installing Windows 10.
- Adjusting VM resources after installation.

Although Windows is used for the walkthrough, the same general VM creation process can be used for Linux distributions such as Ubuntu, Fedora, CentOS, and Kali Linux. Their installation procedures are generally more straightforward and typically involve following the on-screen installation instructions.

## 2. Prerequisites

Before starting, ensure that you have:

- A running VMware ESXi host.
- Access to the ESXi Host Client.
- An uploaded operating-system ISO image.
- An available datastore for VM storage.
- Sufficient available CPU and memory resources.

For this walkthrough, the following configuration was used:

<table>
  <tr>
    <th>Component</th>
    <th>Configuration</th>
  </tr>
  <tr>
    <td>Host Hardware</td>
    <td>Raspberry Pi 4 (8 GB RAM)</td>
  </tr>
  <tr>
    <td>Hypervisor</td>
    <td>VMware ESXi ARM</td>
  </tr>
  <tr>
    <td>Guest operating system</td>
    <td>Windows 10 ARM64</td>
  </tr>
  <tr>
    <td>Initial CPU allocation</td>
    <td>1 vCPU</td>
  </tr>
  <tr>
    <td>Initial memory allocation</td>
    <td>5 GB RAM</td>
  </tr>
  <tr>
    <td>Final CPU allocation</td>
    <td>2 vCPUs</td>
  </tr>
  <tr>
    <td>VM storage</td>
    <td>OpenMediaVault NAS (my_nas)</td>
  </tr>
  <tr>
    <td>Virtual Disk Size</td>
    <td>64 GB</td>
  </tr>
</table>

<strong>Important</strong>: The Raspberry Pi has 8 GB of physical RAM. ESXi requires memory for its own operation, so allocating too much memory to the guest OS can cause resource contention and instability. The hypervisor will always prioritize itself when there is resource contention so the guest OS will simply crash!

## 3. Create a Virtual Machine

After uploading the ISO image to a datastore (see Guide 8), navigate to the ESXi Host Client.

1. Open <strong>Virtual Machines</strong>.
2. Select <strong>Create / Register VM</strong>.
<details>
  <summary>View screenshot</summary>

  ![Create VM Register](./doc_images/create_vm_register_vm.png)
</details>

3. Choose <strong>Create a new virtual machine</strong>.
<details>
  <summary>View screenshot</summary>

  ![Create VM New](./doc_images/create_vm_new.png)
</details>

4. Continue to the configuration screen.

## 4. Select the VM Name and Operating System

Enter a descriptive name for the virtual machine.

Next, select the appropriate guest OS settings.

For the Windows 10 installation used in this project:

<table>
  <tr>
    <th>Setting</th>
    <th>Selection</th>
  </tr>
  <tr>
    <td>Compatibility</td>
    <td>ESXi 8.0 U2 virtual machine</td>
  </tr>
  <tr>
    <td>Guest OS Family</td>
    <td>Windows</td>
  </tr>
  <tr>
    <td>Guest OS Version</td>
    <td>Windows 10 (64-bit)</td>
  </tr>
</table>

If you are installing a Linux distribution or another supported operating system, select the corresponding guest OS family and version instead.

<details>
  <summary>View screenshot</summary>

  ![Select name and OS](./doc_images/create_vm_select_name_os.png)
</details>

## 5. Select the Datastore

Choose where the virtual machine's files and virtual disk will be stored. 

In this project, two storage options are available (created earlier when we created our datastores):

- my_nas - The OpenMediaVault NAS datastore backed by the 1 TB SSD.
- usb_datastore - Local USB storage connected to the ESXi host.

For the final Windows installation select <strong>my_nas</strong>.

Although the USB datastore was tested initially, installing Windows onto USB storage took several hours and was unstable (and very slow). Installing onto the NAS SSD took less than an hour in the original testing.

The virtual machine's storage is isolated from the host's own operating-system storage. The guest OS accesses the virtual disk allocated to it rather than directly accessing the underlying datastore filesystem.

<details>
  <summary>View screenshot</summary>

  ![Select Storage](./doc_images/create_vm_select_storage.png)
</details>

## 6. Customize Virtual Machine Settings

Configure the VM's hardware before proceeding with installation.

