# How User Space Starts

## 1. Introduction

### 1.1 Overview
- User space starts after the kernel initializes the system and starts the first user-space process, `init`.
- The startup sequence is roughly:
  1. `init` process starts.
  2. Essential low-level services (e.g., `udevd`, `syslogd`) initialize.
  3. Network configuration occurs.
  4. Mid- and high-level services (e.g., `cron`, printing) start.
  5. Login prompts, GUIs, and high-level applications initialize.

---

## 2. Identifying Your `init`

### 2.1 Checking the `init` System
- **Objective**: Identify the `init` system in use.
- **Commands**:
  - Check the `init` process:
    ```bash
    ps -p 1 -o comm=
    ```
  - Alternatively, use:
    ```bash
    systemctl
    ```
    If the output shows `systemd`, then the system is using systemd.

- **Notes**:
  - Most modern Linux distributions use `systemd`.
  - Older systems may use System V (`sysvinit`) or Upstart.

---

## 3. systemd

### 3.1 Overview
- `systemd` is the standard `init` system on most modern Linux distributions.
- It manages services and system initialization using units.

### 3.2 Units and Unit Types
- **Objective**: Understand systemd units.
- **Types of Units**:
  - **Service Units** (`.service`): Start, stop, and manage services.
  - **Target Units** (`.target`): Group services to achieve a system state.
  - **Mount Units** (`.mount`): Control filesystem mounting.
  - **Timer Units** (`.timer`): Schedule tasks.

### 3.3 Viewing Units and Status
- **Objective**: List and check the status of systemd units.
- **Commands**:
  - List all units:
    ```bash
    systemctl list-units
    ```
  - List failed units:
    ```bash
    systemctl --failed
    ```
  - Check the status of a specific unit:
    ```bash
    systemctl status <unit_name>
    ```

### 3.4 Managing Units
- **Objective**: Start, stop, enable, and disable units.
- **Commands**:
  - Start a service:
    ```bash
    sudo systemctl start <service_name>
    ```
  - Stop a service:
    ```bash
    sudo systemctl stop <service_name>
    ```
  - Enable a service to start at boot:
    ```bash
    sudo systemctl enable <service_name>
    ```
  - Disable a service:
    ```bash
    sudo systemctl disable <service_name>
    ```
  - Restart a service:
    ```bash
    sudo systemctl restart <service_name>
    ```

---

## 4. systemd Configuration

### 4.1 Viewing Configuration Files
- **Objective**: Locate and view systemd configuration files.
- **Locations**:
  - System units: `/lib/systemd/system/`
  - Custom units: `/etc/systemd/system/`
- **Commands**:
  - View a unit file:
    ```bash
    cat /lib/systemd/system/<unit_name>.service
    ```
  - Check unit dependencies:
    ```bash
    systemctl list-dependencies <unit_name>
    ```

### 4.2 Editing and Reloading Configuration
- **Objective**: Edit unit files and apply changes.
- **Steps**:
  1. Edit a unit file:
      ```bash
      sudo nano /etc/systemd/system/<unit_name>.service
      ```
  2. Save changes and reload systemd:
      ```bash
      sudo systemctl daemon-reload
      ```
  3. Restart the service:
      ```bash
      sudo systemctl restart <unit_name>
      ```

---

## 5. System V Runlevels and systemd Targets

### 5.1 Runlevels in System V
- **Objective**: Understand traditional System V runlevels.
- **Runlevels**:
  - `0`: Halt (shuts down the system)
  - `1`: Single-user mode (rescue mode)
  - `3`: Multi-user mode (no GUI)
  - `5`: Multi-user mode with GUI
  - `6`: Reboot
- **Command**:
  ```bash
  who -r
  ```

### 5.2 systemd Targets
- **Objective**: Map runlevels to systemd targets.
- **Target Equivalents**:
  - `runlevel 0` → `poweroff.target`
  - `runlevel 1` → `rescue.target`
  - `runlevel 3` → `multi-user.target`
  - `runlevel 5` → `graphical.target`
  - `runlevel 6` → `reboot.target`
- **Commands**:
  - Change to a target:
    ```bash
    sudo systemctl isolate <target>
    ```
  - Set the default target:
    ```bash
    sudo systemctl set-default <target>
    ```
  - View the default target:
    ```bash
    systemctl get-default
    ```

---

## 6. Shutting Down and Restarting

### 6.1 systemd Commands
- **Objective**: Shutdown and restart the system.
- **Commands**:
  - Shut down immediately:
    ```bash
    sudo systemctl poweroff
    ```
  - Reboot immediately:
    ```bash
    sudo systemctl reboot
    ```
  - Schedule a shutdown:
    ```bash
    sudo shutdown -h 10 "System maintenance"
    ```
    (Shutdown in 10 minutes with a message)

---

## 7. The Initial RAM Filesystem (initramfs)

### 7.1 Overview
- **Objective**: Understand initramfs.
- `initramfs` is a temporary root filesystem loaded into memory during boot.
- It preloads drivers and modules needed to mount the real root filesystem.

### 7.2 Viewing initramfs Contents
- **Commands**:
  - Locate the initramfs image:
    ```bash
    ls /boot/initramfs-*.img
    ```
  - Unpack the image (using `unmkinitramfs` or `lsinitrd`):
    ```bash
    lsinitrd /boot/initramfs-<version>.img
    ```

### 7.3 Rebuilding initramfs
- **Objective**: Rebuild the initramfs image after modifying drivers or kernel modules.
- **Commands**:
  - For Debian/Ubuntu:
    ```bash
    sudo update-initramfs -u
    ```
  - For Red Hat/CentOS:
    ```bash
    sudo dracut -f
    ```

---

## 8. Emergency Booting and Single-User Mode

### 8.1 Single-User Mode
- **Objective**: Boot into a minimal rescue environment.
- **Steps**:
  1. Reboot the system and access the GRUB menu.
  2. Edit the boot entry by pressing `e`.
  3. Append `systemd.unit=rescue.target` to the `linux` line.
  4. Press `Ctrl+X` to boot.

### 8.2 Emergency Mode
- **Objective**: Boot into a minimal environment for critical repair.
- **Steps**:
  1. Access the GRUB menu.
  2. Edit the boot entry and add:
      ```
      systemd.unit=emergency.target
      ```
  3. Boot with `Ctrl+X`.

---

## 9. Practical Exercises

### Exercise 1: Managing systemd Units
1. List all active units using:
    ```bash
    systemctl list-units
    ```
2. Stop and disable a service, then enable and start it.

### Exercise 2: Switching Targets
1. Check the current target.
2. Switch to `multi-user.target` and back to `graphical.target`.

### Exercise 3: Rebuilding initramfs
1. Rebuild the initramfs image.
2. Reboot and verify the changes.