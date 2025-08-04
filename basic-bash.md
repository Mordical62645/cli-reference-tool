# Bash Basic Commands and Scripts

### Shell Information
- `bash --version` - Check Bash version
- `echo $BASH_VERSION` - Display Bash version
- `echo $SHELL` - Show current shell
- `ps -p $$` - Show current shell process
- `which bash` - Find Bash executable location

### File Navigation
- `pwd` - Print working directory
- `cd directory` - Change directory
- `cd ~` - Go to home directory
- `cd -` - Go to previous directory
- `pushd directory` - Push directory onto stack
- `popd` - Pop directory from stack
- `dirs -v` - List directory stack

### File Operations
- `ls` - List files and directories
- `ls -la` - List all files with details
- `ls -lh` - List with human-readable sizes
- `ls -t` - Sort by modification time
- `ls -S` - Sort by file size
- `cp source destination` - Copy files
- `cp -r source destination` - Copy directories recursively
- `mv source destination` - Move/rename files
- `rm filename` - Remove file
- `rm -r directory` - Remove directory recursively
- `rm -f filename` - Force remove without confirmation

### File Viewing and Editing
- `cat filename` - Display file contents
- `less filename` - View file with pagination
- `head filename` - Show first 10 lines
- `head -n 20 filename` - Show first 20 lines
- `tail filename` - Show last 10 lines
- `tail -f filename` - Follow file changes
- `tail -n 20 filename` - Show last 20 lines

## Bash Built-in Commands

### Variable Management
- `declare variable=value` - Declare variable
- `export variable=value` - Export variable to environment
- `unset variable` - Remove variable
- `readonly variable=value` - Make variable read-only
- `local variable=value` - Declare local variable (in functions)

### String Operations
- `echo "text"` - Print text
- `printf "format" arguments` - Formatted output
- `read variable` - Read input into variable
- `read -p "prompt" variable` - Read with prompt
- `read -s variable` - Read silently (for passwords)

### Array Operations
- `array=(item1 item2 item3)` - Create array
- `echo ${array[0]}` - Access array element
- `echo ${array[@]}` - Access all elements
- `echo ${#array[@]}` - Array length
- `array+=(new_item)` - Add to array

### Arithmetic
- `let "result = 5 + 3"` - Arithmetic operation
- `echo $((5 + 3))` - Arithmetic expansion
- `expr 5 + 3` - External arithmetic
- `bc` - Calculator program

## Bash Control Structures

### Conditional Statements
```bash
# if statement
if [ condition ]; then
    commands
elif [ condition ]; then
    commands
else
    commands
fi

# case statement
case $variable in
    pattern1)
        commands
        ;;
    pattern2)
        commands
        ;;
    *)
        commands
        ;;
esac
```

### Loops
```bash
# for loop
for item in list; do
    commands
done

# for loop with range
for i in {1..10}; do
    echo $i
done

# while loop
while [ condition ]; do
    commands
done

# until loop
until [ condition ]; do
    commands
done

# select loop
select option in "Option 1" "Option 2" "Quit"; do
    case $option in
        "Option 1") echo "You chose 1";;
        "Option 2") echo "You chose 2";;
        "Quit") break;;
    esac
done
```

## Bash Functions

### Function Definition
```bash
# Simple function
function_name() {
    commands
}

# Function with parameters
function_name() {
    local param1=$1
    local param2=$2
    echo "Parameters: $param1, $param2"
}

# Function with return value
function_name() {
    local result=$(( $1 + $2 ))
    return $result
}
```

### Function Usage
```bash
# Call function
function_name

# Call with parameters
function_name "arg1" "arg2"

# Capture return value
function_name 5 3
echo $?  # Print return value
```

## Bash Parameter Expansion

### Basic Expansion
- `$variable` - Variable value
- `${variable}` - Variable value (explicit)
- `${variable:-default}` - Use default if variable is empty
- `${variable:=default}` - Set default if variable is empty
- `${variable:+value}` - Use value if variable is not empty

### String Operations
- `${variable#pattern}` - Remove shortest match from beginning
- `${variable##pattern}` - Remove longest match from beginning
- `${variable%pattern}` - Remove shortest match from end
- `${variable%%pattern}` - Remove longest match from end
- `${variable/pattern/replacement}` - Replace first match
- `${variable//pattern/replacement}` - Replace all matches

### Array Operations
- `${array[@]}` - All array elements
- `${array[*]}` - All array elements as single string
- `${#array[@]}` - Number of array elements
- `${array[@]:start:length}` - Subset of array

## Bash Scripting Examples

### 1. File Backup Script
```bash
#!/bin/bash
# Backup script with error handling

set -e  # Exit on error

BACKUP_DIR="/backup"
SOURCE_DIR="$1"
DATE=$(date +%Y%m%d_%H%M%S)

if [ -z "$SOURCE_DIR" ]; then
    echo "Usage: $0 <source_directory>"
    exit 1
fi

if [ ! -d "$SOURCE_DIR" ]; then
    echo "Error: Source directory does not exist"
    exit 1
fi

BACKUP_NAME="backup_${DATE}.tar.gz"
tar -czf "$BACKUP_DIR/$BACKUP_NAME" "$SOURCE_DIR"

if [ $? -eq 0 ]; then
    echo "Backup completed: $BACKUP_NAME"
else
    echo "Backup failed"
    exit 1
fi
```

