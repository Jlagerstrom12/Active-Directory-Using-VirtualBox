# Active Directory Using VirtualBox

Ever wondered how computer networks work in offices or schools? This guide helps you build a mini version right on your own computer using free tools. You'll set up a Windows Server with Active Directory (a way to manage users and computers) and connect a Windows 10 computer to it, just like a basic corporate network.

## Step 1: Get Your Tools

First, you need to download a few things. Don't worry, we'll grab them one by one.

* **Oracle VirtualBox:** [This free software lets you run virtual computers inside your actual computer. Download it from the official VirtualBox website and install it.](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbFRoa2Q1ZU5QS00zNVhwMGtJVy1DeE9rUm9BZ3xBQ3Jtc0tsUlBXU2NyMm9MR2txXzU1NV9JOU5MandRdmZYZTE3MHpSMmpNb3hqU09TT1dzVXZ2Z3loT0R4ZFFnSXNhaGN0VDdOdWExRW5fVWV2ejN0aGVOUUFzVGNpLXpnN3htN203dDZQdm5NRWNKWFlXcWh5OA&q=https%3A%2F%2Fwww.virtualbox.org%2Fwiki%2FDownloads&v=MHsI8hJmggI)
* **VirtualBox Extension Pack:** This adds extra features to VirtualBox. Find it on the same download page as VirtualBox and install it *after* you install VirtualBox.
* **Windows Server 2019 ISO:** [This is the operating system for your server. You can usually download a free evaluation version from the Microsoft Evaluation Center. Look for the "ISO" download option. Save it somewhere easy to find, like your Desktop.](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbWJZTlp2YXJCR0lEN1lWWmtBcVUwOWdHNE5YZ3xBQ3Jtc0tsUHJTdnRSYW5PSlFJV0tteFFWMWhTTXVKYnE0cTkwZ3lXdnRNWUg4OEloaVRMRndKSnNqdTRlT2E4Ymg3azl5cFhmdEJNbzVSOEVaTWZwaW9STmt5bFRHT2k1b3pNLWpaSXI0V2hKZWV0ak5yZ1Rzbw&q=https%3A%2F%2Fwww.microsoft.com%2Fen-us%2Fevalcenter%2Fdownload-windows-server-2019&v=MHsI8hJmggI)  
* **Windows 10 ISO:** [This is for your "client" computer (like a workstation). You can download this using the Windows 10 Media Creation Tool from Microsoft or find an evaluation ISO from the Microsoft Evaluation Center. Save this ISO file too.](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa3Q4UDJsWFJ5RG1WQWVyZ0lQREFYZDdxaTdfQXxBQ3Jtc0trOEg2TXNhWUFfS3R1VnpSUGlRTGRQVmJlcDdJMnN2UGNxZ3l6eHJ6Z2F5RnBia19EaHJpRmtfLVBxRmxVTXQxbUpfaDBzR0VURGgyX2E4eEs2b3g3bGl1aHdHZWFBeTlLZ2E4Rm52NFZldC15Zkh4RQ&q=https%3A%2F%2Fwww.microsoft.com%2Fen-us%2Fsoftware-download%2Fwindows10ISO&v=MHsI8hJmggI)

## Step 2: Create the Server (Domain Controller) VM

Now let's create the virtual machine (VM) that will act as our server, also called a Domain Controller (DC).

1.  Open VirtualBox and click "New".
2.  **Name:** Call it `DC`.
3.  **Type/Version:** Select `Microsoft Windows` and `Other Windows (64-bit)` (or `Windows 2019 (64-bit)` if available).
4.  **Memory (RAM):** Give it at least 2GB (2048 MB). More is better if your computer has plenty of RAM.
5.  **Hard Disk:** Choose "Create a virtual hard disk now", accept the defaults (VDI, Dynamically allocated), and give it a reasonable size (30-50GB is usually fine for a basic lab).
6.  **Settings:** Select the `DC` VM and click "Settings".
    * **Advanced:** Change "Shared Clipboard" and "Drag'n'Drop" to `Bidirectional`.
    * **System > Processor:** If you know your computer has multiple CPU cores, you can assign 2 or more here. If unsure, leave it at 1.
    * **Network:**
        * **Adapter 1:** Should be enabled and set to `NAT`. This connects your VM to the internet through your computer.
        * **Adapter 2:** Enable this adapter. Change "Attached to" to `Internal Network`. This creates a private network just for your VMs.
7.  Click "OK" to save the settings.

## Step 3: Install Windows Server 2019 on the DC

Time to install the server operating system!

