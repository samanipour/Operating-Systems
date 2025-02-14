# Network Applications and Services

---

## 1. Introduction

### 1.1 Overview
- This manual explains the configuration and management of network applications and services in Linux.
- By the end of this lab, students will be able to:
  - Understand the basics of network services.
  - Configure network servers such as SSH.
  - Use diagnostic tools to troubleshoot network issues.
  - Understand network security and sockets.

---

## 2. The Basics of Services

### 2.1 Understanding TCP Services
- **Objective**: Explore how TCP services work using telnet.
- **Explanation**:
  - TCP services provide uninterrupted two-way data streams.
  - HTTP (Hypertext Transfer Protocol) uses TCP port 80 for communication.

### 2.2 Using `telnet` to Connect to a Web Server
- **Objective**: Manually connect to a web server using TCP.
- **Commands**:
  ```bash
  telnet example.org 80
  ```
- **Steps**:
  1. Open a terminal.
  2. Connect to the IANA example web server on TCP port 80:
      ```bash
      telnet example.org 80
      ```
  3. Once connected, type the following:
      ```
      GET / HTTP/1.1
      Host: example.org
      ```
      (Press ENTER twice)
  4. Observe the HTML response from the server.
  5. Terminate the connection:
      - Press `CTRL+D`

---

## 3. A Closer Look with `curl`

### 3.1 Using `curl` to Communicate with HTTP Servers
- **Objective**: Examine HTTP communication using `curl`.
- **Commands**:
  ```bash
  curl --trace-ascii trace_file http://www.example.org/
  ```
- **Steps**:
  1. Install `curl` if not already installed:
      ```bash
      sudo apt install curl
      ```
  2. Send a request to the web server:
      ```bash
      curl --trace-ascii trace_file http://www.example.org/
      ```
  3. Open the trace file to view the request and response:
      ```bash
      cat trace_file
      ```
  4. Observe the request headers and server response.

---

## 4. Network Servers

### 4.1 Secure Shell (SSH)

#### 4.1.1 Installing and Enabling SSH Server
- **Objective**: Install and enable OpenSSH server.
- **Commands**:
  - Install SSH server:
    ```bash
    sudo apt install openssh-server
    ```
  - Enable and start the SSH service:
    ```bash
    sudo systemctl enable ssh
    sudo systemctl start ssh
    ```
  - Verify the SSH service status:
    ```bash
    sudo systemctl status ssh
    ```

#### 4.1.2 Configuring SSH Server
- **Objective**: Modify SSH server settings for secure access.
- **Steps**:
  1. Edit the SSH configuration file:
      ```bash
      sudo nano /etc/ssh/sshd_config
      ```
  2. Recommended settings:
      - Disable root login:
        ```
        PermitRootLogin no
        ```
      - Specify allowed users:
        ```
        AllowUsers username
        ```
  3. Save and close the file.
  4. Restart SSH to apply changes:
      ```bash
      sudo systemctl restart ssh
      ```

#### 4.1.3 Connecting to SSH Server
- **Objective**: Connect to the SSH server using the client.
- **Commands**:
  ```bash
  ssh username@hostname
  ```
- **Example**:
  ```bash
  ssh user@example.org
  ```

### 4.2 Using `fail2ban` for SSH Security
- **Objective**: Protect SSH server from brute-force attacks.
- **Commands**:
  - Install fail2ban:
    ```bash
    sudo apt install fail2ban
    ```
  - Enable and start fail2ban:
    ```bash
    sudo systemctl enable fail2ban
    sudo systemctl start fail2ban
    ```
  - View fail2ban status:
    ```bash
    sudo fail2ban-client status
    ```
  - View SSH jail status:
    ```bash
    sudo fail2ban-client status sshd
    ```

---

## 5. Diagnostic Tools

### 5.1 Using `lsof` to List Open Files and Ports
- **Objective**: List open network ports and connections.
- **Commands**:
  - List all open ports:
    ```bash
    sudo lsof -i
    ```
  - List open ports for a specific service:
    ```bash
    sudo lsof -i :22
    ```

### 5.2 Using `tcpdump` for Packet Analysis
- **Objective**: Capture and analyze network packets.
- **Commands**:
  - Install `tcpdump`:
    ```bash
    sudo apt install tcpdump
    ```
  - Capture packets on a network interface:
    ```bash
    sudo tcpdump -i enp0s3
    ```
  - Capture packets on a specific port:
    ```bash
    sudo tcpdump -i enp0s3 port 80
    ```

### 5.3 Using `netcat` for Network Connections
- **Objective**: Create network connections and send data.
- **Commands**:
  - Start a listener on a port:
    ```bash
    nc -l 1234
    ```
  - Connect to the listener:
    ```bash
    nc <host-ip> 1234
    ```

### 5.4 Port Scanning with `nmap`
- **Objective**: Scan a network for open ports.
- **Commands**:
  - Install `nmap`:
    ```bash
    sudo apt install nmap
    ```
  - Scan a host for open ports:
    ```bash
    nmap <host-ip>
    ```
  - Perform a detailed scan:
    ```bash
    nmap -A <host-ip>
    ```

---

## 6. Network Security

### 6.1 Identifying Vulnerabilities
- **Objective**: Identify common network vulnerabilities.
- **Types of Vulnerabilities**:
  - Direct attacks (e.g., buffer overflow).
  - Cleartext password sniffing.
  - Unauthenticated services.

### 6.2 Securing Network Services
- **Objective**: Harden network services against attacks.
- **Best Practices**:
  - Disable unnecessary services:
    ```bash
    sudo systemctl disable service_name
    ```
  - Use SSH instead of Telnet.
  - Apply firewall rules:
    ```bash
    sudo ufw enable
    sudo ufw allow ssh
    sudo ufw deny telnet
    ```

---

## 7. Practical Exercises

### Exercise 1: Connecting to a Web Server
1. Connect to a web server using `telnet`.
2. Send a GET request manually.
3. Observe the server response.

### Exercise 2: Configuring and Using SSH
1. Install and enable OpenSSH server.
2. Secure the SSH server by disabling root login.
3. Connect to the SSH server from another machine.

### Exercise 3: Using Diagnostic Tools
1. Monitor open ports with `lsof`.
2. Capture network packets using `tcpdump`.
3. Perform a network scan with `nmap`.