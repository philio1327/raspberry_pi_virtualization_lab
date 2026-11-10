# Guide 10 - Deploying VMware vCenter Server Appliance

## 1. Overview

This guide documents the deployment and configuration of VMware vCenter Server Appliance (VCSA) using VMware Workstation Pro 17.

In this project, vCenter was deployed as a virtual appliance running inside VMware Workstation on a separate laptop. Once configured, it provided the centralized management interface used to administer the four Raspberry Pi 4 devices running VMware ESXi ARM. 

The guide covers:

- Downloading the vCenter Server Appliance installation media.
- Extracting the appliance files.
- Converting the OVA package into OVF format.
- Modifying the OVF configuration.
- Importing the appliance into VMware Workstation.
- Configuring and deploying the appliance.
- Completing the second configuration stage.
- Accessing the vSphere Client.

## 2. Prerequisites

Before deploying vCenter, ensure that you have:

- A laptop with minimum 16 GB of physical RAM.
- Sufficient available storage for the Virtual Appliance.
- A working connection to the lab environment.
- OpenWRT configured with the required DNS records.

## 3. Download and Install VMware Workstation Pro 17

vCenter must be hosted inside a Virtual Machine. We will install VMware Workstation Pro 17, and then deploy vCenter inside this. This requires a laptop of minimum 16 GB of RAM, though 24+ GB would provide better performance.

1. Login to your Broadcom Account (you can create one if this has not already been done).
2. Head to the following link (may have changed since project):
<a href="https://support.broadcom.com/group/ecx/productdownloads?subfamily=VMware%20Workstation%20Pro&freeDownloads=true>VMware Workstation Pro 17 Download Page</a>

<details>
  <summary>View screenshot</summary>

  ![Workstation Pro Download](./doc_images/vcenter_workstation_download.png)
</details>

3. Install Workstation Pro 17 
<details>
  <summary>View screenshot</summary>

  ![Workstation Pro Install](./doc_images/vcenter_workstation_install_1.png)
</details>

<details>
  <summary>View screenshot</summary>

  ![Workstation Pro Install](./doc_images/vcenter_workstation_install_2.png)
</details>

<details>
  <summary>View screenshot</summary>

  ![Workstation Pro Install](./doc_images/vcenter_workstation_install_3.png)
</details>

<details>
  <summary>View screenshot</summary>

  ![Workstation Pro Install](./doc_images/vcenter_workstation_install_4.png)
</details>

<details>
  <summary>View screenshot</summary>

  ![Workstation Pro Install](./doc_images/vcenter_workstation_install_5.png)
</details>

<details>
  <summary>View screenshot</summary>

  ![Workstation Pro Install](./doc_images/vcenter_workstation_install_6.png)
</details>

