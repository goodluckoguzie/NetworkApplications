# Week 4 Lab Guide: DFS Namespace & Replication

Complete guide showing both GUI (DFS Management) and Terminal (PowerShell) methods.

---

## Lab Overview

### What is Week 4 About:
**Creating and Configuring Distributed File System (DFS) Namespace and Replication**

### PC Requirements:
- **One physical PC** with virtualization software (Hyper-V, VMware, or VirtualBox)
- **Two Windows Server 2022 VMs:**
  - **VM 1:** Your existing Week 3 VM (domain controller) - will be used as **LondonFS**
  - **VM 2:** New Windows Server 2022 VM you'll create - will be **ManchesterFS**
- Both servers joined to **lab.local** domain
- Administrator privileges on both servers
- Active Directory Domain Services (AD DS) already installed on VM 1 (from Week 3)
- Network connectivity between both servers
- **See "SETUP: USING TWO WINDOWS SERVER 2022 VMs" section below for detailed instructions**

### What You'll Do:
- **Activity 1:** Create a domain-based DFS namespace (\\lab.local\CompanyData) and add shared folder targets
- **Activity 2:** Configure DFS Replication between servers to synchronize the Projects folder
- **Activity 3:** Verify, monitor, and troubleshoot DFS replication

---

## What is DFS? (Simple Explanation)

**DFS (Distributed File System)** helps you access files on different servers using one simple path, and it can automatically keep files synchronized between servers.

### Real-World Example:

**Without DFS:**
- Users in London need to remember: "Projects folder is on LondonFS server" → \\LondonFS\Shares\Projects
- Users in Manchester need to remember: "Projects folder is on ManchesterFS server" → \\ManchesterFS\Shares\Projects
- If LondonFS server breaks, London users can't access files
- Files on LondonFS and ManchesterFS are different (not synchronized)

**With DFS:**
- Everyone uses the same path: \\lab.local\CompanyData\Projects
- DFS automatically directs users to the nearest server
- If one server breaks, users are automatically directed to the other server
- DFS Replication keeps files synchronized between both servers automatically

### Two Main Parts:

1. **DFS Namespace:** Creates the "virtual folder path" that users access
   - Like creating a shortcut that points to folders on different servers
   - Users don't need to know which server has the files

2. **DFS Replication:** Automatically copies files between servers
   - When you save a file on LondonFS, it automatically appears on ManchesterFS
   - Keeps both servers in sync

---

## Important Terms

| Term | Definition | Simple Explanation |
|------|------------|-------------------|
| DFS Namespace | A virtual view of shared folders on different servers, providing a single logical path | A "fake" folder path that points to real folders on different servers |
| DFS Replication | Service that synchronizes folders between multiple servers automatically | Automatically copies files between servers so they stay the same |
| Domain-based Namespace | DFS namespace stored in Active Directory (e.g., \\domain\Namespace) | The namespace is stored in AD, making it more reliable and easier to manage |
| Folder Target | Physical location (UNC path) that a DFS folder points to | The actual folder on a real server (like \\LondonFS\Shares\Projects) |
| Replication Group | Collection of servers that replicate one or more folders | A group of servers that keep their folders synchronized |
| Topology | Network structure for replication (Full Mesh, Hub and Spoke, etc.) | How the servers are connected for replication (Full Mesh = all servers talk to each other) |
| Site-Aware | DFS automatically directs clients to the nearest server based on AD sites | DFS knows where you are and sends you to the closest server |
| RDC | Remote Differential Compression - reduces bandwidth by only replicating changes | Only copies the parts of files that changed, not the whole file (saves bandwidth) |

---

## Background Knowledge: Prerequisites

**Before Starting:**
- Ensure both servers (LondonFS and ManchesterFS) are domain-joined to lab.local
- Both servers should have static IP addresses
- Create shared folders: **D:\Shares\Projects** on both servers
- Verify network connectivity between servers (ping test)
- Ensure AD replication is working (from Week 3)

### Typical Server Configuration:

| Server | Role | Domain | Shared Folder |
|--------|------|--------|---------------|
| LondonFS | File Server | lab.local | D:\Shares\Projects |
| ManchesterFS | File Server | lab.local | D:\Shares\Projects |

**Network Requirements:**
- Both servers must be able to resolve each other's hostnames
- DNS must be working correctly
- Firewall rules should allow DFS traffic (ports 135, 445, 5722, 49152-65535)

---

## SETUP: USING TWO WINDOWS SERVER 2022 VMs

**For this lab, we will use TWO Windows Server 2022 virtual machines:**

**What you already have:**
- **One Windows Server 2022 VM from Week 3** - This is your domain controller (DC) with the lab.local domain
- This VM is already set up and working

**What you need to create:**
- **One NEW Windows Server 2022 VM** - This will be your second server (ManchesterFS)

**Total VMs needed:** 2 Windows Server 2022 VMs
- VM 1: Your existing Week 3 VM (DC) - we'll use this as **LondonFS**
- VM 2: New VM you'll create - this will be **ManchesterFS**

**What this means:**
- You already have one Windows Server 2022 VM from Week 3 (the domain controller)
- You need to create a second Windows Server 2022 VM for Week 4
- Both VMs will act as file servers for DFS
- Both will be part of the lab.local domain

---

### STEP-BY-STEP SETUP: Create Second Windows Server 2022 VM

**What we're doing:** We need two Windows Server 2022 servers for DFS. You already have one from Week 3 (your domain controller). Now we need to create a second one.

**Setup Summary:**
- **VM 1 (LondonFS):** Your existing Week 3 VM - the domain controller
- **VM 2 (ManchesterFS):** New Windows Server 2022 VM - we'll create this now

**Total:** 2 Windows Server 2022 VMs running on your PC

---

### STEP 1: Prepare Your Week 3 Windows Server 2022 VM (LondonFS)

**What this does:** We'll use your existing Week 3 Windows Server 2022 VM as one of the file servers (LondonFS). This VM is already your domain controller, and we'll also use it as a file server.

1. **Start your Week 3 Windows Server 2022 VM** (the one with lab.local domain)
   - *This is the VM you created in Week 3*
   - *It should already have Windows Server 2022 installed*

2. **Log in** as **LAB\Administrator**
   - *Use the same credentials you set up in Week 3*

3. **Note the IP address** of this VM:
   - Open **Command Prompt** or **PowerShell**
   - Type: `ipconfig`
   - Write down the IPv4 Address (should be something like 192.168.1.10)
   - **This is your DC IP - we'll need it later when setting up the second VM**

**What we just did:** We identified your existing Windows Server 2022 VM (the domain controller) so we can use it as LondonFS and point the new server to it.

---

### STEP 2: Create a New Windows Server 2022 VM (ManchesterFS)

**What this does:** We're creating a second Windows Server 2022 VM that will act as ManchesterFS file server. This is the same type of VM you created in Week 3, but this one will be a file server, not a domain controller.

**Important:** This is the same process you did in Week 3, but for a second server.

1. **Open your virtualization software** (Hyper-V, VMware, or VirtualBox)
   - *The same software you used to create your Week 3 VM*

2. **Create a new virtual machine:**
   - Click **New** or **Create Virtual Machine**
   - Name it: **ManchesterFS**
   - Choose **Windows Server 2022** as the operating system
   - *This is the same Windows Server 2022 you used in Week 3*

3. **Configure VM settings:**
   - **RAM:** Allocate **2GB minimum** (4GB if your PC has 8GB+ RAM)
     - *What this means: Your PC needs enough RAM to run TWO VMs. If your PC has 8GB total, give each VM 2GB.*
   - **Hard Disk:** Create a **40GB** virtual hard disk
     - *What this means: The VM needs its own storage space. 40GB is enough for Windows Server and our lab files.*
   - **Network:** Set to **same virtual network** as your Week 3 VM
     - *What this means: Both VMs need to be on the same network so they can talk to each other.*
     - *This is very important - both VMs must be on the same virtual network!*

