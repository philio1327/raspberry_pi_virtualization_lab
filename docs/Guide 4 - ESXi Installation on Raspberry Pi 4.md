# ESXi Installation on Raspberry Pi 4

## Overview

This guide documents the installation of VMware ESXi ARM on a Raspberry Pi 4. It covers preparing the microSD card with the required Raspberry Pi firmware and UEFI bootloader, creating an ESXi installation USB, configuring the UEFI settings, and installing the hypervisor onto a separate USB storage device.

The objective is to configure the Raspberry Pi to boot directly into ESXi, allowing it to host ARM64 virtual machines. 

The original project used multiple Raspberry Pi 4 devices as ESXi hosts. 

<strong>Important:</strong> These instructions reflect the original project environment and the ESXi ARM Fling available at the time of implementation. Firmware, installer availability, compatibility, and installation procedures may differ with newer software versions.

## 1. Prerequisites

Before beginning, prepare the following: 

### Hardware

- Raspberry Pi 4 Model B
- microSD card
- USB storage device for the ESXi Installer (16-32 GB)
- Separate USB storage device for the ESXi Installation (16-64 GB or larger)
- Monitor and micro-HDMI to HDMI cable or adapter
- USB keyboard
- Ethernet connection
- Computer with an internet connection and USB ports

The microSD card stores the Raspberry Pi firmware and UEFI bootloader. ESXi itself will be installed on a separate USB storage device.

For multiple Raspberry Pi hosts, repeat the preparation and installation process for each device. Each Raspberry Pi requires its own boot media and ESXi installation storage.

### Software

- Raspberry Pi Imager
- Raspberry Pi firmware
- Raspberry Pi 4 UEFI firmware from the Project Firmware repository.
- VMware ESXi ARM Fling Installer
- Rufus (Windows) or balenaEtcher (macOS/Linux)

### Related Guides

- <a href="https://github.com/philio1327/raspberry_pi_virtualization_lab/blob/main/docs/Guide%201%20-%20Raspberry%20Pi%20Boot%20Configuration.md">Raspberry Pi Boot Configuration</a>
- <a href="https://github.com/philio1327/raspberry_pi_virtualization_lab/blob/main/docs/Guide%202%20-%20OpenWRT%20Installation.md">OpenWRT Installation on Raspberry Pi</a>
- <a href="https://github.com/philio1327/raspberry_pi_virtualization_lab/blob/main/docs/Guide%203%20-%20USB%20Storage%20Preparation%20for%20ESXi.md">USB Storage Preparation</a>

## 2. Prepare the microSD Card for ESXi

The microSD card will contain the Raspberry Pi firmware and UEFI bootloader required to initialize the hardware and launch ESXi.

### 2.1 Install Raspberry Pi OS

1. Download and install <a href="https://www.raspberrypi.com/software/">Raspberry Pi Imager</a>, if you haven't already.

<details>
  <summary>View screenshot</summary>

  ![Raspberry Pi Imager Download](./doc_images/esxi_install_rasbian.png)
</details>

2. Insert a microSD card into your computer using an SD card reader or USB adapter.
<details>
  <summary>View screenshot</summary>

  ![USB Adapter](./doc_images/esxi_install_usb_adapter.png)
</details>

3. Open Raspberry Pi Imager.
4. Select the appropriate Raspberry Pi device. For this project, select Raspberry Pi 4.
<details>
  <summary>View screenshot</summary>

  ![Choose Device](./doc_images/esxi_install_choose_device.png)
</details>

<details>
  <summary>View screenshot</summary>

  ![Choose Raspberry Pi 4](./doc_images/esxi_install_choose_rasp4.png)
</details>
5. Under <strong>Choose OS</strong>, select Raspberry Pi OS Lite (64-bit).
6. Select the correct microSD card under <strong>Choose Storage</strong>.
7. Review the selected device carefully before writing.

The purpose of this installation is to update the Raspberry Pi and its EEPROM bootloader. Raspberry Pi OS will be erased later in the procedure. 

⚠️ <strong>Warning</strong>: Writing an operating system to the microSD card erases its existing contents. Verify that you have selected the correct storage device. 

### 2.2 Boot the Raspberry Pi and Update the System 

