# LINUX FUNDAMENTALS

## Linux File System Structure

### What You'll Learn
- Navigate the Linux directory structure
- Understand key directories and their purposes
- Apply filesystem knowledge to DevOps tasks

### Linux Directory Structure

```
/                           ← Root directory (everything starts here)
├── bin/                    ← Essential commands (ls, cp, mv)
├── boot/                   ← Boot files (kernel, GRUB)
├── dev/                    ← Device files (hard drives, USB)
├── etc/                    ← Configuration files
├── home/                   ← User home directories
├── lib/                    ← System libraries
├── media/                  ← Removable media (USB, CD)
├── mnt/                    ← Manual mount points
├── opt/                    ← Third-party software
├── proc/                   ← Process information
├── root/                   ← Root user's home
├── run/                    ← Runtime data
├── sbin/                   ← System administration commands
├── srv/                    ← Service data
├── sys/                    ← System information
├── tmp/                    ← Temporary files
├── usr/                    ← User programs
└── var/                    ← Variable data (logs, databases)
```

### Practice Commands

#### **Basic Navigation:**
```bash
# Check current location
pwd

# Go to root directory
cd /

# List contents
ls

# Go to etc directory
cd /etc

# Go back to previous directory
cd -

# Go to home directory
cd ~
```

#### **Explore Key Directories:**
```bash
# System configuration files
ls /etc/
cat /etc/hostname
cat /etc/hosts

# System logs
ls /var/log/
tail -5 /var/log/syslog

# User programs
ls /usr/bin/ | head -10

# Temporary files
ls /tmp/

# Device files
ls /dev/ | head -10
```

### Practice Exercise
```bash
# Create this directory structure in /tmp/practice/
mkdir -p /tmp/practice
cd /tmp/practice

# Explore system directories
echo "=== Exploring Linux Filesystem ===" > exploration.log
echo "Current directory: $(pwd)" >> exploration.log
echo "Home directory: $HOME" >> exploration.log
echo "System hostname: $(cat /etc/hostname)" >> exploration.log
echo "Available disk space:" >> exploration.log
df -h >> exploration.log
```

---

## Essential Commands

### What You'll Learn
- Master basic file operations
- Use command options effectively
- Practice real-world scenarios

### Commands Reference

#### **Listing Files (ls):**
```bash
ls                      # Basic listing
ls -l                   # Long format (detailed info)
ls -la                  # Include hidden files
ls -lh                  # Human-readable sizes
ls -lt                  # Sort by time (newest first)
ls -lS                  # Sort by size (largest first)
ls -lr                  # Reverse order
```

#### **Creating Files and Directories:**
```bash
touch file.txt                  # Create empty file
touch file1.txt file2.txt      # Multiple files
mkdir directory                 # Create directory
mkdir -p path/to/directory     # Create nested directories
```

#### **Copying (cp):**
```bash
cp file1.txt file2.txt         # Copy file
cp file.txt /tmp/              # Copy to another location
cp -r directory/ /backup/      # Copy directory recursively
cp *.txt /backup/              # Copy all .txt files
cp -v source dest              # Verbose (show what's copied)
cp -i source dest              # Interactive (ask before overwrite)
```

#### **Moving/Renaming (mv):**
```bash
mv oldname.txt newname.txt     # Rename file
mv file.txt /tmp/              # Move file
mv directory/ /opt/            # Move directory
mv *.log /var/log/archived/    # Move multiple files
```

#### **Removing (rm):**
```bash
rm file.txt                    # Remove file
rm -i file.txt                 # Interactive (ask confirmation)
rm -f file.txt                 # Force (no confirmation)
rm *.tmp                       # Remove all .tmp files
rmdir empty_directory          # Remove empty directory
rm -r directory/               # Remove directory and contents
rm -rf directory/              # Force remove (be careful!)
```

### Practice Exercise 
```bash
# Create practice environment
cd /tmp
mkdir linux_practice
cd linux_practice

# Create directory structure
mkdir -p project/{src,docs,tests,config}
mkdir -p backup/{daily,weekly}

# Create files
touch project/src/main.py
touch project/src/utils.py
touch project/docs/README.md
touch project/config/app.conf
touch project/tests/test_main.py

# Practice copying
cp project/config/app.conf project/config/app.conf.backup
cp -r project/ backup/daily/

# Practice moving
mkdir archive
mv backup/daily/project archive/project_$(date +%Y%m%d)

# Check your work
tree . || ls -la
```

---

## File Permissions & Ownership

### What You'll Learn
- Understand Linux security model
- Change file permissions and ownership
- Apply security best practices

### Permission System

#### **Permission Types:**
- **r (Read - 4)**: View file content or list directory
- **w (Write - 2)**: Modify file or create/delete files in directory
- **x (Execute - 1)**: Run file as program or access directory

#### **Permission Groups:**
- **u (User/Owner)**: File creator
- **g (Group)**: Users in file's group
- **o (Others)**: Everyone else
- **a (All)**: User + Group + Others

#### **Reading Permissions:**
```bash
$ ls -l file.txt
-rw-r--r-- 1 user group 1024 Aug 16 10:30 file.txt
│││││││││  │ │    │     │    │
│││││││││  │ │    │     │    └── filename
│││││││││  │ │    │     └── modification time
│││││││││  │ │    └── group owner
│││││││││  │ └── user owner
│││││││││  └── number of links
└┴┴┴┴┴┴┴┴── permissions
```

