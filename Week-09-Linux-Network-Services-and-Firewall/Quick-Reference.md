# Week 9 Quick Reference - Linux Network Services and Firewall

Quick command reference for SSH configuration, UFW firewall, and network service management.

---

## SSH (Secure Shell) Commands

| Task | Command | Example |
|------|---------|---------|
| Install SSH server | `sudo apt install openssh-server -y` | `sudo apt install openssh-server -y` |
| Check SSH status | `systemctl status ssh` | `systemctl status ssh` |
| Start SSH service | `sudo systemctl start ssh` | `sudo systemctl start ssh` |
| Stop SSH service | `sudo systemctl stop ssh` | `sudo systemctl stop ssh` |
| Restart SSH service | `sudo systemctl restart ssh` | `sudo systemctl restart ssh` |
| Enable SSH on boot | `sudo systemctl enable ssh` | `sudo systemctl enable ssh` |
| Disable SSH on boot | `sudo systemctl disable ssh` | `sudo systemctl disable ssh` |
| Test SSH config | `sudo sshd -t` | `sudo sshd -t` |
| View SSH config | `sudo cat /etc/ssh/sshd_config` | `sudo cat /etc/ssh/sshd_config` |
| Edit SSH config | `sudo nano /etc/ssh/sshd_config` | `sudo nano /etc/ssh/sshd_config` |
| Backup SSH config | `sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup` | `sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup` |
| View SSH logs | `journalctl -u ssh -n 50` | `journalctl -u ssh -n 50` |
| Follow SSH logs | `journalctl -u ssh -f` | `journalctl -u ssh -f` |
| Connect via SSH | `ssh user@hostname` | `ssh student@192.168.1.10` |
| Connect with port | `ssh -p port user@hostname` | `ssh -p 22 student@192.168.1.10` |
| Connect verbose | `ssh -v user@hostname` | `ssh -v student@192.168.1.10` |
| Generate SSH key | `ssh-keygen -t rsa -b 4096` | `ssh-keygen -t rsa -b 4096` |
| Copy SSH key | `ssh-copy-id user@hostname` | `ssh-copy-id student@192.168.1.10` |

---

## UFW (Uncomplicated Firewall) Commands

| Task | Command | Example |
|------|---------|---------|
| Install UFW | `sudo apt install ufw -y` | `sudo apt install ufw -y` |
| Check UFW status | `sudo ufw status` | `sudo ufw status` |
| Check UFW status (verbose) | `sudo ufw status verbose` | `sudo ufw status verbose` |
| Check UFW status (numbered) | `sudo ufw status numbered` | `sudo ufw status numbered` |
| Enable UFW | `sudo ufw enable` | `sudo ufw enable` |
| Disable UFW | `sudo ufw disable` | `sudo ufw disable` |
| Reload UFW | `sudo ufw reload` | `sudo ufw reload` |
| Reset UFW | `sudo ufw --force reset` | `sudo ufw --force reset` |
| Allow port | `sudo ufw allow port/tcp` | `sudo ufw allow 22/tcp` |
| Allow port (UDP) | `sudo ufw allow port/udp` | `sudo ufw allow 53/udp` |
| Allow service | `sudo ufw allow service` | `sudo ufw allow ssh` |
| Allow with comment | `sudo ufw allow port/tcp comment 'description'` | `sudo ufw allow 8080/tcp comment 'Web server'` |
| Allow from IP | `sudo ufw allow from ip_address` | `sudo ufw allow from 192.168.1.100` |
| Allow from IP to port | `sudo ufw allow from ip_address to any port port` | `sudo ufw allow from 192.168.1.100 to any port 22` |
| Deny port | `sudo ufw deny port/tcp` | `sudo ufw deny 80/tcp` |
| Delete rule by number | `sudo ufw delete number` | `sudo ufw delete 3` |
| Delete rule by spec | `sudo ufw delete allow port/tcp` | `sudo ufw delete allow 80/tcp` |
| Set default policy | `sudo ufw default deny incoming` | `sudo ufw default deny incoming` |
| Set default allow outgoing | `sudo ufw default allow outgoing` | `sudo ufw default allow outgoing` |
| Enable logging | `sudo ufw logging on` | `sudo ufw logging on` |
| Set log level | `sudo ufw logging level` | `sudo ufw logging low` |
| Disable logging | `sudo ufw logging off` | `sudo ufw logging off` |
| View firewall logs | `sudo tail -f /var/log/ufw.log` | `sudo tail -f /var/log/ufw.log` |

