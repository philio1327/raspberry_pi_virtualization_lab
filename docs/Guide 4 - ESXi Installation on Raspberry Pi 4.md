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

<details>
  <summary>View screenshot</summary>

  ![Choose OS](./doc_images/esxi_install_choose_os.png)
</details>

<details>
  <summary>View screenshot</summary>

  ![Choose Raspberry Pi Other](./doc_images/esxi_install_choose_other.png)
</details>

<details>
  <summary>View screenshot</summary>

  ![Choose Raspberry Pi Lite](./doc_images/esxi_install_choose_rasp_pi_lite.png)
</details>

6. Select the correct microSD card under <strong>Choose Storage</strong>.

<details>
  <summary>View screenshot</summary>

  ![Choose Storage](./doc_images/esxi_install_choose_storage.png)
</details>

<details>
  <summary>View screenshot</summary>

  ![Choose Correct Storage](./doc_images/esxi_install_choose_correct_storage.png)
</details>

7. Review the selected device carefully before writing.

The purpose of this installation is to update the Raspberry Pi and its EEPROM bootloader. Raspberry Pi OS will be erased later in the procedure. 

⚠️ <strong>Warning</strong>: Writing an operating system to the microSD card erases its existing contents. Verify that you have selected the correct storage device. 

Note: You can add in SSH Keys and other authentication controls such as a password for the Rasbian Lite OS; however it is unnecessary as this OS will be overwritten. 

<strong>Ensure you disable the wifi under Edit Settings before writing to this microSD card</strong>

### 2.2 Boot the Raspberry Pi and Update the System 

1. Safely eject the microSD card from your computer.
2. Insert it into the Raspberry Pi.
<details>
  <summary>View screenshot</summary>

  ![Insert microSD](./doc_images/esxi_install_rasp_pi_microSD.png)
</details>

3. Connect a monitor and keyboard to the Raspberry Pi.
<details>
  <summary>View screenshot</summary>

  ![Connect Monitor & Keyboard](./doc_images/esxi_install_connect_monitor.png)
</details>

4. Connect the Raspberry Pi to the internet using Ethernet.
<details>
  <summary>View screenshot</summary>

  ![Connect Ethernet](./doc_images/esxi_install_connect_ethernet.png)
</details>

5. Power on the Raspberry Pi and complete the initial Raspberry Pi OS setup.
6. Update the operating system and bootloader EEPROM.
   To do this, once Raspberry Pi has booted, and you've logged in using your login credentials (it may ask you to create new credentials on first boot, or you can use username "root" when prompted and leave the password blank, though it may ask you to provide a password).
   
   Ensure you have an internet connection by using the command: <code>ping google.com</code>

   <details>
     <summary>View screenshot</summary>

     ![Ping Test](./doc_images/esxi_install_ping_test.png)
   </details>

   Then update your Raspberry Pi using:
   <code>sudo apt update && sudo apt upgrade -y</code>

   Note: You can omit sudo if you are the root user.

   Then,
   
   <code>sudo apt full-upgrade</code>
   
   <code>sudo rpi-update</code>

   <details>
     <summary>View screenshot</summary>

     ![RPI Update](./doc_images/esxi_install_rpi_update.png)
   </details>

   Then finally the following commands:
   
   <code>sudo rpi-eeprom-update -a</code>
   
   <code>sudo reboot</code>

   <details>
     <summary>View screenshot</summary>

     ![Update EEPROM](./doc_images/esxi_install_eeprom_update.png)
   </details>

### 2.3 Verify the Bootloader

Once the update and reboot are complete, sign in to your Raspberry Pi using the credentials you configured and verify that the bootloader installed correctly. Use the following command:
<code>sudo rpi-eeprom-update -a</code>

<details>
  <summary>View screenshot</summary>

  ![Verify EEPROM](./doc_images/esxi_install_verify_eeprom.png)
</details>

Then <strong>sudo reboot</strong> and once the Raspberry Pi is on, unplug the power source to the Raspberry Pi and remove the SD card. 

### 2.4 Erase the microSD Card

Once the firmware and EEPROM updates are complete, the Raspberry Pi OS installation is no longer required. 

1. Remove the microSD card and connect it to your computer.
2. Open the Raspberry Pi Imager.
3. Select the correct Raspberry Pi device.
<details>
  <summary>View screenshot</summary>

  ![Choose Device](./doc_images/esxi_install_choose_device2.png)
</details>

<details>
  <summary>View screenshot</summary>

  ![Choose Device](./doc_images/esxi_install_choose_device3.png)
</details>

4. Under <strong>Choose OS</strong>, scroll down and select <strong>Erase</strong>.

<details>
  <summary>View screenshot</summary>

  ![Choose OS](./doc_images/esxi_install_choose_os2.png)
</details>

<details>
  <summary>View screenshot</summary>
  
  ![Erase](./doc_images/esxi_install_erase.png)
</details>

5. Select the microSD card as the storage device.
6. Confirm the operation and wait for the erase process to complete.