### 2. System Monitor Script
```bash
#!/bin/bash
# System monitoring with functions

get_system_info() {
    echo "=== System Information ==="
    echo "Date: $(date)"
    echo "Uptime: $(uptime)"
    echo "Hostname: $(hostname)"
    echo "Kernel: $(uname -r)"
}

get_memory_info() {
    echo "=== Memory Information ==="
    free -h
    echo
    echo "Memory usage by process:"
    ps aux --sort=-%mem | head -10
}

get_disk_info() {
    echo "=== Disk Information ==="
    df -h
    echo
    echo "Largest directories in /home:"
    du -h /home | sort -hr | head -10
}

get_network_info() {
    echo "=== Network Information ==="
    ip addr show
    echo
    echo "Active connections:"
    ss -tuln
}

# Main execution
main() {
    get_system_info
    echo
    get_memory_info
    echo
    get_disk_info
    echo
    get_network_info
}

main "$@"
```

### 3. Interactive Menu Script
```bash
#!/bin/bash
# Interactive menu system

show_menu() {
    echo "=== System Management Menu ==="
    echo "1. Check system status"
    echo "2. Update system"
    echo "3. Backup important files"
    echo "4. Check disk space"
    echo "5. View system logs"
    echo "6. Exit"
    echo
    read -p "Select an option: " choice
}

check_system_status() {
    echo "System Status:"
    uptime
    echo "Memory Usage:"
    free -h
    echo "Disk Usage:"
    df -h
}

update_system() {
    echo "Updating system..."
    sudo apt update && sudo apt upgrade -y
}

backup_files() {
    echo "Creating backup..."
    tar -czf "backup_$(date +%Y%m%d_%H%M%S).tar.gz" ~/Documents
    echo "Backup completed"
}

check_disk_space() {
    echo "Disk Space:"
    df -h
    echo
    echo "Largest directories:"
    du -h ~ | sort -hr | head -10
}

view_logs() {
    echo "Recent system logs:"
    tail -20 /var/log/syslog
}

# Main loop
while true; do
    show_menu
    case $choice in
        1) check_system_status ;;
        2) update_system ;;
        3) backup_files ;;
        4) check_disk_space ;;
        5) view_logs ;;
        6) echo "Goodbye!"; exit 0 ;;
        *) echo "Invalid option. Please try again." ;;
    esac
    echo
    read -p "Press Enter to continue..."
done
```

### 4. Process Management Script
```bash
#!/bin/bash
# Process management utilities

list_processes() {
    echo "=== Process List ==="
    ps aux --sort=-%cpu | head -10
}

kill_process() {
    local pid=$1
    if [ -z "$pid" ]; then
        read -p "Enter process ID: " pid
    fi
    
    if kill -0 "$pid" 2>/dev/null; then
        echo "Killing process $pid..."
        kill "$pid"
        echo "Process killed"
    else
        echo "Process $pid not found"
    fi
}

monitor_process() {
    local process_name=$1
    if [ -z "$process_name" ]; then
        read -p "Enter process name: " process_name
    fi
    
    echo "Monitoring $process_name..."
    while true; do
        if pgrep "$process_name" >/dev/null; then
            echo "$(date): $process_name is running"
        else
            echo "$(date): $process_name is not running"
        fi
        sleep 5
    done
}

# Usage examples
case "$1" in
    "list") list_processes ;;
    "kill") kill_process "$2" ;;
    "monitor") monitor_process "$2" ;;
    *) echo "Usage: $0 {list|kill|monitor}"; exit 1 ;;
esac
```

### 5. Configuration Management Script
```bash
#!/bin/bash
# Configuration file management

CONFIG_DIR="$HOME/.config"
BACKUP_DIR="$HOME/.config_backup"

backup_config() {
    local config_file="$1"
    if [ -f "$config_file" ]; then
        local backup_name="$(basename "$config_file").backup.$(date +%Y%m%d_%H%M%S)"
        cp "$config_file" "$BACKUP_DIR/$backup_name"
        echo "Backed up: $backup_name"
    else
        echo "Config file not found: $config_file"
    fi
}

restore_config() {
    local config_file="$1"
    local backup_file="$2"
    
    if [ -f "$BACKUP_DIR/$backup_file" ]; then
        cp "$BACKUP_DIR/$backup_file" "$config_file"
        echo "Restored: $config_file"
    else
        echo "Backup file not found: $backup_file"
    fi
}

list_backups() {
    echo "Available backups:"
    ls -la "$BACKUP_DIR" 2>/dev/null || echo "No backups found"
}

# Create backup directory if it doesn't exist
mkdir -p "$BACKUP_DIR"

case "$1" in
    "backup") backup_config "$2" ;;
    "restore") restore_config "$2" "$3" ;;
    "list") list_backups ;;
    *) echo "Usage: $0 {backup|restore|list} [file] [backup_file]"; exit 1 ;;
esac
```

## Bash Configuration

### Bash Profile Files
- `~/.bash_profile` - Login shell configuration
- `~/.bashrc` - Interactive shell configuration
- `~/.bash_logout` - Logout configuration
- `/etc/bash.bashrc` - System-wide configuration

### Common Aliases
```bash
# Add to ~/.bashrc
alias ll='ls -la'
alias la='ls -A'
alias l='ls -CF'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'
alias fgrep='fgrep --color=auto'
alias egrep='egrep --color=auto'
alias h='history'
alias j='jobs -l'
```

### Bash Options
```bash
# Set options in ~/.bashrc
set -o vi  # Use vi keybindings
set -o emacs  # Use emacs keybindings
set -o ignoreeof  # Prevent accidental logout
set -o noclobber  # Prevent overwriting files
```

## Best Practices

### Scripting
- Always use `#!/bin/bash` shebang
- Use `set -e` for error handling
- Quote variables: `"$variable"`
- Use `local` for function variables
- Check return codes

### Security
- Validate input parameters
- Use `readonly` for constants
- Avoid `eval` when possible
- Sanitize user input

### Performance
- Use built-ins over external commands
- Minimize subshells
- Use arrays for multiple values
- Profile scripts with `time` 