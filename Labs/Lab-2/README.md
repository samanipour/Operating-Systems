# Lab-2: Devices

## 1. Introduction to Devices in Linux

### 1.1 Overview
- Linux treats most devices as files, known as device nodes, typically located in the `/dev` directory. 
- Device files allow user programs to interact with hardware devices using standard file operations.

---

## 2. Device Files

### 2.1 Viewing Device Files
- **Objective**: View device files and their details.
- **Commands**:
  - List all device files in `/dev`:
    ```bash
    ls /dev
    ```
  - Display detailed information about device files:
    ```bash
    ls -l /dev
    ```
  - Example output:
    ```
    brw-rw----  1 root disk 8, 1 Sep 6 08:37 sda1
    crw-rw-rw-  1 root root 1, 3 Sep 6 08:37 null
    ```
    - `b`: Block device (e.g., hard disks)
    - `c`: Character device (e.g., serial ports)
    - `p`: Pipe (e.g., named pipes)
    - `s`: Socket (e.g., network communication)

### 2.2 Types of Device Files
- **Block Devices**:
  - Access data in fixed chunks (e.g., `/dev/sda1`).
  - Allow random access to data blocks.
- **Character Devices**:
  - Handle data streams one character at a time (e.g., `/dev/null`).
  - Cannot seek or perform random access.
- **Pipe Devices**:
  - Used for inter-process communication.
- **Socket Devices**:
  - Special interfaces for interprocess communication (e.g., network sockets).

---

## 3. sysfs Device Path
### 3.1 What is sysfs?
- **Virtual File System:**
  Sysfs is not a traditional file system stored on a physical disk. Instead, it's a RAM-based file system generated dynamically by the kernel.   

- **Kernel Interface:**
  It exposes information about the kernel's internal data structures, particularly those related to hardware devices and drivers.   

- **Hierarchical Structure:**
  Sysfs presents this information in a hierarchical directory structure, making it easy for user-space programs to access and interact with kernel data.   

- **Sysfs Paths:**
  - Location:
    Sysfs is typically mounted at /sys. Therefore, all sysfs paths begin with /sys/.
  - Purpose:
    These paths represent various kernel objects, devices, and drivers. By navigating through the directories and files within /sys/, you can:
    - Obtain information about hardware devices (e.g., their attributes, capabilities, and status).   
    - Configure certain device parameters.  
    - Monitor kernel activity.

- **Key Directories:**
  - /sys/devices/: This is a primary location where devices that are found by the kernel are represented. Within this directory you will find subdirectories that represent the hardware bus structure of the computer.   
  - /sys/class/: This directory organizes devices by their function or class (e.g., network devices, block devices, input devices). 
  - /sys/bus/: This directory organizes devices by the bus they are connected to (e.g., PCI, USB).
  - /sys/kernel/: This directory contains information and configuration options related to the kernel itself.

- **In essence:**
  - Sysfs paths provide a standardized way for user-space applications to communicate with the kernel and access hardware-related information.   
  - The hierarchical nature of sysfs makes it relatively easy to navigate and find the information you need.
### 3.2 sysfs Path Examples

1. `/sys/class/block/sda/size`

    - `/sys/class/block/`: This directory contains information about block devices (like hard drives or SSDs).

    - `sda/`: This subdirectory represents a specific block device, in this case, the first SCSI disk (or a disk emulated as SCSI). The device name might vary (e.g., nvme0n1 for an NVMe drive).

    - `size`: This file within the sda directory contains the size of the block device, expressed in 512-byte sectors. Reading this file will give you the disk's capacity.This path provides information about the size of a block device.

2. `/sys/class/net/eth0/address`

    - `/sys/class/net/`: This directory holds information about network interfaces.
    - `eth0/`: This subdirectory represents the first Ethernet interface. Again, the interface name can change (e.g., wlan0 for a Wi-Fi interface).
    - `address`: This file contains the MAC address of the network interface.
    This path provides the mac address of a network interface.

3. `/sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq`
    
    - `/sys/devices/system/cpu/cpu0/`: This leads to information about the first CPU core (CPU 0).
    - `cpufreq/`: This subdirectory contains files related to CPU frequency scaling.
    - `scaling_cur_freq`: This file shows the current operating frequency of the CPU core.
    This path shows the current cpu frequency.

4. `/sys/bus/pci/devices/0000:00:14.0/vendor`
    - `/sys/bus/pci/devices/`: This directory contains information about PCI devices.
    - `0000:00:14.0/`: This subdirectory represents a specific PCI device, identified by its bus, device, and function numbers.
    - `vendor`: This file contains the vendor ID of the PCI device. This path provides the vendor ID of a PCI device.