1.  Select the `DC` VM and click "Start".
2.  When prompted for a start-up disk, click the folder icon, click "Add", and browse to find the **Windows Server 2019 ISO** file you downloaded. Select it and click "Choose", then "Start".
3.  Follow the Windows Setup prompts:
    * Choose your language, time, etc., and click "Next", then "Install Now".
    * Select **Windows Server 2019 Standard (Desktop Experience)** or **Datacenter (Desktop Experience)**. *Do not* choose a version without "Desktop Experience", or you'll only get a command line.

![Screenshot 2025-04-27 164523](https://github.com/user-attachments/assets/09f20c82-2876-4ada-b96f-31cfaf33e56d)


    * Accept the license terms.
    * Choose **Custom: Install Windows only (advanced)**.
    * Select the unallocated disk space and click "Next".
4.  Windows will now install. It will restart several times. **Important:** If you see a message like "Press any key to boot from CD or DVD...", **do not press anything!** Let it continue automatically.

![Screenshot 2025-04-27 164622](https://github.com/user-attachments/assets/fcbda72d-4b73-4ae9-be7a-67bc44c6de40)

6.  Once installed, it will ask you to set a password for the built-in Administrator account. Use `Password1` (with a capital P) you can use whatever password you want, but we'll be using this one for simplicity. **Never use such a simple password in a real environment!** 
7.  Log in: To send the `Ctrl+Alt+Delete` command to the VM, use the VirtualBox menu along the top bar: `Input` > `Keyboard` > `Insert Ctrl+Alt+Del`. Log in as Administrator with the password `Password1`.
8.  **Install Guest Additions:** This makes the VM run smoother and allows screen resizing.
    * In the VirtualBox window menu, go to `Devices` > `Insert Guest Additions CD image...`.
    * Open File Explorer inside the VM, go to "This PC", and double-click the "VirtualBox Guest Additions" CD drive.
    * Run the `VBoxWindowsAdditions-amd64.exe` file. Click "Next" through the installation, accepting defaults and installing any drivers if prompted.
    * When asked to reboot, choose "I want to manually reboot later" and then **shut down** the VM completely (Start Menu > Power > Shut Down). Then start it again, this helps everything work nicer after we finish installing.

## Step 4: Configure Server Network & Name

Let's set up the networking inside the server.

1.  Log back into the DC VM as Administrator.
2.  Open Network Settings: Right-click the network icon in the taskbar (bottom right) and choose "Open Network & Internet settings", then click "Change adapter options".
3.  Identify and Rename Adapters: You'll see two adapters.
    * Find the one getting an IP address automatically from your home network (likely starting with `192.168.x.x` or `10.x.x.x`). Right-click it, choose "Status" > "Details" to confirm. Rename this adapter to `Internet`.
    * The other adapter will likely have an address starting with `169.254.x.x`. This means it couldn't find a DHCP server on the internal network (which is expected). Rename this adapter to `Internal`.
4.  Configure Internal Adapter IP:
    * Right-click the `Internal` adapter, choose "Properties".
    * Select "Internet Protocol Version 4 (TCP/IPv4)" and click "Properties".
    * Choose "Use the following IP address":
        * IP address: `172.16.0.1` 
        * Subnet mask: `255.255.255.0` 
        * Default gateway: *(Leave Blank)* 
    * Choose "Use the following DNS server addresses":
        * Preferred DNS server: `127.0.0.1` (This tells the server to use itself for DNS once we install it).
    * Click "OK" on both windows.
5.  Rename the Server:
    * Right-click the Start button, choose "System".
    * Click "Rename this PC".
    * Enter the new name: `DC`.
    * Click "Next" and restart the computer when prompted.

## Step 5: Install Active Directory

Now we install the core service that makes this a Domain Controller.

1.  Log back into the DC as Administrator. Server Manager should open automatically. If not, find it in the Start Menu.
2.  In Server Manager, click "Add roles and features".
3.  Click "Next" several times until you get to the "Server Roles" screen.
4.  Check the box for **Active Directory Domain Services**. If prompted, click "Add Features".
5.  Click "Next" through the remaining screens and then click "Install". Wait for the installation to finish.
6.  **Promote to Domain Controller:** After installation, click the yellow warning flag icon near the top right of Server Manager, then click "Promote this server to a domain controller".
7.  In the wizard:
    * Select "Add a new forest".
  
      ![Screenshot 2025-04-27 213301](https://github.com/user-attachments/assets/1a80f27f-7dd4-472b-b180-b8ff8e408a5a)

    * Root domain name: `mydomain.com` (You can technically use others, but stick to this for the lab). Click "Next".
    * Enter a Directory Services Restore Mode (DSRM) password. Use `Password1` again for simplicity. Click "Next".
    * Click "Next" through the DNS Options, NetBIOS name, and Paths screens, accepting defaults.
    * Review the options and click "Next", then click "Install".
8.  The server will automatically restart once finished.

## Step 6: Create Your Admin Account

It's best practice not to use the default Administrator account all the time. Let's create your own admin account.

1.  Log in again. Notice the login screen now says `MYDOMAIN\Administrator`. Use the `Password1` password.
2.  Open Active Directory Users and Computers (ADUC): Go to Start Menu > Windows Administrative Tools > Active Directory Users and Computers.

![Screenshot 2025-04-27 213410](https://github.com/user-attachments/assets/2ffc526b-4a2b-4e7c-b998-506bec59bf68)
 
4.  Expand `mydomain.com` on the left.
5.  **Create an OU (Folder):**
    * Right-click `mydomain.com`, choose `New` > `Organizational Unit`.
  
    ![Screenshot 2025-04-27 213444](https://github.com/user-attachments/assets/b77b44a5-828d-4f27-bdbd-cbe55fb3403c)

    * Name it `_MyAdmins` (the underscore helps keep it at the top).
    * **Uncheck** "Protect container from accidental deletion". Click "OK" This way you can delete and recreate it easier later if you'd like.
6.  **Create Your User:**
    * Right-click the `_MyAdmins` OU you just created, choose `New` > `User`.
    * Enter your First name and Last name.
    * For User logon name, use the format `a-FirstInitialLastName` (e.g., `a-jsmith` for John Smith). The `a-` signifies it's an admin account. Click "Next".
    * Set the password to `Password1`.
    * **Uncheck** "User must change password at next logon".
    * **Check** "Password never expires" (Only for labs!). Click "Next", then "Finish".
7.  **Make it a Domain Admin:**
    * Find the user you just created in the `_MyAdmins` OU, right-click it, and choose "Properties".
    * Go to the "Member Of" tab and click "Add...".
    * Type `Domain Admins` and click "Check Names" (it should underline). Click "OK".
    * Click "Apply", then "OK".
8.  **Log off** the Administrator account (Start Menu > Click user icon > Sign out).
9.  Log back in using your **new admin account** (`mydomain\a-YourUserName`) and `Password1`. You might need to click "Other user".

## Step 7: Set Up Internet Access for Lab (NAT/Routing)

This lets your future client VM access the internet through the server.

1.  In Server Manager, go to "Add roles and features" again.
2.  Click "Next" until you get to "Server Roles".
3.  Select **Remote Access**. Click "Next".

   ![Screenshot 2025-04-27 213710](https://github.com/user-attachments/assets/14bef967-f446-4d73-946e-03ba27d3a52b)

5.  On the "Role Services" screen, check the box for **Routing**. Click "Add Features" if prompted.
6.  Click "Next" through the rest and click "Install". Wait for it to finish and close the wizard.
7.  Open Routing and Remote Access: Start Menu > Windows Administrative Tools > Routing and Remote Access.

   ![Screenshot 2025-04-27 213746](https://github.com/user-attachments/assets/09d961a3-57db-4540-85f5-e39860c60f04)

9.  Configure NAT:
    * Right-click your server name (`DC (local)`) on the left and choose "Configure and Enable Routing and Remote Access".
    * Click "Next" on the wizard welcome screen.
    * Select **Network address translation (NAT)**. Click "Next".
    * Select your **Internet** network adapter (the one connected to the outside world) from the list. Click "Next".
    * Click "Finish". Wait for the service to start (the icon should turn green).

## Step 8: Set Up DHCP Server

DHCP automatically gives IP addresses to computers joining the network. Let's set it up on our server.

1.  In Server Manager, "Add roles and features" one more time.
2.  Click "Next" to "Server Roles" and select **DHCP Server**. Click "Add Features" if prompted.
3.  Click "Next" through the rest and click "Install". Wait for it to finish.
4.  **Complete DHCP Configuration:** Click the yellow warning flag in Server Manager again and click "Complete DHCP configuration". Click "Next", ensure "Use the following user's credentials" is selected (should show your admin account), and click "Commit", then "Close".
5.  Open DHCP Manager: Start Menu > Windows Administrative Tools > DHCP.

   ![Screenshot 2025-04-27 214447](https://github.com/user-attachments/assets/59e15521-3107-4c0d-8246-df629d8686a9)

7.  **Authorize the Server:** Expand the server name on the left. Right-click the server name (`dc.mydomain.com`) and choose "Authorize". You might need to right-click and "Refresh" afterward. The IPv4 and IPv6 icons should get green arrows.
8.  **Create a Scope (IP Address Pool):**
    * Expand the server name, right-click "IPv4", and choose "New Scope...".

      ![Screenshot 2025-04-27 214531](https://github.com/user-attachments/assets/459e8028-b506-48ff-b830-b5011503397b)

    * Click "Next". Name the scope something descriptive, like `Lab Network Scope`. Click "Next".
    * Enter the IP address range clients can use:
        * Start IP address: `172.16.0.100` 
        * End IP address: `172.16.0.200` 
        * Subnet mask: `255.255.255.0`. Click "Next".
    * Exclusions: We don't need any. Click "Next".
    * Lease Duration: 8 days is fine for a lab. Click "Next".
    * Configure DHCP Options: Choose "Yes, I want to configure these options now". Click "Next".
    * **Router (Default Gateway):** Enter the IP address of your server's *internal* adapter: `172.16.0.1`. Click "Add", then "Next".
    * **Domain Name and DNS Servers:** It should automatically detect `mydomain.com` and the server's IP (`172.16.0.1`). If not, add `172.16.0.1`. Click "Next".
    * WINS Servers: We don't need this. Click "Next".
    * Activate Scope: Choose "Yes, I want to activate this scope now". Click "Next", then "Finish".
9.  Your DHCP server is ready to give out IP addresses to clients on the `Internal` network.

## Step 9: Add Lots of Users with PowerShell (Optional but Fun!)

Let's automatically create 1000 sample users in Active Directory using a script.

1.  **Disable IE Enhanced Security (for easier downloading):**
    * In Server Manager, click "Local Server" on the left.
    * Find "IE Enhanced Security Configuration" on the right and click the "On" link.

![Screenshot 2025-04-27 215517](https://github.com/user-attachments/assets/25b90cc2-b9ec-41ea-8725-2af94ea6f89d)

    * Turn it **Off** for Administrators. Click "OK".
1.  **Download the Script Files:**
    * [Go to this link to download the powershell script](https://github.com/joshmadakor1/AD_PS/archive/HEAD.zip)
    * Extract the zip file to a folder on the Desktop (e.g., named `ADScript`).
2.  **Edit the Names File:**
    * Open the `names.txt` file inside the `ADScript` folder with Notepad.
    * Add your **own name** (First Last) to the very first line of the file. Save and close the file.
3.  **Prepare PowerShell:**
    * Right-click the Start button, find "Windows PowerShell ISE", right-click it, choose "More" > "Run as administrator". Click "Yes".
    * In the blue command pane at the bottom, type the following command and press Enter to allow scripts to run (this is generally unsafe outside a lab!):
        ```powershell
        Set-ExecutionPolicy Unrestricted
        ```
    * Press `Y` and Enter to confirm.
4.  **Run the Script:**
    * In PowerShell ISE, go to `File` > `Open...` and navigate to the `ADScript` folder on your Desktop. Open the `.ps1` script file.
    * The script creates a new OU called `_Users` and loops through `names.txt` to create user accounts.
    * Click the green "Play" button (Run Script) in the toolbar. If prompted about running a script from the internet, choose "Run once" or similar.

      ![Screenshot 2025-04-27 215740](https://github.com/user-attachments/assets/d57abfe3-f474-40f3-a12c-6cb2e17a6735)

    * The script will start creating users, printing blue text for each one. This will take a while! You might see some red errors if there are duplicate names in the list – that's okay for this lab.
5.  **Verify:** Once done, go back to "Active Directory Users and Computers". Refresh (`F5`) and you should see a new OU called `_Users` filled with accounts. You can use the "Find" feature (right-click `mydomain.com` > `Find...`) to search for your own user account.

## Step 10: Create the Client VM

Time to create the computer that will connect to our domain.

1.  Go back to the main VirtualBox window. Click "New".
2.  **Name:** `Client1`.
3.  **Type/Version:** `Microsoft Windows` / `Windows 10 (64-bit)`. Click "Continue".
4.  **Memory (RAM):** Give it at least 2GB (2048 MB), maybe 4GB (4096 MB) if you have enough. Click "Continue".
5.  **Hard Disk:** Create a new virtual hard disk using the defaults (VDI, Dynamically allocated), 50GB is fine. Click "Create".
6.  **Settings:** Select `Client1` and click "Settings".
    * **Advanced:** Set "Shared Clipboard" and "Drag'n'Drop" to `Bidirectional`.
    * **System > Processor:** Give it 2 cores if you can.
    * **Network > Adapter 1:** Change "Attached to" to `Internal Network`. This connects it to our private lab network where the DC is.
7.  Click "OK".

## Step 11: Install Windows 10 on the Client

Let's install Windows 10 on our new client VM.

1.  Select `Client1` and click "Start".
2.  When prompted for a start-up disk, click the folder icon, "Add", and browse to select the **Windows 10 ISO** file you downloaded. Click "Choose", then "Start".
3.  Follow the Windows Setup prompts:
    * Choose language, etc., click "Next", then "Install Now".
    * Click "I don't have a product key".
    * Select **Windows 10 Pro**. *Do not* choose Home, as it cannot join a domain. Click "Next".
    * Accept license terms.
    * Choose **Custom: Install Windows only (advanced)**.
    * Select the unallocated disk and click "Next".
4.  Windows will install and restart. Again, **don't press any keys** if prompted to boot from CD/DVD.
5.  **Out-of-Box Experience (OOBE):**
    * Select your region (e.g., United States).
    * Select keyboard layout (US), Skip secondary layout.
    * **Crucially:** If asked "How would you like to set up?", choose "Set up for personal use" or if asked about network, click **"I don't have internet"** in the bottom left corner. Then click **"Continue with limited setup"**. This avoids being forced to use a Microsoft account.
    * Create a local user account. Name it `user` (or anything simple). Leave the password blank for simplicity in the lab. Click "Next".
    * Turn off all the privacy settings (location, diagnostics, tailored experiences, etc.). Click "Accept".
    * Maybe decline Cortana ("Not now").
6.  Wait for Windows to finish setting up your profile.
7.  Remember to install **VirtualBox Guest Additions** on this client VM too, just like you did on the server (Step 3, point 7), then restart the client VM.

## Step 12: Join the Client to the Domain

Let's connect our Client1 computer to the `mydomain.com` domain.

1.  Log in to Client1 using the local `user` account (no password).
2.  **Verify Network:**
    * Open Command Prompt (Search > `cmd` > Enter).
    * Type `ipconfig` and press Enter. You should see an IP address starting with `172.16.0.x` (e.g., `172.16.0.100`), the subnet mask `255.255.255.0`, and the Default Gateway `172.16.0.1`. If the gateway is missing, try `ipconfig /renew`.
    * Type `ping google.com` and press Enter. You should get replies, meaning internet access is working through the DC.
    * Type `ping mydomain.com` and press Enter. You should get replies from `172.16.0.1`, meaning it can see the domain controller.
3.  **Rename PC and Join Domain:**
    * Right-click the Start button, choose "System".
    * Scroll down and click "Rename this PC (advanced)". (Do *not* use the basic "Rename this PC" button).
    * In the System Properties window, click the "Change..." button.
    * Change the **Computer name** to `Client1`.
    * Under "Member of", select **Domain:** and type `mydomain.com`.
    * Click "OK".
    * A window will pop up asking for credentials. Enter the username and password of an account authorized to join computers to the domain. You can use the **non-admin user account** you added to `names.txt` (e.g., `jlagerstrom` if your name was JJ Lagerstrom) and the `Password1` password. (Alternatively, your `a-YourUserName` admin account would also work).
    * Click "OK". You should see a "Welcome to the mydomain.com domain" message. Click "OK".
    * Click "OK" to restart the computer. Click "Close", then "Restart Now".

## Step 13: Log In with a Domain Account

The final test! Log in to the client computer using an Active Directory account.

1.  After Client1 restarts, you'll be at the login screen. It might still show the local `user` account.
2.  Click "Other user" in the bottom left.
3.  Notice it now says "Sign in to: MYDOMAIN" underneath the password field.
4.  Enter the username of the account you created for yourself via the PowerShell script (e.g., `jlagerstrom`, *not* the `a-jlagerstrom` admin one this time).
5.  Enter the password: `Password1`.
6.  Press Enter. Windows will set up your profile for the first time on this machine.

## Additional Resources

* **Oracle VirtualBox:** [https://www.virtualbox.org/](https://www.virtualbox.org/) 
* **PowerShell Script:** 

# ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS   = "Password1"
$USER_FIRST_LAST_LIST = Get-Content .\names.txt
# ------------------------------------------------------ #

$password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force
New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false

foreach ($n in $USER_FIRST_LAST_LIST) {
    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()
    $username = "$($first.Substring(0,1))$($last)".ToLower()
    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_USERS,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
}

## You Did It!

Congratulations! We've successfully set up a basic Active Directory lab environment with a Domain Controller and a domain-joined client computer. Configured networking, DHCP, DNS, and even added users with PowerShell. This is a great foundation for learning more about Windows Server and networking. Keep learning! 
