# Guide 7 - OpenMediaVault NAS Installation and NFS Datastore Configuration

## 1. Overview

This guide documents the installation and configuration of OpenMediaVault (OMV) on a Raspberry Pi 5, with an external 1 TB SSD drive configured as network-attached storage.

The NAS provides shared storage for the four Raspberry Pi ESXi hosts in the lab. An NFS share is created in OpenMediaVault and mounted as a datastore on each ESXi host.

The setup consists of:

- Raspberry Pi 5 (8 GB RAM) running Raspberry Pi OS Lite 64-Lite.
- 1 TB external storage drive (SSD)
- USB cable to connect SSD to Raspberry Pi Model 5's USB 3.0 port (blue port)
- OpenMediaVault for NAS Management
- NFS for sharing storage with the ESXi hosts.
- OpenWRT for DHCP and local network management.

<strong>Storage architecture</strong>:

- The physical 1 TB drive is formatted and mounted in OpenMediaVault.
- A shared folder is created on that filesystem.
- The folder is exported through NFS.
- Each ESXi host mounts the NFS export as a datastore.

## 2. Prerequisites

Before starting, ensure that you have:

- Raspberry Pi 5 with 8 GB RAM.
- microSD card for the Raspberry Pi OS.
- 1 TB external storage drive.
- USB 3.0 connection for the external drive.
- Ethernet connection to the lab network.
- Another computer to prepare the microSD card and access the management interfaces.

The Raspberry Pi should be connected to the network through Ethernet. Wi-Fi is not used in this setup.

## 3. Install Raspberry Pi OS Lite (64-bit)

This step has been prepared before. Please refer to Guide 4 Step 2 for more details with screenshots on how to install Raspberry Pi Lite OS. The steps will be re-written here:

1. Download and install Raspberry Pi Imager.
2. Insert the microSD card into your computer.
3. Open Raspberry Pi Imager.
4. Select the Raspberry Pi 5 as the device.
5. Select <strong>Raspberry Pi OS Lite (64-bit)</strong> as the operating system.
6. Select the microSD card as the storage device.
7. Configure the required operating system settings. (disable wifi)
8. Write the OS to the microSD card.

The Lite version provides a minimal OS installation without a desktop environment. 

Connect the Raspberry Pi to the internet via Ethernet (ensure wifi is disabled).

## 4. Update Raspberry Pi OS

Boot the Raspberry Pi and log in through the terminal. I connected via monitor and keyboard and the login details were configured directly in the OS settings. 

<details>
  <summary>View screenshot</summary>

  ![Login to Pi](./doc_images/omv_login_pi.jpg)
</details>

Update the package lists and install available updates using this command:

<code>sudo apt update && sudo apt upgrade -y</code>

<details>
  <summary>View screenshot</summary>

  ![Update Pi](./doc_images/omv_update_pi.jpg)
</details>

Once the update process completes, continue with the OpenMediaVault installation.

## 5. Install OpenMediaVault

Clear the terminal with the following command:

<code>clear</code>

<details>
  <summary>View screenshot</summary>

  ![Clear Command](./doc_images/omv_clear_command.jpg)
</details>

Use the following command to install OpenMediaVault:

<code>wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/install | sudo bash</code>

Once the installation script has completed, reboot the Raspberry Pi:

<code>sudo reboot</code>

Allow the Raspberry Pi to restart before proceeding.

## 6. Verify Configuration

After the Raspberry Pi reboots, login, and verify that the update and upgrade commands have already installed by repeating the earlier command:

<code>sudo apt update && sudo apt upgrade -y</code>

<details>
  <summary>View screenshot</summary>

  ![Verify Update](./doc_images/omv_verify_update.jpg)
</details>

Finally shutdown the Raspberry Pi with:

<code>sudo shutdown</code>

## 7. Connect Raspberry Pi to Lab Network

Once shutdown, disconnect the internet ethernet cable from your Raspberry Pi and connect your Pi to your switch with the OpenWRT DHCP server. Also, disconnect your monitor and keyboard from your Raspberry Pi 5 as they are no longer needed.

The Raspberry Pi 5 has a small white power button on its board. Press this to turn it on. 

<details>
  <summary>View screenshot</summary>

  ![Raspberry Pi](./doc_images/omv_pi_hardware.jpg)
</details>

<details>
  <summary>View screenshot</summary>

  ![Network Switch](./doc_images/omv_network_switch.jpg)
</details>

