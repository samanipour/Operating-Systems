# Lab-11: Network File Transfer and Sharing

---

## 1. Introduction

### 1.1 Overview
- This manual covers methods for distributing and sharing files over a network, including:
  - Temporary file sharing using Python’s SimpleHTTPServer.
  - File synchronization with `rsync`.
  - File sharing with Samba for Windows-Linux interoperability.
  - Mounting remote filesystems using SSHFS and NFS.
- By the end of this lab, students will be able to:
  - Set up temporary and persistent file-sharing solutions.
  - Securely mount remote filesystems on a local machine.
  - Troubleshoot common file-sharing issues.

---

## 2. Temporary File Sharing with Python SimpleHTTPServer

### 2.1 Starting a Temporary HTTP Server
- **Objective**: Quickly share files from a directory using HTTP.
- **Commands**:
  - Using Python 3:
    ```bash
    python3 -m http.server 8080
    ```
  - Using Python 2 (if applicable):
    ```bash
    python -m SimpleHTTPServer 8080
    ```
- **Explanation**:
  - This starts an HTTP server in the current directory on port 8080.
  - Files in this directory are accessible from other devices on the network by navigating to:
    ```
    http://<your_ip>:8080
    ```
  - Replace `<your_ip>` with your machine’s IP address, which can be found using:
    ```bash
    ip addr show
    ```

### 2.2 Stopping the HTTP Server
- **Objective**: Stop the temporary HTTP server.
- **Steps**:
  - Press `CTRL+C` in the terminal where the server is running.
  - Alternatively, find and kill the process:
    ```bash
    ps aux | grep http.server
    sudo kill <PID>
    ```

---

## 3. Distributing Files with `rsync`

### 3.1 Installing `rsync`
- **Objective**: Install `rsync` on both local and remote machines.
- **Commands**:
  - For Debian/Ubuntu:
    ```bash
    sudo apt install rsync
    ```
  - For CentOS/Red Hat:
    ```bash
    sudo yum install rsync
    ```

### 3.2 Using `rsync` for Local Backups
- **Objective**: Sync files between local directories.
- **Commands**:
  ```bash
  rsync -av --delete /source/directory/ /backup/directory/
  ```
- **Explanation**:
  - `-a`: Archive mode (preserves permissions, symlinks, etc.).
  - `-v`: Verbose output.
  - `--delete`: Deletes files in the destination that are not in the source.

### 3.3 Syncing Files Over SSH
- **Objective**: Sync files to a remote server over SSH.
- **Commands**:
  ```bash
  rsync -av -e ssh /local/directory/ user@remote_host:/remote/directory/
  ```
- **Explanation**:
  - `-e ssh`: Uses SSH as the transport protocol.
  - Ensure SSH access to the remote machine is set up.

### 3.4 Using `rsync` for Incremental Backups
- **Objective**: Perform incremental backups to save bandwidth and time.
- **Commands**:
  ```bash
  rsync -av --link-dest=/previous/backup/ /source/directory/ /new/backup/
  ```
- **Explanation**:
  - `--link-dest`: Links unchanged files to save space.

---

## 4. File Sharing with Samba

### 4.1 Installing Samba
- **Objective**: Install Samba on a Linux server for sharing files with Windows clients.
- **Commands**:
  - On Debian/Ubuntu:
    ```bash
    sudo apt install samba
    ```
  - On CentOS/Red Hat:
    ```bash
    sudo yum install samba samba-common
    ```

### 4.2 Configuring a Samba Share
- **Objective**: Share a directory with Windows clients.
- **Steps**:
  1. Edit the Samba configuration file:
      ```bash
      sudo nano /etc/samba/smb.conf
      ```
  2. Add the following configuration:
      ```ini
      [SharedFiles]
      path = /path/to/shared/directory
      browseable = yes
      writable = yes
      guest ok = no
      valid users = username
      ```
  3. Save and exit the editor.

### 4.3 Setting Permissions and Creating Samba User
- **Objective**: Set permissions and create a Samba user.
- **Commands**:
  - Set directory permissions:
    ```bash
    sudo chown -R username:username /path/to/shared/directory
    sudo chmod -R 775 /path/to/shared/directory
    ```
  - Create a Samba user:
    ```bash
    sudo smbpasswd -a username
    ```
  - Enable and start the Samba service:
    ```bash
    sudo systemctl enable smbd
    sudo systemctl start smbd
    ```

### 4.4 Accessing the Samba Share from Windows
- **Objective**: Connect to the Samba share from a Windows client.
- **Steps**:
  1. Open File Explorer.
  2. In the address bar, enter:
      ```
      \\<server_ip>\SharedFiles
      ```
  3. Enter the Samba username and password when prompted.

---

## 5. Mounting Remote Filesystems with SSHFS

### 5.1 Installing SSHFS
- **Objective**: Install SSHFS on the client machine.
- **Commands**:
  - On Debian/Ubuntu:
    ```bash
    sudo apt install sshfs
    ```

### 5.2 Mounting a Remote Directory
- **Objective**: Mount a remote directory over SSH.
- **Commands**:
  ```bash
  mkdir ~/remote_mount
  sshfs user@remote_host:/remote/directory ~/remote_mount
  ```
- **Explanation**:
  - `user@remote_host`: The SSH username and hostname/IP of the remote machine.
  - `~/remote_mount`: Local mount point.

### 5.3 Unmounting the SSHFS Directory
- **Objective**: Unmount the SSHFS filesystem.
- **Commands**:
  ```bash
  fusermount -u ~/remote_mount
  ```

---

## 6. Sharing Files with NFS

### 6.1 Installing NFS
- **Objective**: Install NFS on both server and client.
- **Commands**:
  - On Debian/Ubuntu:
    ```bash
    sudo apt install nfs-kernel-server nfs-common
    ```
  - On CentOS/Red Hat:
    ```bash
    sudo yum install nfs-utils
    ```

### 6.2 Configuring NFS Exports
- **Objective**: Configure NFS exports on the server.
- **Steps**:
  1. Edit the NFS exports file:
      ```bash
      sudo nano /etc/exports
      ```
  2. Add the following line:
      ```
      /path/to/share client_ip(rw,sync,no_subtree_check)
      ```
  3. Save and exit the editor.
  4. Restart the NFS service:
      ```bash
      sudo systemctl restart nfs-kernel-server
      ```

### 6.3 Mounting NFS Shares on the Client
- **Objective**: Mount the NFS share on the client.
- **Commands**:
  ```bash
  sudo mount server_ip:/path/to/share /local/mount/point
  ```
  - To make it persistent, add the following line to `/etc/fstab`:
    ```
    server_ip:/path/to/share /local/mount/point nfs defaults 0 0
    ```

---

## 7. Practical Exercises

### Exercise 1: Temporary File Sharing
1. Start a temporary HTTP server using Python.
2. Access the shared files from another device on the network.

### Exercise 2: Synchronizing Files with `rsync`
1. Sync files from a local directory to a remote server.
2. Perform an incremental backup.

### Exercise 3: Setting Up a Samba Share
1. Configure a Samba share.
2. Access the share from a Windows client.

### Exercise 4: Mounting Remote Filesystems
1. Mount a remote directory using SSHFS.
2. Unmount the directory and verify the changes.