# PowerShell Basic Commands and Scripts

## File System Navigation
- `Get-Location` or `pwd` - Print working directory
- `Get-ChildItem` or `ls` - List files and directories
- `Get-ChildItem -Force` - List all files including hidden
- `Set-Location path` or `cd path` - Change directory
- `cd ..` - Go to parent directory
- `cd ~` - Go to home directory
- `cd C:\` - Go to C drive root

## File Operations
- `New-Item filename.txt -ItemType File` or `ni filename.txt -it file` - Create empty file
- `New-Item foldername -ItemType Directory` or `mkdir foldername` - Create directory
- `New-Item -Path "parent\child" -ItemType Directory -Force` - Create nested directories
- `Copy-Item source destination` or `cp source destination` - Copy files/directories
- `Copy-Item source destination -Recurse` - Copy directories recursively
- `Move-Item source destination` or `mv source destination` - Move/rename files
- `Remove-Item filename` or `rm filename` - Remove file
- `Remove-Item directory -Recurse` - Remove directory recursively
- `Remove-Item filename -Force` - Force remove without confirmation
- `Get-ChildItem -Recurse -Directory | Where-Object {($_.GetFileSystemInfos().Count -eq 0)} | ForEach-Object { New-Item -Path "$($_.FullName)\filename" -ItemType File }` - fill empty folders with file (often used wih .gitkeep)

## File Viewing and Editing
- `Get-Content filename` or `cat filename` - Display file contents
- `Get-Content filename | Out-Host -Paging` - View file with pagination
- `Get-Content filename | Select-Object -First 10` - Show first 10 lines
- `Get-Content filename | Select-Object -Last 10` - Show last 10 lines
- `Get-Content filename -Wait` - Follow file changes
- `notepad filename` - Edit file with Notepad
- `code filename` - Edit file with VS Code (if installed)

## Text Processing
- `Select-String "pattern" filename` - Search for pattern in file
- `Select-String "pattern" filename -CaseSensitive` - Case-sensitive search
- `Get-ChildItem -Recurse | Select-String "pattern"` - Search recursively
- `(Get-Content filename) -replace "old", "new" | Set-Content filename` - Replace text
- `Get-Content filename | ForEach-Object { $_.Split()[0] }` - Print first column
- `Get-Content filename | Sort-Object` - Sort lines
- `Get-Content filename | Get-Unique` - Remove duplicate lines
- `Get-Content filename | Measure-Object -Line -Word -Character` - Count lines, words, characters

## System Information
- `Get-ComputerInfo` - System information
- `$env:USERNAME` - Current user
- `whoami` - Current user
- `Get-Process` - List all processes
- `Get-Process | Sort-Object CPU -Descending | Select-Object -First 10` - Top processes
- `Get-WmiObject -Class Win32_LogicalDisk` - Disk space usage
- `Get-ChildItem directory | Measure-Object -Property Length -Sum` - Directory size
- `Get-WmiObject -Class Win32_OperatingSystem | Select-Object TotalVisibleMemorySize, FreePhysicalMemory` - Memory usage

## Network Commands
- `Test-NetConnection hostname` - Test connectivity
- `Resolve-DnsName domain` - DNS lookup
- `Invoke-WebRequest url -OutFile filename` - Download file
- `Invoke-RestMethod url` - Transfer data
- `Enter-PSSession -ComputerName host` - Connect via PowerShell remoting
- `Copy-Item file -Destination "\\host\path"` - Copy file to remote host

## Windows Features
- `Get-Service` - List all services
- `Start-Service service_name` - Start service
- `Stop-Service service_name` - Stop service
- `Get-WmiObject -Class Win32_Product` - List installed programs
- `Get-HotFix` - List installed updates

## Registry Operations
- `Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion"` - Read registry
- `Set-ItemProperty -Path "HKLM:\SOFTWARE\Example" -Name "ValueName" -Value "Value"` - Write registry
- `New-Item -Path "HKLM:\SOFTWARE\Example" -Force` - Create registry key

## Permissions
- `Get-Acl filename` - Get file permissions
- `Set-Acl filename -AclObject acl` - Set file permissions
- `icacls filename` - View/modify file permissions
- `Start-Process -FilePath "program" -Verb RunAs` - Run as administrator

