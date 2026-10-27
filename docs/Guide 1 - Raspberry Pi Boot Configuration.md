# Raspberry Pi Boot Configuration

This guide documents how to prepare a Raspberry Pi bootloader for a selected boot method using Raspberry Pi Imager. It was developed as part of a larger project to build a virtualized environment for isolated malware-analysis research.

## Objectives

- Identify the available Raspberry Pi boot methods.
- Prepare a microSD card with the appropriate bootloader utility.
- Configure the desired boot method.
- Verify that the bootloader preparation process completes.

## Prerequisites

- A compatible Raspberry Pi.
- A microSD card.
- A microSD card reader or USB adapter.
- A computer with internet access.
- Raspberry Pi Imager
- A monitor and compatible cable for optional local verification

<strong>Warning: Writing an image to a storage device can erase its existing contents. Confirm that the correct microSD card is selected before proceeding and backup any data you need to retain.</strong>

## 1. Understand the Boot Options

The original project documentation considered three boot methods:
- <b>SD card:</b> Boot using a microSD card.
- <b>USD:</b> Boot using a supported USB storage device.
- <b>Network:</b> Boot using a supported network-boot configuration.

The available options depend on the Raspberry Pi model, bootloader configuration, and supported hardware. This procedure prepares the bootloader; it does not configure a complete network-boot environment.

For the original project, USB boot preparation was relevant to the planned virtualization setup.

## 2. Download Raspberry Pi Imager

1. Open the <a href="https://www.raspberrypi.com/software/">official Raspberry Pi software page</a>
2. Download Raspberry Pi Imager for your computer's operating system.
   <details>
     <summary>View screenshot</summary>
     ![Select correct OS](/doc_images/download_rasp_pi_imager.png)
   </details>
3. Install and launch the application.

Use the official download source to reduce the risk of obtaining a modified installer.

## 3. Connect the microSD card

1. Insert the microSD card into your computer, using an adapter if necessary 
   <details>
     <summary>View screenshot</summary>
     ![Adapter example](/doc_images/adapter_example.png)
   </details>