**Breaking down permissions:**
```
-rw-r--r--
│││││││││
│└┴┴┴┴┴┴┴─ Others: r-- (read only) = 4
│  └┴┴┴┴┴─ Group: r-- (read only) = 4  
│    └┴┴┴─ Owner: rw- (read, write) = 6
└────────── File type: - (regular file)
```

### Changing Permissions

#### **chmod (Change Mode):**

**Symbolic Method:**
```bash
chmod u+x script.sh        # Add execute for owner
chmod g-w file.txt         # Remove write for group
chmod o+r file.txt         # Add read for others
chmod a+r file.txt         # Add read for all
chmod u+rw,g+r,o-rwx file  # Multiple changes
```

**Numeric Method:**
```bash
chmod 755 script.sh        # rwxr-xr-x (executable)
chmod 644 file.txt         # rw-r--r-- (regular file)
chmod 600 private.key      # rw------- (private file)
chmod 777 shared/          # rwxrwxrwx (full access - dangerous!)
```

#### **Common Permission Patterns:**
```bash
# Files
644 (rw-r--r--)    # Regular files, configs
600 (rw-------)    # Private files, SSH keys
755 (rwxr-xr-x)    # Executable scripts

# Directories  
755 (rwxr-xr-x)    # Standard directories
700 (rwx------)    # Private directories
775 (rwxrwxr-x)    # Group-writable directories
```

#### **chown (Change Owner):**
```bash
chown user file.txt                # Change owner
chown user:group file.txt          # Change owner and group
chown -R user:group directory/     # Recursive change
chown :group file.txt              # Change group only
```

### Practice Exercise 
```bash
# Create permission practice environment
cd /tmp
mkdir permission_lab
cd permission_lab

# Create test files
touch public_file.txt
touch private_file.txt
touch script.sh
mkdir public_dir
mkdir private_dir

# Set different permissions
chmod 644 public_file.txt      # Everyone can read
chmod 600 private_file.txt     # Only owner can access
chmod 755 script.sh            # Executable by all
chmod 755 public_dir           # Standard directory
chmod 700 private_dir          # Private directory

# Check permissions
ls -la

# Create a simple script
echo '#!/bin/bash' > script.sh
echo 'echo "Hello from script!"' >> script.sh
chmod +x script.sh
./script.sh

# Practice with numbers
chmod 644 public_file.txt
chmod 600 private_file.txt
chmod 755 script.sh

# Verify
ls -l
```

---

## User & Group Management

### What You'll Learn
- Understand users and groups
- Manage user accounts
- Configure sudo access

### User System Overview

#### **User Types:**
- **Root (UID 0)**: System administrator
- **System users (UID 1-999)**: Service accounts
- **Regular users (UID 1000+)**: Human users

#### **Key Files:**
```bash
/etc/passwd    # User accounts info
/etc/shadow    # Encrypted passwords (root only)
/etc/group     # Group information  
/etc/sudoers   # Sudo permissions
```

### User Information Commands

```bash
# Check current user
whoami
id
groups

# View user database
cat /etc/passwd | head -5
# Format: username:x:UID:GID:description:home:shell

# View group information
cat /etc/group | head -5
# Format: groupname:x:GID:members

# Detailed user info
id username
groups username
```

### Practice Exercise

**Note: These commands require administrator privileges. Practice in your own environment.**

```bash
# View current user information
whoami
id
groups

# Check user details
grep $(whoami) /etc/passwd
grep $(whoami) /etc/group

# View system users
head -10 /etc/passwd
tail -10 /etc/passwd

# Check sudo permissions (if available)
sudo -l 2>/dev/null || echo "No sudo access"

# View group membership
groups
id -nG
```

### User Management Commands Reference

#### **Adding Users:**
```bash
sudo useradd -m -s /bin/bash newuser     # Create with home directory
sudo passwd newuser                      # Set password
sudo usermod -aG sudo newuser            # Add to sudo group
```

#### **Managing Groups:**
```bash
sudo groupadd developers                 # Create group
sudo usermod -aG developers user         # Add user to group
sudo gpasswd -d user group              # Remove user from group
```

#### **Removing Users:**
```bash
sudo userdel username                    # Remove user (keep home)
sudo userdel -r username                 # Remove user and home directory
```

---

# LINUX & SHELL SCRIPTING

## Shell Scripting Basics

### What You'll Learn
- Write basic shell scripts
- Use variables, conditions, and loops
- Create automation scripts

### Your First Script

```bash
#!/bin/bash
# My first shell script
echo "Hello, DevOps World!"
echo "Today is: $(date)"
echo "Current user: $(whoami)"
echo "Current directory: $(pwd)"
```

#### **Making Scripts Executable:**
```bash
chmod +x myscript.sh
./myscript.sh
```

### Variables

```bash
#!/bin/bash
# Variable examples
NAME="DevOps Engineer"
DATE=$(date +%Y-%m-%d)
COUNT=10

echo "Hello, $NAME"
echo "Today is $DATE"
echo "Server count: $COUNT"

# Reading user input
read -p "Enter your name: " USERNAME
echo "Welcome, $USERNAME!"
```

### Conditional Statements

```bash
#!/bin/bash
# If statements
if [ -f "/etc/passwd" ]; then
    echo "Password file exists"
fi

# If-else
read -p "Enter a number: " NUM
if [ $NUM -gt 10 ]; then
    echo "Number is greater than 10"
else
    echo "Number is 10 or less"
fi

# Multiple conditions
SERVICE="nginx"
if systemctl is-active --quiet $SERVICE 2>/dev/null; then
    echo "$SERVICE is running"
elif systemctl is-enabled --quiet $SERVICE 2>/dev/null; then
    echo "$SERVICE is enabled but not running"
else
    echo "$SERVICE is not available or disabled"
fi
```

