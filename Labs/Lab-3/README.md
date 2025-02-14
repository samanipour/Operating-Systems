# Lab-3: Disks and Filesystems

## 1. Introduction

### 1.1 Overview
- Linux uses a layered system to access disk data, allowing interaction through filesystems or directly via device files.
- Partitions divide a disk into sections, each treated as a separate block device.

---

## 2. Partitioning Disk Devices

### 2.1 Viewing a Partition Table
- **Objective**: Display existing partition tables.
- **Commands**:
  - Using `parted`:
    ```bash
    sudo parted -l
    ```
  - Using `fdisk`:
    ```bash
    sudo fdisk -l
    ```
  - Example output:
    ```
    Disk /dev/sda: 240GB
    Sector size (logical/physical): 512B/512B
    Partition Table: msdos
    Number  Start   End    Size    Type      File system     Flags
     1      1049kB  223GB  223GB   primary   ext4            boot
     2      223GB   240GB  17.0GB  extended
     5      223GB   240GB  17.0GB  logical   linux-swap(v1)
    ```

### 2.2 Creating a Partition Table
- **Objective**: Create a new partition table.
- **Commands**:
  - Start `fdisk`:
    ```bash
    sudo fdisk /dev/sdx
    ```
    Replace `sdx` with the target disk identifier.
  - Create a new partition table:
    - Press `o` to create a new empty DOS partition table.
    - Press `g` for a new GPT partition table.
  - Verify changes:
    ```bash
    p
    ```
  - Save changes and exit:
    ```bash
    w
    ```

### 2.3 Creating Partitions
- **Objective**: Add partitions to a disk.
- **Commands**:
  - Start `fdisk`:
    ```bash
    sudo fdisk /dev/sdx
    ```
  - Create a new partition:
    - Press `n` to create a new partition.
    - Select type: `p` for primary, `e` for extended.
    - Assign partition number, starting, and ending sectors.
    - Use `+size` to specify partition size (e.g., `+200M` for 200MB).
  - View the partition layout:
    ```bash
    p
    ```
  - Write changes and exit:
    ```bash
    w
    ```

### 2.4 Deleting Partitions
- **Objective**: Remove existing partitions.
- **Commands**:
  - Start `fdisk`:
    ```bash
    sudo fdisk /dev/sdx
    ```
  - Delete a partition:
    - Press `d` and select the partition number.
  - View the updated layout:
    ```bash
    p
    ```
  - Save changes and exit:
    ```bash
    w
    ```

### 2.5 Verifying Partition Changes
- **Objective**: Confirm partition changes.
- **Commands**:
  ```bash
  sudo partprobe
  sudo fdisk -l
  ```

---

## 3. Filesystems

### 3.1 Creating a Filesystem
- **Objective**: Format a partition with a specific filesystem.
- **Commands**:
  - Format as ext4:
    ```bash
    sudo mkfs.ext4 /dev/sdx1
    ```
  - Format as xfs:
    ```bash
    sudo mkfs.xfs /dev/sdx1
    ```
  - Format as FAT32:
    ```bash
    sudo mkfs.vfat -F 32 /dev/sdx1
    ```

### 3.2 Mounting a Filesystem
- **Objective**: Make a filesystem accessible by mounting it to a directory.
- **Commands**:
  - Create a mount point:
    ```bash
    sudo mkdir /mnt/mydisk
    ```
  - Mount the partition:
    ```bash
    sudo mount /dev/sdx1 /mnt/mydisk
    ```
  - Verify the mount:
    ```bash
    df -h
    ```

### 3.3 Unmounting a Filesystem
- **Objective**: Safely detach a filesystem.
- **Commands**:
  ```bash
  sudo umount /mnt/mydisk
  ```

### 3.4 Permanent Mounts with /etc/fstab
- **Objective**: Automatically mount partitions at boot.
- **Steps**:
  1. Get the partition’s UUID:
    ```bash
    sudo blkid /dev/sdx1
    ```
  2. Edit the `/etc/fstab` file:
    ```bash
    sudo nano /etc/fstab
    ```
  3. Add a new entry:
    ```
    UUID=your-uuid-here  /mnt/mydisk  ext4  defaults  0  2
    ```
  4. Save and exit.
  5. Test the configuration:
    ```bash
    sudo mount -a
    ```

---

## 4. Checking and Repairing Filesystems

### 4.1 Checking Filesystem Integrity
- **Objective**: Check for filesystem errors.
- **Commands**:
  ```bash
  sudo fsck /dev/sdx1
  ```

### 4.2 Repairing Filesystems
- **Objective**: Repair filesystem issues.
- **Commands**:
  - Automatically fix issues:
    ```bash
    sudo fsck -y /dev/sdx1
    ```
  - Interactively fix issues:
    ```bash
    sudo fsck -r /dev/sdx1
    ```

---

## 5. Swap Space

### 5.1 Creating a Swap Partition
- **Objective**: Set up a partition for swap space.
- **Commands**:
  - Format as swap:
    ```bash
    sudo mkswap /dev/sdx2
    ```
  - Activate the swap partition:
    ```bash
    sudo swapon /dev/sdx2
    ```
  - Verify swap status:
    ```bash
    swapon --show
    ```

### 5.2 Adding Swap to /etc/fstab
- **Objective**: Automatically activate swap at boot.
- **Steps**:
  1. Get the UUID:
    ```bash
    sudo blkid /dev/sdx2
    ```
  2. Edit the `/etc/fstab` file:
    ```bash
    sudo nano /etc/fstab
    ```
  3. Add the following line:
    ```
    UUID=your-uuid-here  none  swap  sw  0  0
    ```
  4. Save and exit.
  5. Activate the swap:
    ```bash
    sudo swapon -a
    ```

---

## 6. Practical Exercises

### Exercise 1: Partitioning and Formatting
1. Create a new partition of 1GB.
2. Format it as ext4.
3. Mount it to `/mnt/labdisk`.
4. Verify with `df -h`.

### Exercise 2: Creating and Using Swap Space
1. Create a 512MB swap partition.
2. Activate it and verify using `swapon --show`.
3. Add it to `/etc/fstab`.

### Exercise 3: Checking and Repairing Filesystems
1. Check the integrity of a partition using `fsck`.
2. Repair any detected issues.