Key things to remember:
  - Sysfs is dynamic: The contents of /sys/ can change depending on the hardware and drivers present in the system.
  - Files as information: In sysfs, files are often used to represent device attributes. Reading a file gives you the attribute's value, and sometimes writing to a file allows you to change the attribute.
  - Hierarchical organization: The directory structure helps organize the vast amount of information exposed by the kernel.

### 3.3 Viewing sysfs Information
- **Objective**: Use `sysfs` to view detailed device attributes.
- **Commands**:
  - Display a device's `sysfs` path:
    ```bash
    udevadm info --query=path --name=/dev/sda
    ```
  - View details about a device:
    ```bash
    udevadm info --query=all --name=/dev/sda
    ```
  - List device attributes:
    ```bash
    ls -l /sys/class/block/sda
    ```
---

## 4. dd and Devices

### 4.1 Using `dd` for Device Operations
- **Objective**: Copy data between devices or files using `dd`.
- **Commands**:
  - Backup a disk to an image file:
    ```bash
    sudo dd if=/dev/sda of=/path/to/backup.img bs=4M status=progress
    ```
  - Restore a disk from an image file:
    ```bash
    sudo dd if=/path/to/backup.img of=/dev/sda bs=4M status=progress
    ```
  - Wipe a disk by overwriting with zeros:
    ```bash
    sudo dd if=/dev/zero of=/dev/sda bs=4M status=progress
    ```
- **Notes**:
  - `if=`: Input file (source)
  - `of=`: Output file (destination)
  - `bs=`: Block size
  - `status=progress`: Shows progress during execution

---

## 5. Device Name Summary

### 5.1 Common Device Names
- **Hard Disks**: `/dev/sd*` (e.g., `/dev/sda`, `/dev/sdb`)
- **Virtual Disks**: `/dev/xvd*`, `/dev/vd*` (used in virtual machines)
- **NVMe Devices**: `/dev/nvme*`
- **CD/DVD Drives**: `/dev/sr*`
- **Terminals**: `/dev/tty*`, `/dev/pts/*`
- **Audio Devices**: `/dev/snd/*`, `/dev/dsp`, `/dev/audio`

### 5.2 Checking Device Information
- List all storage devices:
  ```bash
  lsblk
  ```
- List all SCSI devices:
  ```bash
  lsscsi
  ```
- List NVMe devices:
  ```bash
  nvme list
  ```

---

## 6. udev

### 6.1 Understanding udev
- **udev** is a device manager for the Linux kernel that dynamically creates device nodes.
- **udevd** listens for kernel events (uevents) and manages `/dev` entries.

### 6.2 Checking udev Rules
- **Objective**: List and understand udev rules.
- **Commands**:
  - View default udev rules:
    ```bash
    ls /lib/udev/rules.d/
    ```
  - View custom udev rules:
    ```bash
    ls /etc/udev/rules.d/
    ```
  - Display a specific rule:
    ```bash
    cat /lib/udev/rules.d/60-persistent-storage.rules
    ```

### 6.3 Monitoring udev Events
- **Objective**: Monitor real-time udev events.
- **Commands**:
  - Monitor kernel events:
    ```bash
    sudo udevadm monitor --kernel
    ```
  - Monitor udev processing:
    ```bash
    sudo udevadm monitor --udev
    ```
  - Monitor events with attributes:
    ```bash
    sudo udevadm monitor --property
    ```

---

## 7. Device File Creation

### 7.1 Creating Device Files with mknod
- **Objective**: Create device files manually (for advanced use only).
- **Commands**:
  - Create a block device:
    ```bash
    sudo mknod /dev/myblock b 8 1
    ```
  - Create a character device:
    ```bash
    sudo mknod /dev/mychar c 1 3
    ```
  - **Notes**:
    - `b`: Block device
    - `c`: Character device
    - Major and minor numbers are required.

---

## 8. Practical Exercises

### Exercise 1: Exploring Device Files
1. List all device files in `/dev`.
2. Identify block and character devices using `ls -l`.
3. Check the sysfs path of a storage device using `udevadm`.

### Exercise 2: Using dd for Backup and Restore
1. Create a backup of `/dev/sda` to an image file.
2. Verify the backup using `ls -lh`.
3. Restore the image to the same device.

### Exercise 3: Monitoring udev Events
1. Open a terminal and run:
    ```bash
    sudo udevadm monitor --udev
    ```
2. Plug in a USB flash drive.
3. Observe and document the events displayed.

### Exercise 4: Creating a Custom Device Node
1. Create a custom character device using `mknod`.
2. Confirm its creation with `ls -l`.
3. Remove the device file:
    ```bash
    sudo rm /dev/mychar
    ```