#### **Test Conditions Reference:**
```bash
# File tests
[ -f file ]      # File exists
[ -d directory ] # Directory exists
[ -r file ]      # File readable
[ -w file ]      # File writable
[ -x file ]      # File executable

# String tests
[ -z "$string" ] # String is empty
[ -n "$string" ] # String is not empty
[ "$str1" = "$str2" ] # Strings equal

# Number tests
[ $num1 -eq $num2 ] # Equal
[ $num1 -ne $num2 ] # Not equal
[ $num1 -gt $num2 ] # Greater than
[ $num1 -lt $num2 ] # Less than
```

### Loops

#### **For Loops:**
```bash
#!/bin/bash
# Simple for loop
for i in 1 2 3 4 5; do
    echo "Number: $i"
done

# Range loop
for i in {1..10}; do
    echo "Processing $i"
done

# File processing
for file in /tmp/*.txt; do
    if [ -f "$file" ]; then
        echo "Found: $file"
    fi
done
```

#### **While Loops:**
```bash
#!/bin/bash
# While loop
COUNT=1
while [ $COUNT -le 5 ]; do
    echo "Count: $COUNT"
    COUNT=$((COUNT + 1))
done
```

### Practice Exercise 

```bash
# Create script directory
mkdir -p ~/scripts
cd ~/scripts

# Create system info script
cat > system_info.sh << 'EOF'
#!/bin/bash
# System Information Script

echo "=== System Information ==="
echo "Hostname: $(hostname)"
echo "Date: $(date)"
echo "Uptime: $(uptime -p 2>/dev/null || uptime)"
echo "Current User: $(whoami)"
echo "Home Directory: $HOME"

echo -e "\n=== Disk Usage ==="
df -h | head -5

echo -e "\n=== Memory Usage ==="
free -h 2>/dev/null || echo "Memory info not available"

echo -e "\n=== Current Directory Contents ==="
ls -la
EOF

# Make executable and run
chmod +x system_info.sh
./system_info.sh
```

### Practice Exercise - File Organizer Script

```bash
# Create file organizer script
cat > organize_files.sh << 'EOF'
#!/bin/bash
# File organizer script

# Create test files
mkdir -p ~/file_organizer
cd ~/file_organizer

# Create sample files
touch document1.txt document2.pdf script1.sh script2.py
touch image1.jpg image2.png video1.mp4 data.csv

echo "Files before organization:"
ls -la

# Create directories for organization
mkdir -p documents scripts images videos data

# Organize files
for file in *.*; do
    if [ -f "$file" ]; then
        case "$file" in
            *.txt|*.pdf|*.doc) mv "$file" documents/ ;;
            *.sh|*.py|*.js) mv "$file" scripts/ ;;
            *.jpg|*.png|*.gif) mv "$file" images/ ;;
            *.mp4|*.avi|*.mkv) mv "$file" videos/ ;;
            *.csv|*.json|*.xml) mv "$file" data/ ;;
        esac
    fi
done

echo -e "\nFiles after organization:"
for dir in documents scripts images videos data; do
    echo "$dir/:"
    ls "$dir/" 2>/dev/null || echo "  (empty)"
done
EOF

chmod +x organize_files.sh
./organize_files.sh
```

---

## Package Managers

### What You'll Learn
- Use APT (Debian/Ubuntu) package manager
- Use YUM/DNF (RHEL/CentOS) package manager
- Install and manage software

### APT Package Manager (Debian/Ubuntu)

#### **Basic Commands:**
```bash
# Update package list
sudo apt update

# Upgrade packages
sudo apt upgrade
sudo apt full-upgrade

# Install packages
sudo apt install package_name
sudo apt install -y package_name    # No confirmation

# Remove packages
sudo apt remove package_name        # Keep config files
sudo apt purge package_name         # Remove everything
sudo apt autoremove                 # Remove unused dependencies
```

#### **Package Information:**
```bash
# Search packages
apt search keyword
apt search nginx

# Show package info
apt show package_name
apt info docker.io

# List installed packages
apt list --installed
apt list --installed | grep nginx

# Check if package is installed
dpkg -l | grep package_name
```

### Practice Exercise - APT Practice

```bash
# Practice APT commands (Ubuntu/Debian)
# Note: Some commands need sudo, practice in your environment

# Update package database
sudo apt update

# Search for text editors
apt search editor | head -10

# Get info about a package
apt show vim

# List installed packages (first 10)
apt list --installed | head -10

# Check if git is installed
dpkg -l | grep git || echo "Git not found"

# Show package dependencies
apt depends vim 2>/dev/null | head -5

# Clean package cache
sudo apt clean
```

### YUM/DNF Package Manager (RHEL/CentOS/Fedora)

#### **YUM Commands (RHEL/CentOS 7):**
```bash
# Update system
sudo yum update
sudo yum check-update

# Install packages
sudo yum install package_name
sudo yum install -y package_name

# Remove packages
sudo yum remove package_name
sudo yum autoremove

# Search and info
yum search keyword
yum info package_name
yum list installed
```

#### **DNF Commands (RHEL 8+/Fedora):**
```bash
# Update system
sudo dnf update
sudo dnf upgrade

# Install packages
sudo dnf install package_name
sudo dnf install -y package_name

# Remove packages
sudo dnf remove package_name
sudo dnf autoremove

# Search and info
dnf search keyword
dnf info package_name
dnf list installed
```

