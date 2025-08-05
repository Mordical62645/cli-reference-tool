# Windows Basic Commands and Scripts

## File System Navigation
- `cd` or `chdir` - Change directory
- `cd ..` - Go to parent directory
- `cd \` - Go to root directory
- `cd %USERPROFILE%` - Go to user profile directory
- `dir` - List files and directories
- `dir /w` - Wide list format
- `dir /a` - Show all files including hidden
- `dir /s` - List files recursively
- `tree` - Display directory structure
- `tree /f` - Display files in tree structure

## File Operations
- `copy source destination` - Copy files
- `copy /s source destination` - Copy directories recursively
- `xcopy source destination /s /e` - Extended copy with subdirectories
- `move source destination` - Move/rename files
- `del filename` - Delete file
- `del /s filename` - Delete files recursively
- `rmdir directory` - Remove empty directory
- `rmdir /s directory` - Remove directory and contents
- `md directory` or `mkdir directory` - Create directory
- `type filename` - Display file contents
- `more filename` - Display file with pagination

## File Attributes and Permissions
- `attrib filename` - Display file attributes
- `attrib +r filename` - Set read-only attribute
- `attrib -r filename` - Remove read-only attribute
- `attrib +h filename` - Set hidden attribute
- `attrib -h filename` - Remove hidden attribute
- `icacls filename` - Display/modify file permissions
- `icacls filename /grant user:permissions` - Grant permissions
- `takeown /f filename` - Take ownership of file

## System Information
- `systeminfo` - Display system information
- `ver` - Display Windows version
- `hostname` - Display computer name
- `whoami` - Display current user
- `whoami /all` - Display user and group information
- `wmic os get Caption,Version` - Get OS information
- `wmic cpu get Name,NumberOfCores` - Get CPU information
- `wmic memorychip get Capacity,Speed` - Get memory information
- `wmic diskdrive get Size,Model` - Get disk information

## Process Management
- `tasklist` - List all running processes
- `tasklist /v` - Detailed process list
- `tasklist /fi "imagename eq process.exe"` - Filter processes
- `taskkill /im process.exe` - Kill process by name
- `taskkill /pid process_id` - Kill process by PID
- `taskkill /f /im process.exe` - Force kill process
- `start program.exe` - Start program
- `start /min program.exe` - Start program minimized
- `start /max program.exe` - Start program maximized

## Network Commands
- `ipconfig` - Display IP configuration
- `ipconfig /all` - Detailed IP configuration
- `ipconfig /release` - Release IP address
- `ipconfig /renew` - Renew IP address
- `ipconfig /flushdns` - Clear DNS cache
- `ping hostname` - Test network connectivity
- `ping -t hostname` - Continuous ping
- `tracert hostname` - Trace route to host
- `nslookup domain` - DNS lookup
- `netstat -an` - Display network connections
- `netstat -r` - Display routing table
- `arp -a` - Display ARP table

## Service Management
- `sc query` - List all services
- `sc query service_name` - Query specific service
- `sc start service_name` - Start service
- `sc stop service_name` - Stop service
- `sc config service_name start=auto` - Set service to auto-start
- `net start service_name` - Start service (alternative)
- `net stop service_name` - Stop service (alternative)

## User and Group Management
- `net user` - List all users
- `net user username` - Display user information
- `net user username password` - Set user password
- `net user username /add` - Add new user
- `net user username /delete` - Delete user
- `net localgroup` - List local groups
- `net localgroup groupname username /add` - Add user to group
- `net localgroup groupname username /delete` - Remove user from group

## Registry Operations
- `reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion"` - Query registry
- `reg add "HKLM\SOFTWARE\Example" /v "ValueName" /t REG_SZ /d "Value"` - Add registry value
- `reg delete "HKLM\SOFTWARE\Example" /v "ValueName"` - Delete registry value
- `reg export "HKLM\SOFTWARE\Example" filename.reg` - Export registry key
- `reg import filename.reg` - Import registry file

## Disk and Storage
- `diskpart` - Disk partitioning tool
- `chkdsk` - Check disk for errors
- `chkdsk /f` - Fix disk errors
- `chkdsk /r` - Locate bad sectors and recover information
- `defrag` - Defragment disk
- `defrag /a` - Analyze disk fragmentation
- `fsutil` - File system utility
- `fsutil volume diskfree C:` - Display disk space
- `wmic logicaldisk get size,freespace,caption` - Get disk space information

