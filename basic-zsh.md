# Zsh Basic Commands and Scripts

## Zsh Shell Overview
Zsh (Z Shell) is a powerful Unix shell designed for interactive use. It's the default shell on macOS since Catalina (2019) and provides advanced features like spelling correction, path expansion, and plugin support.

## Basic Zsh Commands

### Shell Information
- `zsh --version` - Check Zsh version
- `echo $ZSH_VERSION` - Display Zsh version
- `echo $SHELL` - Show current shell
- `ps -p $$` - Show current shell process
- `which zsh` - Find Zsh executable location
- `echo $ZSH_NAME` - Show Zsh name

### File Navigation
- `pwd` - Print working directory
- `cd directory` - Change directory
- `cd ~` - Go to home directory
- `cd -` - Go to previous directory
- `cd -2` - Go to second previous directory
- `pushd directory` - Push directory onto stack
- `popd` - Pop directory from stack
- `dirs -v` - List directory stack
- `cd +2` - Go to directory at position 2 in stack

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

## Zsh Built-in Commands

### Variable Management
- `typeset variable=value` - Declare variable
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
- `echo ${array[1]}` - Access array element (1-indexed)
- `echo ${array[@]}` - Access all elements
- `echo ${#array[@]}` - Array length
- `array+=(new_item)` - Add to array

### Arithmetic
- `let "result = 5 + 3"` - Arithmetic operation
- `echo $((5 + 3))` - Arithmetic expansion
- `expr 5 + 3` - External arithmetic
- `bc` - Calculator program

## Zsh-Specific Features

### Globbing (Path Expansion)
- `ls *.txt` - List all .txt files
- `ls **/*.txt` - Recursive globbing (find all .txt files)
- `ls **/*.{txt,md}` - Multiple extensions
- `ls **/*(.)` - Only regular files
- `ls **/*(/)` - Only directories
- `ls **/*(@)` - Only symbolic links
- `ls **/*(L+100)` - Files larger than 100 bytes

### Brace Expansion
- `echo {1..5}` - Range expansion: 1 2 3 4 5
- `echo {a..e}` - Letter range: a b c d e
- `echo {01..05}` - Zero-padded: 01 02 03 04 05
- `mkdir -p dir{1,2,3}` - Create multiple directories
- `cp file.txt{,.backup}` - Copy with backup extension

### Parameter Expansion
- `${variable:-default}` - Use default if variable is empty
- `${variable:=default}` - Set default if variable is empty
- `${variable:+value}` - Use value if variable is not empty
- `${variable:offset:length}` - Substring extraction
- `${variable#pattern}` - Remove shortest match from beginning
- `${variable##pattern}` - Remove longest match from beginning
- `${variable%pattern}` - Remove shortest match from end
- `${variable%%pattern}` - Remove longest match from end

### History Features
- `history` - Show command history
- `history 10` - Show last 10 commands
- `!n` - Execute command number n
- `!-n` - Execute n-th command from end
- `!string` - Execute last command starting with string
- `!?string` - Execute last command containing string
- `!!` - Execute last command
- `!$` - Last argument of previous command

### Directory Stack
- `pushd directory` - Push directory onto stack
- `popd` - Pop directory from stack
- `dirs -v` - List directory stack with numbers
- `cd +n` - Go to directory at position n
- `cd -n` - Go to directory n positions from end

## Zsh Control Structures

### Conditional Statements
```zsh
# if statement
if [[ condition ]]; then
    commands
elif [[ condition ]]; then
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
```zsh
# for loop
for item in list; do
    commands
done

# for loop with range
for i in {1..10}; do
    echo $i
done

# while loop
while [[ condition ]]; do
    commands
done

# until loop
until [[ condition ]]; do
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

## Zsh Functions

### Function Definition
```zsh
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
```zsh
# Call function
function_name

# Call with parameters
function_name "arg1" "arg2"

# Capture return value
function_name 5 3
echo $?  # Print return value
```

## Zsh Scripting Examples

### 1. Advanced File Management Script
```zsh
#!/bin/zsh
# Advanced file operations with Zsh features

setopt extended_glob  # Enable extended globbing