The Ethernet cables in the image above are as follows from left to right:
1. Laptop
2. Raspberry Pi 4-1 (red cable)
3. Raspberry Pi 4-2 (blue cable)
4. Raspberry Pi 4-3 (green cable)
5. Raspberry Pi 4-4 (black cable)
6. Raspberry Pi 5-1 (blue cable) - OMV
7. Raspberry Pi 3-0 (blue cable) - OpenWRT DHCP server

## 8. Access the OpenMediaVault Web Interface

After installation, shutdown, connecting Raspberry Pi 5 to lab network and reboot, identify the Raspberry Pi's IP address

On your laptop, login to OpenWRT at <code>192.168.1.1</code> via a web browser. 
In my case, OpenWRT identified 192.168.1.194 as the Raspberry Pi 5 IP address. 

<details>
  <summary>View screenshot</summary>

  ![IP Address List](./doc_images/omv_ip_address.png)
</details>

Set this IP address as a static IP address so the NAS retains a consistent IP address.

Open another tab, or browser and navigate to:

<code>http://192.168.1.194</code>

(the ip address OpenWRT assigned to your Raspberry Pi 5 device containing OMV)

Login using the default OpenMediaVault credentials

- <strong>Username</strong>: admin
- <strong>Password</strong>: openmediavault

<details>
  <summary>View screenshot</summary>

  ![Login Dashboard](./doc_images/omv_login_dashboard.png)
</details>

Change this default password before relying on the NAS in an operational environment.

<details>
  <summary>View screenshot</summary>

  ![OMV Dashboard](./doc_images/omv_dashboard.png)
</details>

## 9. Prepare the External Storage Drive

Connect the 1 TB external drive to a USB 3.0 port on the Raspberry Pi.

OpenMediaVault must recognize the drive before it can be configured as storage.

### 9.1 Identify the Disk 

1. In the OpenMediaVault web interface, navigate to <strong>Storage -> Disks</strong>.
<details>
  <summary>View screenshot</summary>

  ![Storage Disks](./doc_images/omv_storage_disks.png)
</details>

2. Locate the connected 1 TB SSD Drive.
<details>
  <summary>View screenshot</summary>

  ![Storage Drive](./doc_images/omv_ssd_drive.png)
</details>

3. Confirm that the selected device is the external drive.

### 9.2 Wipe the Disk

Select the external drive and use the wipe function.

<strong>Warning</strong>: Wiping the disk permanently removes its existing partition information and data. Confirm that you have selected the correct drive.

1. Select your Hard Drive.
2. Click the <strong>Eraser</strong> icon.
<details>
  <summary>View screenshot</summary>

  ![Eraser](./doc_images/omv_eraser.png)
</details>

3. Confirm Wipe.
<details>
  <summary>View screenshot</summary>

  ![Confirm Wipe](./doc_images/omv_confirm_wipe.png)
</details>

### 9.3 Create and mount an EXT4 filesystem.

Linux distributions (such as the ESXi hosts) prefer the ext4 filesystem partition scheme. Note that Windows uses SMB and this can be enabled in OMV later on.

1. Navigate to <strong>Storage -> File Systems</strong>.
<details>
  <summary>View screenshot</summary>

  ![File Systems](./doc_images/omv_filesystems.png)
</details>

2. Select <strong>Create and Mount</strong>.
<details>
  <summary>View screenshot</summary>

  ![Create and Mount](./doc_images/omv_create_and_mount.png)
</details>

3. Select <strong>EXT4</strong> as the filesystem.
<details>
  <summary>View screenshot</summary>

  ![Select EXT4](./doc_images/omv_select_ext4.png)
</details>

4. Click <strong>Select a device</strong> dropdown and select the Hard Drive.
<details>
  <summary>View screenshot</summary>

  ![Select Hard Drive](./doc_images/omv_select_a_device.png)
</details>

5. Select <strong>Save</strong>.
<details>
  <summary>View screenshot</summary>

  ![Select Save](./doc_images/omv_select_save.png)
</details>

6. Allow the Partitioning to Complete.
<details>
  <summary>View screenshot</summary>

  ![Partitioning](./doc_images/omv_partitioning.png)
</details>

7. Go to <strong>File System</strong>.
<details>
  <summary>View screenshot</summary>

  ![Select Filesystem](./doc_images/omv_select_filesystem.png)
</details>

8. Click <strong>Mount</strong>.
<details>
  <summary>View screenshot</summary>

  ![Click Mount](./doc_images/omv_mount.png
</details>

9. Click <strong>Save</strong> then <strong>Apply</strong> at the top to save your configurations.
<details>
  <summary>View screenshot</summary>

  ![Click Save](./doc_images/omv_click_save.png)
</details>

<details>
  <summary>View screenshot</summary>

  ![Click Apply](./doc_images/omv_click_apply.png)
</details>