## System Maintenance
- `sfc /scannow` - System file checker
- `dism /online /cleanup-image /restorehealth` - Repair Windows image
- `cleanmgr` - Disk cleanup utility
- `msconfig` - System configuration
- `eventvwr` - Event viewer
- `perfmon` - Performance monitor
- `resmon` - Resource monitor
- `taskmgr` - Task manager

## Basic Scripts

### 1. System Information Script
```batch
@echo off
echo === System Information ===
echo Date: %date%
echo Time: %time%
echo Computer Name: %computername%
echo User: %username%
echo.
echo OS Information:
ver
echo.
echo System Information:
systeminfo | findstr /C:"OS Name" /C:"OS Version" /C:"System Type"
echo.
echo Disk Space:
wmic logicaldisk get size,freespace,caption
echo.
echo Memory Information:
wmic memorychip get capacity,speed
pause
```

### 2. Network Diagnostic Script
```batch
@echo off
echo === Network Diagnostics ===
echo.
echo IP Configuration:
ipconfig /all
echo.
echo Testing connectivity to Google DNS:
ping -n 4 8.8.8.8
echo.
echo DNS Resolution Test:
nslookup google.com
echo.
echo Network Connections:
netstat -an | findstr ESTABLISHED
pause
```

### 3. Service Management Script
```batch
@echo off
set /p service_name="Enter service name: "
set /p action="Enter action (start/stop/status): "

if "%action%"=="start" (
    sc start %service_name%
    echo Service %service_name% started
) else if "%action%"=="stop" (
    sc stop %service_name%
    echo Service %service_name% stopped
) else if "%action%"=="status" (
    sc query %service_name%
) else (
    echo Invalid action. Use start, stop, or status.
)
pause
```

### 4. File Backup Script
```batch
@echo off
set /p source="Enter source directory: "
set /p destination="Enter destination directory: "
set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%_%time:~0,2%%time:~3,2%%time:~6,2%

if not exist "%destination%" mkdir "%destination%"
xcopy "%source%" "%destination%\backup_%timestamp%" /s /e /i

echo Backup completed: backup_%timestamp%
pause
```

### 5. Process Monitor Script
```batch
@echo off
echo === Process Monitor ===
echo.
echo Top 10 processes by CPU usage:
wmic process get name,processid,workingsetsize /format:table | findstr /v "Name ProcessId WorkingSetSize" | sort /r
echo.
echo Memory usage by process:
tasklist /v /fo table | findstr /v "Image Name"
pause
```

## Environment Variables
- `set` - Display all environment variables
- `set variable=value` - Set environment variable
- `set /p variable=prompt` - Set variable with user input
- `echo %variable%` - Display environment variable
- `%USERPROFILE%` - User profile directory
- `%APPDATA%` - Application data directory
- `%TEMP%` - Temporary directory
- `%PATH%` - Executable search path

## Batch File Programming

### Conditional Statements
```batch
@echo off
if exist "file.txt" (
    echo File exists
) else (
    echo File does not exist
)

if "%1"=="" (
    echo No parameter provided
) else (
    echo Parameter: %1
)
```

### Loops
```batch
@echo off
for %%i in (1 2 3 4 5) do (
    echo Number: %%i
)

for /f "tokens=*" %%i in (file.txt) do (
    echo Line: %%i
)

for /l %%i in (1,1,10) do (
    echo Counter: %%i
)
```

### Functions
```batch
@echo off
call :myfunction "Hello World"
goto :eof

:myfunction
echo Function called with: %1
goto :eof
```

## Troubleshooting Commands
- `sfc /scannow` - Scan and repair system files
- `dism /online /cleanup-image /restorehealth` - Repair Windows image
- `chkdsk /f /r` - Check and repair disk
- `bootrec /fixmbr` - Fix master boot record
- `bootrec /fixboot` - Fix boot sector
- `bootrec /rebuildbcd` - Rebuild boot configuration data
- `netsh winsock reset` - Reset Winsock catalog
- `netsh int ip reset` - Reset TCP/IP stack

## Best Practices
- Use `xcopy` instead of `copy` for directories
- Use `robocopy` for robust file copying
- Use `wmic` for system information queries
- Use `sc` for service management
- Use `icacls` for advanced permission management
- Use `reg` for registry operations
- Use `netsh` for network configuration 