---

## Network Service Management Commands

| Task | Command | Example |
|------|---------|---------|
| Check service status | `systemctl status service_name` | `systemctl status ssh` |
| Start service | `sudo systemctl start service_name` | `sudo systemctl start ssh` |
| Stop service | `sudo systemctl stop service_name` | `sudo systemctl stop ssh` |
| Restart service | `sudo systemctl restart service_name` | `sudo systemctl restart ssh` |
| Reload service | `sudo systemctl reload service_name` | `sudo systemctl reload ssh` |
| Enable service | `sudo systemctl enable service_name` | `sudo systemctl enable ssh` |
| Disable service | `sudo systemctl disable service_name` | `sudo systemctl disable ssh` |
| List all services | `systemctl list-units --type=service` | `systemctl list-units --type=service` |
| List running services | `systemctl list-units --type=service --state=running` | `systemctl list-units --type=service --state=running` |
| View service dependencies | `systemctl list-dependencies service_name` | `systemctl list-dependencies ssh` |
| View reverse dependencies | `systemctl list-dependencies service_name --reverse` | `systemctl list-dependencies ssh --reverse` |
| View service logs | `journalctl -u service_name` | `journalctl -u ssh` |
| View recent logs | `journalctl -u service_name -n 50` | `journalctl -u ssh -n 50` |
| Follow service logs | `journalctl -u service_name -f` | `journalctl -u ssh -f` |

---

## Port and Network Connection Commands

| Task | Command | Example |
|------|---------|---------|
| View listening ports | `sudo ss -tlnp` | `sudo ss -tlnp` |
| View IPv4 ports | `sudo ss -tlnp4` | `sudo ss -tlnp4` |
| View IPv6 ports | `sudo ss -tlnp6` | `sudo ss -tlnp6` |
| View UDP ports | `sudo ss -ulnp` | `sudo ss -ulnp` |
| View specific port | `sudo ss -tlnp | grep :port` | `sudo ss -tlnp | grep :22` |
| View connections | `sudo ss -tnp` | `sudo ss -tnp` |
| View established connections | `sudo ss -tnp state established` | `sudo ss -tnp state established` |
| View all connections | `sudo ss -a` | `sudo ss -a` |
| Test port connectivity | `nc -zv hostname port` | `nc -zv 192.168.1.10 22` |
| Test port with telnet | `telnet hostname port` | `telnet 192.168.1.10 22` |
| View network connections (netstat) | `sudo netstat -tlnp` | `sudo netstat -tlnp` |
| View routing table | `ip r` | `ip r` |
| View IP addresses | `ip a` | `ip a` |
| Get hostname IP | `hostname -I` | `hostname -I` |

---

## SSH Configuration Options

### Common SSH Configuration Settings

Edit `/etc/ssh/sshd_config`:

```bash
# Port configuration
Port 22

# Authentication
PermitRootLogin no
PasswordAuthentication yes
PubkeyAuthentication yes

# Security settings
MaxAuthTries 3
LoginGraceTime 60

# X11 Forwarding
X11Forwarding yes

# Allow/Deny users
AllowUsers student
DenyUsers root

# Logging
LogLevel INFO
```

### SSH Configuration Commands

| Task | Command |
|------|---------|
| View active config | `sudo cat /etc/ssh/sshd_config | grep -v "^#" | grep -v "^$"` |
| Test config syntax | `sudo sshd -t` |
| Apply config changes | `sudo systemctl restart ssh` |
| View default config | `cat /etc/ssh/sshd_config.d/*.conf` |

---

## UFW Firewall Rule Examples

### Basic Rules

```bash
# Allow SSH
sudo ufw allow ssh
sudo ufw allow 22/tcp

# Allow HTTP/HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Allow specific port with comment
sudo ufw allow 8080/tcp comment 'Custom web server'

# Deny port
sudo ufw deny 3306/tcp
```

### Advanced Rules

