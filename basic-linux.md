# Linux Basic Commands and Scripts

## What to do if starting to use Linux?

### 1. Initial Setup and Orientation
```bash
# Check your Linux distribution
cat /etc/os-release
lsb_release -a

# Install and use neofetch for system information
sudo apt install neofetch  # Ubuntu/Debian
# or
sudo yum install neofetch  # CentOS/RHEL
# or
sudo dnf install neofetch  # Fedora

# Display system information
neofetch                   

# Update your system (Ubuntu/Debian)
sudo apt update && sudo apt upgrade

# Update your system (CentOS/RHEL/Fedora)
sudo yum update
# or
sudo dnf update

# Install essential tools
sudo apt install curl wget git vim htop tree neofetch
```

### 2. Understanding the File System
```bash
# Key directories to know:
/           # Root directory
/home/      # User home directories
/etc/       # System configuration files
/var/       # Variable data (logs, etc.)
/usr/       # User programs and data
/bin/       # Essential system binaries
/opt/       # Optional application software
/tmp/       # Temporary files
```

### 3. User Management Basics
```bash
# Check current user
whoami
id

# Change password
passwd

# Switch user
su - username

# Add user to sudo group (Ubuntu/Debian)
sudo usermod -aG sudo username

# Add user to wheel group (CentOS/RHEL)
sudo usermod -aG wheel username
```

### 4. Package Management
```bash
# Ubuntu/Debian
sudo apt update          # Update package list
sudo apt upgrade         # Upgrade installed packages
sudo apt install package # Install package
sudo apt remove package  # Remove package
sudo apt search package  # Search for packages

# CentOS/RHEL/Fedora
sudo yum update          # Update packages
sudo yum install package # Install package
sudo yum remove package  # Remove package
sudo yum search package  # Search for packages
```

### 5. File Permissions Understanding
```bash
# View file permissions
ls -la

# Understanding permission format: rwxrwxrwx
# r = read, w = write, x = execute
# First group: owner permissions
# Second group: group permissions  
# Third group: others permissions

# Common permission numbers:
# 755 = rwxr-xr-x (owner can read/write/execute, others can read/execute)
# 644 = rw-r--r-- (owner can read/write, others can read only)
# 600 = rw------- (owner can read/write, others have no access)
```

### 6. Essential Commands for Beginners
```bash
# Navigation
pwd                     # Show current directory
ls -la                 # List files with details
cd directory           # Change directory
cd ~                   # Go to home directory

# File operations
cp source dest         # Copy file
mv source dest         # Move/rename file
rm filename            # Remove file
mkdir directory        # Create directory

# Text editing
nano filename          # Simple text editor
vim filename           # Advanced text editor
cat filename           # Display file contents

# System information
top                    # Process monitor
df -h                  # Disk usage
free -h                # Memory usage
```

### 7. Network and Internet
```bash
# Check network connectivity
ping google.com
ip addr show           # Show IP addresses
ip route show          # Show routing table

# Download files
wget https://example.com/file
curl -O https://example.com/file

# Install software from repositories
sudo apt install package_name
```

### 8. Troubleshooting Basics
```bash
# Check system logs
sudo journalctl -f     # Follow system logs
sudo dmesg             # Kernel messages

# Check disk space
df -h
du -sh directory       # Directory size

# Check running processes
ps aux | grep process_name
top                     # Interactive process viewer
```

### 9. Security Best Practices
```bash
# Keep system updated
sudo apt update && sudo apt upgrade

# Use strong passwords
passwd

# Don't run as root
sudo command            # Use sudo instead of su

# Check file permissions
ls -la sensitive_file

# Use firewall
sudo ufw enable         # Ubuntu
sudo firewall-cmd --permanent --add-service=ssh  # CentOS
```

### 10. Getting Help
```bash
# Command help
command --help
man command
info command

# System documentation
/usr/share/doc/
/usr/share/man/

# Online resources
# - Official distribution documentation
# - Stack Overflow
# - Linux forums
# - Reddit r/linux4noobs
```

## File System Navigation
- `pwd` - Print working directory
- `ls` - List files and directories
- `ls -la` - List all files with details
- `cd directory` - Change directory
- `cd ..` - Go to parent directory
- `cd ~` - Go to home directory
- `cd /` - Go to root directory

## File Operations
- `touch filename.txt` - Create empty file
- `mkdir foldername` - Create directory
- `mkdir -p parent/child` - Create nested directories
- `cp source destination` - Copy files/directories
- `cp -r source destination` - Copy directories recursively
- `mv source destination` - Move/rename files
- `rm filename` - Remove file
- `rm -r directory` - Remove directory recursively
- `rm -f filename` - Force remove without confirmation

## File Viewing and Editing
- `cat filename` - Display file contents
- `less filename` - View file with pagination
- `head filename` - Show first 10 lines
- `head -n 20 filename` - Show first 20 lines
- `tail filename` - Show last 10 lines
- `tail -f filename` - Follow file changes
- `nano filename` - Edit file with nano
- `vim filename` - Edit file with vim