# Function to find and process files
process_files() {
    local pattern=$1
    local action=$2
    
    case $action in
        "list")
            echo "Files matching pattern: $pattern"
            ls -la $~pattern 2>/dev/null || echo "No files found"
            ;;
        "backup")
            echo "Creating backups..."
            for file in $~pattern; do
                if [[ -f $file ]]; then
                    cp "$file" "${file}.backup.$(date +%Y%m%d_%H%M%S)"
                    echo "Backed up: $file"
                fi
            done
            ;;
        "compress")
            echo "Compressing files..."
            for file in $~pattern; do
                if [[ -f $file ]]; then
                    gzip "$file"
                    echo "Compressed: $file"
                fi
            done
            ;;
        *)
            echo "Usage: $0 <pattern> {list|backup|compress}"
            exit 1
            ;;
    esac
}

# Usage
process_files "$1" "$2"
```

### 2. Interactive Development Environment Script
```zsh
#!/bin/zsh
# Interactive development environment setup

# Function to create project structure
create_project() {
    local project_name=$1
    local project_type=$2
    
    if [[ -z $project_name ]]; then
        read "project_name?Enter project name: "
    fi
    
    mkdir -p "$project_name"
    cd "$project_name"
    
    case $project_type in
        "python")
            echo "Setting up Python project..."
            mkdir -p {src,tests,docs}
            touch {README.md,requirements.txt,.gitignore}
            echo "# $project_name" > README.md
            ;;
        "node")
            echo "Setting up Node.js project..."
            mkdir -p {src,tests,docs}
            touch {README.md,package.json,.gitignore}
            echo "# $project_name" > README.md
            ;;
        "web")
            echo "Setting up Web project..."
            mkdir -p {css,js,images}
            touch {index.html,README.md}
            echo "# $project_name" > README.md
            ;;
        *)
            echo "Creating basic project structure..."
            mkdir -p {src,docs}
            touch README.md
            echo "# $project_name" > README.md
            ;;
    esac
    
    echo "Project '$project_name' created successfully!"
}

# Function to manage development tools
manage_dev_tools() {
    echo "=== Development Tools Manager ==="
    echo "1. Install development tools"
    echo "2. Update development tools"
    echo "3. Check tool versions"
    echo "4. Setup development environment"
    echo "5. Exit"
    
    read "choice?Select option: "
    
    case $choice in
        1)
            echo "Installing development tools..."
            # Add installation commands here
            ;;
        2)
            echo "Updating development tools..."
            # Add update commands here
            ;;
        3)
            echo "Tool versions:"
            echo "Python: $(python3 --version 2>/dev/null || echo 'Not installed')"
            echo "Node.js: $(node --version 2>/dev/null || echo 'Not installed')"
            echo "Git: $(git --version 2>/dev/null || echo 'Not installed')"
            ;;
        4)
            echo "Setting up development environment..."
            # Add setup commands here
            ;;
        5)
            echo "Goodbye!"
            exit 0
            ;;
        *)
            echo "Invalid option"
            ;;
    esac
}

# Main execution
case "$1" in
    "create") create_project "$2" "$3" ;;
    "tools") manage_dev_tools ;;
    *) echo "Usage: $0 {create|tools} [project_name] [type]"; exit 1 ;;
esac
```

### 3. Advanced System Monitor Script
```zsh
#!/bin/zsh
# Advanced system monitoring with Zsh features

# Colors for output
autoload -U colors && colors

# Function to get system information
get_system_info() {
    echo "${fg[green]}=== System Information ===${reset_color}"
    echo "Date: $(date)"
    echo "Uptime: $(uptime)"
    echo "Hostname: $(hostname)"
    echo "Kernel: $(uname -r)"
    echo "Shell: $SHELL"
    echo "Zsh version: $ZSH_VERSION"
}

# Function to get memory information with colors
get_memory_info() {
    echo "${fg[yellow]}=== Memory Information ===${reset_color}"
    free -h | while read line; do
        if [[ $line == *"Mem:"* ]]; then
            echo "${fg[cyan]}$line${reset_color}"
        elif [[ $line == *"Swap:"* ]]; then
            echo "${fg[red]}$line${reset_color}"
        else
            echo "$line"
        fi
    done
}

