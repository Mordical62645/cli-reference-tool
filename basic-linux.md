# Linux Basic Commands and Scripts

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

## Permissions
- `chmod 755 filename` - Change file permissions
- `chmod +x filename` - Make file executable
- `chown user:group filename` - Change ownership
- `sudo` - Run command as superuser

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
