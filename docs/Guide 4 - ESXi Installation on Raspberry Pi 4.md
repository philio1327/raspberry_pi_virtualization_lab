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

<details>
  <summary>View screenshot</summary>

  ![Choose Storage](./doc_images/esxi_install_choose_storage2.png)
</details>

6. Confirm the operation and wait for the erase process to complete.

This reformats the microSD card in preparation for copying the Raspberry Pi firmware and UEFI files.

⚠️ <strong>Verify the selected storage device before confirming. This operation erases the selected card.</strong>

### 2.5 Download Raspberry Pi Firmware and UEFI Firmware

Two separate firmware packages are required:

1. <strong>Raspberry Pi Firmware</strong>: Provides the files required to initialize and boot the Raspberry Pi hardware.
2. <strong>UEFI Firmware</strong>: Provides the UEFI environment required to boot the ESXi installer.

The original project used the following sources:
- Raspberry Pi Firmware: <a href="https://bit.ly/2HpIaG6">Raspberry Pi Firmware Repository</a>
- Raspberry Pi 4 UEFI Firmware: <a href="https://github.com/pftf/RPi4/releases/">Project Firmware - Raspberry Pi 4 Releases</a>

Download the appropriate versions and extract both archives. 

<details>
  <summary>View screenshot</summary>

  ![Extract Archives](./doc_images/esxi_install_extract_folders.png)
</details>

<strong>Compatibility Note</strong>: Use firmware versions appropriate for your Raspberry Pi model and the ESXi ARM build. The original project used Raspberry Pi 4 hardware. The original document also identified compatibility problems with Raspberry Pi 5, and those issues have since been resolved.

### 2.6 Copy the Raspberry Pi Firmware Files

After extracting the Raspberry Pi archive:

1. Open the extracted <code>firmware-master</code> directory.
2. Navigate to: <code>firmware/boot</code>
3. Copy the contents of the <code>boot</code> folder to the root of the microSD card.

<details>
  <summary>View screenshot</summary>

  ![Paste Firmware Files](./doc_images/esxi_install_paste_firmware_files.png)
</details>

4. If Windows reports duplicate files, skip the duplicates. 

### 2.7 Remove Kernel Image Files

Navigate to the root of the microSD card.

Delete the <code>kernel.img</code> image files identified in the original procedure

<details>
  <summary>View screenshot</summary>

  ![Remove Kernel Images](./doc_images/esxi_install_remove_kernel_images.png)
</details>

### 2.8 Copy the UEFI Firmware Files

1. Open the extracted Raspberry Pi 4 UEFI firmware directory.
2. Select all files and folders inside it.
3. Copy them to the root of the microSD card.
4. If Windows reports duplicate files, replace the existing files with the new ones.

<details>
  <summary>View screenshot</summary>

  ![Copy UEFI Files](./doc_images/esxi_install_copy_uefi.png)
</details>

### 2.9 Modify <code>config.txt</code>

Open the <code>config.txt</code> file located on the microSD card.

Add the following line at the end of the file:

<code>gpu_mem=16</code>

Save the file and safely eject the microSD card. 

The microSD card is now prepared with the Raspberry Pi Firmware and UEFI bootloader required for the ESXi Installation process. 

<details>
  <summary>View screenshot</summary>

  ![Modify Config](./doc_images/esxi_install_modify_config.png)
</details>

## 3. Prepare the ESXi Installation USB

The next step is to write the ESXi ARM Installer image to a separate USB Storage device.

### 3.1 Download the ESXi ARM Installer 

The original project used the VMware ESXi ARM Fling.

The historical download source was the following link: 

<a href="https://community.broadcom.com/flings/home">VMware Flings - Broadcom</a>

You will need to create an account and then access to the Flings will be granted. 

Download the appropriate ESXi ARM installer package and extract the downloaded archive to access the ISO image.

<details>
  <summary>View screenshot</summary>

  ![Download ARM Fling](./doc_images/esxi_install_download_fling.png)
</details>

<strong>Note</strong>: The availability and distribution of historical Fling downloads may change. Confirm that you have a compatible installer before proceeding.

### 3.2 Download an Imaging Tool

The original procedure used Rufus on Windows. 
- <a href="https://rufus.ie/en/">Rufus</a>

For macOS and Linux, balenaEtcher is recommended:
- <a href="https://etcher.balena.io/">balenaEtcher</a>

The following steps will use Rufus.

Download the appropriate one for your version of Windows (go to taskbar and type About to see what type of Processor your Windows OS is running on):

<details>
  <summary>View screenshot</summary>

  ![Download Rufus](./doc_images/esxi_install_download_rufus.png)
</details>

### 3.3 Write the ESXi ISO to USB using Rufus

1. Insert a USB storage device (16-32 GB) into your computer.
2. Open Rufus.
3. Under <strong>Device</strong>, select the USB device intended for the ESXi installer.
4. Under the <strong>Boot selection</strong>, click <strong>SELECT</strong>.
<details>
  <summary>View screenshot</summary>

  ![Select Image](./doc_images/esxi_install_select_button.png)
</details>
5. Navigate to the extracted ESXi ARM ISO image.
<details>
  <summary>View screenshot</summary>

  ![Select ISO Image](./doc_images/esxi_install_select_iso_image.png)
</details>
6. Select the ISO image.

⚠️ <strong>Warning</strong>: This is a destructive operation. Rufus will erase and reformat the selected USB device. Back up any important files before continuing. 

### 3.4 Configure the Partition Scheme and Cluster Size

The original project used the following Rufus settings:

Partition scheme: GPT
Cluster Size: 32 kB

The original instructions explicitly specified GPT rather than MBR. Once the settings are configured, click <strong>Start</strong> and allow Rufus to write the image to the USB device. When complete, safely eject the USB drive.

<details>
  <summary>View screenshot</summary>

  ![Partition Scheme](./doc_images/esxi_install_partition_scheme.png)
</details>

## 4. Connect the Installation Hardware

Before powering on the Raspberry Pi, connect the required hardware. 

- Raspberry Pi 4
- Prepared microSD card.
- USB drive containing the ESXi installer.
- Monitor connected through micro-HDMI.
- USB keyboard.
- Ethernet connection. 

<details>
  <summary>View screenshot</summary>

  ![Hardware](./doc_images/esxi_install_hardware.png)
</details>

In image, cables left to right are:
- Power Cable
- microHDMI to HDMI to monitor
- USB keyboard
- Lexar USB with ESXi VMware ISO
- Ethernet cable for internet connection

Insert the prepared microSD card into the Raspberry Pi and connect the ESXi installer USB. The Raspberry Pi should remain powered off until all connections are complete.

## 5. Enter the UEFI Configuration Menu

1. Power on the Raspberry Pi.
2. As the Raspberry Pi starts, repeatedly press <strong>ESC</strong> on the connected keyboard.
<details>
  <summary>View screenshot</summary>

  ![Boot Screen](./doc_images/esxi_install_press_esc.png)
</details>

3. Enter the UEFI configuration interface.

<details>
  <summary>View screenshot</summary>

  ![UEFI Menu](./doc_images/esxi_install_uefi_menu.jpg)
</details>

The UEFI menu provides access to the boot settings and hardware configuration needed for ESXi. 

