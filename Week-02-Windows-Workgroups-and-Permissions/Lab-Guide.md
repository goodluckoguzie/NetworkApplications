# Week 2 Lab Guide: GUI vs Terminal

Side-by-side comparison of GUI (clicking) and Terminal (command line) methods.

---

## Important Terms

| Definition | Term |
|------------|------|
| A group name for computers that want to share files with each other | Workgroup |
| Letting your computer find other computers nearby | Network Discovery |
| Sending a "hello" message to another computer to check if it's there | Ping |
| A folder that other computers on the network can access | Share/Shared Folder |
| A username and password that lets someone use the computer | User Account |
| A collection of users | Group |
| Rules about who can open, edit, or delete files | Permissions |
| Graphical User Interface - clicking buttons and windows | GUI |
| Command Prompt - typing commands | Terminal/CMD |
| A special account that can change important computer settings | Administrator |

---

## TASK 1: RENAME YOUR COMPUTER

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Click **Start** button (bottom-left corner of screen) | 1. Right-click **Start** button (bottom-left corner) |
| 2. Click **Settings** icon (gear icon, above the power button) | 2. Click **Windows PowerShell (Admin)** |
| 3. In Settings window, click **System** (first option, top-left, has a laptop icon) | 3. Type: `Rename-Computer -NewName "PC1"` |
| 4. On the left sidebar, scroll to the very bottom | 4. Press **Enter** |
| 5. Click **About** (last option in left menu) | 5. Type: `Restart-Computer` |
| 6. On the right side, scroll down to "Device specifications" section | 6. Press **Enter** |
| 7. Click **Rename this PC** button (blue button near computer name) | |
| 8. A small window pops up in the center | |
| 9. Type: **PC1** in the text box | |
| 10. Click **Next** button (bottom-right of popup) | |
| 11. Click **Restart now** button | |

---

## TASK 2: CHANGE WORKGROUP NAME

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Right-click **Start** button (bottom-left corner, Windows logo) | 1. Right-click **Start** button |
| 2. In the menu that pops up, click **System** (about 4th option from top, has a laptop icon) | 2. Click **Command Prompt (Admin)** or **PowerShell (Admin)** |
| 3. The Settings window opens to "About" page | 3. Type: `wmic computersystem where name="%computername%" call joindomainorworkgroup name="NetAppsLab"` |
| 4. On the right side, look for "Related settings" section (blue links) | 4. Press **Enter** |
| 5. Click **Advanced system settings** (blue link on the right side) | 5. Restart computer |
| 6. A "System Properties" window opens | |
| 7. At the top of this window, click the **Computer Name** tab | |
| 8. At the bottom of the window, click **Change...** button | |
| 9. A smaller window opens titled "Computer Name/Domain Changes" | |
| 10. In "Member of" section (bottom half), click the **Workgroup** circle/radio button | |
| 11. In the text box next to Workgroup, delete old text and type: **NetAppsLab** | |
| 12. Click **OK** button (bottom-right) | |
| 13. A welcome message appears - click **OK** | |
| 14. Another message says restart - click **OK** | |
| 15. Click **Close** on System Properties window | |
| 16. Click **Restart Now** button | |

---

## TASK 3: ENABLE NETWORK DISCOVERY

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Click **Start** button (bottom-left, Windows logo) | 1. Right-click **Start** → Click **Command Prompt (Admin)** |
| 2. Type: **Control Panel** (start typing, no need to click search box) | 2. Type: `netsh advfirewall firewall set rule group="Network Discovery" new enable=Yes` |
| 3. Press **Enter** - Control Panel window opens | 3. Press **Enter** |
| 4. In the top-right corner, make sure "View by:" is set to **Category** | 4. Type: `netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=Yes` |
| 5. Click **Network and Internet** (green globe icon, first option) | 5. Press **Enter** |
| 6. Click **Network and Sharing Center** (second option, has network icon) | |
| 7. On the LEFT sidebar, click **Change advanced sharing settings** (blue link) | |
| 8. You'll see "Private", "Guest or Public", and "All Networks" sections | |
| 9. Click the down arrow next to **Private** (if not already expanded) | |
| 10. Under Private section, find "Network discovery" | |
| 11. Click the circle next to **Turn on network discovery** | |
| 12. Also check the box: **Turn on automatic setup of network connected devices** | |
| 13. Scroll down a bit to "File and printer sharing" | |
| 14. Click the circle next to **Turn on file and printer sharing** | |
| 15. Scroll to the very bottom of the page | |
| 16. Click **Save changes** button (bottom-right corner) | |

