# System Configuration: Logging, System Time, Batch Jobs, and Users

---

## 1. System Logging

### 1.1 Checking Your Log Setup
- **Objective**: Inspect the system’s logging setup.
- **Commands**:
  - Check if `journald` is active:
    ```bash
    journalctl
    ```
  - Check for `rsyslogd`:
    ```bash
    ps aux | grep rsyslogd
    ls /etc/rsyslog.conf
    ```
  - Check for `syslog-ng`:
    ```bash
    ls /etc/syslog-ng
    ```
  - View logs in `/var/log`:
    ```bash
    ls /var/log
    ```

### 1.2 Searching and Monitoring Logs
- **Objective**: Search and monitor log messages.
- **Commands**:
  - View all logs:
    ```bash
    journalctl
    ```
  - Search by process ID:
    ```bash
    journalctl _PID=<pid>
    ```
  - Filter logs by time:
    ```bash
    journalctl -S -4h
    journalctl -S '2020-01-14 14:30:00'
    ```
  - Monitor logs in real-time:
    ```bash
    journalctl -f
    ```

### 1.3 Logfile Rotation
- **Objective**: Manage logfiles using log rotation.
- **Explanation**:
  - `logrotate` rotates logfiles to prevent them from consuming too much storage.
  - Example of rotated files:
    ```
    auth.log -> auth.log.1 -> auth.log.2.gz -> auth.log.3.gz
    ```
- **Commands**:
  - Force log rotation:
    ```bash
    sudo logrotate -f /etc/logrotate.conf
    ```
  - Check the log rotation status:
    ```bash
    sudo logrotate -d /etc/logrotate.conf
    ```

### 1.4 Journal Maintenance
- **Objective**: Manage systemd journal logs.
- **Commands**:
  - View journal size:
    ```bash
    journalctl --disk-usage
    ```
  - Clear journal logs older than 7 days:
    ```bash
    sudo journalctl --vacuum-time=7d
    ```
  - Limit the journal size:
    ```bash
    sudo journalctl --vacuum-size=100M
    ```

---

## 2. The Structure of /etc

### 2.1 Understanding /etc Directory
- **Objective**: Explore the `/etc` directory structure.
- **Explanation**:
  - `/etc`: Contains system configuration files.
  - Subdirectories organize configuration files for different services.
  - Example:
    ```bash
    ls -F /etc
    ```
    This lists all directories and files in `/etc`, where most items are now subdirectories.

### 2.2 Customizing Configuration
- **Objective**: Customize system configuration.
- **Steps**:
  1. Locate the configuration file, e.g., `/etc/systemd/system`.
  2. Edit using a text editor:
    ```bash
    sudo nano /etc/systemd/system/example.service
    ```
  3. Save changes and reload systemd:
    ```bash
    sudo systemctl daemon-reload
    ```

---

## 3. User Management Files

### 3.1 The /etc/passwd File
- **Objective**: Understand and manage user information.
- **Explanation**:
  - `/etc/passwd` maps usernames to user IDs (UIDs).
  - Format:
    ```
    username:x:UID:GID:Full Name:Home Directory:Shell
    ```
- **Commands**:
  - View the `/etc/passwd` file:
    ```bash
    cat /etc/passwd
    ```

### 3.2 The /etc/shadow File
- **Objective**: Manage user passwords.
- **Explanation**:
  - `/etc/shadow` stores encrypted user passwords.
  - Only accessible by the root user for security.
- **Commands**:
  - View the `/etc/shadow` file:
    ```bash
    sudo cat /etc/shadow
    ```

### 3.3 Manipulating Users and Passwords
- **Objective**: Add, modify, and delete users.
- **Commands**:
  - Add a new user:
    ```bash
    sudo adduser username
    ```
  - Delete a user:
    ```bash
    sudo deluser username
    ```
  - Change a user’s password:
    ```bash
    sudo passwd username
    ```

### 3.4 Working with Groups
- **Objective**: Manage user groups.
- **Commands**:
  - List groups:
    ```bash
    cat /etc/group
    ```
  - Add a group:
    ```bash
    sudo groupadd groupname
    ```
  - Add a user to a group:
    ```bash
    sudo usermod -aG groupname username
    ```

---

## 4. Setting the Time

### 4.1 Setting Time Zone
- **Objective**: Configure the system time zone.
- **Commands**:
  - List available time zones:
    ```bash
    timedatectl list-timezones
    ```
  - Set the time zone:
    ```bash
    sudo timedatectl set-timezone Region/City
    ```
  - Verify the current time and time zone:
    ```bash
    timedatectl
    ```

### 4.2 Synchronizing Time with NTP
- **Objective**: Sync time using NTP (Network Time Protocol).
- **Commands**:
  - Enable and start systemd-timesyncd:
    ```bash
    sudo systemctl enable systemd-timesyncd
    sudo systemctl start systemd-timesyncd
    ```
  - Check synchronization status:
    ```bash
    timedatectl status
    ```

---

## 5. Scheduling Tasks

### 5.1 Scheduling Recurring Tasks with cron
- **Objective**: Automate tasks using cron jobs.
- **Commands**:
  - Edit crontab:
    ```bash
    crontab -e
    ```
  - List current cron jobs:
    ```bash
    crontab -l
    ```
  - Example cron entry (runs daily at 9:15 AM):
    ```
    15 09 * * * /path/to/command
    ```

### 5.2 Scheduling with systemd Timer Units
- **Objective**: Schedule tasks with systemd timers.
- **Commands**:
  - Create a timer unit:
    ```bash
    sudo nano /etc/systemd/system/example.timer
    ```
    Example configuration:
    ```
    [Unit]
    Description=Example Timer

    [Timer]
    OnCalendar=*-*-* *:00
    Persistent=true

    [Install]
    WantedBy=timers.target
    ```
  - Enable and start the timer:
    ```bash
    sudo systemctl enable example.timer
    sudo systemctl start example.timer
    ```
  - Check timer status:
    ```bash
    systemctl list-timers
    ```

---

## 6. Practical Exercises

### Exercise 1: Managing Logs
1. Check system logs using `journalctl`.
2. Rotate logs manually using `logrotate`.
3. Clear old logs using `journalctl --vacuum-time`.

### Exercise 2: User and Group Management
1. Create a new user and set a password.
2. Add the user to a new group.
3. Verify the user’s group memberships.

### Exercise 3: Scheduling Tasks
1. Schedule a cron job to display a message every hour.
2. Create a systemd timer that runs a script daily.