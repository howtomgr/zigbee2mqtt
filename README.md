# zigbee2mqtt Installation Guide

zigbee2mqtt is a free and open-source Zigbee to MQTT bridge. Zigbee2MQTT bridges Zigbee devices to MQTT

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Supported Operating Systems](#supported-operating-systems)
3. [Installation](#installation)
4. [Configuration](#configuration)
5. [Service Management](#service-management)
6. [Troubleshooting](#troubleshooting)
7. [Security Considerations](#security-considerations)
8. [Performance Tuning](#performance-tuning)
9. [Backup and Restore](#backup-and-restore)
10. [System Requirements](#system-requirements)
11. [Support](#support)
12. [Contributing](#contributing)
13. [License](#license)
14. [Acknowledgments](#acknowledgments)
15. [Version History](#version-history)
16. [Appendices](#appendices)

## 1. Prerequisites

- **Hardware Requirements**:
  - CPU: 1 core minimum
  - RAM: 256MB minimum
  - Storage: 500MB for data
  - Network: Zigbee/MQTT
- **Operating System**: 
  - Linux: Any modern distribution (RHEL, Debian, Ubuntu, CentOS, Fedora, Arch, Alpine, openSUSE)
  - macOS: 10.14+ (Mojave or newer)
  - Windows: Windows Server 2016+ or Windows 10
  - FreeBSD: 11.0+
- **Network Requirements**:
  - Port N/A (default zigbee2mqtt port)
  - Frontend on 8080
- **Dependencies**:
  - See official documentation for specific requirements
- **System Access**: root or sudo privileges required


## 2. Supported Operating Systems

This guide supports installation on:
- RHEL 8/9 and derivatives (CentOS Stream, Rocky Linux, AlmaLinux)
- Debian 11/12
- Ubuntu 20.04/22.04/24.04 LTS
- Arch Linux (rolling release)
- Alpine Linux 3.18+
- openSUSE Leap 15.5+ / Tumbleweed
- SUSE Linux Enterprise Server (SLES) 15+
- macOS 12+ (Monterey and later) 
- FreeBSD 13+
- Windows 10/11/Server 2019+ (where applicable)

## 3. Installation

### RHEL/CentOS/Rocky Linux/AlmaLinux

```bash
# Install EPEL repository if needed
sudo dnf install -y epel-release

# Install zigbee2mqtt
sudo dnf install -y zigbee2mqtt

# Enable and start service
sudo systemctl enable --now zigbee2mqtt

# Configure firewall
sudo firewall-cmd --permanent --add-port=N/A/tcp
sudo firewall-cmd --reload

# Verify installation
zigbee2mqtt --version
```

### Debian/Ubuntu

```bash
# Update package index
sudo apt update

# Install zigbee2mqtt
sudo apt install -y zigbee2mqtt

# Enable and start service
sudo systemctl enable --now zigbee2mqtt

# Configure firewall
sudo ufw allow N/A

# Verify installation
zigbee2mqtt --version
```

### Arch Linux

```bash
# Install zigbee2mqtt
sudo pacman -S zigbee2mqtt

# Enable and start service
sudo systemctl enable --now zigbee2mqtt

# Verify installation
zigbee2mqtt --version
```

### Alpine Linux

```bash
# Install zigbee2mqtt
apk add --no-cache zigbee2mqtt

# Enable and start service
rc-update add zigbee2mqtt default
rc-service zigbee2mqtt start

# Verify installation
zigbee2mqtt --version
```

### openSUSE/SLES

```bash
# Install zigbee2mqtt
sudo zypper install -y zigbee2mqtt

# Enable and start service
sudo systemctl enable --now zigbee2mqtt

# Configure firewall
sudo firewall-cmd --permanent --add-port=N/A/tcp
sudo firewall-cmd --reload

# Verify installation
zigbee2mqtt --version
```

### macOS

```bash
# Using Homebrew
brew install zigbee2mqtt

# Start service
brew services start zigbee2mqtt

# Verify installation
zigbee2mqtt --version
```

### FreeBSD

```bash
# Using pkg
pkg install zigbee2mqtt

# Enable in rc.conf
echo 'zigbee2mqtt_enable="YES"' >> /etc/rc.conf

# Start service
service zigbee2mqtt start

# Verify installation
zigbee2mqtt --version
```

### Windows

```bash
# Using Chocolatey
choco install zigbee2mqtt

# Or using Scoop
scoop install zigbee2mqtt

# Verify installation
zigbee2mqtt --version
```

## Initial Configuration

### Basic Configuration

```bash
# Create configuration directory
sudo mkdir -p /etc/zigbee2mqtt

# Set up basic configuration
# See official documentation for detailed configuration options

# Test configuration
zigbee2mqtt --version
```

## 5. Service Management

### systemd (RHEL, Debian, Ubuntu, Arch, openSUSE)

```bash
# Enable service
sudo systemctl enable zigbee2mqtt

# Start service
sudo systemctl start zigbee2mqtt

# Stop service
sudo systemctl stop zigbee2mqtt

# Restart service
sudo systemctl restart zigbee2mqtt

# Check status
sudo systemctl status zigbee2mqtt

# View logs
sudo journalctl -u zigbee2mqtt -f
```

### OpenRC (Alpine Linux)

```bash
# Enable service
rc-update add zigbee2mqtt default

# Start service
rc-service zigbee2mqtt start

# Stop service
rc-service zigbee2mqtt stop

# Restart service
rc-service zigbee2mqtt restart

# Check status
rc-service zigbee2mqtt status
```

### rc.d (FreeBSD)

```bash
# Enable in /etc/rc.conf
echo 'zigbee2mqtt_enable="YES"' >> /etc/rc.conf

# Start service
service zigbee2mqtt start

# Stop service
service zigbee2mqtt stop

# Restart service
service zigbee2mqtt restart

# Check status
service zigbee2mqtt status
```

### launchd (macOS)

```bash
# Using Homebrew services
brew services start zigbee2mqtt
brew services stop zigbee2mqtt
brew services restart zigbee2mqtt

# Check status
brew services list | grep zigbee2mqtt
```

### Windows Service Manager

```powershell
# Start service
net start zigbee2mqtt

# Stop service
net stop zigbee2mqtt

# Using PowerShell
Start-Service zigbee2mqtt
Stop-Service zigbee2mqtt
Restart-Service zigbee2mqtt

# Check status
Get-Service zigbee2mqtt
```

## Advanced Configuration

See the official documentation for advanced configuration options.

## Reverse Proxy Setup

### nginx Configuration

```nginx
upstream zigbee2mqtt_backend {
    server 127.0.0.1:N/A;
}

server {
    listen 80;
    server_name zigbee2mqtt.example.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name zigbee2mqtt.example.com;

    ssl_certificate /etc/ssl/certs/zigbee2mqtt.example.com.crt;
    ssl_certificate_key /etc/ssl/private/zigbee2mqtt.example.com.key;

    location / {
        proxy_pass http://zigbee2mqtt_backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Apache Configuration

```apache
<VirtualHost *:80>
    ServerName zigbee2mqtt.example.com
    Redirect permanent / https://zigbee2mqtt.example.com/
</VirtualHost>

<VirtualHost *:443>
    ServerName zigbee2mqtt.example.com
    
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/zigbee2mqtt.example.com.crt
    SSLCertificateKeyFile /etc/ssl/private/zigbee2mqtt.example.com.key
    
    ProxyRequests Off
    ProxyPreserveHost On
    
    ProxyPass / http://127.0.0.1:N/A/
    ProxyPassReverse / http://127.0.0.1:N/A/
</VirtualHost>
```

### HAProxy Configuration

```haproxy
frontend zigbee2mqtt_frontend
    bind *:80
    bind *:443 ssl crt /etc/ssl/certs/zigbee2mqtt.pem
    redirect scheme https if !{ ssl_fc }
    default_backend zigbee2mqtt_backend

backend zigbee2mqtt_backend
    balance roundrobin
    server zigbee2mqtt1 127.0.0.1:N/A check
```

## Security Configuration

### Basic Security Setup

```bash
# Set appropriate permissions
sudo chown -R zigbee2mqtt:zigbee2mqtt /etc/zigbee2mqtt
sudo chmod 750 /etc/zigbee2mqtt

# Configure firewall
sudo firewall-cmd --permanent --add-port=N/A/tcp
sudo firewall-cmd --reload

# Enable SELinux policies (if applicable)
sudo setsebool -P httpd_can_network_connect on
```

## Database Setup

See official documentation for database configuration requirements.

## Performance Optimization

### System Tuning

```bash
# Basic system tuning
echo 'net.core.somaxconn = 65535' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv4.tcp_max_syn_backlog = 65535' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

## Monitoring

### Basic Monitoring

```bash
# Check service status
sudo systemctl status zigbee2mqtt

# View logs
sudo journalctl -u zigbee2mqtt -f

# Monitor resource usage
top -p $(pgrep zigbee2mqtt)
```

## 9. Backup and Restore

### Backup Script

```bash
#!/bin/bash
# Basic backup script
BACKUP_DIR="/backup/zigbee2mqtt"
DATE=$(date +%Y%m%d_%H%M%S)

mkdir -p "$BACKUP_DIR"
tar -czf "$BACKUP_DIR/zigbee2mqtt-backup-$DATE.tar.gz" /etc/zigbee2mqtt /var/lib/zigbee2mqtt

echo "Backup completed: $BACKUP_DIR/zigbee2mqtt-backup-$DATE.tar.gz"
```

### Restore Procedure

```bash
# Stop service
sudo systemctl stop zigbee2mqtt

# Restore from backup
tar -xzf /backup/zigbee2mqtt/zigbee2mqtt-backup-*.tar.gz -C /

# Start service
sudo systemctl start zigbee2mqtt
```

## 6. Troubleshooting

### Common Issues

1. **Service won't start**:
```bash
# Check logs
sudo journalctl -u zigbee2mqtt -n 100
sudo tail -f /var/log/zigbee2mqtt/zigbee2mqtt.log

# Check configuration
zigbee2mqtt --version

# Check permissions
ls -la /etc/zigbee2mqtt
```

2. **Connection issues**:
```bash
# Check if service is listening
sudo ss -tlnp | grep N/A

# Test connectivity
telnet localhost N/A

# Check firewall
sudo firewall-cmd --list-all
```

3. **Performance issues**:
```bash
# Check resource usage
top -p $(pgrep zigbee2mqtt)

# Check disk I/O
iotop -p $(pgrep zigbee2mqtt)

# Check connections
ss -an | grep N/A
```

## Integration Examples

### Docker Compose Example

```yaml
version: '3.8'
services:
  zigbee2mqtt:
    image: zigbee2mqtt:latest
    ports:
      - "N/A:N/A"
    volumes:
      - ./config:/etc/zigbee2mqtt
      - ./data:/var/lib/zigbee2mqtt
    restart: unless-stopped
```

## Maintenance

### Update Procedures

```bash
# RHEL/CentOS/Rocky/AlmaLinux
sudo dnf update zigbee2mqtt

# Debian/Ubuntu
sudo apt update && sudo apt upgrade zigbee2mqtt

# Arch Linux
sudo pacman -Syu zigbee2mqtt

# Alpine Linux
apk update && apk upgrade zigbee2mqtt

# openSUSE
sudo zypper update zigbee2mqtt

# FreeBSD
pkg update && pkg upgrade zigbee2mqtt

# Always backup before updates
tar -czf /backup/zigbee2mqtt-pre-update-$(date +%Y%m%d).tar.gz /etc/zigbee2mqtt

# Restart after updates
sudo systemctl restart zigbee2mqtt
```

### Regular Maintenance

```bash
# Log rotation
sudo logrotate -f /etc/logrotate.d/zigbee2mqtt

# Clean old logs
find /var/log/zigbee2mqtt -name "*.log" -mtime +30 -delete

# Check disk usage
du -sh /var/lib/zigbee2mqtt
```

## Additional Resources

- Official Documentation: https://docs.zigbee2mqtt.org/
- GitHub Repository: https://github.com/zigbee2mqtt/zigbee2mqtt
- Community Forum: https://forum.zigbee2mqtt.org/
- Best Practices Guide: https://docs.zigbee2mqtt.org/best-practices

---

**Note:** This guide is part of the [HowToMgr](https://howtomgr.github.io) collection. Always refer to official documentation for the most up-to-date information.