## Text Processing
- `grep "pattern" filename` - Search for pattern in file
- `grep -i "pattern" filename` - Case-insensitive search
- `grep -r "pattern" directory` - Search recursively
- `sed 's/old/new/g' filename` - Replace text
- `awk '{print $1}' filename` - Print first column
- `sort filename` - Sort lines
- `uniq filename` - Remove duplicate lines
- `wc filename` - Count lines, words, characters

## System Information
- `uname -a` - System information
- `whoami` - Current user
- `id` - User and group information
- `ps aux` - List all processes
- `top` - Interactive process viewer
- `htop` - Enhanced process viewer
- `df -h` - Disk space usage
- `du -h directory` - Directory size
- `free -h` - Memory usage

## Network Commands
- `ping hostname` - Test connectivity
- `nslookup domain` - DNS lookup
- `wget url` - Download file
- `curl url` - Transfer data
- `ssh user@host` - Connect via SSH
- `scp file user@host:path` - Copy file via SSH

## Package Management (Ubuntu/Debian)
- `sudo apt update` - Update package list
- `sudo apt upgrade` - Upgrade packages
- `sudo apt install package` - Install package
- `sudo apt remove package` - Remove package
- `sudo apt search package` - Search packages

## Package Management (CentOS/RHEL)
- `sudo yum update` - Update packages
- `sudo yum install package` - Install package
- `sudo yum remove package` - Remove package
- `sudo yum search package` - Search packages

## Permissions and Sudo
- `chmod 755 filename` - Change file permissions
- `chmod +x filename` - Make file executable
- `chown user:group filename` - Change ownership
- `sudo` - Run command as superuser
- `sudo -i` - Start interactive shell as root
- `sudo -u username command` - Run command as specific user
- `sudo !!` - Run last command with sudo
- `sudo -l` - List user's sudo privileges
- `visudo` - Edit sudoers file safely
- `sudo passwd username` - Change user password
- `sudo systemctl start service` - Start system service
- `sudo systemctl stop service` - Stop system service
- `sudo systemctl restart service` - Restart system service
- `sudo systemctl status service` - Check service status
- `sudo journalctl -u service` - View service logs

## Compression and Archives
- `tar -czf archive.tar.gz directory` - Create tar.gz archive
- `tar -xzf archive.tar.gz` - Extract tar.gz archive
- `zip archive.zip files` - Create zip archive
- `unzip archive.zip` - Extract zip archive

## Running Programs with C
Make sure gcc is installed.

- `gcc [c filename].c -o [exe name]` - Compile C program
- `./[exe name]` - Run compiled program

## Basic Scripts

### 1. Backup Script
```bash
#!/bin/bash
# Backup script
SOURCE_DIR="/path/to/source"
BACKUP_DIR="/path/to/backup"
DATE=$(date +%Y%m%d_%H%M%S)

tar -czf "$BACKUP_DIR/backup_$DATE.tar.gz" "$SOURCE_DIR"
echo "Backup completed: backup_$DATE.tar.gz"
```

### 2. System Monitor Script
```bash
#!/bin/bash
# System monitoring script
echo "=== System Information ==="
echo "Date: $(date)"
echo "Uptime: $(uptime)"
echo "Memory Usage:"
free -h
echo "Disk Usage:"
df -h
echo "Top Processes:"
ps aux --sort=-%cpu | head -10
```

### 3. File Search Script
```bash
#!/bin/bash
# Search for files by name
if [ $# -eq 0 ]; then
    echo "Usage: $0 <search_term>"
    exit 1
fi

SEARCH_TERM="$1"
echo "Searching for files containing '$SEARCH_TERM'..."
find . -name "*$SEARCH_TERM*" -type f 2>/dev/null
```

### 4. Log Monitor Script
```bash
#!/bin/bash
# Monitor log file for errors
LOG_FILE="/var/log/syslog"
if [ ! -f "$LOG_FILE" ]; then
    echo "Log file not found: $LOG_FILE"
    exit 1
fi

echo "Monitoring $LOG_FILE for errors..."
tail -f "$LOG_FILE" | grep --color=always -i "error\|fail\|critical"
```

### 5. Network Check Script
```bash
#!/bin/bash
# Check network connectivity
HOSTS=("google.com" "github.com" "8.8.8.8")

for host in "${HOSTS[@]}"; do
    if ping -c 1 "$host" >/dev/null 2>&1; then
        echo "✓ $host is reachable"
    else
        echo "✗ $host is not reachable"
    fi
done
```

## Environment Variables
- `export VAR=value` - Set environment variable
- `echo $VAR` - Display environment variable
- `env` - List all environment variables
- `source ~/.bashrc` - Reload bash configuration

## Process Management
- `jobs` - List background jobs
- `bg` - Send job to background
- `fg` - Bring job to foreground
- `kill PID` - Kill process by PID
- `killall process_name` - Kill all processes by name
- `nohup command &` - Run command immune to hangups