### Practice Exercise - Package Management Script

```bash
# Create package info script
cat > package_info.sh << 'EOF'
#!/bin/bash
# Package management information script

echo "=== Package Management System Detection ==="

if command -v apt >/dev/null 2>&1; then
    echo "Debian/Ubuntu system detected (APT)"
    echo "Package manager: apt"
    echo "Update command: sudo apt update && sudo apt upgrade"
    
    echo -e "\n=== Installed Package Count ==="
    apt list --installed 2>/dev/null | wc -l
    
    echo -e "\n=== Recent Package Operations ==="
    grep " install " /var/log/apt/history.log 2>/dev/null | tail -3 || echo "No recent install history found"
    
elif command -v yum >/dev/null 2>&1; then
    echo "RHEL/CentOS system detected (YUM)"
    echo "Package manager: yum"
    echo "Update command: sudo yum update"
    
    echo -e "\n=== Installed Package Count ==="
    yum list installed 2>/dev/null | wc -l
    
elif command -v dnf >/dev/null 2>&1; then
    echo "Fedora/RHEL 8+ system detected (DNF)"
    echo "Package manager: dnf"  
    echo "Update command: sudo dnf update"
    
    echo -e "\n=== Installed Package Count ==="
    dnf list installed 2>/dev/null | wc -l
    
else
    echo "Unknown package management system"
fi

echo -e "\n=== System Information ==="
cat /etc/os-release 2>/dev/null | head -5 || echo "OS info not available"
EOF

chmod +x package_info.sh
./package_info.sh
```

---

## : Networking Commands

### What You'll Learn
- Test network connectivity
- Analyze network configuration
- Troubleshoot network issues

### Connectivity Testing

#### **ping - Test Connectivity:**
```bash
ping google.com                    # Basic ping
ping -c 4 google.com              # 4 pings only
ping -c 4 8.8.8.8                 # Ping Google DNS
ping -W 3 192.168.1.1             # 3 second timeout
```

#### **curl - HTTP Requests:**
```bash
curl http://httpbin.org/ip        # Get your public IP
curl -I google.com                # HTTP headers only
curl -s http://httpbin.org/json   # Silent mode
curl -o output.html google.com    # Save to file
```

#### **wget - Download Files:**
```bash
wget http://httpbin.org/robots.txt     # Download file
wget -O myfile.txt http://example.com  # Save with different name
wget -q http://example.com             # Quiet mode
```

### 🔧 Network Configuration

#### **ip - Modern Network Tool:**
```bash
ip addr show                      # Show all interfaces
ip addr show eth0                 # Show specific interface
ip route show                     # Show routing table
ip neighbor show                  # Show ARP table
```

#### **ifconfig - Legacy Network Tool:**
```bash
ifconfig                         # Show all interfaces
ifconfig eth0                    # Show specific interface
```

### Network Connections

#### **netstat - Network Statistics:**
```bash
netstat -tulpn                   # Show all listening ports
netstat -an                      # All connections, numerical
netstat -r                       # Routing table
```

#### **ss - Socket Statistics (Modern):**
```bash
ss -tulpn                        # Listening ports with processes
ss -an                           # All connections
ss -tulpn | grep :80             # Check port 80
ss -tulpn | grep :22             # Check SSH port
```

### Practice Exercise  - Network Testing

```bash
# Create network testing script
cat > network_test.sh << 'EOF'
#!/bin/bash
# Network connectivity testing script

echo "=== Network Connectivity Test ==="
echo "Date: $(date)"

# Test internet connectivity
echo -e "\n=== Internet Connectivity ==="
if ping -c 1 -W 3 8.8.8.8 >/dev/null 2>&1; then
    echo "✓ Internet connection: OK"
else
    echo "✗ Internet connection: FAILED"
fi

# Test DNS resolution
echo -e "\n=== DNS Resolution ==="
if ping -c 1 -W 3 google.com >/dev/null 2>&1; then
    echo "✓ DNS resolution: OK"
else
    echo "✗ DNS resolution: FAILED"
fi

# Show network interfaces
echo -e "\n=== Network Interfaces ==="
ip addr show 2>/dev/null | grep -E "^[0-9]|inet " || ifconfig | grep -E "^[a-z]|inet "

# Show default gateway
echo -e "\n=== Default Gateway ==="
ip route | grep default 2>/dev/null || route -n | grep "^0.0.0.0"

# Test HTTP connectivity
echo -e "\n=== HTTP Connectivity ==="
if curl -s -I google.com >/dev/null 2>&1; then
    echo "✓ HTTP connection: OK"
else
    echo "✗ HTTP connection: FAILED"
fi

# Show listening ports
echo -e "\n=== Common Service Ports ==="
for port in 22 80 443; do
    if ss -tulpn 2>/dev/null | grep -q ":$port " || netstat -tulpn 2>/dev/null | grep -q ":$port "; then
        echo "✓ Port $port is listening"
    else
        echo "✗ Port $port is not listening"
    fi
done
EOF

chmod +x network_test.sh
./network_test.sh
```

###  Practice Exercise - URL Testing

