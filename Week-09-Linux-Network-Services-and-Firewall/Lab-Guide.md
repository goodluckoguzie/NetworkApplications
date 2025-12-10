# Week 9 Lab Guide: Linux Network Services and Firewall Configuration

Complete guide showing both GUI (Graphical User Interface) and CLI (Command Line Interface) methods.

---

## Lab Overview

### What is Week 9 About:
**Configuring Network Services (SSH) and Firewall (UFW) on Ubuntu Linux**

### PC Requirements:
- **Ubuntu 24.04 LTS** (Virtual Machine, lab PC, or LOD platform)
  - **Download Ubuntu 24.04.3 Desktop ISO:** [Direct Download Link](https://releases.ubuntu.com/24.04/ubuntu-24.04.3-desktop-amd64.iso)
  - **Alternative:** [Ubuntu Download Page](https://releases.ubuntu.com/24.04/)
  - **File:** `ubuntu-24.04.3-desktop-amd64.iso` (5.9 GB)
- Terminal/Shell access
- Administrator/sudo privileges
- Network connectivity
- Two Ubuntu systems (or one system for local testing)
- Basic familiarity with CLI commands from previous weeks

### What You'll Do:
- **Activity 1:** Install and configure SSH (Secure Shell) service
- **Activity 2:** Configure UFW (Uncomplicated Firewall) firewall rules
- **Activity 3:** Manage network services and ports
- **Activity 4:** Test and troubleshoot SSH and firewall configurations
- **Activity 5:** Secure network services with firewall rules

---

## Important Terms

| Term | Definition | Simple Explanation |
|------|------------|-------------------|
| SSH | Secure Shell protocol | Encrypted remote access to Linux systems |
| UFW | Uncomplicated Firewall | Easy-to-use firewall configuration tool for Ubuntu |
| Port | Network communication endpoint | Number that identifies a specific service (e.g., 22 for SSH, 80 for HTTP) |
| Firewall | Network security system | Controls incoming and outgoing network traffic |
| Service | Background program | Runs continuously to provide functionality (e.g., SSH server) |
| systemd | System and service manager | Manages services and system initialization |
| Public Key Authentication | SSH authentication method | Uses cryptographic keys instead of passwords |
| Port Forwarding | Network routing technique | Redirects traffic from one port to another |

---

## Background Knowledge: Network Services and Firewalls

**How Network Services Work:**

1. **Services** run in the background and listen on specific ports
2. **Clients** connect to services using IP addresses and port numbers
3. **Firewalls** control which ports are accessible
4. **SSH** provides secure remote access to Linux systems

**Key Concepts:**

- **Ports:** Services listen on specific ports (SSH = 22, HTTP = 80, HTTPS = 443)
- **Firewall Rules:** Allow or deny traffic based on port, protocol, and source
- **Service Management:** Start, stop, enable, and disable services using systemd
- **Security:** Firewalls protect systems by blocking unauthorized access

**Key Files and Directories:**
- `/etc/ssh/sshd_config` - SSH server configuration
- `/etc/ufw/` - UFW firewall configuration directory
- `/etc/systemd/system/` - Systemd service files
- `journalctl -u ssh` - SSH service logs

---

## SETUP: PREPARING FOR WEEK 9

**What we're doing:** We need to set up our Ubuntu environment for network services and firewall configuration.

### Prerequisites:

1. **Start your Ubuntu system**
   - Open your Ubuntu VM, lab PC, or LOD platform
   - Log in with your credentials

2. **Open Terminal**
   - Press **Ctrl + Alt + T**
   - *Terminal window opens*

3. **Verify you have sudo access**
   - Type: `sudo whoami`
   - Press Enter
   - *Enter your password when prompted*
   - *You should see: `root`*

4. **Check your network connectivity**
   - Type: `ip a`
   - Press Enter
   - *Verify you have an IP address*

5. **Update system packages**
   - Type: `sudo apt update`
   - Press Enter
   - *Wait for update to complete*

**What you should see:**
```
$ sudo whoami
[sudo] password for student: 
root
$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> ...
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> ...
    inet 192.168.1.10/24 ...
```

---

## ACTIVITY 1: INSTALLING AND CONFIGURING SSH

**Do on:** Ubuntu Linux system

**What we're doing:** We're installing and configuring SSH (Secure Shell) to enable secure remote access to the Linux system.

**Goal:** Install SSH server, configure it, and test remote connections.

**Time:** 30 minutes

---

## TASK 1: INSTALL SSH SERVER

**What this does:** We're installing the OpenSSH server package to enable SSH remote access.

### CLI METHOD

1. **Check if SSH is already installed**
   - Type: `systemctl status ssh`
   - Press Enter
   - *Shows SSH service status*

2. **If SSH is not installed, install it**
   - Type: `sudo apt install openssh-server -y`
   - Press Enter
   - *Installs SSH server package*
   - *Wait for installation to complete*

3. **Verify installation**
   - Type: `systemctl status ssh`
   - Press Enter
   - *Should show SSH service is active*

**What you should see:**
```
$ systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
   Active: active (running) since ...
```

---

## TASK 2: CONFIGURE SSH SERVER

**What this does:** We're configuring SSH server settings for security and functionality.

### CLI METHOD

1. **Backup SSH configuration**
   - Type: `sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup`
   - Press Enter
   - *Creates backup of configuration*

2. **View current SSH configuration**
   - Type: `sudo cat /etc/ssh/sshd_config | grep -v "^#" | grep -v "^$"`
   - Press Enter
   - *Shows active configuration lines*

3. **Edit SSH configuration**
   - Type: `sudo nano /etc/ssh/sshd_config`
   - Press Enter
   - *Opens SSH configuration file in editor*

4. **Common configuration options to check/modify:**
   - `Port 22` - SSH port (default is 22)
   - `PermitRootLogin no` - Disable root login (security best practice)
   - `PasswordAuthentication yes` - Allow password authentication
   - `PubkeyAuthentication yes` - Allow public key authentication
   - `X11Forwarding yes` - Allow X11 forwarding (for GUI apps)

5. **Save and exit**
   - Press **Ctrl + X**
   - Press **Y** to confirm
   - Press **Enter** to save

6. **Test SSH configuration**
   - Type: `sudo sshd -t`
   - Press Enter
   - *Tests configuration for syntax errors*
   - *If no errors, proceed to next step*

7. **Restart SSH service**
   - Type: `sudo systemctl restart ssh`
   - Press Enter
   - *Applies new configuration*

8. **Verify SSH is running**
   - Type: `systemctl status ssh`
   - Press Enter
   - *Should show active (running)*

---

## TASK 3: TEST SSH CONNECTION

**What this does:** We're testing SSH connectivity to verify the service is working.

### CLI METHOD

1. **Find your IP address**
   - Type: `hostname -I`
   - Press Enter
   - *Shows IP address(es) of the system*
   - *Note the IP address (e.g., 192.168.1.10)*

2. **Test SSH connection locally**
   - Type: `ssh localhost`
   - Press Enter
   - *First time: Type "yes" to accept host key*
   - *Enter your password when prompted*
   - *You should be logged in via SSH*

3. **Exit SSH session**
   - Type: `exit`
   - Press Enter
   - *Returns to original terminal*

4. **Check SSH port is listening**
   - Type: `sudo ss -tlnp | grep :22`
   - Press Enter
   - *Shows SSH is listening on port 22*

**What you should see:**
```
$ hostname -I
192.168.1.10
$ ssh localhost
The authenticity of host 'localhost (127.0.0.1)' can't be established.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
student@localhost's password: 
Welcome to Ubuntu 24.04.3 LTS
...
$ exit
logout
Connection to localhost closed.
```

---

## ACTIVITY 2: CONFIGURING UFW FIREWALL

**Do on:** Ubuntu Linux system

**What we're doing:** We're configuring UFW (Uncomplicated Firewall) to control network traffic and secure the system.

**Goal:** Install UFW, configure firewall rules, and test firewall functionality.

**Time:** 25 minutes

---

## TASK 1: INSTALL AND ENABLE UFW

**What this does:** We're installing UFW firewall and enabling it.

### CLI METHOD

1. **Check if UFW is installed**
   - Type: `which ufw`
   - Press Enter
   - *Shows UFW location if installed*

2. **Install UFW (if not installed)**
   - Type: `sudo apt install ufw -y`
   - Press Enter
   - *Installs UFW firewall*

3. **Check UFW status**
   - Type: `sudo ufw status`
   - Press Enter
   - *Shows firewall status (should be inactive)*

4. **Enable UFW**
   - Type: `sudo ufw enable`
   - Press Enter
   - *Type "y" to confirm*
   - *Firewall is now active*

5. **Verify UFW is enabled**
   - Type: `sudo ufw status verbose`
   - Press Enter
   - *Shows detailed firewall status*

**What you should see:**
```
$ sudo ufw status
Status: inactive
$ sudo ufw enable
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
$ sudo ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
```

---

## TASK 2: CONFIGURE DEFAULT UFW POLICIES

**What this does:** We're setting default firewall policies for incoming, outgoing, and routed traffic.

### CLI METHOD

1. **View current default policies**
   - Type: `sudo ufw status verbose`
   - Press Enter
   - *Shows default policies*

2. **Set default policies (if needed)**
   - Type: `sudo ufw default deny incoming`
   - Press Enter
   - *Denies all incoming connections by default*

   - Type: `sudo ufw default allow outgoing`
   - Press Enter
   - *Allows all outgoing connections*

3. **Verify policies**
   - Type: `sudo ufw status verbose`
   - Press Enter
   - *Shows updated policies*

**Default Policy Explanation:**
- **Deny incoming:** Blocks all incoming connections unless explicitly allowed
- **Allow outgoing:** Permits all outgoing connections
- **Deny routed:** Blocks forwarded traffic (for routers)

---

## TASK 3: ALLOW SSH THROUGH FIREWALL

**What this does:** We're creating a firewall rule to allow SSH connections (critical before enabling firewall!).

### CLI METHOD

1. **Allow SSH port (port 22)**
   - Type: `sudo ufw allow 22/tcp`
   - Press Enter
   - *Allows SSH on port 22*

2. **Alternative: Allow SSH by service name**
   - Type: `sudo ufw allow ssh`
   - Press Enter
   - *Allows SSH service (same as port 22)*

3. **Verify SSH rule**
   - Type: `sudo ufw status numbered`
   - Press Enter
   - *Shows numbered list of rules including SSH*

**What you should see:**
```
$ sudo ufw allow ssh
Rules updated
Rules updated (v6)
$ sudo ufw status numbered
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 22/tcp                     ALLOW IN    Anywhere
[ 2] 22/tcp (v6)                ALLOW IN    Anywhere (v6)
```

**⚠️ IMPORTANT:** Always allow SSH before enabling UFW, or you may lock yourself out!

---

## TASK 4: CONFIGURE ADDITIONAL FIREWALL RULES

**What this does:** We're adding more firewall rules for common services.

### CLI METHOD

1. **Allow HTTP (port 80)**
   - Type: `sudo ufw allow 80/tcp`
   - Press Enter
   - *Allows HTTP web traffic*

2. **Allow HTTPS (port 443)**
   - Type: `sudo ufw allow 443/tcp`
   - Press Enter
   - *Allows HTTPS secure web traffic*

3. **Allow specific port with comment**
   - Type: `sudo ufw allow 8080/tcp comment 'Custom web server'`
   - Press Enter
   - *Allows port 8080 with description*

4. **Allow from specific IP address**
   - Type: `sudo ufw allow from 192.168.1.100`
   - Press Enter
   - *Allows all traffic from specific IP*

5. **Allow from specific IP to specific port**
   - Type: `sudo ufw allow from 192.168.1.100 to any port 22`
   - Press Enter
   - *Allows SSH only from specific IP*

6. **View all rules**
   - Type: `sudo ufw status numbered`
   - Press Enter
   - *Shows all firewall rules*

---

## ACTIVITY 3: MANAGING NETWORK SERVICES AND PORTS

**Do on:** Ubuntu Linux system

**What we're doing:** We're learning to manage network services, check listening ports, and understand service dependencies.

**Goal:** Understand how to view, start, stop, and manage network services.

**Time:** 20 minutes

---

## TASK 1: VIEW LISTENING PORTS AND SERVICES

**What this does:** We're checking which ports are open and which services are listening.

### CLI METHOD

1. **View all listening ports**
   - Type: `sudo ss -tlnp`
   - Press Enter
   - *Shows all TCP listening ports with processes*

2. **View only IPv4 listening ports**
   - Type: `sudo ss -tlnp4`
   - Press Enter
   - *Shows IPv4 TCP listening ports*

3. **View specific port (e.g., SSH port 22)**
   - Type: `sudo ss -tlnp | grep :22`
   - Press Enter
   - *Shows if port 22 is listening*

4. **Alternative: Use netstat**
   - Type: `sudo netstat -tlnp`
   - Press Enter
   - *Shows listening ports (older command)*

**What you should see:**
```
$ sudo ss -tlnp | grep :22
LISTEN 0      128          0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=1234,fd=3))
LISTEN 0      128             [::]:22           [::]:*    users:(("sshd",pid=1234,fd=3))
```

**Key Information:**
- `0.0.0.0:22` - Listening on all interfaces, port 22
- `sshd` - SSH daemon process
- `pid=1234` - Process ID

---

## TASK 2: MANAGE SSH SERVICE

**What this does:** We're learning to start, stop, restart, and check the status of the SSH service.

### CLI METHOD

1. **Check SSH service status**
   - Type: `systemctl status ssh`
   - Press Enter
   - *Shows SSH service status*

2. **Stop SSH service**
   - Type: `sudo systemctl stop ssh`
   - Press Enter
   - *Stops SSH service*

3. **Start SSH service**
   - Type: `sudo systemctl start ssh`
   - Press Enter
   - *Starts SSH service*

4. **Restart SSH service**
   - Type: `sudo systemctl restart ssh`
   - Press Enter
   - *Restarts SSH service*

5. **Enable SSH to start on boot**
   - Type: `sudo systemctl enable ssh`
   - Press Enter
   - *Enables SSH to start automatically*

6. **Disable SSH from starting on boot**
   - Type: `sudo systemctl disable ssh`
   - Press Enter
   - *Disables auto-start (not recommended for SSH)*

7. **View SSH service logs**
   - Type: `journalctl -u ssh -n 50`
   - Press Enter
   - *Shows last 50 log entries for SSH*

---

## TASK 3: CHECK SERVICE DEPENDENCIES

**What this does:** We're understanding which services depend on others.

### CLI METHOD

1. **View SSH service dependencies**
   - Type: `systemctl list-dependencies ssh`
   - Press Enter
   - *Shows services that SSH depends on*

2. **View what depends on SSH**
   - Type: `systemctl list-dependencies ssh --reverse`
   - Press Enter
   - *Shows services that depend on SSH*

3. **View all active services**
   - Type: `systemctl list-units --type=service --state=running`
   - Press Enter
   - *Shows all running services*

---

## ACTIVITY 4: TESTING AND TROUBLESHOOTING SSH AND FIREWALL

**Do on:** Ubuntu Linux system (and second system if available)

**What we're doing:** We're testing SSH connections, verifying firewall rules, and troubleshooting common issues.

**Goal:** Verify SSH and firewall configurations work correctly and fix common problems.

**Time:** 30 minutes

---

## TASK 1: TEST SSH CONNECTION FROM REMOTE SYSTEM

**What this does:** We're testing SSH connection from another system to verify remote access works.

### CLI METHOD

**On Remote System (or second terminal):**

1. **Test SSH connection**
   - Type: `ssh student@192.168.1.10`
   - Press Enter
   - *Replace with your Ubuntu system's IP address*
   - *Enter password when prompted*

2. **Test SSH with verbose output**
   - Type: `ssh -v student@192.168.1.10`
   - Press Enter
   - *Shows detailed connection information*

3. **Test SSH connection on specific port**
   - Type: `ssh -p 22 student@192.168.1.10`
   - Press Enter
   - *Explicitly specifies port 22*

**On Local System:**

4. **Check SSH connection attempts in logs**
   - Type: `sudo tail -f /var/log/auth.log`
   - Press Enter
   - *Shows real-time authentication attempts*
   - *Press Ctrl+C to stop*

---

## TASK 2: TEST FIREWALL RULES

**What this does:** We're verifying firewall rules are working correctly.

### CLI METHOD

1. **View current firewall rules**
   - Type: `sudo ufw status numbered`
   - Press Enter
   - *Shows all active rules*

2. **Test if port is blocked**
   - Type: `sudo ufw status | grep 80`
   - Press Enter
   - *Checks if port 80 rule exists*

3. **Test firewall from another system (if available)**
   - From another system, try: `telnet 192.168.1.10 22`
   - *Should connect if SSH is allowed*
   - *Should timeout if blocked*

4. **View firewall logs**
   - Type: `sudo tail -f /var/log/ufw.log`
   - Press Enter
   - *Shows real-time firewall activity*
   - *Press Ctrl+C to stop*

---

## TASK 3: TROUBLESHOOT COMMON SSH ISSUES

**What this does:** We're diagnosing and fixing common SSH connection problems.

### CLI METHOD

1. **Check if SSH service is running**
   - Type: `systemctl status ssh`
   - Press Enter
   - *Should show active (running)*

2. **Check if SSH port is listening**
   - Type: `sudo ss -tlnp | grep :22`
   - Press Enter
   - *Should show SSH listening on port 22*

3. **Check firewall allows SSH**
   - Type: `sudo ufw status | grep 22`
   - Press Enter
   - *Should show SSH rule*

4. **Check SSH configuration for errors**
   - Type: `sudo sshd -t`
   - Press Enter
   - *Should show no errors*

5. **View SSH error logs**
   - Type: `sudo journalctl -u ssh -n 100 --no-pager`
   - Press Enter
   - *Shows recent SSH log entries*

**Common Issues and Solutions:**

- **Connection refused:** SSH service not running → `sudo systemctl start ssh`
- **Connection timeout:** Firewall blocking → `sudo ufw allow ssh`
- **Permission denied:** Wrong credentials or SSH config issue
- **Host key verification failed:** Remove old key: `ssh-keygen -R hostname`

---

## TASK 4: TROUBLESHOOT FIREWALL ISSUES

**What this does:** We're diagnosing and fixing common firewall problems.

### CLI METHOD

1. **Check UFW status**
   - Type: `sudo ufw status verbose`
   - Press Enter
   - *Shows firewall status and policies*

2. **Check if rule exists**
   - Type: `sudo ufw status numbered | grep 22`
   - Press Enter
   - *Shows if SSH rule exists*

3. **Reload firewall rules**
   - Type: `sudo ufw reload`
   - Press Enter
   - *Reloads firewall without disabling*

4. **Reset firewall (use with caution!)**
   - Type: `sudo ufw --force reset`
   - Press Enter
   - *Resets firewall to defaults (removes all rules)*
   - *⚠️ Only use if you have physical access!*

5. **View firewall logs**
   - Type: `sudo cat /var/log/ufw.log | tail -50`
   - Press Enter
   - *Shows recent firewall log entries*

---

## ACTIVITY 5: SECURING NETWORK SERVICES WITH FIREWALL

**Do on:** Ubuntu Linux system

**What we're doing:** We're implementing security best practices for firewall configuration and SSH.

**Goal:** Configure firewall and SSH with security best practices.

**Time:** 25 minutes

---

## TASK 1: CONFIGURE SSH SECURITY SETTINGS

**What this does:** We're hardening SSH configuration for better security.

### CLI METHOD

1. **Edit SSH configuration**
   - Type: `sudo nano /etc/ssh/sshd_config`
   - Press Enter

2. **Recommended security settings:**
   - Set `PermitRootLogin no` - Prevents root login via SSH
   - Set `PasswordAuthentication yes` - Keep enabled for lab (can disable for production)
   - Set `PubkeyAuthentication yes` - Enable key-based authentication
   - Set `X11Forwarding no` - Disable if not needed
   - Set `MaxAuthTries 3` - Limit authentication attempts

3. **Save and test configuration**
   - Press **Ctrl + X**, **Y**, **Enter**
   - Type: `sudo sshd -t`
   - Press Enter
   - *Verify no errors*

4. **Restart SSH service**
   - Type: `sudo systemctl restart ssh`
   - Press Enter

---

## TASK 2: CONFIGURE FIREWALL LOGGING

**What this does:** We're enabling firewall logging to monitor network activity.

### CLI METHOD

1. **Enable UFW logging**
   - Type: `sudo ufw logging on`
   - Press Enter
   - *Enables firewall logging*

2. **Set logging level**
   - Type: `sudo ufw logging low`
   - Press Enter
   - *Sets logging to low level (options: off, low, medium, high, full)*

3. **View logging status**
   - Type: `sudo ufw status verbose`
   - Press Enter
   - *Shows logging is enabled*

4. **View firewall logs**
   - Type: `sudo tail -f /var/log/ufw.log`
   - Press Enter
   - *Shows real-time firewall logs*

---

## TASK 3: DELETE FIREWALL RULES

**What this does:** We're learning to remove firewall rules when they're no longer needed.

### CLI METHOD

1. **View numbered firewall rules**
   - Type: `sudo ufw status numbered`
   - Press Enter
   - *Shows rules with numbers*

2. **Delete rule by number**
   - Type: `sudo ufw delete 3`
   - Press Enter
   - *Deletes rule number 3*
   - *Type "y" to confirm*

3. **Delete rule by specification**
   - Type: `sudo ufw delete allow 80/tcp`
   - Press Enter
   - *Deletes HTTP allow rule*

4. **Verify rule was deleted**
   - Type: `sudo ufw status numbered`
   - Press Enter
   - *Shows updated rules list*

---

## QUICK REFERENCE: SSH AND FIREWALL COMMANDS

| Task | Command |
|------|---------|
| Install SSH server | `sudo apt install openssh-server -y` |
| Check SSH status | `systemctl status ssh` |
| Start SSH | `sudo systemctl start ssh` |
| Stop SSH | `sudo systemctl stop ssh` |
| Restart SSH | `sudo systemctl restart ssh` |
| Test SSH config | `sudo sshd -t` |
| View SSH logs | `journalctl -u ssh -n 50` |
| Connect via SSH | `ssh user@hostname` |
| Install UFW | `sudo apt install ufw -y` |
| Enable UFW | `sudo ufw enable` |
| Disable UFW | `sudo ufw disable` |
| Check UFW status | `sudo ufw status verbose` |
| Allow port | `sudo ufw allow 22/tcp` |
| Allow service | `sudo ufw allow ssh` |
| Deny port | `sudo ufw deny 80/tcp` |
| Delete rule | `sudo ufw delete 1` |
| View listening ports | `sudo ss -tlnp` |

---

## Troubleshooting Common Issues

### Issue: "Connection refused" when connecting via SSH

**Solutions:**
1. Check SSH service: `systemctl status ssh`
2. Start SSH if stopped: `sudo systemctl start ssh`
3. Check if port is listening: `sudo ss -tlnp | grep :22`
4. Check firewall: `sudo ufw status | grep 22`

### Issue: "Connection timeout" when connecting via SSH

**Solutions:**
1. Check firewall allows SSH: `sudo ufw allow ssh`
2. Verify SSH port in config: `sudo grep Port /etc/ssh/sshd_config`
3. Check network connectivity: `ping target_ip`
4. Verify firewall rule: `sudo ufw status numbered`

### Issue: "Permission denied" when connecting via SSH

**Solutions:**
1. Verify username is correct
2. Check password is correct
3. Verify user account exists: `getent passwd username`
4. Check SSH config: `sudo sshd -t`
5. View SSH logs: `sudo journalctl -u ssh -n 50`

### Issue: "UFW blocking legitimate traffic"

**Solutions:**
1. Check firewall rules: `sudo ufw status numbered`
2. Add allow rule: `sudo ufw allow port/tcp`
3. Check default policies: `sudo ufw status verbose`
4. Temporarily disable to test: `sudo ufw disable` (then re-enable!)

### Issue: "Can't delete UFW rule"

**Solutions:**
1. View numbered rules: `sudo ufw status numbered`
2. Delete by number: `sudo ufw delete number`
3. Or delete by specification: `sudo ufw delete allow port/tcp`

---

## Tips for Choosing a Method

**Use GUI When:**
- You're learning and want visual feedback
- You need to browse through settings
- You're not comfortable with command line

**Use CLI When:**
- You need precise control over configuration
- You're working on a server (no GUI)
- You want to automate configuration
- You're documenting steps for others
- You're working remotely via SSH

---

## Important Reminders

1. **Always allow SSH** before enabling UFW, or you may lock yourself out!
2. **Backup configurations** before making changes
3. **Test SSH configuration** with `sshd -t` before restarting service
4. **Use `ufw status numbered`** to see rule numbers for deletion
5. **Check firewall logs** when troubleshooting connection issues
6. **Keep SSH service enabled** for remote access
7. **Use strong passwords** or SSH keys for authentication

---

**Practice both methods to become a networking pro!**

