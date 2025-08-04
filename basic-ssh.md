# SSH Basic Commands and Configurations

## Basic SSH Connections
- `ssh user@hostname` - Connect to remote host
- `ssh user@hostname -p port` - Connect to specific port
- `ssh -i keyfile user@hostname` - Connect using specific key
- `ssh -v user@hostname` - Verbose connection (debug)
- `ssh -X user@hostname` - Enable X11 forwarding
- `ssh -L local_port:remote_host:remote_port user@hostname` - Local port forwarding
- `ssh -R remote_port:local_host:local_port user@hostname` - Remote port forwarding
- `ssh -D local_port user@hostname` - Dynamic port forwarding (SOCKS proxy)

## SSH Key Management

### Generate SSH Keys
- `ssh-keygen -t rsa -b 4096` - Generate RSA key (4096 bits)
- `ssh-keygen -t ed25519` - Generate Ed25519 key (recommended)
- `ssh-keygen -t ecdsa -b 521` - Generate ECDSA key
- `ssh-keygen -f ~/.ssh/id_rsa` - Generate key with specific filename
- `ssh-keygen -C "your_email@example.com"` - Add comment to key

### Copy Keys to Remote Host
- `ssh-copy-id user@hostname` - Copy public key to remote host
- `ssh-copy-id -i ~/.ssh/id_ed25519.pub user@hostname` - Copy specific key
- `cat ~/.ssh/id_rsa.pub | ssh user@hostname "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"` - Manual copy

### Key Permissions
- `chmod 700 ~/.ssh` - Set directory permissions
- `chmod 600 ~/.ssh/id_rsa` - Set private key permissions
- `chmod 644 ~/.ssh/id_rsa.pub` - Set public key permissions
- `chmod 600 ~/.ssh/authorized_keys` - Set authorized keys permissions

## SSH Configuration

### SSH Config File (~/.ssh/config)
```bash
# Basic host configuration
Host myserver
    HostName 192.168.1.100
    User myuser
    Port 22
    IdentityFile ~/.ssh/id_ed25519

# Jump host configuration
Host internal-server
    HostName 10.0.0.50
    User admin
    ProxyJump jump-server
    IdentityFile ~/.ssh/internal_key

# Development server
Host dev
    HostName dev.example.com
    User developer
    ForwardAgent yes
    LocalForward 3000 localhost:3000
```

### SSH Daemon Configuration (/etc/ssh/sshd_config)
```bash
# Security settings
Port 22
Protocol 2
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PermitEmptyPasswords no
MaxAuthTries 3
ClientAliveInterval 300
ClientAliveCountMax 3

# X11 forwarding
X11Forwarding yes
X11DisplayOffset 10

# Port forwarding
AllowTcpForwarding yes
GatewayPorts no
```

## File Transfer with SSH

### SCP (Secure Copy)
- `scp file.txt user@hostname:/path/` - Copy file to remote host
- `scp user@hostname:/path/file.txt .` - Copy file from remote host
- `scp -r directory/ user@hostname:/path/` - Copy directory recursively
- `scp -P port file.txt user@hostname:/path/` - Use specific port
- `scp -i keyfile file.txt user@hostname:/path/` - Use specific key

### RSYNC over SSH
- `rsync -avz file.txt user@hostname:/path/` - Sync file with progress
- `rsync -avz --delete source/ user@hostname:/path/destination/` - Mirror directories
- `rsync -avz -e "ssh -p 2222" file.txt user@hostname:/path/` - Use specific port
- `rsync -avz --exclude '*.tmp' source/ user@hostname:/path/` - Exclude files

### SFTP (SSH File Transfer Protocol)
- `sftp user@hostname` - Start SFTP session
- `sftp -i keyfile user@hostname` - Use specific key
- `sftp -P port user@hostname` - Use specific port

## Port Forwarding and Tunneling

### Local Port Forwarding
- `ssh -L 8080:localhost:80 user@hostname` - Forward local port 8080 to remote port 80
- `ssh -L 3306:localhost:3306 user@hostname` - Forward MySQL port
- `ssh -L 5432:localhost:5432 user@hostname` - Forward PostgreSQL port