---

## TASK 4: TURN OFF FIREWALL (Temporarily for lab)

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Click **Start** button (bottom-left corner) | 1. Right-click **Start** → Click **Command Prompt (Admin)** |
| 2. Type: **firewall** | 2. Type: `netsh advfirewall set privateprofile state off` |
| 3. In the results, click **Windows Defender Firewall** (has a shield icon) | 3. Press **Enter** |
| 4. Windows Defender Firewall window opens | |
| 5. On the LEFT sidebar, click **Turn Windows Defender Firewall on or off** (second option, has a pencil icon) | **To turn back ON:** |
| 6. You'll see two sections: "Private network settings" and "Public network settings" | Type: `netsh advfirewall set privateprofile state on` |
| 7. Under **Private network settings** (top section): | |
| 8. Click the circle next to **Turn off Windows Defender Firewall (not recommended)** | |
| 9. At the bottom of the window, click **OK** button | |
| **Remember: Turn it back ON after lab!** | |
| - Repeat steps 1-6 | |
| - Click circle next to **Turn on Windows Defender Firewall** | |
| - Click **OK** | |

---

## TASK 5: CHECK YOUR COMPUTER NAME

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Click **Start** button (bottom-left corner) | 1. Click **Start** |
| 2. Click **Settings** icon (gear icon) | 2. Type: **cmd** |
| 3. Click **System** (first big icon, top-left) | 3. Press **Enter** |
| 4. On the LEFT sidebar, scroll to bottom | 4. Type: `hostname` |
| 5. Click **About** (last option) | 5. Press **Enter** |
| 6. On the RIGHT side, look at the top section called "Device specifications" | 6. You'll see: **PC1** or **PC2** |
| 7. Find the line that says **Device name:** - this shows **PC1** or **PC2** | |
| 8. Below that, find **Workgroup:** - should show **NetAppsLab** | |

---

## TASK 6: PING (TEST IF COMPUTERS CAN TALK)