```bash
# Test multiple websites
cat > url_checker.sh << 'EOF'
#!/bin/bash
# URL checker script

urls=(
    "http://google.com"
    "http://github.com"
    "http://stackoverflow.com"
    "http://httpbin.org/status/200"
    "http://httpbin.org/status/404"
)

echo "=== URL Connectivity Checker ==="
echo "Testing multiple websites..."

for url in "${urls[@]}"; do
    echo -n "Testing $url: "
    
    if curl -s -I -L --max-time 10 "$url" >/dev/null 2>&1; then
        status=$(curl -s -I -L --max-time 10 "$url" | head -1 | cut -d' ' -f2)
        echo "✓ Response: $status"
    else
        echo "✗ Failed to connect"
    fi
done
EOF

chmod +x url_checker.sh
./url_checker.sh
```

---

## Process Management

### What You'll Learn
- Monitor running processes
- Control process execution  
- Manage system resources

### 👀 Viewing Processes

#### **ps - Process Status:**
```bash
ps                              # Current user's processes
ps aux                          # All processes (detailed)
ps -ef                          # All processes (different format)
ps aux | grep nginx             # Find specific process
ps -u username                  # Processes by user
```

#### **Understanding ps aux Output:**
```
USER    PID  %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root      1   0.0  0.1 225316  4856 ?        Ss   Aug16   0:02 /sbin/init
```
- **USER**: Process owner
- **PID**: Process ID
- **%CPU**: CPU usage percentage
- **%MEM**: Memory usage percentage  
- **STAT**: Process state (R=Running, S=Sleeping, Z=Zombie)

#### **top - Real-time Process Monitor:**
```bash
top                             # Interactive process monitor
top -u username                 # Show processes for user
top -p PID                      # Monitor specific process
```

**Top Commands (while running):**
- `q` - Quit
- `k` - Kill process (enter PID)
- `M` - Sort by memory usage
- `P` - Sort by CPU usage
- `1` - Show all CPUs

### Process Control

#### **kill - Terminate Processes:**
```bash
kill PID                        # Graceful termination
kill -9 PID                     # Force kill
kill -15 PID                    # Same as kill PID
killall process_name            # Kill by name
pkill pattern                   # Kill by pattern
```

#### **Background Processes:**
```bash
command &                       # Run in background
nohup command &                 # Run in background, ignore hangup
jobs                           # List background jobs
fg %1                          # Bring job 1 to foreground
bg %1                          # Send job 1 to background
```

### Practice Exercise - Process Monitoring

```bash
# Create process monitoring script
cat > process_monitor.sh << 'EOF'
#!/bin/bash
# Process monitoring script

echo "=== Process Monitoring Report ==="
echo "Generated: $(date)"
echo "Hostname: $(hostname)"

# System load
echo -e "\n=== System Load ==="
uptime

# Memory usage
echo -e "\n=== Memory Usage ==="
free -h 2>/dev/null || echo "Memory info not available"

# Top CPU consuming processes
echo -e "\n=== Top 5 CPU Consuming Processes ==="
ps aux --sort=-%cpu | head -6

# Top memory consuming processes
echo -e "\n=== Top 5 Memory Consuming Processes ==="
ps aux --sort=-%mem | head -6

# Process count by user
echo -e "\n=== Process Count by User ==="
ps aux | awk 'NR>1 {print $1}' | sort | uniq -c | sort -nr

# Check for zombie processes
echo -e "\n=== Zombie Processes ==="
zombie_count=$(ps aux | awk '$8 ~ /^Z/ {print $0}' | wc -l)
if [ $zombie_count -gt 0 ]; then
    echo "Found $zombie_count zombie processes:"
    ps aux | awk '$8 ~ /^Z/ {print $0}'
else
    echo "No zombie processes found"
fi
EOF

chmod +x process_monitor.sh
./process_monitor.sh
```

### Practice Exercise - Process Management Demo

```bash
# Create process management demo
cat > process_demo.sh << 'EOF'
#!/bin/bash
# Process management demonstration

echo "=== Process Management Demo ==="

# Start a background process
echo "Starting background sleep process..."
sleep 300 &
SLEEP_PID=$!
echo "Background process PID: $SLEEP_PID"

# Show the process
echo -e "\n=== Finding our process ==="
ps aux | grep "sleep 300" | grep -v grep

# Show process with pgrep
echo -e "\n=== Using pgrep ==="
pgrep -f "sleep 300"

# Show jobs
echo -e "\n=== Background Jobs ==="
jobs

# Kill the process
echo -e "\n=== Terminating Background Process ==="
kill $SLEEP_PID
sleep 2

# Verify it's gone
if ps -p $SLEEP_PID > /dev/null 2>&1; then
    echo "Process still running, force killing..."
    kill -9 $SLEEP_PID
else
    echo "Process terminated successfully"
fi

echo -e "\n=== Demo Complete ==="
EOF

chmod +x process_demo.sh
./process_demo.sh
```

---

# COMPREHENSIVE PRACTICE EXERCISES

## System Health Check Script