### Remote Port Forwarding
- `ssh -R 8080:localhost:80 user@hostname` - Forward remote port 8080 to local port 80
- `ssh -R 0.0.0.0:8080:localhost:80 user@hostname` - Bind to all interfaces

### Dynamic Port Forwarding (SOCKS Proxy)
- `ssh -D 1080 user@hostname` - Create SOCKS proxy on port 1080
- `ssh -D 0.0.0.0:1080 user@hostname` - Bind SOCKS proxy to all interfaces

## SSH Multiplexing and Control

### SSH Control Master
```bash
# First connection (creates control socket)
ssh -M -S ~/.ssh/control-%h-%p-%r user@hostname

# Subsequent connections (use control socket)
ssh -S ~/.ssh/control-%h-%p-%r user@hostname

# Close control socket
ssh -O exit -S ~/.ssh/control-%h-%p-%r user@hostname
```

### SSH Config for Multiplexing
```bash
Host myserver
    HostName example.com
    User myuser
    ControlMaster auto
    ControlPath ~/.ssh/control-%h-%p-%r
    ControlPersist 10m
```

## Basic Scripts

### 1. SSH Connection Test Script
```bash
#!/bin/bash
# Test SSH connectivity to multiple hosts

HOSTS=("server1.example.com" "server2.example.com" "192.168.1.100")
USER="admin"

for host in "${HOSTS[@]}"; do
    echo "Testing connection to $host..."
    if ssh -o ConnectTimeout=5 -o BatchMode=yes "$USER@$host" exit 2>/dev/null; then
        echo "✓ $host is reachable"
    else
        echo "✗ $host is not reachable"
    fi
done
```

### 2. SSH Key Distribution Script
```bash
#!/bin/bash
# Distribute SSH key to multiple servers

KEY_FILE="$HOME/.ssh/id_ed25519.pub"
HOSTS_FILE="servers.txt"
USER="admin"

if [ ! -f "$KEY_FILE" ]; then
    echo "SSH key not found: $KEY_FILE"
    exit 1
fi

if [ ! -f "$HOSTS_FILE" ]; then
    echo "Hosts file not found: $HOSTS_FILE"
    exit 1
fi

while read -r host; do
    echo "Copying key to $host..."
    if ssh-copy-id -i "$KEY_FILE" "$USER@$host"; then
        echo "✓ Key copied to $host"
    else
        echo "✗ Failed to copy key to $host"
    fi
done < "$HOSTS_FILE"
```

### 3. SSH Tunnel Manager Script
```bash
#!/bin/bash
# Manage SSH tunnels

case "$1" in
    "start")
        echo "Starting SSH tunnel..."
        ssh -f -N -L 8080:localhost:80 user@hostname
        echo "Tunnel started on port 8080"
        ;;
    "stop")
        echo "Stopping SSH tunnel..."
        pkill -f "ssh.*-L 8080"
        echo "Tunnel stopped"
        ;;
    "status")
        if pgrep -f "ssh.*-L 8080" > /dev/null; then
            echo "Tunnel is running"
        else
            echo "Tunnel is not running"
        fi
        ;;
    *)
        echo "Usage: $0 {start|stop|status}"
        exit 1
        ;;
esac
```

### 4. SSH Backup Script
```bash
#!/bin/bash
# Backup remote files via SSH

REMOTE_HOST="user@hostname"
REMOTE_PATH="/path/to/backup"
LOCAL_PATH="./backup"
DATE=$(date +%Y%m%d_%H%M%S)

echo "Creating backup from $REMOTE_HOST:$REMOTE_PATH"
rsync -avz --delete \
    -e "ssh -o ConnectTimeout=10" \
    "$REMOTE_HOST:$REMOTE_PATH/" \
    "$LOCAL_PATH/backup_$DATE/"

if [ $? -eq 0 ]; then
    echo "✓ Backup completed: backup_$DATE"
else
    echo "✗ Backup failed"
    exit 1
fi
```