# Function to get disk information with smart formatting
get_disk_info() {
    echo "${fg[blue]}=== Disk Information ===${reset_color}"
    df -h | while read line; do
        if [[ $line == *"/dev/"* ]]; then
            # Color code based on usage
            usage=$(echo $line | awk '{print $5}' | sed 's/%//')
            if [[ $usage -gt 90 ]]; then
                echo "${fg[red]}$line${reset_color}"
            elif [[ $usage -gt 70 ]]; then
                echo "${fg[yellow]}$line${reset_color}"
            else
                echo "${fg[green]}$line${reset_color}"
            fi
        else
            echo "$line"
        fi
    done
}

# Function to get network information
get_network_info() {
    echo "${fg[magenta]}=== Network Information ===${reset_color}"
    echo "Active interfaces:"
    ip addr show | grep -E "^[0-9]+:" | while read line; do
        echo "${fg[cyan]}$line${reset_color}"
    done
    
    echo "Active connections:"
    ss -tuln | head -10
}

# Function to monitor specific processes
monitor_processes() {
    local processes=($@)
    
    if [[ ${#processes[@]} -eq 0 ]]; then
        processes=(nginx apache mysql postgres)
    fi
    
    echo "${fg[green]}=== Process Monitoring ===${reset_color}"
    for process in $processes; do
        if pgrep $process >/dev/null; then
            echo "${fg[green]}✓ $process is running${reset_color}"
        else
            echo "${fg[red]}✗ $process is not running${reset_color}"
        fi
    done
}

# Main function
main() {
    get_system_info
    echo
    get_memory_info
    echo
    get_disk_info
    echo
    get_network_info
    echo
    monitor_processes $@
}

# Execute main function with all arguments
main $@
```

### 4. Smart File Operations Script
```zsh
#!/bin/zsh
# Smart file operations using Zsh features

setopt extended_glob  # Enable extended globbing
setopt null_glob      # Don't error on no matches

# Function to find files by type
find_files_by_type() {
    local type=$1
    local pattern=$2
    
    case $type in
        "text")
            echo "Text files:"
            ls -la **/*.(txt|md|log|conf|cfg|ini|json|xml|yaml|yml) 2>/dev/null
            ;;
        "code")
            echo "Code files:"
            ls -la **/*.(py|js|ts|java|c|cpp|h|hpp|php|rb|go|rs|swift) 2>/dev/null
            ;;
        "media")
            echo "Media files:"
            ls -la **/*.(mp3|mp4|avi|mkv|jpg|jpeg|png|gif|pdf) 2>/dev/null
            ;;
        "archive")
            echo "Archive files:"
            ls -la **/*.(zip|tar|gz|bz2|rar|7z) 2>/dev/null
            ;;
        *)
            echo "Usage: $0 {text|code|media|archive} [pattern]"
            exit 1
            ;;
    esac
}

# Function to organize files
organize_files() {
    local source_dir=$1
    
    if [[ -z $source_dir ]]; then
        source_dir="."
    fi
    
    echo "Organizing files in $source_dir..."
    
    # Create directories
    mkdir -p {Documents,Images,Music,Videos,Archives,Code}
    
    # Move files by type
    mv **/*.(txt|md|pdf|doc|docx) Documents/ 2>/dev/null || true
    mv **/*.(jpg|jpeg|png|gif|bmp|svg) Images/ 2>/dev/null || true
    mv **/*.(mp3|wav|flac|ogg) Music/ 2>/dev/null || true
    mv **/*.(mp4|avi|mkv|mov|wmv) Videos/ 2>/dev/null || true
    mv **/*.(zip|tar|gz|bz2|rar|7z) Archives/ 2>/dev/null || true
    mv **/*.(py|js|ts|java|c|cpp|php|rb|go|rs) Code/ 2>/dev/null || true
    
    echo "Files organized successfully!"
}