## Compression and Archives
- `Compress-Archive -Path directory -DestinationPath archive.zip` - Create zip archive
- `Expand-Archive -Path archive.zip -DestinationPath directory` - Extract zip archive
- `tar -czf archive.tar.gz directory` - Create tar.gz archive (if tar available)
- `tar -xzf archive.tar.gz` - Extract tar.gz archive

## Running Programs with C
Make sure gcc is installed.

- `gcc [c filename].c -o [exe name]` - Compile C program
- `.\`[exe name].exe` - Run compiled program

## Basic Scripts

### 1. Backup Script
```powershell
# Backup script
$SourceDir = "C:\path\to\source"
$BackupDir = "C:\path\to\backup"
$Date = Get-Date -Format "yyyyMMdd_HHmmss"

$BackupFile = "$BackupDir\backup_$Date.zip"
Compress-Archive -Path $SourceDir -DestinationPath $BackupFile
Write-Host "Backup completed: $BackupFile"
```

### 2. System Monitor Script
```powershell
# System monitoring script
Write-Host "=== System Information ==="
Write-Host "Date: $(Get-Date)"
Write-Host "Uptime: $((Get-Date) - (Get-CimInstance Win32_OperatingSystem).LastBootUpTime)"

Write-Host "Memory Usage:"
Get-WmiObject -Class Win32_OperatingSystem | Select-Object TotalVisibleMemorySize, FreePhysicalMemory

Write-Host "Disk Usage:"
Get-WmiObject -Class Win32_LogicalDisk | Select-Object DeviceID, Size, FreeSpace

Write-Host "Top Processes:"
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 Name, CPU, WorkingSet
```

### 3. File Search Script
```powershell
# Search for files by name
param(
    [Parameter(Mandatory=$true)]
    [string]$SearchTerm
)

Write-Host "Searching for files containing '$SearchTerm'..."
Get-ChildItem -Recurse -Name "*$SearchTerm*" -ErrorAction SilentlyContinue
```

### 4. Log Monitor Script
```powershell
# Monitor Windows Event Log for errors
$LogName = "Application"
$Events = Get-WinEvent -LogName $LogName -MaxEvents 100 | Where-Object { $_.Level -eq 2 }

Write-Host "Recent Error Events from $LogName:"
$Events | ForEach-Object {
    Write-Host "$($_.TimeCreated): $($_.Message)" -ForegroundColor Red
}
```

### 5. Network Check Script
```powershell
# Check network connectivity
$Hosts = @("google.com", "github.com", "8.8.8.8")

foreach ($host in $Hosts) {
    if (Test-NetConnection -ComputerName $host -InformationLevel Quiet) {
        Write-Host "✓ $host is reachable" -ForegroundColor Green
    } else {
        Write-Host "✗ $host is not reachable" -ForegroundColor Red
    }
}
```

### 6. Service Management Script
```powershell
# Service management script
param(
    [Parameter(Mandatory=$true)]
    [string]$ServiceName,
    [Parameter(Mandatory=$true)]
    [ValidateSet("Start", "Stop", "Restart", "Status")]
    [string]$Action
)

switch ($Action) {
    "Start" { Start-Service -Name $ServiceName }
    "Stop" { Stop-Service -Name $ServiceName }
    "Restart" { Restart-Service -Name $ServiceName }
    "Status" { Get-Service -Name $ServiceName }
}
```

## Environment Variables
- `$env:VAR = "value"` - Set environment variable
- `$env:VAR` - Display environment variable
- `Get-ChildItem Env:` - List all environment variables
- `. $PROFILE` - Reload PowerShell profile

## Process Management
- `Get-Job` - List background jobs
- `Start-Job -ScriptBlock { command }` - Start background job
- `Receive-Job -Job job_id` - Get job output
- `Stop-Job -Job job_id` - Stop background job
- `Remove-Job -Job job_id` - Remove completed job
- `Get-Process | Where-Object { $_.ProcessName -eq "process_name" } | Stop-Process` - Kill process by name

## PowerShell Profiles
- `$PROFILE` - Display profile path
- `Test-Path $PROFILE` - Check if profile exists
- `New-Item -Path $PROFILE -Type File -Force` - Create profile
- `notepad $PROFILE` - Edit profile

## Remote Management
- `Enter-PSSession -ComputerName host` - Enter remote session
- `Invoke-Command -ComputerName host -ScriptBlock { command }` - Run command remotely
- `New-PSSession -ComputerName host` - Create persistent session
- `Get-PSSession` - List active sessions