### 5. SSH Security Check Script
```bash
#!/bin/bash
# Check SSH security settings

echo "=== SSH Security Check ==="
echo

# Check SSH daemon configuration
if [ -f /etc/ssh/sshd_config ]; then
    echo "SSH Daemon Configuration:"
    echo "Root login: $(grep -i "PermitRootLogin" /etc/ssh/sshd_config | grep -v "^#" | tail -1)"
    echo "Password auth: $(grep -i "PasswordAuthentication" /etc/ssh/sshd_config | grep -v "^#" | tail -1)"
    echo "Key auth: $(grep -i "PubkeyAuthentication" /etc/ssh/sshd_config | grep -v "^#" | tail -1)"
    echo "Port: $(grep -i "Port" /etc/ssh/sshd_config | grep -v "^#" | tail -1)"
    echo
fi

# Check SSH keys
echo "SSH Keys:"
ls -la ~/.ssh/
echo

# Check authorized keys
if [ -f ~/.ssh/authorized_keys ]; then
    echo "Authorized Keys:"
    wc -l ~/.ssh/authorized_keys
    echo
fi

# Check SSH connections
echo "Active SSH Connections:"
ss -tuln | grep :22
```

## Security Best Practices

### Key Security
- Use Ed25519 keys (faster and more secure than RSA)
- Use strong passphrases for private keys
- Rotate keys regularly
- Use different keys for different services
- Never share private keys

### Server Security
- Disable root login
- Disable password authentication
- Use key-based authentication only
- Change default SSH port (optional)
- Use fail2ban to prevent brute force attacks
- Keep SSH daemon updated

### Connection Security
- Use SSH config files for complex setups
- Use multiplexing for multiple connections
- Use port forwarding instead of exposing services
- Monitor SSH logs for suspicious activity
- Use SSH certificates for large deployments

## Troubleshooting

### Common Issues
```bash
# Permission denied (publickey)
ssh -v user@hostname  # Check verbose output
ssh-copy-id user@hostname  # Copy key again

# Connection refused
telnet hostname 22  # Test if port is open
nmap -p 22 hostname  # Scan for SSH port

# Host key verification failed
ssh-keygen -R hostname  # Remove old host key
ssh -o StrictHostKeyChecking=no user@hostname  # Skip host key check (temporary)

# Connection timeout
ping hostname  # Test basic connectivity
traceroute hostname  # Check network path
```

### Debugging Commands
- `ssh -v user@hostname` - Verbose SSH connection
- `ssh -vv user@hostname` - More verbose output
- `ssh -vvv user@hostname` - Maximum verbosity
- `ssh -T user@hostname` - Test connection without executing command
- `ssh -n user@hostname` - Don't execute remote command

## Advanced Features

### SSH Certificates
```bash
# Generate CA key
ssh-keygen -f ca_key -C "SSH CA"

# Sign user key
ssh-keygen -s ca_key -I user_id -n username -V +52w user_key.pub

# Sign host key
ssh-keygen -s ca_key -I host_id -h -n hostname -V +52w host_key.pub
```

### SSH ProxyJump
```bash
# Jump through multiple hosts
ssh -J user1@jump1,user2@jump2 user3@target

# Or use config file
Host target
    HostName target.example.com
    User user3
    ProxyJump user1@jump1,user2@jump2
```

### SSH Config Examples
```bash
# Development environment
Host dev
    HostName dev.example.com
    User developer
    ForwardAgent yes
    LocalForward 3000 localhost:3000
    LocalForward 5432 localhost:5432
    ServerAliveInterval 60

# Production server
Host prod
    HostName prod.example.com
    User admin
    IdentityFile ~/.ssh/prod_key
    StrictHostKeyChecking yes
    ServerAliveInterval 30

# Jump host
Host jump
    HostName jump.example.com
    User jumper
    IdentityFile ~/.ssh/jump_key
    ControlMaster auto
    ControlPath ~/.ssh/control-%h-%p-%r
    ControlPersist 10m
``` 