```bash
# Allow from specific IP
sudo ufw allow from 192.168.1.100

# Allow from IP to specific port
sudo ufw allow from 192.168.1.100 to any port 22

# Allow from subnet
sudo ufw allow from 192.168.1.0/24

# Allow port range
sudo ufw allow 8000:8010/tcp

# Allow specific protocol
sudo ufw allow 53/udp
```

### Default Policies

```bash
# Set default policies
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw default deny routed
```

---

## Common Task Combinations

### Complete SSH Setup

```bash
# Install SSH
sudo apt update
sudo apt install openssh-server -y

# Check status
systemctl status ssh

# Configure SSH
sudo nano /etc/ssh/sshd_config

# Test configuration
sudo sshd -t

# Restart SSH
sudo systemctl restart ssh

# Verify
systemctl status ssh
sudo ss -tlnp | grep :22
```

### Complete UFW Setup

```bash
# Install UFW
sudo apt install ufw -y

# Set defaults
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH (IMPORTANT!)
sudo ufw allow ssh

# Allow other services
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Enable firewall
sudo ufw enable

# Verify
sudo ufw status verbose
```

### Test SSH Connection

```bash
# Find IP address
hostname -I

# Test local connection
ssh localhost

# Test remote connection
ssh user@192.168.1.10

# Test with verbose output
ssh -v user@192.168.1.10

# Test specific port
ssh -p 22 user@192.168.1.10
```

### Troubleshoot Network Services

```bash
# Check service status
systemctl status ssh

# Check if port is listening
sudo ss -tlnp | grep :22

# Check firewall rules
sudo ufw status numbered

# View service logs
journalctl -u ssh -n 50

# Test SSH configuration
sudo sshd -t

# Check firewall logs
sudo tail -f /var/log/ufw.log
```

---

## File Locations Reference

| Purpose | File/Directory |
|---------|---------------|
| SSH server config | `/etc/ssh/sshd_config` |
| SSH client config | `~/.ssh/config` |
| SSH authorized keys | `~/.ssh/authorized_keys` |
| SSH host keys | `/etc/ssh/ssh_host_*` |
| UFW config | `/etc/ufw/` |
| UFW rules | `/etc/ufw/user.rules` |
| UFW logs | `/var/log/ufw.log` |
| System logs | `/var/log/syslog` |
| Auth logs | `/var/log/auth.log` |
| Service logs | `journalctl -u service_name` |

---

## Troubleshooting Commands

| Problem | Diagnostic Command | Fix Command |
|---------|-------------------|-------------|
| SSH not working | `systemctl status ssh` | `sudo systemctl start ssh` |
| Connection refused | `sudo ss -tlnp | grep :22` | `sudo systemctl start ssh` |
| Connection timeout | `sudo ufw status | grep 22` | `sudo ufw allow ssh` |
| Permission denied | `journalctl -u ssh -n 50` | Check credentials/config |
| Firewall blocking | `sudo ufw status numbered` | `sudo ufw allow port/tcp` |
| Service won't start | `journalctl -u service -n 50` | Check config/logs |
| Port not listening | `sudo ss -tlnp | grep :port` | Start service |
| Can't delete rule | `sudo ufw status numbered` | `sudo ufw delete number` |

---

## Security Best Practices

### SSH Security

```bash
# Disable root login
PermitRootLogin no

# Limit authentication attempts
MaxAuthTries 3

# Use key-based authentication
PubkeyAuthentication yes
PasswordAuthentication no  # After setting up keys

# Change default port (optional)
Port 2222

# Allow specific users only
AllowUsers student admin
```

### Firewall Security

```bash
# Default deny incoming
sudo ufw default deny incoming

# Allow only necessary ports
sudo ufw allow ssh
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Enable logging
sudo ufw logging on

# Restrict access by IP (if possible)
sudo ufw allow from 192.168.1.0/24 to any port 22
```

---

## Quick Diagnostic Checklist

```bash
# 1. Check SSH service
systemctl status ssh

# 2. Check SSH port
sudo ss -tlnp | grep :22

# 3. Check firewall
sudo ufw status verbose

# 4. Test SSH locally
ssh localhost

# 5. View SSH logs
journalctl -u ssh -n 20

# 6. Check firewall logs
sudo tail -20 /var/log/ufw.log

# 7. Test SSH config
sudo sshd -t
```

