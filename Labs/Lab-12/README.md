# Lab-8: Understanding Your Network and Its Configuration

---

## 1. Introduction

### 1.1 Overview
- This manual explains the fundamentals of Linux networking, including:
  - Viewing network configuration
  - Managing IP addresses
  - Configuring routing
  - Understanding network layers
  - Using basic network diagnostic tools
- By the end of this lab, students will be able to analyze and configure Linux network interfaces and troubleshoot network connectivity.

---

## 2. Network Basics

### 2.1 Checking Network Interfaces
- **Objective**: List all active and inactive network interfaces.
- **Commands**:
  - Using `ip`:
    ```bash
    ip link show
    ```
  - Using `ifconfig` (older systems):
    ```bash
    ifconfig -a
    ```
  - Using `nmcli` (for NetworkManager systems):
    ```bash
    nmcli device status
    ```

### 2.2 Viewing Interface Details
- **Objective**: Display detailed information about network interfaces.
- **Commands**:
  ```bash
  ip addr show
  ip -s link show
  ```
  - Example output:
    ```
    2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 08:00:27:4e:7a:1b brd ff:ff:ff:ff:ff:ff
        inet 192.168.1.100/24 brd 192.168.1.255 scope global dynamic enp0s3
           valid_lft 86394sec preferred_lft 86394sec
    ```

---

## 3. IP Addresses

### 3.1 Viewing IP Addresses
- **Objective**: Display the IP address configuration of all interfaces.
- **Commands**:
  ```bash
  ip addr show
  ```
  - Or, using the older method:
    ```bash
    ifconfig
    ```

### 3.2 Adding an IP Address
- **Objective**: Assign a new IP address to an interface.
- **Commands**:
  ```bash
  sudo ip addr add 192.168.1.10/24 dev enp0s3
  ```

### 3.3 Deleting an IP Address
- **Objective**: Remove an assigned IP address.
- **Commands**:
  ```bash
  sudo ip addr del 192.168.1.10/24 dev enp0s3
  ```

### 3.4 Enabling and Disabling Interfaces
- **Objective**: Bring an interface up or down.
- **Commands**:
  - Bring up the interface:
    ```bash
    sudo ip link set enp0s3 up
    ```
  - Bring down the interface:
    ```bash
    sudo ip link set enp0s3 down
    ```

---

## 4. Subnets and Routing

### 4.1 Understanding Subnets
- **Explanation**:
  - Subnet masks define the network and host portions of an IP address.
  - Example:
    - IP Address: `192.168.1.10`
    - Subnet Mask: `255.255.255.0` or `/24`
    - Network Address: `192.168.1.0`

### 4.2 Viewing the Routing Table
- **Objective**: Display the current routing table.
- **Commands**:
  ```bash
  ip route show
  ```
  - Or:
    ```bash
    route -n
    ```

### 4.3 Adding a Route
- **Objective**: Add a new route to a network.
- **Commands**:
  ```bash
  sudo ip route add 192.168.2.0/24 via 192.168.1.1 dev enp0s3
  ```

### 4.4 Deleting a Route
- **Objective**: Remove an existing route.
- **Commands**:
  ```bash
  sudo ip route del 192.168.2.0/24
  ```

### 4.5 Setting the Default Gateway
- **Objective**: Configure the default gateway.
- **Commands**:
  ```bash
  sudo ip route add default via 192.168.1.1 dev enp0s3
  ```

---

## 5. IPv6 Configuration

### 5.1 Viewing IPv6 Addresses
- **Objective**: Display IPv6 configuration.
- **Commands**:
  ```bash
  ip -6 addr show
  ```

### 5.2 Adding an IPv6 Address
- **Objective**: Assign an IPv6 address to an interface.
- **Commands**:
  ```bash
  sudo ip -6 addr add 2001:db8::1/64 dev enp0s3
  ```

### 5.3 Enabling IPv6 Forwarding
- **Objective**: Allow IPv6 traffic forwarding.
- **Commands**:
  ```bash
  sudo sysctl -w net.ipv6.conf.all.forwarding=1
  ```

---

## 6. Basic ICMP and DNS Tools

### 6.1 Testing Connectivity with ping
- **Objective**: Check connectivity to a host.
- **Commands**:
  - IPv4:
    ```bash
    ping 8.8.8.8
    ```
  - IPv6:
    ```bash
    ping6 google.com
    ```

### 6.2 Checking DNS Resolution
- **Objective**: Verify DNS name resolution.
- **Commands**:
  ```bash
  host google.com
  nslookup google.com
  dig google.com
  ```

---

## 7. Network Interface Configuration

### 7.1 Manually Configuring Interfaces
- **Objective**: Configure network interfaces manually.
- **Commands**:
  - Edit the configuration file:
    ```bash
    sudo nano /etc/network/interfaces
    ```
  - Example configuration:
    ```
    auto enp0s3
    iface enp0s3 inet static
        address 192.168.1.50
        netmask 255.255.255.0
        gateway 192.168.1.1
    ```
  - Restart the network service:
    ```bash
    sudo systemctl restart networking
    ```

### 7.2 Adding and Deleting Routes
- **Objective**: Manage static routes manually.
- **Commands**:
  - Add a route:
    ```bash
    sudo route add -net 10.0.0.0/24 gw 192.168.1.1 dev enp0s3
    ```
  - Delete a route:
    ```bash
    sudo route del -net 10.0.0.0/24 dev enp0s3
    ```

---

## 8. Practical Exercises

### Exercise 1: Basic IP Configuration
1. View the current IP configuration using `ip addr show`.
2. Add a new IP address to an interface.
3. Test connectivity with `ping`.

### Exercise 2: Static Routing
1. View the current routing table.
2. Add a new route to a different network.
3. Verify the route using `traceroute`.

### Exercise 3: DNS Resolution
1. Test DNS resolution using `nslookup`.
2. Change the DNS server in `/etc/resolv.conf`.
3. Verify with `dig`.