# Function to clean up files
cleanup_files() {
    local days=$1
    
    if [[ -z $days ]]; then
        days=30
    fi
    
    echo "Cleaning up files older than $days days..."
    
    # Find and remove old files
    find . -type f -mtime +$days -exec rm -f {} \; 2>/dev/null
    find . -type d -empty -delete 2>/dev/null
    
    echo "Cleanup completed!"
}

# Main execution
case "$1" in
    "find") find_files_by_type "$2" "$3" ;;
    "organize") organize_files "$2" ;;
    "cleanup") cleanup_files "$2" ;;
    *) echo "Usage: $0 {find|organize|cleanup} [type|directory|days]"; exit 1 ;;
esac
```

### 5. Development Environment Script
```zsh
#!/bin/zsh
# Development environment management

# Function to setup development environment
setup_dev_env() {
    local project_type=$1
    
    echo "Setting up development environment for $project_type..."
    
    case $project_type in
        "python")
            echo "Setting up Python environment..."
            python3 -m venv venv
            source venv/bin/activate
            pip install --upgrade pip
            touch requirements.txt
            echo "Python environment ready!"
            ;;
        "node")
            echo "Setting up Node.js environment..."
            npm init -y
            npm install --save-dev nodemon
            echo "Node.js environment ready!"
            ;;
        "react")
            echo "Setting up React environment..."
            npx create-react-app .
            echo "React environment ready!"
            ;;
        "vue")
            echo "Setting up Vue environment..."
            npm create vue@latest .
            echo "Vue environment ready!"
            ;;
        *)
            echo "Unknown project type: $project_type"
            echo "Available types: python, node, react, vue"
            exit 1
            ;;
    esac
}

# Function to manage dependencies
manage_dependencies() {
    local action=$1
    local package=$2
    
    case $action in
        "install")
            if [[ -f package.json ]]; then
                npm install $package
            elif [[ -f requirements.txt ]]; then
                pip install $package
            else
                echo "No package manager found"
            fi
            ;;
        "update")
            if [[ -f package.json ]]; then
                npm update
            elif [[ -f requirements.txt ]]; then
                pip install --upgrade -r requirements.txt
            else
                echo "No package manager found"
            fi
            ;;
        "list")
            if [[ -f package.json ]]; then
                npm list
            elif [[ -f requirements.txt ]]; then
                pip list
            else
                echo "No package manager found"
            fi
            ;;
        *)
            echo "Usage: $0 deps {install|update|list} [package]"
            exit 1
            ;;
    esac
}

# Main execution
case "$1" in
    "setup") setup_dev_env "$2" ;;
    "deps") manage_dependencies "$2" "$3" ;;
    *) echo "Usage: $0 {setup|deps} [type|action] [package]"; exit 1 ;;
esac
```

## Zsh Configuration

### Zsh Profile Files
- `~/.zshrc` - Main configuration file
- `~/.zshenv` - Environment variables
- `~/.zprofile` - Login shell configuration
- `~/.zlogout` - Logout configuration
- `/etc/zshrc` - System-wide configuration

### Common Aliases
```zsh
# Add to ~/.zshrc
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
alias cls='clear'
alias md='mkdir -p'
alias rd='rmdir'
```

### Zsh Options
```zsh
# Set options in ~/.zshrc
setopt auto_cd              # Change directory without cd
setopt auto_pushd           # Push directories onto stack
setopt pushd_ignore_dups    # Don't push duplicate directories
setopt extended_glob        # Enable extended globbing
setopt null_glob           # Don't error on no matches
setopt hist_ignore_dups    # Don't store duplicate commands
setopt hist_ignore_space   # Don't store commands starting with space
setopt share_history       # Share history between sessions
```

## Best Practices

### Scripting
- Always use `#!/bin/zsh` shebang
- Use `setopt` for Zsh-specific features
- Use `[[ ]]` for conditional tests
- Quote variables: `"$variable"`
- Use `local` for function variables

### Performance
- Use built-ins over external commands
- Use arrays for multiple values
- Profile scripts with `time`
- Use Zsh-specific features when appropriate

### Compatibility
- Test scripts on different systems
- Use POSIX-compliant syntax when needed
- Document Zsh-specific features
- Provide fallbacks for missing features 