---

## Emergency Recovery Commands

**If locked out of SSH:**

```bash
# If you have physical/console access:
# 1. Stop UFW
sudo ufw disable

# 2. Start SSH
sudo systemctl start ssh

# 3. Re-enable UFW with SSH allowed
sudo ufw allow ssh
sudo ufw enable
```

**If SSH service won't start:**

```bash
# Check configuration
sudo sshd -t

# View logs
journalctl -u ssh -n 100

# Restore backup config
sudo cp /etc/ssh/sshd_config.backup /etc/ssh/sshd_config

# Restart service
sudo systemctl restart ssh
```

**If firewall blocks everything:**

```bash
# Disable firewall
sudo ufw disable

# Reset firewall
sudo ufw --force reset

# Reconfigure
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw enable
```

---

## 💡 Tips & Tricks

### Command Line Shortcuts
- **Up Arrow** - Recall previous command
- **Tab** - Autocomplete file/folder names
- **Ctrl + C** - Stop current command
- **Ctrl + L** - Clear screen
- **Ctrl + R** - Search command history

### Useful Aliases
Add to `~/.bashrc`:
```bash
alias ssh-status='systemctl status ssh'
alias ssh-restart='sudo systemctl restart ssh'
alias ufw-status='sudo ufw status numbered'
alias ports='sudo ss -tlnp'
```

### SSH Key Authentication Setup

```bash
# Generate SSH key pair
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# Copy public key to remote server
ssh-copy-id user@remote_host

# Test key authentication
ssh user@remote_host  # Should not prompt for password
```

### View Configuration Safely

```bash
# View SSH config without editing
sudo cat /etc/ssh/sshd_config | grep -v "^#" | grep -v "^$"

# View UFW rules
sudo ufw status numbered

# View listening ports
sudo ss -tlnp
```

---

## 📝 Examples for This Week's Lab

### Activity 1: Install and Configure SSH

```bash
# Install SSH
sudo apt update
sudo apt install openssh-server -y

# Check status
systemctl status ssh

# Configure SSH
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
sudo nano /etc/ssh/sshd_config

# Test config
sudo sshd -t

# Restart SSH
sudo systemctl restart ssh

# Test connection
ssh localhost
```

### Activity 2: Configure UFW Firewall

```bash
# Install UFW
sudo apt install ufw -y

# Set defaults
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH (CRITICAL!)
sudo ufw allow ssh

# Allow other services
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Enable firewall
sudo ufw enable

# Verify
sudo ufw status verbose
```

### Activity 3: Manage Services and Ports

```bash
# View listening ports
sudo ss -tlnp

# Check SSH port
sudo ss -tlnp | grep :22

# Manage SSH service
systemctl status ssh
sudo systemctl restart ssh

# View service logs
journalctl -u ssh -n 50
```

### Activity 4: Test and Troubleshoot

```bash
# Test SSH connection
ssh user@192.168.1.10

# Check firewall rules
sudo ufw status numbered

# View firewall logs
sudo tail -f /var/log/ufw.log

# Troubleshoot SSH
systemctl status ssh
sudo ss -tlnp | grep :22
journalctl -u ssh -n 50
```

### Activity 5: Secure Services

```bash
# Harden SSH config
sudo nano /etc/ssh/sshd_config
# Set: PermitRootLogin no, MaxAuthTries 3

# Enable firewall logging
sudo ufw logging on
sudo ufw logging low

# View logs
sudo tail -f /var/log/ufw.log
```

---

## 📌 Remember!

- **Always allow SSH** before enabling UFW, or you may lock yourself out!
- **Backup configurations** before making changes
- **Test SSH configuration** with `sshd -t` before restarting
- **Use `ufw status numbered`** to see rule numbers for deletion
- **Check firewall logs** when troubleshooting connection issues
- **Keep SSH service enabled** for remote access
- **Use strong passwords** or SSH keys for authentication
- **Default deny incoming** is a security best practice

---

**💾 Save this page!** You'll use these commands throughout the course!

**🔖 Pro tip:** Print this page or keep it open on a second screen during labs!

