# acit2420_assignment_one_zinat

## Installing and Configuring doctl on Arch Linux
This part of the tutorial will demonstrate how to install and configure **doctl** on Arch Linux. Additionally, the steps to create a new droplet from an existing droplet using **doctl** will be illustrated later.

### Step 1: Install doctl on Arch Linux

It is assumed that you have a existing droplet based on Arch made from custom image. Please follow the steps below to install **doctl** on the droplet.

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
- The ``-S`` option tells **pacman** to synchronize the package database and install the specified package
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

## Generate an SSH key pair on your local machine
This part describes how to create SSH keys to securely connect to the server. SSH keys is preferred over password authentication because it has enhanced security and cannot be bypassed by bruteforse attack; and it allows for passwordless entry once set up correctly.
### Step 1: Creating SSH key pair
1. Log into your existing droplet from your local machine. And create a **.ssh** directory in the **home directory**. Type in the following command to go to your home directory.
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
Follow the  steps that are prompted in the terminal, you can choose to have paraphrase or you can ignore.
In the above command -t stands for "type" which is the type of encryption used for the key, -f stands for "filaname" and specifies filename and location, -C stands for "comment" which attaches a comment to the key
The above command will generate two plain text in the .ssh directory: "do-key" which is your private key and "do-key.pub" which is your public key and need to copy to your DigitalOcean accout. this will be used to create **droplets** which can be securedly logged in.

## Uploading SSH key pair to DigitalOcean using doctl
### Step 1: Uploading the SSH key
1. Type in the following command to upload the ssh key to your DigitalOcean account.
```bash
doctl compute ssh-key import "your-key-name" --public-key-file ~/.ssh/do-key.pub
```
Replace "your-key-name" with a meaningfull name for the SSH key(e.g., "my-droplet-key")
The --public-key-file option is used to specify the path to your public key.
### Step 2: Verify the SSH key was uploaded
1. Type in the following command to verify the ssh key was uploaded successfully.
```bash
doctl compute ssh-key list
```
You should see something like below:
```bash
   ID          Name            FingerPrint
59949345    your-key-name    xx:24:70:yy:67:30:70:7e:ea:3b:a9:60:1f:fc:95:4a
```
### Step 3: Creating a cloud-init config
On your terminal create a cloud-init.yml file that contains the following:
```bash
   #cloud-config
users:
  - name: user-name #change me
    primary_group: group-name #change me
    groups: wheel
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-ed25519 ...

packages:
  - ripgrep
  - rsync
  - neovim
  - fd
  - less
  - man-db
  - bash-completion
  - tmux

disable_root: true

```
You should give your own user-name that creates a user account in the server. Keep the primary group name same as user name. Copy your ssh key from `/.ssh/do-key.pub` and paste it under `ssh-authorized-keys:` after the `-`. This public ssh key will be added to the authorized_keys file in your new user home directory. You can select predetermined set of packages to be installed in your new droplets by typing their name under the packages after the `-`.
The `disable_root: true` is used to disable direct login as the root user on the droplet.

## Creating a new droplet running Arch Linux:
## step 1: Check your available custom image IDs in DigitalOcean
```bash
doctl compute image list-user
```
## step 2: Create the droplet with the custom image and cloud-init
```bash
doctl compute droplet create <droplet-name> \
--size s-1vcpu-1gb --image <arch-linux-id> --region sfo3 \
--ssh-keys <your-ssh-key-id> --user-data-file cloud-init.yml --wait

```
If the above is successfull, you should see something like below:
```bash
   ID           Name          Public IPv4      Private IPv4    Public IPv6    Memory    VCPUs    Disk    Region Image                                                          VPC UUID                                Status    Tags    Features              Volumes
238999715      droplet-name  42.23.244.179    78.201.7.1                     1024      1        25      sfo3      Arch Linux Arch-Linux-x86_64-cloudimg-20240901.259602.qcow2    9153234f-3d72-3333-8260-6aa489466569    active            private_networking
```










   