4. **Install Windows Server 2022:**
   - Start the VM
   - Install Windows Server 2022 (exactly the same process as Week 3)
   - When asked for computer name, type: **ManchesterFS**
   - Set Administrator password (remember this!)
   - *Note: This is the LOCAL Administrator password, not the domain password*

**What we just did:** We created a second Windows Server 2022 VM. Now you have TWO Windows Server 2022 VMs:
- VM 1: Your Week 3 VM (DC/LondonFS)
- VM 2: New VM (ManchesterFS)

---

### STEP 3: Configure Network on ManchesterFS VM

**What this does:** We're setting up the network so ManchesterFS can communicate with your domain controller.

1. **Log into ManchesterFS VM** as Administrator (local account)

2. **Set a static IP address:**
   - Right-click **Start** → **Settings** → **Network & Internet**
   - Click **Ethernet** → **Change adapter options**
   - Right-click your network adapter → **Properties**
   - Select **Internet Protocol Version 4 (TCP/IPv4)** → **Properties**
   - Select **Use the following IP address:**
     - **IP address:** `192.168.1.12`
     - **Subnet mask:** `255.255.255.0`
     - **Default gateway:** `192.168.1.1` (or your router IP)
   - Select **Use the following DNS server addresses:**
     - **Preferred DNS server:** `192.168.1.10` (your DC IP from Step 1)
     - **Alternate DNS server:** Leave empty
   - Click **OK** → **OK**

3. **Test network connectivity:**
   - Open **Command Prompt**
   - Type: `ping 192.168.1.10` (your DC IP)
   - You should see replies. If not, check your network settings.

**What we just did:** We configured ManchesterFS to use a fixed IP address and point to your domain controller for DNS. This is important so the server can find and join the domain.

---

### STEP 4: Join ManchesterFS to the Domain

**What this does:** We're making ManchesterFS part of your lab.local domain so it can work with DFS.

1. **On ManchesterFS VM**, open **Server Manager**
   - It should open automatically, or click **Start** → **Server Manager**

