# OpenWRT Installation on Raspberry Pi

This guide documents the installation of OpenWRT on a Raspberry Pi for use as a DHCP and DNS server in a small virtualized lab environment. 

The original project used a Raspberry Pi 3 Model B to provide network services to the lab's virtualization hosts. The procedure below follows that reference setup; menu names and supported installation options may differ between software versions and Raspberry Pi Models; however, the steps will be similar for other Raspberry Pi Models.

## Objectives 

- Download an OpenWRT firmware image for the target Raspberry Pi.
- Write the firmware image to a microSD card.
- Prepare authentication settings where supported.
- Boot the Raspberry Pi into OpenWRT.
- Access the LuCI web interface.

## Prerequisites

- A compatible Raspberry Pi (Raspberry Pi 3 Model B in the original setup).
- A microSD card and compatible card reader.
- A computer with internet access.
- OpenWRT Firmware Selector.
- Raspberry Pi Imager
- An Ethernet cable and network switch,
- A computer with Ethernet connectivity for accessing the device.

<strong>Warning: </strong> Writing firmware to a storage device erases its existing contents. Confirm that the correct microSD card is selected before proceeding. Back up any data you need to retain.

## 1. Download the OpenWRT firmware image

1. Open the <a href="https://firmware-selector.openwrt.org/">OpenWRT Firmware Selector.</a>
2. Select the target device corresponding to your Raspberry Pi model.

<details>
  <summary>View screenshot</summary>
  
  ![Select Raspberry Pi Model](./doc_images/openwrt_rasp_pi_model_select.png)
</details>

3. Locate the appropriate firmware image for that device.
4. Download the image to your computer.

If you are unsure which Raspberry Pi model you have, check the device's model information before selecting firmware. Firmware images are hardware-specific, so do not assume that an image for one model will work on another.
