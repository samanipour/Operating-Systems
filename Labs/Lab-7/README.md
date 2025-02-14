# A Closer Look at Processes and Resource Utilization

---

## 1. Introduction

### 1.1 Overview
- In Linux, processes compete for system resources such as CPU, memory, and I/O.
- The kernel manages resource allocation to maintain system stability and performance.
- This manual covers:
  - Tracking processes
  - Viewing open files
  - Tracing program execution
  - Monitoring resource utilization
  - Managing control groups (cgroups)

---

## 2. Tracking Processes

### 2.1 Using `ps` to List Processes
- **Objective**: View running processes and their details.
- **Commands**:
  - List all running processes:
    ```bash
    ps -ef
    ```
  - View a tree of processes:
    ```bash
    ps -e --forest
    ```
  - Display detailed information:
    ```bash
    ps aux
    ```

### 2.2 Monitoring Processes with `top`
- **Objective**: Monitor real-time system performance and resource usage.
- **Commands**:
  - Start the `top` interface:
    ```bash
    top
    ```
  - Useful shortcuts:
    - `P`: Sort by CPU usage
    - `M`: Sort by memory usage
    - `T`: Sort by running time
    - `k`: Kill a process
    - `q`: Quit `top`

### 2.3 Advanced Monitoring with `htop`
- **Objective**: Use an enhanced version of `top` with a user-friendly interface.
- **Commands**:
  - Install `htop`:
    ```bash
    sudo apt install htop
    ```
  - Launch `htop`:
    ```bash
    htop
    ```
  - Navigate using arrow keys, and use `F9` to kill processes.

---

## 3. Finding Open Files with `lsof`

### 3.1 Viewing Open Files
- **Objective**: List all open files by processes.
- **Commands**:
  - List all open files:
    ```bash
    sudo lsof
    ```
  - View files opened by a specific user:
    ```bash
    sudo lsof -u username
    ```
  - View files opened by a specific process:
    ```bash
    sudo lsof -p PID
    ```
  - Display open network connections:
    ```bash
    sudo lsof -i
    ```

### 3.2 Monitoring File Access in Real-Time
- **Objective**: Monitor file access activity.
- **Commands**:
  - Monitor access to a file or directory:
    ```bash
    sudo watch -n 1 'lsof /path/to/directory'
    ```

---

## 4. Tracing Program Execution and System Calls

### 4.1 Using `strace` to Trace System Calls
- **Objective**: Trace system calls made by a program.
- **Commands**:
  - Trace a program's system calls:
    ```bash
    strace -o output.txt -e trace=all ./program_name
    ```
  - Trace a running process by PID:
    ```bash
    sudo strace -p PID
    ```
  - Display network-related system calls:
    ```bash
    sudo strace -e trace=network ./program_name
    ```

### 4.2 Using `ltrace` to Trace Library Calls
- **Objective**: Trace library calls made by a program.
- **Commands**:
  - Trace library function calls:
    ```bash
    ltrace ./program_name
    ```
  - Log the output to a file:
    ```bash
    ltrace -o ltrace_output.txt ./program_name
    ```

---

## 5. Threads

### 5.1 Understanding Threads
- **Objective**: Identify single-threaded and multithreaded processes.
- **Explanation**:
  - **Single-threaded**: One execution flow per process.
  - **Multithreaded**: Multiple execution flows within a single process.

### 5.2 Viewing Threads with `ps` and `top`
- **Commands**:
  - Display threads with `ps`:
    ```bash
    ps -eLf
    ```
  - Show threads in `top`:
    - Press `H` in the `top` interface.

---

## 6. Monitoring Resource Utilization

### 6.1 Monitoring CPU Usage
- **Objective**: Check CPU utilization.
- **Commands**:
  - Using `top` or `htop`.
  - Using `mpstat` (from the `sysstat` package):
    ```bash
    sudo apt install sysstat
    mpstat -P ALL 2
    ```

### 6.2 Monitoring Memory Usage
- **Objective**: View memory usage statistics.
- **Commands**:
  - Using `free`:
    ```bash
    free -h
    ```
  - Using `vmstat`:
    ```bash
    vmstat 2
    ```

### 6.3 Monitoring Disk Usage
- **Objective**: Check disk space and I/O performance.
- **Commands**:
  - Check disk space:
    ```bash
    df -h
    ```
  - Monitor disk I/O:
    ```bash
    iostat -xz 2
    ```

### 6.4 Monitoring Network Usage
- **Objective**: Monitor network traffic and connections.
- **Commands**:
  - Using `ss`:
    ```bash
    ss -tunap
    ```
  - Using `iftop` (must be installed):
    ```bash
    sudo apt install iftop
    sudo iftop
    ```

---

## 7. Control Groups (cgroups)

### 7.1 Overview
- **Objective**: Manage resource allocation using cgroups.
- **Explanation**:
  - **cgroups** limit and isolate resource usage for processes.
  - Versions:
    - **cgroup v1**: Hierarchical resource management.
    - **cgroup v2**: Unified resource management.

### 7.2 Viewing cgroups
- **Commands**:
  - List all cgroups:
    ```bash
    cat /proc/cgroups
    ```
  - Display a process’s cgroup membership:
    ```bash
    cat /proc/<PID>/cgroup
    ```

### 7.3 Creating and Managing cgroups
- **Objective**: Create and manage cgroups manually.
- **Commands**:
  - Create a cgroup:
    ```bash
    sudo cgcreate -g cpu,memory:/my_cgroup
    ```
  - Limit CPU usage:
    ```bash
    sudo cgset -r cpu.shares=512 my_cgroup
    ```
  - Add a process to the cgroup:
    ```bash
    sudo cgclassify -g cpu,memory:/my_cgroup <PID>
    ```

### 7.4 Viewing Resource Utilization in cgroups
- **Commands**:
  - Check CPU usage:
    ```bash
    cat /sys/fs/cgroup/cpu/my_cgroup/cpuacct.usage
    ```
  - Check memory usage:
    ```bash
    cat /sys/fs/cgroup/memory/my_cgroup/memory.usage_in_bytes
    ```

---

## 8. Practical Exercises

### Exercise 1: Monitoring Processes
1. Use `ps`, `top`, and `htop` to monitor system processes.
2. Sort by CPU and memory usage.

### Exercise 2: Using lsof and strace
1. List open files by a specific user with `lsof`.
2. Trace system calls of a program using `strace`.

### Exercise 3: Creating and Managing cgroups
1. Create a cgroup to limit CPU usage.
2. Add a process to the cgroup and monitor its resource usage.