2. **Join to domain:**
   - In Server Manager, look at the left side
   - Click **Local Server**
   - Find **WORKGROUP** (it will show your current workgroup name)
   - **Click on "WORKGROUP"** (it's a link)
   - A "System Properties" window opens
   - Click **Change...** button
   - Select **Domain**
   - Type: `lab.local`
   - Click **OK**
   - A login window appears
   - Enter:
     - **Username:** `LAB\Administrator`
     - **Password:** (your DC Administrator password from Week 3)
   - Click **OK**
   - You'll see: "Welcome to the lab.local domain"
   - Click **OK**
   - **Important:** It will ask you to restart - click **Restart Now**

3. **After restart:**
   - Log in as **LAB\Administrator** (not local Administrator)
   - Use the same password as your DC

**What we just did:** ManchesterFS is now part of your domain. This means it can access domain resources and work with DFS.

---

### STEP 5: Verify Setup

**What this does:** We're checking that everything is set up correctly before we start the DFS lab.

**On ManchesterFS VM, run these checks:**

1. **Check domain membership:**
   - Open **PowerShell**
   - Type: `(Get-WmiObject Win32_ComputerSystem).Domain`
   - Press Enter
   - **Expected result:** Should show `lab.local`
   - If it doesn't, go back to Step 4

2. **Test connection to DC:**
   - In PowerShell, type: `Test-Connection 192.168.1.10` (your DC IP)
   - Press Enter
   - **Expected result:** Should show 4 successful pings
   - If it fails, check network settings

3. **Test DNS resolution:**
   - In PowerShell, type: `Resolve-DnsName lab.local`
   - Press Enter
   - **Expected result:** Should show your DC's IP address
   - If it fails, check DNS settings (should point to 192.168.1.10)

**On your Week 3 DC VM, verify it's running:**
- Make sure the DC VM is started
- Log in and verify it's working
- Note: We'll use this DC as **LondonFS** for the lab

**What we just did:** We confirmed that ManchesterFS can communicate with the domain and DNS is working. This is essential for DFS to work.

---

### Network Configuration Summary

**Here's what we have now - TWO Windows Server 2022 VMs:**

| Server | Hostname | IP Address | DNS | Role |
|--------|----------|------------|-----|------|
| VM 1 (Week 3) | Your-DC-Name | 192.168.1.10 | 127.0.0.1 (self) | Domain Controller (will also be LondonFS) |
| VM 2 (New) | ManchesterFS | 192.168.1.12 | 192.168.1.10 | File Server |

**What this means:**
- **VM 1:** Your existing Week 3 Windows Server 2022 VM - this is the domain controller and will also act as LondonFS
- **VM 2:** New Windows Server 2022 VM we just created - this is ManchesterFS
- Both are Windows Server 2022
- Both are on the same network and can communicate
- Both will be part of the lab.local domain

---

### Troubleshooting Setup

**Problem:** Can't ping the DC from ManchesterFS  
**Solution:**
- Check both VMs are on the same virtual network
- Verify IP addresses are correct
- Check Windows Firewall (temporarily disable to test)
- Make sure DC VM is running

**Problem:** Can't join domain  
**Solution:**
- Verify DNS on ManchesterFS points to DC (192.168.1.10)
- Check DC is running and accessible
- Verify you're using correct credentials: `LAB\Administrator`
- Make sure domain name is exactly: `lab.local` (lowercase)

**Problem:** Low performance  
**Solution:**
- Close other applications on your physical PC
- Reduce RAM allocated to VMs if needed
- Only run the VMs you need (DC + ManchesterFS)

---

**Once all checks pass, you're ready to start Activity 1!**


---

## ACTIVITY 1: CREATE A DOMAIN-BASED DFS NAMESPACE

**Do on:** Your Week 3 DC VM (this will be LondonFS)

**What we're doing:** We're creating a DFS namespace. Think of it like creating a "virtual folder" that points to real folders on different servers. Users will access files through this virtual path instead of remembering which server has the files.

**Example:** Instead of users needing to know "the Projects folder is on LondonFS server", they can just access "\\lab.local\CompanyData\Projects" and DFS will automatically direct them to the right server.

**Goal:** Create a domain-based DFS namespace (\\lab.local\CompanyData) and add a shared folder (Projects) with targets on both servers.

**Time:** 30 minutes

---

## TASK 1: INSTALL DFS NAMESPACES ROLE

**What this does:** We're installing the DFS Namespaces feature on your server. This is like installing a program that allows your server to create and manage DFS namespaces.

**Do on:** Your Week 3 DC VM (LondonFS)

### GUI METHOD (Server Manager)

1. **Log into your Week 3 DC VM** with Administrator account (LAB\Administrator)
   - *This is the VM you created in Week 3 that has the lab.local domain*

2. **Server Manager** opens automatically (or click **Start** → **Server Manager**)
   - *Server Manager is the main tool for managing Windows Server*

3. Click **Manage** (top-right corner)
   - *This opens a menu with management options*

4. Click **Add Roles and Features**
   - *This starts the wizard to install new features on the server*

5. **"Add Roles and Features Wizard" window opens**
   - *In your window, you will see a wizard with several pages*
   - *The first page says "Before You Begin"*

6. **Click Next** (Before You Begin page)
   - *In your window, you will see information about roles and features*
   - *Click the **Next** button at the bottom*

7. **Select "Role-based or feature-based installation"**
   - *In your window, you will see two options*
   - *Select the first option: "Role-based or feature-based installation"*
   - *Click **Next***

8. **Select your server from the server pool**
   - *In your window, you will see a list of servers*
   - *You should see your server name (your DC name) in the list*
   - *Select it (it may already be selected)*
   - *Click **Next***

9. **On "Server Roles" page, expand "File and Storage Services"**
   - *In your window, you will see a list of server roles with checkboxes*
   - *Find "File and Storage Services" in the list*
   - *Click the arrow (►) next to it to expand it*
   - *You will see it expand to show more options*

10. **Expand "File and iSCSI Services"**
    - *In your window, you will see "File and iSCSI Services" under File and Storage Services*
    - *Click the arrow (►) next to it*
    - *You will see it expand to show more options including "DFS Namespaces"*

11. **Check the box for "DFS Namespaces"**
    - *In your window, you will see "DFS Namespaces" with a checkbox*
    - *Click the checkbox to select it*
    - *The checkbox will show a checkmark (✓)*

12. **A popup window appears: "Add features that are required for DFS Namespaces?"**
    - *In your window, you will see a small popup window*
    - *The message says "Add features that are required for DFS Namespaces?"*
    - *You will see two buttons: "Add Features" and "Cancel"*
    - *Click **Add Features***
    - *The popup closes and you will see additional features automatically selected*

13. **Click Next**
    - *In your window, you will see the "DFS Namespaces" role is now selected*
    - *Click **Next** at the bottom*

14. **Click Next (Features page)**
    - *In your window, you will see a page titled "Features"*
    - *You don't need to select anything here*
    - *Click **Next** to continue*

15. **Click Install**
    - *In your window, you will see a confirmation page showing what will be installed*
    - *Review the information*
    - *Click **Install** at the bottom*
    - *The installation begins*

16. **Wait for installation to complete (may take a few minutes)**
    - *In your window, you will see a progress bar showing installation progress*
    - *You will see messages like "Installing DFS Namespaces..."*
    - *Wait until you see "Installation succeeded" or similar message*
    - *Be patient - this may take 2-5 minutes!*

17. **Click Close when done**
    - *In your window, you will see "Installation succeeded" or a green checkmark*
    - *Click **Close** to finish*
    - *The wizard closes*

### TERMINAL METHOD (PowerShell)

1. Right-click **Start** → Click **Windows PowerShell (Admin)**
2. Copy and run this command:

```powershell
Install-WindowsFeature FS-DFS-Namespace -IncludeManagementTools
```

This installs DFS Namespaces role and management tools.

**Wait for completion.** You'll see:
```
Success: True
Restart Needed: No
```

---

## TASK 2: CREATE SHARED FOLDERS ON BOTH SERVERS

**What this does:** Before we can use DFS, we need actual folders on both servers that we can share. We're creating a "Projects" folder on each server and making it accessible over the network (sharing it).

**Do on:** Both your DC VM (LondonFS) and ManchesterFS VM

### GUI METHOD

**On LondonFS (Your Week 3 DC VM):**

1. **Open File Explorer**
   - *Click the folder icon on the taskbar, or press Windows key + E*

2. **Navigate to D:\ drive**
   - *Click on "This PC" in the left pane*
   - *If you don't have a D: drive, that's okay - we can use C:\ instead*
   - *If using C:\, navigate to C:\Shares instead*

3. **Create the Shares folder:**
   - Right-click in empty space → **New** → **Folder**
   - Name it **Shares**
   - *This will be our main folder for shared files*

4. **Open the Shares folder**
   - Double-click **Shares** to open it

5. **Create the Projects folder:**
   - Right-click in empty space → **New** → **Folder**
   - Name it **Projects**
   - *This is the folder we'll use for DFS*

6. **Share the folder:**
   - Right-click **Projects** folder → **Properties**
   - *A "Properties" window opens*

7. **Click the "Sharing" tab**
   - *In the Properties window, you will see several tabs at the top*
   - *Click on the **Sharing** tab*
   - *You will see sharing options*

8. **Click the "Share..." button**
   - *In your window, you will see a "Share..." button*
   - *Click it*
   - *A "Network File and Folder Sharing" window opens*

9. **In the "Network File and Folder Sharing" window:**
   - *In your window, you will see a dropdown box and an "Add" button*
   - *Click the dropdown arrow*
   - *You will see a list of users/groups*
   - *Select **Everyone** from the list (or type **Domain Users** and click Add)*
   - *Click **Add***
   - *You will see "Everyone" appear in the list below with "Read" permission*

10. **Set permissions:**
    - *In your window, you will see "Everyone" in the list*
    - *Next to it, you will see a dropdown that says "Read"*
    - *Click the dropdown arrow*
    - *You will see options: "Read" and "Read/Write"*
    - *Select **Read/Write***
    - *The dropdown will now show "Read/Write"*

11. **Click Share**
    - *In your window, you will see a "Share" button at the bottom*
    - *Click it*
    - *You will see a message: "Your folder is shared"*

12. **Click Done**
    - *In your window, you will see a "Done" button*
    - *Click it*
    - *The sharing window closes*

13. **Note the share path:**
    - *In the Properties window, you will see the network path*
    - *It should show: **\\LondonFS\Shares\Projects** (or your server name)*
    - *Write this down - we'll need it later*
    - *Close the Properties window*

**On ManchesterFS VM:**

1. **Repeat all steps 1-12 above on the ManchesterFS VM**
   - *Do exactly the same thing, but on the ManchesterFS server*

2. **Note the share path:** **\\ManchesterFS\Shares\Projects**
   - *This will be slightly different because it's on a different server*

**On ManchesterFS:**
1. Repeat steps 1-12 above
2. Note the share path: **\\ManchesterFS\Shares\Projects**

### TERMINAL METHOD (PowerShell)

**On LondonFS:**

```powershell
# Create folder structure
New-Item -Path "D:\Shares\Projects" -ItemType Directory -Force

# Share the folder
New-SmbShare -Name "Shares" -Path "D:\Shares" -FullAccess "LAB\Domain Users"
```

**On ManchesterFS:**

```powershell
# Create folder structure
New-Item -Path "D:\Shares\Projects" -ItemType Directory -Force

# Share the folder
New-SmbShare -Name "Shares" -Path "D:\Shares" -FullAccess "LAB\Domain Users"
```

**Verify shares:**

```powershell
Get-SmbShare
```

---

## TASK 3: CREATE DOMAIN-BASED DFS NAMESPACE

**What this does:** We're creating the DFS namespace. This is like creating a "virtual folder path" that users will use to access files. Instead of remembering which server has the files, users just access \\lab.local\CompanyData and DFS figures out which server to use.

**Think of it like this:** 
- **Without DFS:** Users need to know "Projects folder is on LondonFS server" → \\LondonFS\Shares\Projects
- **With DFS:** Users just access "CompanyData\Projects" → \\lab.local\CompanyData\Projects
- DFS automatically directs them to the right server!

**Do on:** Your Week 3 DC VM (LondonFS)

### GUI METHOD (DFS Management)

1. **On your DC VM (LondonFS)**, open **Server Manager**
   - *Make sure you're logged in as LAB\Administrator*

2. **Open DFS Management:**
   - Click **Tools** (top-right) → **DFS Management**
   - OR press **Win + R** → Type **dfsmgmt.msc** → Press Enter
   - *DFS Management is the tool we use to configure DFS*

3. **In the left pane, right-click "Namespaces"**
   - *In your DFS Management window, you will see "Namespaces" in the left pane (tree view)*
   - *Right-click on "Namespaces"*
   - *A context menu appears*

4. **Click "New Namespace..."**
   - *In the context menu, you will see "New Namespace..."*
   - *Click it*
   - *The "New Namespace Wizard" window opens*

5. **"New Namespace Wizard" window opens**
   - *In your window, you will see the wizard with a "Namespace Server" page*
   - *You will see a text box asking for the namespace server*

6. **Namespace Server:**
   - *In your window, you will see a text box*
   - *Type your server name (your DC name, or type **LondonFS** if you renamed it)*
   - *OR click "Browse..." to find your server*
   - *Click **Next***
   - *The wizard moves to the next page*

7. **Namespace Name:**
   - *In your window, you will see a page asking for "Namespace Name"*
   - *You will see a text box*
   - *Type **CompanyData** in the text box*
   - *Click **Next***

8. **Namespace Type:**
   - *In your window, you will see a page asking for "Namespace Type"*
   - *You will see two options: "Domain-based namespace" and "Standalone namespace"*
   - *Select **Domain-based namespace** (the first option)*
   - *You will see a checkbox: "Enable Windows Server 2008 mode"*
   - *Make sure this checkbox is checked (it should be by default)*
   - *Click **Next***

9. **Review settings:**
   - *In your window, you will see a "Review Settings" page*
   - *You will see a summary showing:*
     - *Namespace Server: Your server name*
     - *Namespace Name: CompanyData*
     - *Namespace Path: **\\lab.local\CompanyData***
   - *Review to make sure everything is correct*

10. **Click Create**
    - *In your window, you will see a "Create" button*
    - *Click it*
    - *You will see a progress message: "Creating namespace..."*
    - *Then you will see: "The namespace was successfully created"*

11. **Click Close**
    - *In your window, you will see a "Close" button*
    - *Click it*
    - *The wizard closes*

12. **Verify:**
    - *Back in DFS Management, look at the left pane*
    - *You should now see **\\lab.local\CompanyData** under "Namespaces"*
    - *It will appear as a folder icon with the name "\\lab.local\CompanyData"*
    - *If you see it, the namespace was created successfully!*

### TERMINAL METHOD (PowerShell)

1. Open **Windows PowerShell (Admin)** on LondonFS
2. Copy and run this command:

```powershell
New-DfsnRoot -TargetPath "\\LondonFS\CompanyData" -Type DomainV2 -Path "\\lab.local\CompanyData"
```

**What this does:**
- Creates a domain-based namespace (DomainV2 = Windows Server 2008 mode)
- Namespace path: \\lab.local\CompanyData
- Target server: LondonFS

**Verify:**

```powershell
Get-DfsnRoot
```

---

## TASK 4: ADD SECOND NAMESPACE SERVER (ManchesterFS)

**What this does:** We're adding ManchesterFS as a second server that can host the namespace. This provides redundancy - if one server goes down, the other can still serve the namespace. This makes the system more reliable.

**Do on:** Your DC VM (LondonFS) - same server where we created the namespace

### GUI METHOD

1. **In DFS Management, expand "Namespaces"**
   - *In your DFS Management window, look at the left pane*
   - *You will see "Namespaces" with an arrow (►) next to it*
   - *Click the arrow to expand it*
   - *You will see it expand to show **\\lab.local\CompanyData***

2. **Expand \\lab.local\CompanyData**
   - *In the left pane, you will see **\\lab.local\CompanyData** with an arrow (►)*
   - *Click the arrow to expand it*
   - *You will see it expand (it may show nothing yet, or show folders if any exist)*

3. **Click on \\lab.local\CompanyData** (the namespace itself)
   - *In the left pane, click directly on **\\lab.local\CompanyData** (not the arrow)*
   - *The right pane will show details about the namespace*
   - *You will see tabs at the top of the right pane*

4. **In the right pane, click the "Namespace Servers" tab**
   - *In the right pane, you will see tabs: "General", "Namespace Servers", "Delegation", etc.*
   - *Click on the **Namespace Servers** tab*
   - *You will see a list showing your current namespace server (LondonFS or your DC name)*

5. **Right-click in the empty area** → Click **Add Namespace Server...**
   - *In the right pane, right-click in the white space below the existing server*
   - *A context menu appears*
   - *You will see "Add Namespace Server..." in the menu*
   - *Click it*
   - *An "Add Namespace Server" window opens*

6. **Namespace Server:**
   - *In the window, you will see a text box asking for "Namespace Server"*
   - *Type **ManchesterFS** in the text box*
   - *OR click "Browse..." to find ManchesterFS*
   - *Click **OK***
   - *You will see a message: "Adding namespace server..."*

7. **Wait for synchronization** (may take a minute)
   - *In your window, you will see a progress message*
   - *It will say something like "Synchronizing namespace..."*
   - *Wait until you see "The namespace server was successfully added" or similar*
   - *This may take 30-60 seconds*

8. **Verify:**
   - *Back in the right pane, look at the "Namespace Servers" tab*
   - *You should now see both servers listed:*
     - *Your DC name (or LondonFS)*
     - *ManchesterFS*
   - *Both should show as "Online" or have a green status*
   - *If you see both, the setup is working correctly!*

### TERMINAL METHOD (PowerShell)

```powershell
New-DfsnRootTarget -Path "\\lab.local\CompanyData" -TargetPath "\\ManchesterFS\CompanyData"
```

**Verify:**

```powershell
Get-DfsnRootTarget -Path "\\lab.local\CompanyData"
```

---

## TASK 5: CREATE DFS FOLDER (Projects)

**What this does:** We're creating a "virtual folder" called "Projects" inside our namespace. This folder will point to the actual Projects folders on both servers. When users access \\lab.local\CompanyData\Projects, DFS will automatically direct them to one of the real folders (either on LondonFS or ManchesterFS).

**Think of it like this:**
- We have two real folders: \\LondonFS\Shares\Projects and \\ManchesterFS\Shares\Projects
- We're creating one virtual folder: \\lab.local\CompanyData\Projects
- The virtual folder "points to" both real folders
- Users access the virtual folder, and DFS picks which real folder to use

**Do on:** Your DC VM (LondonFS)

### GUI METHOD

1. **In DFS Management, expand "Namespaces"**
   - *In your DFS Management window, look at the left pane*
   - *Make sure **\\lab.local\CompanyData** is expanded (you can see it)*

2. **Right-click \\lab.local\CompanyData**
   - *In the left pane, right-click directly on **\\lab.local\CompanyData***
   - *A context menu appears*

3. **Click "New Folder..."**
   - *In the context menu, you will see "New Folder..."*
   - *Click it*
   - *A "New Folder" window opens*

4. **Name: Type "Projects"**
   - *In the window, you will see a text box labeled "Name:"*
   - *Type **Projects** in the text box*
   - *Don't click OK yet!*

5. **Click the "Add..." button** to add folder targets
   - *In the window, you will see an "Add..." button*
   - *Click it*
   - *A "Add Folder Target" window opens*
   - *You will see a text box asking for "Path to folder target"*

6. **Folder Target Path: Type \\LondonFS\Shares\Projects**
   - *In the window, type **\\LondonFS\Shares\Projects** (or your server name)*
   - *Click **OK***
   - *The window closes and you return to the "New Folder" window*
   - *You will see the path appear in the list*

7. **Click "Add..." again**
   - *In the "New Folder" window, click **Add...** again*
   - *The "Add Folder Target" window opens again*

8. **Folder Target Path: Type \\ManchesterFS\Shares\Projects**
   - *In the window, type **\\ManchesterFS\Shares\Projects***
   - *Click **OK***
   - *You return to the "New Folder" window*

9. **Verify targets:**
   - *In the "New Folder" window, you will see a list showing both targets:*
     - *\\LondonFS\Shares\Projects*
     - *\\ManchesterFS\Shares\Projects*
   - *Both should show as "Online" (you may see a status column)*
   - *If both show as "Online", you're good to go!*

10. **Click OK**
    - *In the "New Folder" window, click **OK** at the bottom*
    - *The window closes*
    - *You will see a message: "The folder was successfully created" or similar*

11. **Verify:**
    - *Back in DFS Management, look at the left pane*
    - *Under **\\lab.local\CompanyData**, you should now see **Projects** as a folder*
    - *It will appear as a folder icon with the name "Projects"*
    - *If you see it, the folder was created successfully!*

### TERMINAL METHOD (PowerShell)

**Create folder with first target:**

```powershell
New-DfsnFolder -Path "\\lab.local\CompanyData\Projects" -TargetPath "\\LondonFS\Shares\Projects"
```

**Add second target:**

```powershell
New-DfsnFolderTarget -Path "\\lab.local\CompanyData\Projects" -TargetPath "\\ManchesterFS\Shares\Projects"
```

**Verify:**

```powershell
Get-DfsnFolder -Path "\\lab.local\CompanyData\Projects"
Get-DfsnFolderTarget -Path "\\lab.local\CompanyData\Projects"
```

---

## TASK 6: VERIFY DFS NAMESPACE

**What this does:** We're checking that our DFS namespace is working correctly. We'll verify that both folder targets are online and that we can actually access the namespace.

**Do on:** Your DC VM (LondonFS)

### GUI METHOD

1. **In DFS Management, expand \\lab.local\CompanyData**
   - *In your DFS Management window, look at the left pane*
   - *You will see **\\lab.local\CompanyData** with an arrow (►)*
   - *Click the arrow to expand it*
   - *You will see **Projects** folder appear underneath*

2. **Click on "Projects" folder**
   - *In the left pane, click directly on **Projects***
   - *The right pane will show details about the Projects folder*
   - *You will see tabs at the top: "General", "Folder Targets", "Delegation", etc.*

3. **Click the "Folder Targets" tab**
   - *In the right pane, click on the **Folder Targets** tab*
   - *You will see a list showing:*
     - *\\LondonFS\Shares\Projects (Online)*
     - *\\ManchesterFS\Shares\Projects (Online)*
   - *Both should show as "Online" - if they do, the namespace is working!*

4. **Test access: Open File Explorer**
   - *Click the **Start** button → **File Explorer** (or press Windows key + E)*
   - *File Explorer window opens*

5. **Navigate to \\lab.local\CompanyData\Projects**
   - *In File Explorer, click in the address bar at the top*
   - *Type: **\\lab.local\CompanyData\Projects***
   - *Press Enter*
   - *You will see the folder open*

6. **You should be able to access the folder**
   - *In File Explorer, you will see the contents of the Projects folder*
   - *It may be empty, or it may show files if any exist*
   - *If you can see the folder, DFS is working!*

7. **Create a test file to verify write access**
   - *Right-click in the empty space in File Explorer*
   - *Select **New** → **Text Document***
   - *Name it **TestFile.txt***
   - *If you can create the file, write access is working!*

### TERMINAL METHOD (PowerShell)

**Check namespace:**

```powershell
Get-DfsnRoot -Path "\\lab.local\CompanyData"
```

**Check folder targets:**

```powershell
Get-DfsnFolderTarget -Path "\\lab.local\CompanyData\Projects"
```

**Test access:**

```powershell
Test-Path "\\lab.local\CompanyData\Projects"
```

**List all folders in namespace:**

```powershell
Get-DfsnFolder -Path "\\lab.local\CompanyData" -Recurse
```

---

## TROUBLESHOOTING: DFS Namespace Issues

**Issue:** Namespace creation fails  
**Solution:** 
- Verify DNS resolution: `nslookup lab.local`
- Check AD replication: `repadmin /showrepl`
- Ensure both servers are domain-joined
- Verify firewall allows DFS traffic

**Issue:** Folder targets show as "Offline"  
**Solution:**
- Verify shares exist: `Get-SmbShare`
- Check share permissions
- Test UNC access: `\\LondonFS\Shares\Projects`
- Verify network connectivity: `ping LondonFS`

**Issue:** Can't access namespace from client  
**Solution:**
- Verify client is domain-joined
- Check DNS settings point to domain controller
- Test: `nslookup lab.local`
- Verify DFS client service is running

---

## ACTIVITY 2: CONFIGURE DFS REPLICATION FOR PROJECTS FOLDER

**Do on:** LondonFS (or either server)

**Goal:** Enable DFS Replication between LondonFS and ManchesterFS to synchronize the Projects folder.

**Time:** 25 minutes

---

## TASK 7: INSTALL DFS REPLICATION FEATURE

**What this does:** We're installing the DFS Replication feature on both servers. This allows the servers to automatically synchronize (copy) files between them.

**Do on:** Both your DC VM (LondonFS) and ManchesterFS VM

### GUI METHOD (Server Manager)

**On LondonFS (Your Week 3 DC VM):**

1. **Open Server Manager**
   - *Server Manager should open automatically, or click **Start** → **Server Manager***

2. **Click Manage → Add Roles and Features**
   - *In Server Manager, click **Manage** (top-right)*
   - *A menu appears*
   - *Click **Add Roles and Features***
   - *The "Add Roles and Features Wizard" window opens*

3. **Click Next (Before You Begin)**
   - *In your window, you will see the "Before You Begin" page*
   - *Click **Next** at the bottom*

4. **Select "Role-based or feature-based installation"**
   - *In your window, you will see two options*
   - *Select the first option: "Role-based or feature-based installation"*
   - *Click **Next***

5. **Select your server**
   - *In your window, you will see a list of servers*
   - *Select your server (LondonFS or your DC name)*
   - *Click **Next***

6. **Click Next (Server Roles page)**
   - *In your window, you will see the "Server Roles" page*
   - *DO NOT select anything here - we're installing a Feature, not a Role*
   - *Click **Next** to skip this page*

7. **On "Features" page, expand "File and Storage Services"**
   - *In your window, you will see the "Features" page*
   - *You will see a list of features with checkboxes*
   - *Find "File and Storage Services" in the list*
   - *Click the arrow (►) next to it to expand it*
   - *You will see it expand to show more options*

8. **Check "DFS Replication"**
   - *In your window, you will see "DFS Replication" in the expanded list*
   - *Click the checkbox next to "DFS Replication"*
   - *The checkbox will show a checkmark (✓)*

9. **Click Next**
   - *In your window, click **Next** at the bottom*
   - *You will see a confirmation page*

10. **Click Install**
    - *In your window, you will see an "Install" button*
    - *Click it*
    - *You will see a progress bar showing installation progress*

11. **Wait for installation**
    - *In your window, you will see messages like "Installing DFS Replication..."*
    - *Wait until you see "Installation succeeded" or a green checkmark*
    - *This may take 2-5 minutes*

12. **Click Close**
    - *In your window, you will see "Installation succeeded"*
    - *Click **Close***
    - *The wizard closes*

**On ManchesterFS VM:**

13. **Repeat all steps 1-12 above on the ManchesterFS VM**
    - *Do exactly the same thing, but on the ManchesterFS server*
    - *Log into ManchesterFS as LAB\Administrator*
    - *Follow the same steps to install DFS Replication*

### TERMINAL METHOD (PowerShell)

**On LondonFS:**

```powershell
Install-WindowsFeature FS-DFS-Replication -IncludeManagementTools
```

**On ManchesterFS:**

```powershell
Install-WindowsFeature FS-DFS-Replication -IncludeManagementTools
```

**Verify installation:**

```powershell
Get-WindowsFeature | Where-Object {$_.Name -like "*DFS*"}
```

---

## TASK 8: CREATE REPLICATION GROUP

**What this does:** We're creating a replication group that tells DFS which servers should replicate (synchronize) files and which folders to replicate. This is like setting up automatic file copying between the two servers.

**Do on:** Your DC VM (LondonFS)

### GUI METHOD (DFS Management)

1. **On LondonFS, open DFS Management**
   - *Click **Start** → **DFS Management** (or **Server Manager** → **Tools** → **DFS Management**)*
   - *DFS Management window opens*

2. **In the left pane, click "Replication"**
   - *In your DFS Management window, look at the left pane*
   - *You will see "Replication" in the tree*
   - *Click on it*
   - *The right pane will show replication information*

3. **Right-click "Replication" → Click "New Replication Group..."**
   - *In the left pane, right-click on "Replication"*
   - *A context menu appears*
   - *You will see "New Replication Group..." in the menu*
   - *Click it*
   - *The "New Replication Group Wizard" window opens*

4. **"New Replication Group Wizard" window opens**
   - *In your window, you will see the first page of the wizard*

5. **Replication Group Type: Select "Multipurpose replication group"**
   - *In your window, you will see two options:*
     - *"Multipurpose replication group"*
     - *"Replication group for data collection"*
   - *Select the first option: **Multipurpose replication group***
   - *Click **Next***

6. **Name: Type "ProjectsReplication"**
   - *In your window, you will see a page asking for "Name and Domain"*
   - *You will see a text box labeled "Name:"*
   - *Type **ProjectsReplication** in the text box*

7. **Description: (Optional) Type "Replication for Projects folder"**
   - *In the same window, you will see a text box labeled "Description:"*
   - *Type **Replication for Projects folder** (optional, but helpful)*
   - *Click **Next***

8. **Replication Group Members: Click "Add..."**
   - *In your window, you will see a page asking for "Replication Group Members"*
   - *You will see an "Add..." button*
   - *Click it*
   - *A "Select Computers" window opens*

9. **Type "LondonFS" → Click OK**
   - *In the window, type **LondonFS** (or your DC name)*
   - *Click **OK***
   - *You return to the wizard and will see LondonFS in the list*

10. **Click "Add..." again**
    - *In the wizard window, click **Add...** again*
    - *The "Select Computers" window opens again*

11. **Type "ManchesterFS" → Click OK**
    - *In the window, type **ManchesterFS***
    - *Click **OK***
    - *You return to the wizard*

12. **Both servers should be listed**
    - *In your window, you will see both servers listed:*
      - *LondonFS*
      - *ManchesterFS*
    - *Click **Next***

13. **Topology Selection: Select "Full mesh"**
    - *In your window, you will see a page asking for "Topology Selection"*
    - *You will see several options:*
      - *Full mesh*
      - *Hub and spoke*
      - *No topology*
    - *Select **Full mesh** (the first option)*
    - *Click **Next***

14. **Replication Group Schedule and Bandwidth:**
    - *In your window, you will see a page asking for schedule*
    - *You will see options for when replication should happen*
    - *Select **"Replicate during the specified days and times"***
    - *Click **Edit Schedule...** button*
    - *A "Schedule" window opens*

15. **Set schedule: Monday-Friday, 20:00-06:00**
    - *In the Schedule window, you will see a grid showing days and times*
    - *Select **Monday through Friday** (click and drag to select)*
    - *Find the time slots for **20:00 (8 PM) to 06:00 (6 AM)***
    - *Click and drag to select those time slots*
    - *The selected area will be highlighted*
    - *Click **OK***
    - *You return to the wizard*

16. **Click Next**
    - *In the wizard, click **Next** to continue*

17. **Primary Member: Select "LondonFS"**
    - *In your window, you will see a page asking for "Primary Member"*
    - *You will see a dropdown or list showing both servers*
    - *Select **LondonFS** (or your DC name)*
    - *This means LondonFS is the "source of truth" for files*
    - *Click **Next***

18. **Folders to Replicate: Click "Add..."**
    - *In your window, you will see a page asking for "Folders to Replicate"*
    - *You will see an "Add..." button*
    - *Click it*
    - *A "Add Folder to Replicate" window opens*

19. **Folder name: Type "Projects"**
    - *In the window, you will see a text box labeled "Folder name:"*
    - *Type **Projects***

20. **Folder path on LondonFS: Type "D:\Shares\Projects"**
    - *In the same window, you will see a text box labeled "Folder path on [server]:"*
    - *Type **D:\Shares\Projects** (or C:\Shares\Projects if you used C: drive)*
    - *Click **OK***
    - *You return to the wizard and will see "Projects" in the list*

21. **Click Next**
    - *In the wizard, click **Next** to continue*

22. **Other Members: ManchesterFS should be listed**
    - *In your window, you will see a page showing "Other Members"*
    - *You will see ManchesterFS listed*
    - *Next to it, you will see an "Edit..." button*

23. **Click "Edit..." next to ManchesterFS**
    - *Click the **Edit...** button next to ManchesterFS*
    - *A "Edit Replicated Folder" window opens*

24. **Folder path on ManchesterFS: Type "D:\Shares\Projects"**
    - *In the window, you will see a text box asking for the folder path*
    - *Type **D:\Shares\Projects** (or C:\Shares\Projects if you used C: drive)*
    - *Click **OK***
    - *You return to the wizard*

25. **Click Next**
    - *In the wizard, click **Next** to continue*

26. **Review settings**
    - *In your window, you will see a "Review Settings" page*
    - *You will see a summary showing:*
      - *Replication Group: ProjectsReplication*
      - *Members: LondonFS, ManchesterFS*
      - *Folder: Projects*
      - *Topology: Full mesh*
    - *Review to make sure everything is correct*

27. **Click Create**
    - *In your window, you will see a "Create" button*
    - *Click it*
    - *You will see progress messages: "Creating replication group..."*
    - *Then: "The replication group was successfully created"*

28. **Click Close**
    - *In your window, you will see a "Close" button*
    - *Click it*
    - *The wizard closes*

29. **Wait 5-10 minutes for AD synchronization and initial replication**
    - *Back in DFS Management, you will see "ProjectsReplication" appear under "Replication" in the left pane*
    - *Wait 5-10 minutes for everything to sync*
    - *You can check the status in DFS Management*

### TERMINAL METHOD (PowerShell)

**Create replication group:**

```powershell
New-DfsReplicationGroup -GroupName "ProjectsReplication" -Description "Replication for Projects folder"
```

**Add members:**

```powershell
Add-DfsrMember -GroupName "ProjectsReplication" -ComputerName "LondonFS"
Add-DfsrMember -GroupName "ProjectsReplication" -ComputerName "ManchesterFS"
```

**Create replicated folder:**

```powershell
New-DfsReplicatedFolder -GroupName "ProjectsReplication" -FolderName "Projects"
```

**Add folder to members:**

```powershell
Set-DfsrMembership -GroupName "ProjectsReplication" -FolderName "Projects" -ContentPath "D:\Shares\Projects" -ComputerName "LondonFS" -PrimaryMember $true
Set-DfsrMembership -GroupName "ProjectsReplication" -FolderName "Projects" -ContentPath "D:\Shares\Projects" -ComputerName "ManchesterFS" -PrimaryMember $false
```

**Set topology (Full Mesh):**

```powershell
Set-DfsrMembership -GroupName "ProjectsReplication" -FolderName "Projects" -ComputerName "LondonFS" -ContentPath "D:\Shares\Projects"
Set-DfsrMembership -GroupName "ProjectsReplication" -FolderName "Projects" -ComputerName "ManchesterFS" -ContentPath "D:\Shares\Projects"
Add-DfsrConnection -GroupName "ProjectsReplication" -SourceComputerName "LondonFS" -DestinationComputerName "ManchesterFS"
Add-DfsrConnection -GroupName "ProjectsReplication" -SourceComputerName "ManchesterFS" -DestinationComputerName "LondonFS"
```

**Note:** PowerShell method is complex. GUI method is recommended for initial setup.

---

## TASK 9: CONFIGURE REPLICATION SCHEDULE

### GUI METHOD

1. In **DFS Management**, expand **Replication**
2. Click on **ProjectsReplication**
3. In the right pane, click **Memberships** tab
4. Right-click a membership → Click **Properties**
5. Click **Schedule** tab
6. Click **Edit Schedule...**
7. Configure:
   - **Monday-Friday:** 20:00-06:00 (replication enabled)
   - **Saturday-Sunday:** 00:00-24:00 (replication enabled, or disabled if preferred)
8. Click **OK**
9. Click **OK**

### TERMINAL METHOD (PowerShell)

**Set replication schedule:**

```powershell
Set-DfsrMembership -GroupName "ProjectsReplication" -FolderName "Projects" -ComputerName "LondonFS" -ScheduleType "Scheduled"
```

**Note:** Detailed schedule configuration via PowerShell requires additional scripting.

---

## TASK 10: VERIFY REPLICATION CONFIGURATION

### GUI METHOD

1. In **DFS Management**, expand **Replication**
2. Click **ProjectsReplication**
3. Check **Memberships** tab - should show:
   - LondonFS: D:\Shares\Projects (Primary)
   - ManchesterFS: D:\Shares\Projects
4. Check **Connections** tab - should show:
   - LondonFS → ManchesterFS (Enabled)
   - ManchesterFS → LondonFS (Enabled)
5. Check **Replicated Folders** tab - should show:
   - Projects folder

### TERMINAL METHOD (PowerShell)

**Check replication group:**

```powershell
Get-DfsReplicationGroup -GroupName "ProjectsReplication"
```

**Check members:**

```powershell
Get-DfsrMember -GroupName "ProjectsReplication"
```

**Check replicated folders:**

```powershell
Get-DfsReplicatedFolder -GroupName "ProjectsReplication"
```

**Check memberships:**

```powershell
Get-DfsrMembership -GroupName "ProjectsReplication"
```

**Check connections:**

```powershell
Get-DfsrConnection -GroupName "ProjectsReplication"
```

---

## TASK 11: FORCE AD POLLING AND INITIAL REPLICATION

### GUI METHOD

1. Wait 5-10 minutes after creating replication group
2. Replication should start automatically
3. Check **Event Viewer** → **Applications and Services Logs** → **DFS Replication**
4. Look for Event ID **4102** (replication started) or **4104** (replication completed)

### TERMINAL METHOD (PowerShell)

**Force AD polling:**

```powershell
dfsrdiag PollAD
```

**Check replication status:**

```powershell
Get-DfsrState -GroupName "ProjectsReplication" -FolderName "Projects"
```

**Force replication:**

```powershell
Sync-DfsReplicationGroup -GroupName "ProjectsReplication" -SourceComputerName "LondonFS" -DestinationComputerName "ManchesterFS"
```

**Check replication health:**

```powershell
Get-DfsrBacklog -GroupName "ProjectsReplication" -FolderName "Projects" -SourceComputerName "LondonFS" -DestinationComputerName "ManchesterFS"
```

**Backlog of 0 means replication is up to date.**

---

## ACTIVITY 3: VERIFY, MONITOR, AND TROUBLESHOOT DFS REPLICATION

**Do on:** Either server

**Goal:** Verify replication is working, monitor status, and troubleshoot any issues.

**Time:** 15 minutes

---

## TASK 12: TEST REPLICATION

### GUI METHOD

1. On **LondonFS**, navigate to **D:\Shares\Projects**
2. Create a test file: **TestFile.txt** with some content
3. Wait 2-3 minutes (or force replication)
4. On **ManchesterFS**, navigate to **D:\Shares\Projects**
5. Verify **TestFile.txt** appears (replication successful!)
6. On **ManchesterFS**, modify the file
7. Wait 2-3 minutes
8. On **LondonFS**, verify changes appear

### TERMINAL METHOD (PowerShell)

**On LondonFS - Create test file:**

```powershell
New-Item -Path "D:\Shares\Projects\TestFile.txt" -ItemType File -Value "Test content from LondonFS"
```

**Wait a few minutes, then check on ManchesterFS:**

```powershell
Test-Path "D:\Shares\Projects\TestFile.txt"
Get-Content "D:\Shares\Projects\TestFile.txt"
```

---

## TASK 13: MONITOR REPLICATION STATUS

### GUI METHOD

1. In **DFS Management**, expand **Replication**
2. Click **ProjectsReplication**
3. Click **Memberships** tab
4. Check status indicators:
   - Green checkmark = Healthy
   - Yellow warning = Warning
   - Red X = Error
5. Open **Event Viewer**
6. Navigate to **Applications and Services Logs** → **DFS Replication**
7. Look for recent events:
   - **4102:** Replication started
   - **4104:** Replication completed successfully
   - **4004:** Replication error

### TERMINAL METHOD (PowerShell)

**Check replication state:**

```powershell
Get-DfsrState -GroupName "ProjectsReplication" -FolderName "Projects" | Format-List
```

**Check backlog (pending files):**

```powershell
Get-DfsrBacklog -GroupName "ProjectsReplication" -FolderName "Projects" -SourceComputerName "LondonFS" -DestinationComputerName "ManchesterFS"
```

**Check replication statistics:**

```powershell
Get-DfsrStatistics -GroupName "ProjectsReplication" -FolderName "Projects"
```

**View recent replication events:**

```powershell
Get-WinEvent -LogName "DFS Replication" -MaxEvents 20 | Format-Table TimeCreated, Id, Message -AutoSize
```

---

## TASK 14: TROUBLESHOOT REPLICATION ISSUES

### Common Issues and Solutions

**Issue:** Replication not starting  
**Solution:**
```powershell
# Force AD polling
dfsrdiag PollAD

# Check service status
Get-Service DFSR

# Restart DFS Replication service
Restart-Service DFSR
```

**Issue:** High backlog (many pending files)  
**Solution:**
```powershell
# Check backlog
Get-DfsrBacklog -GroupName "ProjectsReplication" -FolderName "Projects" -SourceComputerName "LondonFS" -DestinationComputerName "ManchesterFS"

# Force replication
Sync-DfsReplicationGroup -GroupName "ProjectsReplication" -SourceComputerName "LondonFS" -DestinationComputerName "ManchesterFS"
```

**Issue:** Replication errors in Event Viewer  
**Solution:**
- Check Event Viewer for specific error codes
- Verify network connectivity: `Test-Connection ManchesterFS`
- Check firewall rules
- Verify folder permissions
- Check disk space on both servers

**Issue:** Files not syncing  
**Solution:**
```powershell
# Check if files are in use or locked
Get-Process | Where-Object {$_.Path -like "*D:\Shares\Projects*"}

# Check folder permissions
Get-Acl "D:\Shares\Projects" | Format-List

# Verify replication group health
Get-DfsrState -GroupName "ProjectsReplication" -FolderName "Projects"
```

---

## Verification Commands Summary

### DFS Namespace Commands

```powershell
# List all namespaces
Get-DfsnRoot

# Get namespace details
Get-DfsnRoot -Path "\\lab.local\CompanyData"

# List all folders in namespace
Get-DfsnFolder -Path "\\lab.local\CompanyData" -Recurse

# Get folder targets
Get-DfsnFolderTarget -Path "\\lab.local\CompanyData\Projects"

# Test namespace access
Test-Path "\\lab.local\CompanyData\Projects"
```

### DFS Replication Commands

```powershell
# List all replication groups
Get-DfsReplicationGroup

# Get replication group details
Get-DfsReplicationGroup -GroupName "ProjectsReplication"

# List members
Get-DfsrMember -GroupName "ProjectsReplication"

# List replicated folders
Get-DfsReplicatedFolder -GroupName "ProjectsReplication"

# Check memberships
Get-DfsrMembership -GroupName "ProjectsReplication"

# Check connections
Get-DfsrConnection -GroupName "ProjectsReplication"

# Check replication state
Get-DfsrState -GroupName "ProjectsReplication" -FolderName "Projects"

# Check backlog
Get-DfsrBacklog -GroupName "ProjectsReplication" -FolderName "Projects" -SourceComputerName "LondonFS" -DestinationComputerName "ManchesterFS"

# Force AD polling
dfsrdiag PollAD

# Force replication
Sync-DfsReplicationGroup -GroupName "ProjectsReplication" -SourceComputerName "LondonFS" -DestinationComputerName "ManchesterFS"
```

### Diagnostic Commands

```powershell
# Check DFS services
Get-Service DFSR, DFS

# Check DFS replication statistics
Get-DfsrStatistics -GroupName "ProjectsReplication" -FolderName "Projects"

# View replication events
Get-WinEvent -LogName "DFS Replication" -MaxEvents 20

# Test network connectivity
Test-Connection LondonFS
Test-Connection ManchesterFS

# Check shares
Get-SmbShare

# Check DNS resolution
Resolve-DnsName lab.local
Resolve-DnsName LondonFS
Resolve-DnsName ManchesterFS
```

---

## QUICK REFERENCE CHEAT SHEET

| Action | PowerShell Command | GUI Location |
|--------|-------------------|--------------|
| Install DFS Namespaces | `Install-WindowsFeature FS-DFS-Namespace -IncludeManagementTools` | Server Manager → Add Roles → DFS Namespaces |
| Create namespace | `New-DfsnRoot -TargetPath "\\Server\Name" -Type DomainV2 -Path "\\domain\Name"` | DFS Management → New Namespace |
| Add namespace server | `New-DfsnRootTarget -Path "\\domain\Name" -TargetPath "\\Server\Name"` | DFS Management → Namespace → Add Server |
| Create DFS folder | `New-DfsnFolder -Path "\\domain\Name\Folder" -TargetPath "\\Server\Share"` | DFS Management → New Folder |
| Add folder target | `New-DfsnFolderTarget -Path "\\domain\Name\Folder" -TargetPath "\\Server\Share"` | DFS Management → Folder → Add Target |
| Install DFS Replication | `Install-WindowsFeature FS-DFS-Replication -IncludeManagementTools` | Server Manager → Add Features → DFS Replication |
| Create replication group | `New-DfsReplicationGroup -GroupName "Name"` | DFS Management → Replication → New Group |
| Add replication member | `Add-DfsrMember -GroupName "Name" -ComputerName "Server"` | Replication Group → Add Member |
| Create replicated folder | `New-DfsReplicatedFolder -GroupName "Name" -FolderName "Folder"` | Replication Group → Add Folder |
| Set membership | `Set-DfsrMembership -GroupName "Name" -FolderName "Folder" -ContentPath "Path" -ComputerName "Server"` | Replication Group → Membership → Properties |
| Force replication | `Sync-DfsReplicationGroup -GroupName "Name" -SourceComputerName "Server1" -DestinationComputerName "Server2"` | Replication Group → Membership → Replicate Now |
| Check backlog | `Get-DfsrBacklog -GroupName "Name" -FolderName "Folder" -SourceComputerName "Server1" -DestinationComputerName "Server2"` | PowerShell |
| Force AD polling | `dfsrdiag PollAD` | Command Prompt |
| List namespaces | `Get-DfsnRoot` | DFS Management |
| List replication groups | `Get-DfsReplicationGroup` | DFS Management |
| Check replication state | `Get-DfsrState -GroupName "Name" -FolderName "Folder"` | PowerShell |

---

## Screenshots to Capture for PLR

Take these screenshots for your Personal Learning Record:

**Activity 1:**
1. Server Manager showing DFS Namespaces role installed
2. DFS Management showing \\lab.local\CompanyData namespace
3. Namespace Servers tab showing LondonFS and ManchesterFS
4. Projects folder with both targets (LondonFS and ManchesterFS)
5. File Explorer accessing \\lab.local\CompanyData\Projects
6. `Get-DfsnRoot` PowerShell output
7. `Get-DfsnFolderTarget` PowerShell output

**Activity 2:**
8. Server Manager showing DFS Replication feature installed
9. New Replication Group Wizard - Group name and members
10. Replication Group - Topology (Full Mesh)
11. Replication Group - Schedule configuration
12. Replication Group - Memberships showing both servers
13. Replication Group - Connections showing bidirectional links
14. `Get-DfsReplicationGroup` PowerShell output
15. `Get-DfsrMember` PowerShell output
16. Event Viewer showing replication events (4102, 4104)

**Activity 3:**
17. Test file created on LondonFS
18. Same file replicated to ManchesterFS
19. `Get-DfsrBacklog` showing 0 backlog
20. `Get-DfsrState` showing healthy replication
21. Event Viewer replication log showing successful replication

---

## Assessment Notes

### For Your PLR, Document:

**Introduction:**
- What DFS Namespace is and why it was implemented
- How it relates to the needs of the case study (centralized access, site-aware referrals)
- What DFS Replication is and why it's needed (data consistency, high availability)
- Brief overview of tasks performed (namespace creation, replication configuration)

**Discussion:**
- Step-by-step explanation of namespace creation
- Why domain-based namespace was chosen (vs standalone)
- How folder targets enable redundancy and load distribution
- Replication group configuration and topology choice (Full Mesh)
- Why replication schedule was set (bandwidth optimization)
- Commands/tools used (New-DfsnRoot, New-DfsReplicationGroup, dfsrdiag PollAD, Event Viewer)
- How replication behavior supports business needs (data consistency, disaster recovery)

**Challenges:**
- Any errors encountered (DNS resolution, AD replication, firewall)
- How you troubleshooted (dfsrdiag PollAD, Event Viewer, backlog checks)
- Why AD polling is important
- Network and firewall considerations

**Summary:**
- What you achieved (centralized file access, automated replication)
- How DFS improves on simple file shares
- Benefits for the organization (scalability, fault tolerance, site awareness)
- Next steps (monitoring, additional folders, backup strategies)

---

## Tips for Success

**Before Starting:**
- Take a snapshot of both VMs
- Ensure both servers are domain-joined to lab.local
- Verify network connectivity between servers
- Create shared folders on both servers before starting
- Ensure AD replication is working (from Week 3)

**During Lab:**
- Take screenshots as you go
- Note any warnings or errors
- Document the namespace path and folder targets
- Test access from both servers
- Wait for AD synchronization (5-10 minutes after changes)

**After Lab:**
- Document everything in PLR immediately
- Keep VM snapshots for future reference
- Test replication with actual files
- Monitor Event Viewer for replication events
- Verify namespace access from a client computer

---

## Common Issues and Solutions

**Issue:** Namespace creation fails  
**Solution:** Ensure DNS resolution works, check AD replication, verify both servers are domain-joined

**Issue:** Folder targets show as "Offline"  
**Solution:** Verify shares exist and are accessible, check share permissions, test UNC paths

**Issue:** Replication not starting  
**Solution:** Run `dfsrdiag PollAD`, check DFS Replication service is running, verify AD replication

**Issue:** High backlog (many pending files)  
**Solution:** Check network connectivity, verify folder permissions, ensure sufficient disk space, force replication

**Issue:** Files not syncing  
**Solution:** Check if files are in use, verify replication schedule, check Event Viewer for errors, verify connections

**Issue:** Can't access namespace from client  
**Solution:** Verify client is domain-joined, check DNS settings, ensure DFS client service is running

---

**Practice both GUI and PowerShell methods to become proficient!**