```bash
# Create comprehensive system health check
mkdir -p ~/final_practice
cd ~/final_practice

cat > health_check.sh << 'EOF'
#!/bin/bash
# Comprehensive System Health Check Script

LOG_FILE="health_check_$(date +%Y%m%d_%H%M%S).log"

# Function to log messages
log_message() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"
}

log_message "=== SYSTEM HEALTH CHECK STARTED ==="
log_message "Hostname: $(hostname)"

# Check disk usage
log_message ""
log_message "=== DISK USAGE CHECK ==="
df -h | while read line; do
    log_message "$line"
    # Check if any partition is > 80% full
    usage=$(echo "$line" | awk '{print $5}' | grep -o '[0-9]*')
    if [[ "$usage" =~ ^[0-9]+$ ]] && [ "$usage" -gt 80 ]; then
        log_message "WARNING: High disk usage detected: $usage%"
    fi
done

# Check memory usage
log_message ""
log_message "=== MEMORY USAGE CHECK ==="
free -h | while read line; do
    log_message "$line"
done

# Check system load
log_message ""
log_message "=== SYSTEM LOAD CHECK ==="
load_avg=$(uptime | awk -F'load average:' '{print $2}')
log_message "Load Average: $load_avg"

# Check running services
log_message ""
log_message "=== CRITICAL SERVICES CHECK ==="
services=("ssh" "cron")

for service in "${services[@]}"; do
    if systemctl is-active --quiet "$service" 2>/dev/null; then
        log_message "✓ $service: Running"
    elif service "$service" status >/dev/null 2>&1; then
        log_message "✓ $service: Running (SysV)"
    else
        log_message "✗ $service: Not running or not installed"
    fi
done

# Check network connectivity
log_message ""
log_message "=== NETWORK CONNECTIVITY CHECK ==="
if ping -c 1 -W 3 8.8.8.8 >/dev/null 2>&1; then
    log_message "✓ Internet connectivity: OK"
else
    log_message "✗ Internet connectivity: FAILED"
fi

# Check large log files
log_message ""
log_message "=== LARGE LOG FILES CHECK ==="
find /var/log -name "*.log" -size +50M 2>/dev/null | while read logfile; do
    size=$(du -sh "$logfile" 2>/dev/null | cut -f1)
    log_message "Large log file: $logfile ($size)"
done

# Check failed login attempts
log_message ""
log_message "=== SECURITY CHECK ==="
failed_logins=$(grep "Failed password" /var/log/auth.log 2>/dev/null | wc -l)
log_message "Failed login attempts today: $failed_logins"

log_message ""
log_message "=== SYSTEM HEALTH CHECK COMPLETED ==="
log_message "Report saved to: $LOG_FILE"

echo "Health check completed. Report saved to: $LOG_FILE"
EOF

chmod +x health_check.sh
./health_check.sh
```

## File Management System

```bash
# Create advanced file management script
cat > file_manager.sh << 'EOF'
#!/bin/bash
# Advanced File Management System

WORK_DIR="$HOME/file_management_demo"
mkdir -p "$WORK_DIR"
cd "$WORK_DIR"

# Function to create sample files
create_sample_files() {
    echo "Creating sample files..."
    
    # Create directory structure
    mkdir -p {documents,scripts,images,archives,logs}/{2023,2024}
    
    # Create sample files with different extensions and dates
    touch documents/2023/report_jan.pdf
    touch documents/2023/notes.txt
    touch documents/2024/presentation.pptx
    
    touch scripts/2023/backup_old.sh
    touch scripts/2024/deploy.py
    touch scripts/2024/monitor.js
    
    touch images/2023/photo1.jpg
    touch images/2024/screenshot.png
    
    touch archives/2023/backup.tar.gz
    touch archives/2024/project.zip
    
    touch logs/2023/error.log
    touch logs/2024/access.log
    touch logs/2024/debug.log
    
    # Create some files in root for testing
    touch old_file.txt
    touch temp_data.csv
    touch script.sh
    
    echo "Sample files created!"
}

# Function to organize files by extension
organize_by_extension() {
    echo "Organizing files by extension..."
    
    for file in *.{txt,pdf,sh,py,js,jpg,png,csv,zip,gz}; do
        [ -f "$file" ] || continue
        
        case "$file" in
            *.txt|*.pdf) 
                [ ! -d "documents" ] && mkdir -p documents
                mv "$file" documents/
                echo "Moved $file to documents/"
                ;;
            *.sh|*.py|*.js)
                [ ! -d "scripts" ] && mkdir -p scripts  
                chmod +x "$file" 2>/dev/null
                mv "$file" scripts/
                echo "Moved $file to scripts/"
                ;;
            *.jpg|*.png)
                [ ! -d "images" ] && mkdir -p images
                mv "$file" images/
                echo "Moved $file to images/"
                ;;
            *.csv)
                [ ! -d "data" ] && mkdir -p data
                mv "$file" data/
                echo "Moved $file to data/"
                ;;
            *.zip|*.gz)
                [ ! -d "archives" ] && mkdir -p archives
                mv "$file" archives/
                echo "Moved $file to archives/"
                ;;
        esac
    done
}

# Function to find and report large files
find_large_files() {
    echo "Finding large files (>1MB)..."
    find . -type f -size +1M 2>/dev/null | while read file; do
        size=$(du -sh "$file" | cut -f1)
        echo "Large file: $file ($size)"
    done
}

# Function to create backup
create_backup() {
    backup_name="backup_$(date +%Y%m%d_%H%M%S).tar.gz"
    echo "Creating backup: $backup_name"
    tar -czf "$backup_name" --exclude="backup_*.tar.gz" .
    echo "Backup created: $backup_name"
}

# Function to clean temporary files
clean_temp_files() {
    echo "Cleaning temporary files..."
    find . -name "*.tmp" -delete 2>/dev/null
    find . -name "*~" -delete 2>/dev/null
    echo "Temporary files cleaned"
}

# Function to show directory tree
show_structure() {
    echo "Current directory structure:"
    if command -v tree >/dev/null 2>&1; then
        tree -L 3
    else
        find . -type d | head -20 | sort
    fi
}

# Main menu
while true; do
    echo ""
    echo "=== File Management System ==="
    echo "Current directory: $(pwd)"
    echo ""
    echo "1. Create sample files"
    echo "2. Organize files by extension"  
    echo "3. Find large files"
    echo "4. Create backup"
    echo "5. Clean temporary files"
    echo "6. Show directory structure"
    echo "7. Exit"
    echo ""
    read -p "Choose an option (1-7): " choice
    
    case $choice in
        1) create_sample_files ;;
        2) organize_by_extension ;;
        3) find_large_files ;;
        4) create_backup ;;
        5) clean_temp_files ;;
        6) show_structure ;;
        7) echo "Goodbye!"; break ;;
        *) echo "Invalid option. Please choose 1-7." ;;
    esac
done
EOF

chmod +x file_manager.sh
echo "File manager created! Run with: ./file_manager.sh"
```

