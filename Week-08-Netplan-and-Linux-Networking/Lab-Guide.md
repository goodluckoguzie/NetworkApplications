# Week 8 Lab Guide: Netplan & Linux Networking

Complete guide showing both GUI (Graphical User Interface) and CLI (Command Line Interface) methods.

---

## Lab Overview

### What is Week 8 About:
**Configuring and Managing Network Settings Using Netplan on Ubuntu Linux**

### PC Requirements:
- **Ubuntu 24.04 LTS** (Virtual Machine, lab PC, or LOD platform)
  - **Download Ubuntu 24.04.3 Desktop ISO:** [Direct Download Link](https://releases.ubuntu.com/24.04/ubuntu-24.04.3-desktop-amd64.iso)
  - **Alternative:** [Ubuntu Download Page](https://releases.ubuntu.com/24.04/)
  - **File:** `ubuntu-24.04.3-desktop-amd64.iso` (5.9 GB)
  - **For VirtualBox:** Use Desktop ISO (not Server)
- Terminal/Shell access
- Administrator/sudo privileges
- Network connectivity
- Basic familiarity with CLI commands from previous weeks

### What You'll Do:
- **Activity 1:** Understand Ubuntu networking architecture (Netplan, NetworkManager, systemd-resolved)
- **Activity 2:** View and understand current network configuration
- **Activity 3:** Configure DHCP networking using Netplan
- **Activity 4:** Configure static IP addressing using Netplan (Bridged mode only)
- **Activity 5:** Troubleshoot common Netplan and networking issues

---

## Important Terms

| Term | Definition | Simple Explanation |
|------|------------|-------------------|
| Netplan | Ubuntu's network configuration system | High-level YAML configuration that translates to network services |
| NetworkManager | Service that applies actual networking settings | Manages IP addresses, routes, and DNS on Desktop Ubuntu |
| systemd-networkd | Alternative network service for Ubuntu Server | Used on server installations instead of NetworkManager |
| systemd-resolved | DNS resolution service | Handles name lookups and DNS caching |
| YAML | Data serialization language | Human-readable format for configuration files |
| DHCP | Dynamic Host Configuration Protocol | Automatically assigns IP addresses from a server |
| Static IP | Manually configured IP address | Fixed IP address that doesn't change |
| Renderer | Backend service that applies Netplan config | NetworkManager or systemd-networkd |
| Interface | Network adapter/connection | Physical or virtual network card (e.g., enp0s3, ens33) |

---

## Background Knowledge: Ubuntu Networking Architecture

**How Ubuntu Networking Works:**

Ubuntu uses a layered approach to networking:

1. **Netplan** (Top Layer)
   - Reads YAML configuration files from `/etc/netplan/`
   - Does NOT configure the network directly
   - Translates YAML to backend service configuration

2. **NetworkManager** (Desktop) or **systemd-networkd** (Server)
   - Applies actual networking settings
   - Manages IP addresses, routes, DNS
   - Handles connection profiles

3. **systemd-resolved**
   - Handles DNS queries
   - Caches DNS responses
   - Manages DNS server configuration

4. **Kernel Networking Stack**
   - Low-level Linux networking
   - Handles packets, interfaces, routing

**Key Files and Directories:**
- `/etc/netplan/*.yaml` - Netplan configuration files
- `/etc/NetworkManager/system-connections/` - NetworkManager profiles
- `/run/systemd/resolve/resolv.conf` - DNS configuration
- `journalctl -u NetworkManager` - NetworkManager logs

---

## SETUP: PREPARING FOR WEEK 8

**What we're doing:** We need to set up our Ubuntu environment for network configuration.

### If you need to download Ubuntu:

**Ubuntu 24.04.3 LTS Desktop ISO Download:**
- **Direct Download:** [https://releases.ubuntu.com/24.04/ubuntu-24.04.3-desktop-amd64.iso](https://releases.ubuntu.com/24.04/ubuntu-24.04.3-desktop-amd64.iso)
- **Download Page:** [https://releases.ubuntu.com/24.04/](https://releases.ubuntu.com/24.04/)
- **File Size:** 5.9 GB
- **Version:** Ubuntu 24.04.3 LTS (Noble Numbat)
- **For VirtualBox:** Download the Desktop ISO (includes GUI)

---

## INSTALLING UBUNTU ON VIRTUALBOX

**If you need to install Ubuntu on VirtualBox, follow these steps:**

### Prerequisites:
- **VirtualBox** installed on your computer ([Download VirtualBox](https://www.virtualbox.org/wiki/Downloads))
- **Ubuntu 24.04.3 Desktop ISO** downloaded (see link above)
- **At least 25 GB free disk space** (50 GB recommended)
- **4 GB RAM available** (8 GB recommended)

---

### STEP 1: CREATE A NEW VIRTUAL MACHINE

1. **Open VirtualBox**
   - Launch VirtualBox application

2. **Click "New" button**
   - Top-left corner of VirtualBox window

3. **Configure VM Settings:**
   - **Name:** `Ubuntu 24.04` (or any name you prefer)
   - **Machine Folder:** Leave default
   - **Type:** `Linux`
   - **Version:** `Ubuntu (64-bit)`
   - Click **Next**

4. **Set Memory (RAM):**
   - **Recommended:** 4096 MB (4 GB) or more
   - **Minimum:** 2048 MB (2 GB)
   - Move the slider or type the value
   - Click **Next**

5. **Create Virtual Hard Disk:**
   - Select **"Create a virtual hard disk now"**
   - Click **Create**

6. **Hard Disk File Type:**
   - Select **VDI (VirtualBox Disk Image)**
   - Click **Next**

7. **Storage on Physical Hard Disk:**
   - Select **"Dynamically allocated"** (recommended)
   - Click **Next**

8. **File Location and Size:**
   - **Location:** Leave default or choose custom location
   - **Size:** Set to **25 GB minimum** (50 GB recommended)
   - Move slider or type the value
   - Click **Create**

**VM is now created!**

---

### STEP 2: CONFIGURE VIRTUAL MACHINE SETTINGS

1. **Select your VM** in VirtualBox (left panel)
   - Click on "Ubuntu 24.04" (or your VM name)

2. **Click "Settings" button** (gear icon)

3. **System Settings:**
   - **Motherboard Tab:**
     - Boot Order: Check **Optical** and **Hard Disk**
     - Move **Optical** to top (drag it up)
   - **Processor Tab:**
     - Processors: Set to **2** (if you have 4+ cores)
     - Enable **PAE/NX** (if available)

4. **Display Settings:**
   - **Screen Tab:**
     - Video Memory: **128 MB** (or higher)
     - Enable **3D Acceleration** (if available)

5. **Storage Settings:**
   - Click **Storage** in left menu
   - Under **Controller: IDE**, click the **empty disk icon**
   - Click the **CD/DVD icon** on the right
   - Click **"Choose a disk file..."**
   - Navigate to your downloaded Ubuntu ISO file
   - Select `ubuntu-24.04.3-desktop-amd64.iso`
   - Click **Open**

6. **Network Settings:**
   - Click **Network** in left menu
   - **Adapter 1 Tab:**
     - **Attached to:** `NAT` (default - good for internet access)
     - **Note:** For static IP configuration, you'll need **Bridged Adapter** (we'll cover this in the lab)

7. **Click OK** to save settings

---

### STEP 3: INSTALL UBUNTU

1. **Start the Virtual Machine:**
   - Select your VM
   - Click **Start** button (green arrow)

2. **Ubuntu Boot Menu:**
   - You'll see Ubuntu boot options
   - Select **"Try or Install Ubuntu"** (first option)
   - Press **Enter**

3. **Wait for Ubuntu to Load:**
   - Ubuntu will boot from the ISO
   - This may take a few minutes
   - You'll see the Ubuntu desktop

4. **Start Installation:**
   - Double-click **"Install Ubuntu"** icon on desktop
   - OR click **"Install Ubuntu"** button if shown

5. **Installation Wizard:**

   **Step 1: Welcome**
   - Select your **Language** (English)
   - Click **Continue**

   **Step 2: Keyboard Layout**
   - Select your keyboard layout
   - Click **Continue**

   **Step 3: Updates and Other Software**
   - Select **"Normal installation"**
   - Check **"Install third-party software"** (optional)
   - Click **Continue**

   **Step 4: Installation Type**
   - Select **"Erase disk and install Ubuntu"**
   - **Don't worry!** This only erases the virtual disk, not your real computer
   - Click **Install Now**
   - Click **Continue** to confirm

   **Step 5: Where Are You?**
   - Select your **time zone** (click on map or type city)
   - Click **Continue**

   **Step 6: Who Are You?**
   - **Your name:** `student` (or your name)
   - **Your computer's name:** `ubuntu-vm` (or any name)
   - **Pick a username:** `student` (or your choice)
   - **Choose a password:** Create a strong password (remember it!)
   - **Confirm your password:** Type it again
   - **Require my password to log in:** Check this box
   - Click **Continue**

6. **Installation Progress:**
   - Ubuntu will install (takes 10-20 minutes)
   - You'll see progress slides while it installs
   - **Don't close the window!**

7. **Installation Complete:**
   - You'll see **"Installation Complete"** message
   - Click **Restart Now**

8. **Restart:**
   - VM will restart
   - You may see a message about removing installation media
   - Press **Enter** when prompted
   - Ubuntu will boot to login screen

---

### STEP 4: FIRST BOOT AND SETUP

1. **Login:**
   - Enter your password
   - Press **Enter**

2. **Welcome Screen:**
   - Ubuntu will show welcome/setup screens
   - Follow the prompts (language, privacy, etc.)
   - Click **Next** or **Skip** as appropriate

3. **Ubuntu Desktop:**
   - You should now see the Ubuntu desktop
   - Installation is complete!

---

### STEP 5: INSTALL VIRTUALBOX GUEST ADDITIONS (OPTIONAL BUT RECOMMENDED)

**Guest Additions improve VM performance and features:**

1. **In Ubuntu VM:**
   - Click **Devices** menu (top of VM window)
   - Select **"Insert Guest Additions CD image..."**

2. **In Ubuntu:**
   - A dialog may appear - click **Run**
   - OR open Terminal and run:
     ```bash
     cd /media/student/VBox_GAs_*
     sudo ./VBoxLinuxAdditions.run
     ```

3. **Enter password** when prompted

4. **Restart VM:**
   - Type: `sudo reboot`
   - Press Enter

---

### TROUBLESHOOTING INSTALLATION

**Problem: VM won't start**
- **Solution:** Enable virtualization in BIOS (Intel VT-x or AMD-V)

**Problem: Installation is slow**
- **Solution:** Increase RAM allocation, close other programs

**Problem: Screen is too small**
- **Solution:** Install Guest Additions (see Step 5 above)

**Problem: Can't see mouse cursor**
- **Solution:** Press **Right Ctrl** key to release mouse from VM

**Problem: Network not working**
- **Solution:** Check Network settings in VM, ensure NAT is enabled

---

### VERIFY INSTALLATION

1. **Open Terminal:**
   - Press **Ctrl + Alt + T**

2. **Check Ubuntu version:**
   ```bash
   lsb_release -a
   ```
   Should show: Ubuntu 24.04.3 LTS

3. **Check network:**
   ```bash
   ip a
   ```
   Should show network interface with IP address

4. **Test internet:**
   ```bash
   ping -c 4 8.8.8.8
   ```
   Should show successful ping responses

---

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

4. **Check your network interface name**
   - Type: `ip a`
   - Press Enter
   - *Note the interface name (e.g., enp0s3, ens33, eth0)*

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

## ACTIVITY 1: UNDERSTANDING UBUNTU NETWORKING ARCHITECTURE

**Do on:** Ubuntu Linux system

**What we're doing:** We're exploring how Ubuntu handles networking to understand the relationship between Netplan, NetworkManager, and systemd-resolved.

**Goal:** Understand where network configuration lives and how it's applied.

**Time:** 20 minutes

---

## TASK 1: VIEW CURRENT NETPLAN CONFIGURATION

**What this does:** We're checking the current Netplan configuration to see how networking is set up.

### CLI METHOD

1. **View Netplan configuration files**
   - Type: `sudo cat /etc/netplan/*.yaml`
   - Press Enter
   - *You will see the current Netplan configuration*

**What you should see:**
```yaml
# Let NetworkManager manage all devices on this system
network:
    version: 2
    renderer: NetworkManager
```

**OR:**
```yaml
network:
    version: 2
    ethernets:
        enp0s3:
            dhcp4: true
```

2. **List all Netplan files**
   - Type: `ls -la /etc/netplan/`
   - Press Enter
   - *You will see all Netplan configuration files*

**What you should see:**
```
$ ls -la /etc/netplan/
total 16
drwxr-xr-x  2 root root  4096 Dec  3 10:00 .
drwxr-xr-x 95 root root  4096 Dec  3 10:00 ..
-rw-r--r--  1 root root   104 Dec  3 10:00 01-network-manager-all.yaml
```

---

## TASK 2: UNDERSTAND NETWORK INTERFACE NAMES

**What this does:** We're identifying the network interface name, which is required for Netplan configuration.

### CLI METHOD

1. **View all network interfaces**
   - Type: `ip a`
   - Press Enter
   - *You will see all network interfaces with their IP addresses*

**What you should see:**
```
$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:xx:xx:xx brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.10/24 brd 192.168.1.255 scope global dynamic noprefixroute enp0s3
       valid_lft 86399sec preferred_lft 86399sec
```

**Key Information:**
- Interface name: `enp0s3` (or `ens33`, `eth0`, etc.)
- Current IP: `192.168.1.10`
- Subnet: `/24` (255.255.255.0)
- State: `UP` (interface is active)

2. **View only interface names**
   - Type: `ip link show`
   - Press Enter
   - *You will see interface names and their status*

---

## TASK 3: CHECK NETWORK SERVICES STATUS

**What this does:** We're verifying that NetworkManager and systemd-resolved are running.

### CLI METHOD

1. **Check NetworkManager status**
   - Type: `systemctl status NetworkManager`
   - Press Enter
   - *You will see the service status*

**What you should see:**
```
$ systemctl status NetworkManager
● NetworkManager.service - Network Manager
   Loaded: loaded (/lib/systemd/system/NetworkManager.service; enabled; vendor preset: enabled)
   Active: active (running) since ...
```

2. **Check systemd-resolved status**
   - Type: `systemctl status systemd-resolved`
   - Press Enter

3. **View DNS configuration**
   - Type: `resolvectl status`
   - Press Enter
   - *You will see DNS servers and configuration*

**What you should see:**
```
$ resolvectl status
Global
       Protocols: LLMNR=resolve -mDNS -DNSOverTLS DNSSEC=no/unsupported
resolv.conf mode: stub
     DNS Servers: 8.8.8.8
                  8.8.4.4
```

---

## ACTIVITY 2: VIEWING CURRENT NETWORK CONFIGURATION

**Do on:** Ubuntu Linux system

**What we're doing:** We're using network commands to view current IP, routing, and DNS settings.

**Goal:** Understand how to check network configuration using command-line tools.

**Time:** 15 minutes

---

## TASK 1: VIEW IP ADDRESS AND INTERFACE INFORMATION

**What this does:** We're checking the current IP address and network interface details.

### CLI METHOD

1. **View IP addresses**
   - Type: `ip a`
   - Press Enter
   - *Shows all interfaces with IP addresses*

2. **View only IPv4 addresses**
   - Type: `ip -4 a`
   - Press Enter
   - *Shows only IPv4 addresses*

3. **View IP address for specific interface**
   - Type: `ip a show enp0s3`
   - Press Enter
   - *Replace `enp0s3` with your interface name*

4. **Alternative: Use hostname command**
   - Type: `hostname -I`
   - Press Enter
   - *Shows IP address(es) of the system*

---

## TASK 2: VIEW ROUTING TABLE

**What this does:** We're checking the routing table to see the default gateway and network routes.

### CLI METHOD

1. **View routing table**
   - Type: `ip r`
   - Press Enter
   - *Shows routing table*

**What you should see:**
```
$ ip r
default via 192.168.1.1 dev enp0s3 proto dhcp metric 100
192.168.1.0/24 dev enp0s3 proto kernel scope link src 192.168.1.10 metric 100
```

**Key Information:**
- Default gateway: `192.168.1.1`
- Network: `192.168.1.0/24`
- Interface: `enp0s3`

2. **View default route only**
   - Type: `ip r | grep default`
   - Press Enter

---

## TASK 3: VIEW DNS CONFIGURATION

**What this does:** We're checking DNS server configuration.

### CLI METHOD

1. **View DNS status**
   - Type: `resolvectl status`
   - Press Enter
   - *Shows DNS servers and configuration*

2. **View DNS from resolv.conf**
   - Type: `cat /run/systemd/resolve/resolv.conf`
   - Press Enter
   - *Shows DNS configuration file*

3. **Test DNS resolution**
   - Type: `nslookup google.com`
   - Press Enter
   - *Tests if DNS is working*

---

## ACTIVITY 3: CONFIGURING DHCP NETWORKING

**Do on:** Ubuntu Linux system

**What we're doing:** We're configuring the network to use DHCP (automatic IP assignment). This is the default and recommended for most setups, especially VirtualBox NAT mode.

**Goal:** Configure DHCP networking using Netplan.

**Time:** 20 minutes

---

## TASK 1: CREATE OR MODIFY NETPLAN CONFIGURATION FOR DHCP

**What this does:** We're creating a Netplan configuration file to enable DHCP.

### CLI METHOD

1. **Backup existing configuration**
   - Type: `sudo cp /etc/netplan/*.yaml /etc/netplan/backup.yaml`
   - Press Enter
   - *Creates a backup*

2. **Identify your network interface**
   - Type: `ip a`
   - Press Enter
   - *Note your interface name (e.g., enp0s3)*

3. **Create or edit Netplan configuration**
   - Type: `sudo nano /etc/netplan/01-netcfg.yaml`
   - Press Enter
   - *Opens the editor*

4. **Add DHCP configuration**
   - Type the following (replace `enp0s3` with your interface name):

```yaml
network:
    version: 2
    renderer: NetworkManager
    ethernets:
        enp0s3:
            dhcp4: true
```

5. **Save and exit**
   - Press **Ctrl + X**
   - Press **Y** to confirm
   - Press **Enter** to save

**Important Notes:**
- **VirtualBox NAT mode:** Always use DHCP (static IP won't work)
- **Bridged mode:** Can use either DHCP or static IP
- Use `NetworkManager` as renderer for Desktop Ubuntu
- Use `networkd` as renderer for Server Ubuntu

---

## TASK 2: TEST NETPLAN CONFIGURATION

**What this does:** We're testing the Netplan configuration before applying it to avoid breaking network connectivity.

### CLI METHOD

1. **Test Netplan configuration**
   - Type: `sudo netplan try`
   - Press Enter
   - *Tests the configuration for 120 seconds*

**What you should see:**
```
$ sudo netplan try
Do you want to keep these settings?

Press ENTER before the timeout to accept the new configuration
```

2. **If configuration is correct:**
   - Press **Enter** within 120 seconds to keep the settings
   - *Configuration is applied*

3. **If configuration has errors:**
   - Wait 120 seconds or press **Ctrl + C**
   - *Configuration reverts to previous settings*

**Common Errors:**
- Wrong interface name → Use `ip a` to find correct name
- YAML syntax errors → Check indentation (use spaces, not tabs)
- Permission errors → Ensure file permissions are correct (644)

---

## TASK 3: APPLY NETPLAN CONFIGURATION

**What this does:** We're applying the Netplan configuration permanently.

### CLI METHOD

1. **Apply Netplan configuration**
   - Type: `sudo netplan apply`
   - Press Enter
   - *Applies the configuration immediately*

2. **Verify network connectivity**
   - Type: `ip a`
   - Press Enter
   - *Check if IP address is assigned*

3. **Test connectivity**
   - Type: `ping -c 4 8.8.8.8`
   - Press Enter
   - *Tests internet connectivity*

**What you should see:**
```
$ ping -c 4 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=118 time=12.3 ms
...
```

---

## ACTIVITY 4: CONFIGURING STATIC IP ADDRESSING

**⚠️ IMPORTANT WARNING:**
- **Only configure static IP in Bridged mode**
- **VirtualBox NAT mode does NOT support static IP**
- **If you set static IP in NAT mode, you will lose network connectivity**

**Do on:** Ubuntu Linux system (Bridged mode only)

**What we're doing:** We're configuring a static IP address for situations where a fixed IP is required.

**Goal:** Configure static IP addressing using Netplan.

**Time:** 25 minutes

---

## TASK 1: GATHER NETWORK INFORMATION

**What this does:** We're collecting the network information needed for static IP configuration.

### CLI METHOD

1. **Check current network settings**
   - Type: `ip a`
   - Press Enter
   - *Note current IP, subnet, and interface*

2. **Check default gateway**
   - Type: `ip r | grep default`
   - Press Enter
   - *Note the gateway IP*

3. **Check DNS servers**
   - Type: `resolvectl status`
   - Press Enter
   - *Note DNS server IPs*

**Example Information:**
- Interface: `enp0s3`
- Current IP: `192.168.1.10`
- Subnet: `/24` (255.255.255.0)
- Gateway: `192.168.1.1`
- DNS: `8.8.8.8`, `8.8.4.4`

---

## TASK 2: CREATE STATIC IP NETPLAN CONFIGURATION

**What this does:** We're creating a Netplan configuration file with static IP settings.

### CLI METHOD

1. **Backup existing configuration**
   - Type: `sudo cp /etc/netplan/*.yaml /etc/netplan/backup-dhcp.yaml`
   - Press Enter

2. **Edit Netplan configuration**
   - Type: `sudo nano /etc/netplan/01-netcfg.yaml`
   - Press Enter

3. **Add static IP configuration**
   - Type the following (replace with your values):

```yaml
network:
    version: 2
    renderer: NetworkManager
    ethernets:
        enp0s3:
            dhcp4: false
            addresses:
                - 192.168.1.100/24
            routes:
                - to: default
                  via: 192.168.1.1
            nameservers:
                addresses:
                    - 8.8.8.8
                    - 8.8.4.4
```

**Configuration Explanation:**
- `dhcp4: false` - Disables DHCP
- `addresses: - 192.168.1.100/24` - Sets static IP and subnet
- `routes: - to: default via: 192.168.1.1` - Sets default gateway
- `nameservers: addresses:` - Sets DNS servers

4. **Save and exit**
   - Press **Ctrl + X**
   - Press **Y** to confirm
   - Press **Enter**

---

## TASK 3: TEST AND APPLY STATIC IP CONFIGURATION

**What this does:** We're testing and applying the static IP configuration.

### CLI METHOD

1. **Test configuration**
   - Type: `sudo netplan try`
   - Press Enter
   - *Tests for 120 seconds*

2. **If test is successful:**
   - Press **Enter** to accept
   - *Configuration is applied*

3. **Verify static IP**
   - Type: `ip a`
   - Press Enter
   - *Check if static IP is assigned*

4. **Test connectivity**
   - Type: `ping -c 4 192.168.1.1`
   - Press Enter
   - *Test gateway connectivity*

5. **Test DNS**
   - Type: `nslookup google.com`
   - Press Enter
   - *Test DNS resolution*

---

## ACTIVITY 5: TROUBLESHOOTING NETPLAN AND NETWORKING

**Do on:** Ubuntu Linux system

**What we're doing:** We're learning to troubleshoot common Netplan and networking issues.

**Goal:** Identify and fix common networking problems.

**Time:** 30 minutes

---

## TASK 1: TROUBLESHOOT YAML SYNTAX ERRORS

**What this does:** We're identifying and fixing YAML syntax errors in Netplan configuration.

### CLI METHOD

1. **Validate Netplan configuration**
   - Type: `sudo netplan --debug generate`
   - Press Enter
   - *Shows detailed error messages*

**Common YAML Errors:**
- Wrong indentation (must use spaces, not tabs)
- Missing colons (`:`) after keys
- Incorrect structure

2. **Check file permissions**
   - Type: `ls -la /etc/netplan/`
   - Press Enter
   - *Should show 644 permissions*

3. **Fix permissions if needed**
   - Type: `sudo chmod 644 /etc/netplan/*.yaml`
   - Press Enter

---

## TASK 2: TROUBLESHOOT DNS ISSUES

**What this does:** We're diagnosing and fixing DNS resolution problems.

### CLI METHOD

1. **Check DNS configuration**
   - Type: `resolvectl status`
   - Press Enter

2. **Test DNS resolution**
   - Type: `nslookup google.com`
   - Press Enter

3. **If DNS fails, check Netplan configuration**
   - Type: `sudo cat /etc/netplan/*.yaml`
   - Press Enter
   - *Verify nameservers are configured*

4. **Restart systemd-resolved**
   - Type: `sudo systemctl restart systemd-resolved`
   - Press Enter

5. **Flush DNS cache**
   - Type: `sudo systemd-resolve --flush-caches`
   - Press Enter

---

## TASK 3: TROUBLESHOOT NETWORK INTERFACE ISSUES

**What this does:** We're diagnosing network interface problems.

### CLI METHOD

1. **Check interface status**
   - Type: `ip link show`
   - Press Enter
   - *Check if interface is UP*

2. **If interface is DOWN:**
   - Type: `sudo ip link set enp0s3 up`
   - Press Enter
   - *Replace `enp0s3` with your interface name*

3. **Check NetworkManager status**
   - Type: `systemctl status NetworkManager`
   - Press Enter

4. **Restart NetworkManager**
   - Type: `sudo systemctl restart NetworkManager`
   - Press Enter

5. **View NetworkManager logs**
   - Type: `journalctl -u NetworkManager -n 50`
   - Press Enter
   - *Shows recent NetworkManager logs*

---

## TASK 4: RESTORE NETWORK CONFIGURATION

**What this does:** We're restoring a previous working configuration if something goes wrong.

### CLI METHOD

1. **List backup files**
   - Type: `ls -la /etc/netplan/backup*`
   - Press Enter

2. **Restore from backup**
   - Type: `sudo cp /etc/netplan/backup.yaml /etc/netplan/01-netcfg.yaml`
   - Press Enter

3. **Apply restored configuration**
   - Type: `sudo netplan apply`
   - Press Enter

4. **Verify connectivity**
   - Type: `ping -c 4 8.8.8.8`
   - Press Enter

---

## QUICK REFERENCE: NETPLAN COMMANDS

| Task | Command |
|------|---------|
| View Netplan config | `sudo cat /etc/netplan/*.yaml` |
| Test Netplan config | `sudo netplan try` |
| Apply Netplan config | `sudo netplan apply` |
| Generate config | `sudo netplan generate` |
| View IP address | `ip a` or `hostname -I` |
| View routing table | `ip r` |
| View DNS config | `resolvectl status` |
| Check interface | `ip link show` |
| Restart NetworkManager | `sudo systemctl restart NetworkManager` |

---

## Troubleshooting Common Issues

### Issue: "Network unreachable" or "No internet"

**Solutions:**
1. Check if interface is UP: `ip link show`
2. Verify Netplan config: `sudo cat /etc/netplan/*.yaml`
3. Test configuration: `sudo netplan try`
4. Restart NetworkManager: `sudo systemctl restart NetworkManager`

### Issue: "YAML syntax error"

**Solutions:**
1. Check indentation (use spaces, not tabs)
2. Validate syntax: `sudo netplan --debug generate`
3. Check file permissions: `ls -la /etc/netplan/`

### Issue: "DNS not working"

**Solutions:**
1. Check DNS config: `resolvectl status`
2. Verify nameservers in Netplan config
3. Restart systemd-resolved: `sudo systemctl restart systemd-resolved`
4. Flush DNS cache: `sudo systemd-resolve --flush-caches`

### Issue: "Wrong interface name"

**Solutions:**
1. Find correct name: `ip a`
2. Update Netplan config with correct interface name
3. Apply: `sudo netplan apply`

---

## Tips for Choosing a Method

**Use GUI When:**
- You're learning and want to see visual network settings
- You need to quickly change network settings
- You're not comfortable with YAML syntax

**Use CLI When:**
- You need precise control over configuration
- You're working on a server (no GUI)
- You want to automate network configuration
- You're documenting steps for others

---

## Important Reminders

1. **Always backup** Netplan configuration before making changes
2. **Use `netplan try`** before `netplan apply` to test safely
3. **VirtualBox NAT mode** requires DHCP (static IP won't work)
4. **Bridged mode** supports both DHCP and static IP
5. **Check interface name** using `ip a` before configuring
6. **Use spaces, not tabs** for YAML indentation
7. **Keep NetworkManager as renderer** for Desktop Ubuntu

---

**Practice both methods to become a networking pro!**
