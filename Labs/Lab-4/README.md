# How the Linux Kernel Boots

## 1. Introduction

### 1.1 Overview
- This manual explains the Linux boot process, from powering on the machine to starting the user space.
- A simplified sequence:
  1. BIOS or UEFI firmware loads and runs the boot loader.
  2. The boot loader locates and loads the kernel into memory.
  3. The kernel initializes devices and drivers.
  4. The kernel mounts the root filesystem.
  5. The kernel starts the `init` process (PID 1), marking the user space start.

---

## 2. Startup Messages

### 2.1 Viewing Startup Messages
- **Objective**: View boot and runtime diagnostic messages.
- **Commands**:
  - If using systemd, use `journalctl` to view kernel messages:
    ```bash
    sudo journalctl -k
    ```
  - View messages from a previous boot:
    ```bash
    sudo journalctl -k -b -1
    ```
  - If systemd is not available, check logs:
    ```bash
    cat /var/log/kern.log
    ```
  - Alternatively, use `dmesg` to display the kernel ring buffer:
    ```bash
    dmesg
    ```

### 2.2 Understanding Startup Messages
- Kernel messages include:
  - Hardware detection (e.g., CPU, RAM, storage devices)
  - Driver initialization
  - Filesystem mounting details
- Example output:
  ```
  Linux version 4.15.0-112-generic
  Command line: BOOT_IMAGE=/boot/vmlinuz-4.15.0-112-generic root=UUID=... ro quiet splash
  ```
- `quiet` and `splash` parameters control display verbosity and graphical boot screens.

---

## 3. Kernel Initialization and Boot Options

### 3.1 Initialization Order
- **Objective**: Understand the kernel's initialization order.
- **Sequence**:
  1. CPU and memory inspection
  2. Device bus discovery
  3. Device discovery and driver initialization
  4. Auxiliary subsystem setup (e.g., networking)
  5. Root filesystem mounting
  6. User space start by launching `init`

### 3.2 Kernel Dependencies
- Device drivers may depend on other kernel modules (e.g., disk drivers require bus support).
- In some systems, kernel modules must be loaded before mounting the root filesystem.
- Initial RAM filesystem (`initrd` or `initramfs`) is used to pre-load required modules.

---

## 4. Kernel Parameters

### 4.1 Viewing Kernel Parameters
- **Objective**: Display the current kernel parameters.
- **Commands**:
  ```bash
  cat /proc/cmdline
  ```
- Example output:
  ```
  BOOT_IMAGE=/boot/vmlinuz-4.15.0-43-generic root=UUID=... ro quiet splash vt.handoff=1
  ```

### 4.2 Common Parameters
- `root`: Location of the root filesystem (e.g., `root=UUID=...`)
- `ro`: Mounts the root filesystem as read-only initially.
- `quiet`: Suppresses most boot messages.
- `splash`: Displays a graphical splash screen during boot.
- `-s`: Single-user mode (passed to `init` for maintenance tasks).

### 4.3 Modifying Kernel Parameters
- **Objective**: Temporarily edit kernel parameters for troubleshooting.
- **Steps**:
  1. Reboot the system.
  2. At the GRUB menu, select the kernel and press `e`.
  3. Find the line starting with `linux` and edit the parameters (e.g., remove `quiet` to see detailed messages).
  4. Press `Ctrl+X` to boot with the modified parameters.

---

## 5. Boot Loaders

### 5.1 Overview
- Boot loaders load the kernel into memory and pass kernel parameters.
- They allow:
  - Selecting from multiple kernels.
  - Switching kernel parameters.
  - Booting other operating systems.
- Common Linux boot loaders:
  - GRUB (Grand Unified Boot Loader)
  - systemd-boot
  - SYSLINUX
  - LILO (Legacy)
  - coreboot (formerly LinuxBIOS)

### 5.2 Checking the Boot Loader
- **Objective**: Identify the boot loader in use.
- **Commands**:
  - Check if GRUB is used:
    ```bash
    sudo grub-install --version
    ```
  - Check if systemd-boot is used:
    ```bash
    bootctl status
    ```

---

## 6. GRUB (Grand Unified Boot Loader)

### 6.1 Accessing GRUB Menu
- **Objective**: Access and navigate the GRUB menu.
- **Steps**:
  1. Reboot the system.
  2. During startup:
     - Hold `Shift` (for BIOS systems).
     - Press `Esc` (for UEFI systems).
  3. Use arrow keys to navigate the menu.

### 6.2 Editing GRUB Entries
- **Objective**: Edit boot parameters temporarily.
- **Steps**:
  1. In the GRUB menu, select the desired entry and press `e`.
  2. Edit the `linux` line, adding or modifying parameters.
  3. Press `Ctrl+X` or `F10` to boot with the edited parameters.

### 6.3 Persisting GRUB Changes
- **Objective**: Make permanent changes to GRUB settings.
- **Steps**:
  1. Edit the GRUB configuration file:
    ```bash
    sudo nano /etc/default/grub
    ```
  2. Modify parameters in the `GRUB_CMDLINE_LINUX` line.
  3. Save the file and update GRUB:
    ```bash
    sudo update-grub
    ```

### 6.4 Reinstalling GRUB
- **Objective**: Reinstall GRUB to the boot sector.
- **Commands**:
  ```bash
  sudo grub-install /dev/sda
  sudo update-grub
  ```

---

## 7. UEFI Secure Boot

### 7.1 Overview
- Secure Boot requires boot loaders to be digitally signed.
- If installing an unsigned boot loader, disable Secure Boot in UEFI settings.

### 7.2 Checking UEFI or BIOS
- **Objective**: Determine if the system uses UEFI or BIOS.
- **Commands**:
  ```bash
  ls /sys/firmware/efi
  ```
  - If the directory exists, the system uses UEFI.

---

## 8. Practical Exercises

### Exercise 1: Viewing Boot Messages
1. Reboot the system and view the startup messages using:
    ```bash
    sudo journalctl -k
    ```
2. Identify at least three types of messages (e.g., hardware detection, driver initialization).

### Exercise 2: Editing Kernel Parameters
1. Reboot and access the GRUB menu.
2. Edit the parameters to remove `quiet` and `splash`.
3. Boot and observe the detailed startup messages.

### Exercise 3: Reinstalling GRUB
1. Boot into a live Linux environment.
2. Mount the root filesystem:
    ```bash
    sudo mount /dev/sda1 /mnt
    ```
3. Reinstall GRUB:
    ```bash
    sudo grub-install --boot-directory=/mnt/boot /dev/sda
    ```