## DevOps Automation Script

```bash
# Create DevOps automation script
cat > devops_toolkit.sh << 'EOF'
#!/bin/bash
# DevOps Automation Toolkit

# Color codes for output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

# Function to print colored output
print_status() {
    case $1 in
        "success") echo -e "${GREEN}✓ $2${NC}" ;;
        "error") echo -e "${RED}✗ $2${NC}" ;;
        "warning") echo -e "${YELLOW}⚠ $2${NC}" ;;
        "info") echo -e "ℹ $2" ;;
    esac
}

# Function to check system requirements
check_system() {
    print_status "info" "Checking system requirements..."
    
    # Check OS
    if [ -f /etc/os-release ]; then
        . /etc/os-release
        print_status "success" "OS: $PRETTY_NAME"
    fi
    
    # Check available tools
    tools=("curl" "wget" "git" "ssh")
    for tool in "${tools[@]}"; do
        if command -v "$tool" >/dev/null 2>&1; then
            print_status "success" "$tool is installed"
        else
            print_status "warning" "$tool is not installed"
        fi
    done
}

# Function to monitor system resources
monitor_resources() {
    print_status "info" "System Resource Monitoring"
    echo "================================"
    
    # CPU usage
    echo "CPU Usage:"
    top -bn1 | grep "Cpu(s)" | awk '{print "  " $2 " " $3 " " $4 " " $8}' 2>/dev/null || echo "  CPU info not available"
    
    # Memory usage
    echo "Memory Usage:"
    free -h | grep "Mem:" | awk '{printf "  Used: %s/%s (%.1f%%)\n", $3, $2, ($3/$2)*100}' 2>/dev/null || echo "  Memory info not available"
    
    # Disk usage
    echo "Disk Usage:"
    df -h | grep -E '^/dev/' | awk '{printf "  %s: %s/%s (%s)\n", $6, $3, $2, $5}'
    
    # Load average
    echo "Load Average:"
    uptime | awk -F'load average:' '{printf "  %s\n", $2}'
}

# Function to backup configuration files
backup_configs() {
    backup_dir="$HOME/config_backup_$(date +%Y%m%d)"
    mkdir -p "$backup_dir"
    
    print_status "info" "Backing up configuration files to $backup_dir"
    
    # Common config files to backup
    configs=(
        "$HOME/.bashrc"
        "$HOME/.profile"
        "$HOME/.ssh/config"
        "/etc/hostname"
        "/etc/hosts"
    )
    
    for config in "${configs[@]}"; do
        if [ -f "$config" ]; then
            cp "$config" "$backup_dir/" 2>/dev/null && \
                print_status "success" "Backed up $config" || \
                print_status "error" "Failed to backup $config"
        else
            print_status "warning" "$config not found"
        fi
    done
    
    print_status "success" "Backup completed in $backup_dir"
}

# Function to clean system
clean_system() {
    print_status "info" "Cleaning system..."
    
    # Clean temporary files
    temp_files=$(find /tmp -type f -mtime +7 2>/dev/null | wc -l)
    if [ "$temp_files" -gt 0 ]; then
        print_status "info" "Found $temp_files old temporary files"
        sudo find /tmp -type f -mtime +7 -delete 2>/dev/null && \
            print_status "success" "Cleaned old temporary files"
    fi
    
    # Clean user cache
    if [ -d "$HOME/.cache" ]; then
        cache_size=$(du -sh "$HOME/.cache" 2>/dev/null | cut -f1)
        print_status "info" "User cache size: $cache_size"
    fi
    
    print_status "success" "System cleaning completed"
}

# Function to generate system report
generate_report() {
    report_file="system_report_$(date +%Y%m%d_%H%M%S).txt"
    
    print_status "info" "Generating system report: $report_file"
    
    cat > "$report_file" << EOL
System Report - Generated $(date)
=====================================

Hostname: $(hostname)
Uptime: $(uptime -p 2>/dev/null || uptime)
User: $(whoami)

System Information:
$(uname -a)

CPU Information:
$(grep "model name" /proc/cpuinfo | head -1 | cut -d: -f2 | sed 's/^ *//' 2>/dev/null || echo "CPU info not available")

Memory Information:
$(free -h 2>/dev/null || echo "Memory info not available")

Disk Usage:
$(df -h)

Network Interfaces:
$(ip addr show 2>/dev/null || ifconfig 2>/dev/null || echo "Network info not available")

Running Processes (top 10):
$(ps aux --sort=-%cpu | head -11)

Recent Log Entries:
$(tail -10 /var/log/syslog 2>/dev/null || echo "System log not accessible")
EOL

    print_status "success" "Report saved to $report_file"
}

# Main menu
show_menu() {
    echo ""
    echo "==============================="
    echo "    DevOps Automation Toolkit"
    echo "==============================="
    echo "1. Check System Requirements"
    echo "2. Monitor Resources"
    echo "3. Backup Configurations"
    echo "4. Clean System"
    echo "5. Generate System Report"
    echo "6. Exit"
    echo ""
}

# Main loop
while true; do
    show_menu
    read -p "Choose an option (1-6): " choice
    
    case $choice in
        1) check_system ;;
        2) monitor_resources ;;
        3) backup_configs ;;
        4) clean_system ;;
        5) generate_report ;;
        6) print_status "success" "Goodbye!"; break ;;
        *) print_status "error" "Invalid option. Please choose 1-6." ;;
    esac
    
    echo ""
    read -p "Press Enter to continue..."
done
EOF

chmod +x devops_toolkit.sh
echo "DevOps toolkit created! Run with: ./devops_toolkit.sh"
```

