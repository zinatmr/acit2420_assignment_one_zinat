# acit2420_assignment_one_zinat
Install and configure `doctl` in an existing droplet and use it to create a new droplet. The `doctl` command line tool is in the Arch package repositories, so you can install it with Pacman.

Your tutorial should include instructions on installing and configuring `doctl`.
---------------------------------------------------------------------
## Generate an SSH key pair on your local machine
This part describes how to create SSH keys to securely connect to the server. SSH keys is preferred over password authentication because it has enhanced security and cannot be bypassed by bruteforse attack; and it allows for passwordless entry once set up correctly.
### Step 1: Creating SSH key pair
1. Open **Powershell on Windows** and create a **.ssh** directory in the **home directory**. Type in the following command to go to your home directory.
```bash
cd ~
```
Here `~` refers to the home directory. Then create a **.ssh** directory by typing the following command in Powershell
```bash
mkdir .ssh
```

2. Type in the following command to cerate the SSH key:
```bash
ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "your email address"
```
In the above command -t stands for "type" which is the type of encryption used for the key, -f stands for "filaname" and specifies filename and location, -C stands for "comment" which attaches a comment to the key
The above command will generate two plain text in the .ssh directory: "do-key" which is your private key and "do-key.pub" which is your public key and need to copy to your DigitalOcean accout. this will be used to create **droplets** which can be securedly logged in from your windows powershell.


## Installing and Configuring doctl on Arch Linux
This part of the tutorial will demonstrate how to install and configure **doctl** on Arch Linux. Additionally, the steps to create a new droplet from an existing droplet using **doctl** will be illustrated here.

### Step 1: Install doctl on Arch Linux
1. Log into your Arch Linux server on Digital Ocean using **SSH**. Use **Powershell** on **Windows** and type in the following command:
```bash
ssh <Your_droplet_server_name>
```
2. Update your **package database**. Type in the following command. (Skip this step if you have already updated the package database):
```bash
sudo pacman -Syu
```
3. Install **doctl** using **pacman**. 
```bash
sudo pacman -S doctl
```
(The ``-S`` option tells **pacman** to synchronize the package database and install the specified package)
4. Verify the installation and check the version. Type in the following:
```bash
doctl version
```
The output should be as follows:
![[./assets/doctl_version.png]]

### Step 2: Create a DigitalOcean API Token
Before authenticating **doctl**, create an API token with both read and write access. Follow the following steps:
1. Log in to your DigitalOcean accout.
2. Go to the **API** section in the control panel. (On the left side of the page).
![[./assets/api.png]]
Then click on **Generate New Token**.
![[./assets/generate_token.png]]
3. Name the Token and select **Custom Scopes** marked by red square and select the all four options in the **Quick bulk** marked by blue square.
![[./assets/customing.png]]
Then click on **Generate Token**.
![[./assets/generate_token_two.png]]
4. Save the token in a secure place, as it will only be shown once.

### Step 3: Authenticate **doctl** with your DigitalOcean Account
After having the API token, you need to authenticate **doctl** to grant access to your DigitalOcean account.
1. Type in the following command and give a name:
```bash
doctl auth init --context <NAME>
```
replace name with a meaningful name.
It will ask for entering you **access token** that you have created before, enter the **Token** and you will be authenticated.
4. If you have multiple account, to list your authentication context, use:
```bash
doctl auth list
```
then to switch between accounts , use:
```bash
doctl auth switch --context <NAME>
```
### Step 4: Validate that doctl is working
Now to check that **doctl** is authorized to use your account, you need to try some test commands.
1. Type in the following command to **interact** with your DigitalOcean account :
```bash
doctl auth init
```
Paste your API token when asked.

2. Type in the following command to review your account details and confirm that you have **successfully authorized** **doctl**.
```bash
doctl account get
```
3. You should see the output looks something similiar to the following:
```bash
ID           Name            Public IPv4    Private IPv4    Public IPv6    Memory    VCPUs    Disk    Region    Image                       Status    Tags    Features    Volumes
187949338    droplet-name                                                  1024      1        25      sfo2      Ubuntu 18.04.3 (LTS) x64    new
```
This verifies that **doctl** is properly configured and authorized.





---------------------------------------------------------------------
 Level 3 (Max grade 100%) This one is significantly more difficult than 1 and 2:
    - Create SSH keys on your local machine
    - Create a Droplet running Arch Linux using the `doctl` command-line tool.
    - Use `doctl` and cloud-init for every stage of setting up an Arch Linux droplet

   