| TERMINAL METHOD | GUI METHOD (Alternative) |
|-----------------|--------------------------|
| 1. Click **Start** button | **No direct GUI for ping, but you can check if computer is visible:** |
| 2. Type: **cmd** | 1. Click **File Explorer** icon (folder icon on taskbar at bottom) |
| 3. Press **Enter** - Black window opens | - OR press **Windows key + E** |
| 4. Type: `ping PC2` (if you're on PC1) | 2. File Explorer window opens |
| OR `ping PC1` (if you're on PC2) | 3. On the LEFT sidebar, scroll down |
| 5. Press **Enter** | 4. Click **Network** (near the bottom, has a network icon) |
| 6. SUCCESS looks like: | 5. In the main area, you should see icons for: |
| `Reply from PC2... bytes=32 time<1ms TTL=128` | - **PC1** (if you're on PC2) |
| 7. FAILURE looks like: | - **PC2** (if you're on PC1) |
| `Request timed out` | 6. If you see the other computer = SUCCESS! |
| | 7. If you see "Network discovery is turned off" = Go back to Task 3 |

---

## TASK 7: VIEW SHARED FOLDERS ON ANOTHER COMPUTER

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Click **File Explorer** icon (folder on taskbar) | 1. Open **Command Prompt** |
| 2. File Explorer opens | 2. Type: `net view \\PC2` |
| 3. At the TOP, find the address bar (shows "This PC" or "Quick access") | (Replace PC2 with the other computer's name) |
| 4. Click inside the address bar (the white/gray bar at top) | 3. Press **Enter** |
| 5. Type: `\\PC2` (use two backslashes, then the other computer name) | 4. Shows list of shared folders like: |
| - If on PC2, type: `\\PC1` | `Share name    Resource` |
| 6. Press **Enter** | `-----------------------------` |
| 7. A folder window opens showing all shared folders on that computer | `SharedData    C:\SharedData` |
| 8. You should see **SharedData** folder (after you create it in next tasks) | |
| 9. If you see folders = SUCCESS | |
| 10. If you get error "Windows cannot access..." = Check firewall, network discovery | |

---

## TASK 8: CHECK IF NETWORK SERVICES ARE RUNNING

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Press **Windows key + R** (on keyboard, hold Windows key, press R) | 1. Click **Start** |
| 2. A small "Run" box appears at bottom-left | 2. Type: **powershell** |
| 3. Type: **services.msc** | 3. Press **Enter** |
| 4. Click **OK** button (or press Enter) | 4. Type: `Get-Service LanmanServer` |
| 5. "Services" window opens with a big list | 5. Press **Enter** - Should show **Running** |
| 6. The list is alphabetical - scroll down to the **S** section | 6. Type: `Get-Service LanmanWorkstation` |
| 7. Look for **Server** (Description says "Supports file, print...") | 7. Press **Enter** - Should show **Running** |
| 8. Check the **Status** column (middle) - should say **Running** | |
| 9. Scroll down to the **W** section | |
| 10. Look for **Workstation** (Description says "Creates and maintains...") | |
| 11. Check the **Status** column - should say **Running** | |
| **If either shows "Stopped":** | |
| - Right-click the service name | |
| - Click **Start** from menu | |
| - Wait for Status to change to "Running" | |

---

## TASK 9: CREATE A USER ACCOUNT

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Press **Windows key + R** keys together | 1. Right-click **Start** → Click **Command Prompt (Admin)** |
| 2. A small "Run" box appears in bottom-left corner | 2. Type: `net user User1 P@ssword123 /add` |
| 3. Type: **compmgmt.msc** | 3. Press **Enter** |
| 4. Press **Enter** or click **OK** | 4. Should see: "The command completed successfully" |
| 5. "Computer Management" window opens (big window with tree on left) | |
| 6. On the LEFT panel, look for **Local Users and Groups** | **To verify:** |
| 7. Click the arrow next to "Local Users and Groups" to expand it | `net user` |
| 8. Two items appear below it: "Users" and "Groups" | Shows list of all users (User1 should be there) |
| 9. Click **Users** | |
| 10. On the RIGHT panel, you see a list of existing users | |
| 11. Right-click in the EMPTY WHITE SPACE (not on an existing user) | |
| 12. A menu pops up - Click **New User...** | |
| 13. A "New User" window opens | |
| 14. Fill in the form: | |
| - **User name:** Type **User1** | |
| - **Full name:** (leave empty or type Full Name) | |
| - **Description:** Type **Test User** | |
| - **Password:** Type **P@ssword123** | |
| - **Confirm password:** Type **P@ssword123** again | |
| 15. Look at the checkboxes below: | |
| - UNCHECK "User must change password at next logon" | |
| - CHECK "Password never expires" | |
| 16. Click **Create** button (bottom-right) | |
| 17. User1 appears in the list! | |
| 18. Click **Close** button | |

---

## TASK 10: CREATE A GROUP

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Press **Windows key + R** | 1. Open **Command Prompt (Admin)** |
| 2. Type: **compmgmt.msc** | 2. Type: `net localgroup ProjectTeam /add` |
| 3. Press **Enter** | 3. Press **Enter** |
| 4. Computer Management window opens | 4. Should see: "The command completed successfully" |
| 5. On the LEFT panel, find **Local Users and Groups** | |
| 6. Click the arrow to expand it (if not already expanded) | **To verify:** |
| 7. You see two folders: "Users" and "Groups" | `net localgroup` |
| 8. Click **Groups** | Shows all groups (ProjectTeam should be there) |
| 9. On the RIGHT panel, you see existing groups (like Administrators, Users, etc.) | |
| 10. Right-click in the EMPTY WHITE SPACE (below the groups list) | |
| 11. A menu pops up - Click **New Group...** | |
| 12. A "New Group" window opens | |
| 13. Fill in: | |
| - **Group name:** Type **ProjectTeam** | |
| - **Description:** Type **Project Team Members** (optional) | |
| 14. Don't add members yet (we'll do that next) | |
| 15. Click **Create** button (bottom-right) | |
| 16. ProjectTeam appears in the list! | |
| 17. Click **Close** button | |

---

## TASK 11: ADD USER TO A GROUP

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Press **Windows key + R** | 1. Open **Command Prompt (Admin)** |
| 2. Type: **compmgmt.msc** | 2. Type: `net localgroup ProjectTeam User1 /add` |
| 3. Press **Enter** | 3. Press **Enter** |
| 4. Expand **Local Users and Groups** (click arrow) | 4. Should see: "The command completed successfully" |
| 5. Click **Groups** (not Users) | |
| 6. On the RIGHT side, find **ProjectTeam** in the list | |
| 7. **Double-click ProjectTeam** (double-click, not single!) | |
| 8. "ProjectTeam Properties" window opens | |
| 9. In the middle section, you see "Members:" with an empty box below | |
| 10. At the bottom, click **Add...** button | |
| 11. "Select Users" window pops up | |
| 12. In the bottom section, there's a box "Enter the object names to select" | |
| 13. Click inside that box and type: **User1** | |
| 14. Click **Check Names** button (bottom-left of this window) | |
| 15. If correct, "User1" becomes underlined | |
| 16. Click **OK** (bottom-right) | |
| 17. Back in ProjectTeam Properties, you see **User1** in the Members list! | |
| 18. Click **OK** to close | |

---

## TASK 12: CHECK WHO IS IN A GROUP

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Press **Windows key + R** | 1. Open **Command Prompt** |
| 2. Type: **compmgmt.msc** → Press **Enter** | 2. Type: `net localgroup ProjectTeam` |
| 3. Expand **Local Users and Groups** → Click **Groups** | 3. Press **Enter** |
| 4. Find **ProjectTeam** in the list on the right | 4. Shows list: |
| 5. **Double-click ProjectTeam** | `Members` |
| 6. "ProjectTeam Properties" window opens | `---------------` |
| 7. In the middle section, under "Members:", you see the list | `User1` |
| 8. Should show: **User1** | `The command completed successfully.` |
| 9. Click **OK** or **Cancel** to close | |

---

## TASK 13: CREATE A FOLDER

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Click **File Explorer** icon (folder on taskbar at bottom) | 1. Open **Command Prompt** |
| - OR press **Windows key + E** | 2. Type: `mkdir C:\SharedData` |
| 2. File Explorer opens | 3. Press **Enter** |
| 3. On the LEFT sidebar, click **This PC** (has a computer monitor icon) | 4. Folder created! |
| 4. In the main area, you see drives (like "Local Disk (C:)") | |
| 5. **Double-click Local Disk (C:)** | **To verify:** |
| 6. You're now inside the C: drive (address bar at top shows "This PC > Local Disk (C:)") | `dir C:\` |
| 7. Right-click in the EMPTY WHITE SPACE (not on an existing folder) | Shows all folders on C: drive (SharedData should be there) |
| 8. A menu pops up → Hover over **New** → A side menu appears | |
| 9. Click **Folder** from the side menu | |
| 10. A new folder appears named "New folder" with the name highlighted in blue | |
| 11. Type: **SharedData** (this replaces "New folder") | |
| 12. Press **Enter** | |
| 13. Folder created! You should see SharedData folder in C: drive | |

---

## TASK 14: SHARE A FOLDER (Give Network Access)

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Open **File Explorer** → Go to **C:\ drive** | 1. Open **Command Prompt (Admin)** |
| 2. Find the **SharedData** folder you just created | 2. Type: |
| 3. **Right-click SharedData** folder | `net share SharedData=C:\SharedData /grant:ProjectTeam,CHANGE` |
| 4. A big menu appears → Click **Properties** (at the very bottom) | 3. Press **Enter** |
| 5. "SharedData Properties" window opens with tabs at the top | 4. Should see: "SharedData was shared successfully" |
| 6. Click the **Sharing** tab (3rd or 4th tab at top) | |
| 7. You see "Network File and Folder Sharing" section | |
| 8. In the middle of this tab, click **Advanced Sharing** button | |
| 9. "Advanced Sharing" window pops up (smaller window) | |
| 10. At the top, CHECK the box: **Share this folder** | |
| 11. Below that, "Share name:" should show **SharedData** (don't change it) | |
| 12. Below that, click **Permissions** button | |
| 13. "Permissions for SharedData" window opens | |
| 14. In the top section "Group or user names:", you see **Everyone** | |
| 15. Click **Everyone** to select it | |
| 16. At the bottom, click **Remove** button (Everyone disappears) | |
| 17. Now click **Add** button (to add ProjectTeam) | |
| 18. "Select Users or Groups" window pops up | |
| 19. In the box at bottom, type: **ProjectTeam** | |
| 20. Click **Check Names** button (bottom-left) - ProjectTeam gets underlined | |
| 21. Click **OK** | |
| 22. Back in Permissions window, **ProjectTeam** now appears in the list | |
| 23. Click **ProjectTeam** to select it | |
| 24. In the bottom section "Permissions for ProjectTeam": | |
| 25. CHECK the box under **Allow** for **Change** (Read should auto-check too) | |
| 26. Click **OK** (bottom-right) | |
| 27. Click **OK** on Advanced Sharing window | |
| 28. Click **Close** on SharedData Properties (or keep open for next task) | |

---

## TASK 15: SET NTFS PERMISSIONS (File-Level Security)

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. If SharedData Properties is still open, skip to step 4 | 1. Open **Command Prompt (Admin)** |
| 2. Otherwise: Right-click **SharedData** folder → Click **Properties** | 2. Type: |
| 3. SharedData Properties window opens | `icacls "C:\SharedData" /grant ProjectTeam:(OI)(CI)M` |
| 4. At the top, click the **Security** tab (should be 4th tab) | 3. Press **Enter** |
| 5. You see two sections: | 4. Should see: "processed 1 files successfully" |
| - Top: "Group or user names:" (lists users/groups) | |
| - Bottom: "Permissions for [whoever is selected]" | **What the codes mean:** |
| 6. In the middle, click **Edit** button | - `icacls` = change permissions |
| 7. "Permissions for SharedData" window pops up | - `(OI)` = Object Inherit (applies to files) |
| 8. Click **Add** button (bottom-left area) | - `(CI)` = Container Inherit (applies to folders) |
| 9. "Select Users or Groups" window appears | - `M` = Modify permission |
| 10. In the bottom box "Enter the object names", type: **ProjectTeam** | |
| 11. Click **Check Names** (bottom-left) - ProjectTeam gets underlined | |
| 12. Click **OK** | |
| 13. Back in Permissions window, in the top list, click **ProjectTeam** to select it | |
| 14. In the bottom section "Permissions for ProjectTeam": | |
| 15. Find the row that says **Modify** (about 3rd row) | |
| 16. CHECK the box under **Allow** column for **Modify** | |
| 17. When you check Modify, it automatically checks: | |
| - Read & execute | |
| - List folder contents | |
| - Read | |
| - Write | |
| 18. Click **OK** button (bottom-right) | |
| 19. Click **OK** or **Close** to close SharedData Properties | |

---

## TASK 16: VIEW EFFECTIVE PERMISSIONS (What User1 Can Actually Do)

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Right-click **SharedData** folder → **Properties** | 1. Open **Command Prompt** |
| 2. Click **Security** tab | 2. Type: `icacls "C:\SharedData"` |
| 3. Click **Advanced** button (bottom area) | 3. Press **Enter** |
| 4. "Advanced Security Settings for SharedData" window opens | 4. Shows all permissions: |
| 5. At the top, click **Effective Access** tab (last tab on the right) | `C:\SharedData ProjectTeam:(OI)(CI)M` |
| 6. You see "View the effective access for a user, group, or device" | |
| 7. Click **Select a user** link (blue link) | **Permission codes:** |
| 8. "Select User or Group" window pops up | - `F` = Full control |
| 9. In the bottom box, type: **User1** | - `M` = Modify |
| 10. Click **Check Names** button - User1 gets underlined | - `RX` = Read & execute |
| 11. Click **OK** | - `R` = Read-only |
| 12. Back in Advanced Security Settings | |
| 13. Click **View effective access** button | |
| 14. A list appears showing exactly what User1 can do: | |
| - Traverse folder / execute file | |
| - List folder / read data | |
| - Read attributes | |
| - Read extended attributes | |
| - Create files / write data | |
| - Create folders / append data | |
| - Write attributes | |
| - Write extended attributes | |
| - Delete | |
| - Read permissions | |
| 15. Click **OK** to close all windows | |

---

## TASK 17: MAP NETWORK DRIVE (Connect to Shared Folder from Other PC)

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Open **File Explorer** | 1. Open **Command Prompt** |
| 2. Click **This PC** (left sidebar) | 2. Type: |
| 3. At the top ribbon, click **Computer** tab (or you might already see the ribbon) | `net use Z: \\PC1\SharedData /user:PC1\User1 P@ssword123` |
| 4. In the ribbon, click **Map network drive** button (has a drive icon with network cable) | 3. Press **Enter** |
| 5. A "Map Network Drive" window pops up | 4. Should see: |
| 6. **Drive:** Click the dropdown and select **Z:** | `The command completed successfully.` |
| 7. **Folder:** Click in the box and type `\\PC1\SharedData` | |
| (Replace PC1 with actual computer name) | **To make it permanent (reconnect after restart):** |
| 8. CHECK the box: **Connect using different credentials** | Add `/persistent:yes` at the end: |
| 9. At the bottom, click **Finish** button | `net use Z: \\PC1\SharedData /user:PC1\User1 P@ssword123 /persistent:yes` |
| 10. A login window appears "Windows Security" | |
| 11. **User name:** Type **PC1\User1** (computer name backslash username) | |
| 12. **Password:** Type **P@ssword123** | |
| 13. CHECK the box: **Remember my credentials** | |
| 14. Click **OK** | |
| 15. Z: drive appears in File Explorer! | |
| 16. You can now browse SharedData as if it's a local drive | |

---

## TASK 18: VIEW ALL MAPPED DRIVES

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Open **File Explorer** | 1. Open **Command Prompt** |
| 2. Click **This PC** (left sidebar) | 2. Type: `net use` |
| 3. In the main area, look under "Network locations" section | 3. Press **Enter** |
| 4. You'll see drives like: | 4. Shows table like: |
| **SharedData (\\PC1) (Z:)** | `Status       Local     Remote` |
| 5. Z: drive is your mapped network drive | `OK           Z:        \\PC1\SharedData` |

---

## TASK 19: DISCONNECT/REMOVE MAPPED DRIVE

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Open **File Explorer** | 1. Open **Command Prompt** |
| 2. Click **This PC** | 2. Type: `net use Z: /delete` |
| 3. Under "Network locations", find **Z:** drive | 3. Press **Enter** |
| 4. **Right-click Z:** drive | 4. Should see: `Z: was deleted successfully.` |
| 5. A menu appears → Click **Disconnect** | |
| 6. Z: drive disappears! | **To remove ALL mapped drives at once:** |
| | Type: `net use * /delete` |
| | Press Y to confirm for each drive |

---

## TASK 20: REMOVE USER FROM GROUP

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Press **Windows key + R** | 1. Open **Command Prompt (Admin)** |
| 2. Type: **compmgmt.msc** → Press **Enter** | 2. Type: `net localgroup ProjectTeam User1 /delete` |
| 3. Expand **Local Users and Groups** → Click **Groups** | 3. Press **Enter** |
| 4. Find and **double-click ProjectTeam** | 4. Should see: `The command completed successfully.` |
| 5. "ProjectTeam Properties" window opens | 5. User1 removed from ProjectTeam! |
| 6. In the Members list, click **User1** to select it | |
| 7. Click **Remove** button (below the Members list) | |
| 8. User1 disappears from the list | |
| 9. Click **OK** to close | |

---

## TASK 21: DELETE A USER ACCOUNT

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Press **Windows key + R** | 1. Open **Command Prompt (Admin)** |
| 2. Type: **compmgmt.msc** → Press **Enter** | 2. Type: `net user User1 /delete` |
| 3. Expand **Local Users and Groups** → Click **Users** | 3. Press **Enter** |
| 4. In the right panel, find **User1** | 4. Should see: `The command completed successfully.` |
| 5. **Right-click User1** | 5. User1 deleted! |
| 6. A menu appears → Click **Delete** | |
| 7. A warning appears: "Are you sure you want to delete the user User1?" | |
| 8. Click **Yes** | |
| 9. User1 disappears from the list | |

---

## TASK 22: DELETE A GROUP

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Press **Windows key + R** | 1. Open **Command Prompt (Admin)** |
| 2. Type: **compmgmt.msc** → Press **Enter** | 2. Type: `net localgroup ProjectTeam /delete` |
| 3. Expand **Local Users and Groups** → Click **Groups** | 3. Press **Enter** |
| 4. In the right panel, find **ProjectTeam** | 4. Should see: `The command completed successfully.` |
| 5. **Right-click ProjectTeam** | 5. Group deleted! |
| 6. A menu appears → Click **Delete** | |
| 7. A warning appears: "Are you sure you want to delete the group ProjectTeam?" | |
| 8. Click **Yes** | |
| 9. ProjectTeam disappears from the list | |

---

## TASK 23: REMOVE SHARE FROM FOLDER (Stop Sharing)

| GUI METHOD | TERMINAL METHOD |
|------------|-----------------|
| 1. Go to **C:\** drive in File Explorer | 1. Open **Command Prompt (Admin)** |
| 2. Find **SharedData** folder | 2. Type: `net share SharedData /delete` |
| 3. **Right-click SharedData** → **Properties** | 3. Press **Enter** |
| 4. Click **Sharing** tab | 4. A confirmation message: |
| 5. Click **Advanced Sharing** button | `SharedData was deleted successfully.` |
| 6. UNCHECK the box: **Share this folder** | 5. Folder is no longer shared on the network |
| 7. Click **OK** | |
| 8. Click **OK** or **Close** | |
| 9. Folder is no longer shared! | |

---

## QUICK REFERENCE CHEAT SHEET

| Terminal Command | Action | GUI Location |
|------------------|--------|--------------|
| `Rename-Computer -NewName "PC1"` | Rename computer | Settings → System → About → Rename this PC |
| `wmic computersystem where name="%computername%" call joindomainorworkgroup name="NetAppsLab"` | Change workgroup | System → Advanced system settings → Computer Name → Change |
| `hostname` | Check computer name | Settings → System → About |
| `netsh advfirewall firewall set rule group="Network Discovery" new enable=Yes` | Enable Network Discovery | Control Panel → Network and Sharing Center → Advanced sharing settings |
| `netsh advfirewall set privateprofile state off` | Turn off firewall | Control Panel → Windows Defender Firewall → Turn off |
| `ping PC2` | Ping another PC | (No GUI - use terminal) |
| `net view \\PC2` | View shared folders | File Explorer address bar: `\\PC2` |
| `Get-Service LanmanServer` | Check services | Win+R → services.msc |
| `net user User1 Password /add` | Create user | Win+R → compmgmt.msc → Users → New User |
| `net localgroup ProjectTeam /add` | Create group | Win+R → compmgmt.msc → Groups → New Group |
| `net localgroup ProjectTeam User1 /add` | Add user to group | compmgmt.msc → Groups → Double-click group → Add |
| `net localgroup ProjectTeam` | Check group members | compmgmt.msc → Groups → Double-click group |
| `mkdir C:\SharedData` | Create folder | File Explorer → Right-click → New → Folder |
| `net share SharedData=C:\SharedData /grant:ProjectTeam,CHANGE` | Share folder | Right-click folder → Properties → Sharing → Advanced Sharing |
| `icacls "C:\SharedData" /grant ProjectTeam:(OI)(CI)M` | Set NTFS permissions | Right-click folder → Properties → Security → Edit → Add |
| `icacls "C:\SharedData"` | View permissions | Right-click folder → Properties → Security → Advanced → Effective Access |
| `net use Z: \\PC1\SharedData /user:PC1\User1 Password` | Map network drive | File Explorer → This PC → Map network drive |
| `net use` | View mapped drives | File Explorer → This PC → Network locations |
| `net use Z: /delete` | Disconnect drive | Right-click drive → Disconnect |
| `net localgroup ProjectTeam User1 /delete` | Remove user from group | compmgmt.msc → Groups → Double-click → Select user → Remove |
| `net user User1 /delete` | Delete user | compmgmt.msc → Users → Right-click user → Delete |
| `net localgroup ProjectTeam /delete` | Delete group | compmgmt.msc → Groups → Right-click group → Delete |
| `net share SharedData /delete` | Stop sharing folder | Right-click folder → Properties → Sharing → Advanced Sharing → Uncheck |

---

## Tips for Choosing a Method

**Use GUI When:**
- You're learning and want to see what options are available
- You need to browse through settings
- You're not sure of the exact command
- You want visual confirmation of changes

**Use Terminal When:**
- You know exactly what you want to do
- You need to repeat the same task multiple times
- You want to create scripts to automate tasks
- You're working on a remote computer (via SSH/RDP)
- You're documenting steps for others

---

**Practice both methods to become a networking pro!**