---

# REFERENCE SECTION

## Quick Command Reference

### File Operations
```bash
# Navigation
pwd                    # Print working directory
cd /path              # Change directory
ls -la                # List files (detailed, including hidden)

# File manipulation
touch file.txt        # Create empty file
mkdir -p path/dir     # Create directories
cp -r source dest     # Copy recursively
mv source dest        # Move/rename
rm -rf directory      # Remove recursively (dangerous!)
```

### Permissions
```bash
# View permissions
ls -l file.txt

# Change permissions
chmod 755 script.sh   # rwxr-xr-x
chmod +x script.sh    # Add execute permission
chown user:group file # Change ownership
```

### Process Management
```bash
# View processes
ps aux               # All processes
top                  # Real-time monitor
pgrep pattern        # Find process by pattern

# Control processes
kill PID             # Terminate process
killall name         # Kill by name
command &            # Run in background
```

### Networking
```bash
ping -c 4 google.com    # Test connectivity
curl -I website.com     # HTTP headers
ss -tulpn              # Show listening ports
netstat -tulpn         # Legacy network statistics
```

### Package Management
```bash
# Debian/Ubuntu
sudo apt update && sudo apt upgrade
sudo apt install package_name
sudo apt remove package_name

# RHEL/CentOS
sudo yum update
sudo yum install package_name
sudo yum remove package_name
```

---

## Common File Permissions

| Permission | Numeric | Files | Directories |
|------------|---------|-------|-------------|
| `---` | 0 | No access | No access |
| `r--` | 4 | Read only | List contents |
| `rw-` | 6 | Read/Write | Invalid |
| `rwx` | 7 | Full access | Full access |
| `r-x` | 5 | Read/Execute | Read/Access |

### DevOps Permission Patterns
```bash
644  # Config files (rw-r--r--)
600  # Secret files (rw-------)
755  # Executable files (rwxr-xr-x)
700  # Private directories (rwx------)
```

---

## Environment Variables

```bash
# Common variables
echo $HOME            # User home directory
echo $USER            # Current username
echo $PATH            # Executable search path
echo $PWD             # Current directory

# Set variables
export MY_VAR="value"
echo $MY_VAR
```

---

## Useful Keyboard Shortcuts

| Shortcut | Function |
|----------|----------|
| `Ctrl+C` | Kill current command |
| `Ctrl+Z` | Suspend current command |
| `Ctrl+D` | Exit/End of file |
| `Ctrl+L` | Clear screen |
| `Tab` | Auto-complete |
| `↑/↓` | Command history |
| `Ctrl+R` | Search command history |

---

## Practice Tips

### 1. **Always Practice in Safe Environments**
- Use `/tmp/` for practice files
- Test scripts before running on important data
- Keep backups of important files

### 2. **Start Simple, Build Complexity**
- Master basic commands first
- Add features to scripts gradually
- Test each component separately

### 3. **Use Version Control**
```bash
cd ~/scripts
git init
git add *.sh
git commit -m "Initial scripts"
```

### 4. **Document Your Scripts**
- Add comments explaining complex logic
- Include usage examples
- Document required permissions

### 5. **Error Handling**
```bash
#!/bin/bash
# Always include error handling

if [ $# -eq 0 ]; then
    echo "Usage: $0 <filename>"
    exit 1
fi

if [ ! -f "$1" ]; then
    echo "Error: File $1 not found"
    exit 1
fi
```

---

## Next Steps for Learning

### 1. **Advanced Topics to Explore**
- Regular expressions (grep, sed, awk)
- System services (systemd)
- Log analysis and monitoring
- Automation with cron
- SSH key management
- Container technologies (Docker)

### 2. **DevOps Tools Integration**
- Git version control
- CI/CD pipelines
- Configuration management (Ansible)
- Infrastructure as Code (Terraform)
- Monitoring tools (Prometheus, Grafana)

### 3. **Practice Projects**
- Automated backup scripts
- Log rotation and cleanup
- System monitoring dashboards
- Deployment automation
- Security auditing scripts

---

## Troubleshooting Common Issues

### Permission Denied
```bash
# Check permissions
ls -l filename

# Fix permissions
chmod +x script.sh
sudo chown $USER:$USER filename
```

### Command Not Found
```bash
# Check if command exists
which command_name

# Check PATH
echo $PATH

# Install missing package
sudo apt install package_name  # Ubuntu/Debian
sudo yum install package_name  # RHEL/CentOS
```

### Script Won't Execute
```bash
# Make executable
chmod +x script.sh

# Check shebang line
head -1 script.sh  # Should be #!/bin/bash

# Run with bash explicitly
bash script.sh
```

---
