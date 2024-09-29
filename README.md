# acit2420_assignment_one_zinat
Install and configure `doctl` in an existing droplet and use it to create a new droplet. The `doctl` command line tool is in the Arch package repositories, so you can install it with Pacman.

Your tutorial should include instructions on installing and configuring `doctl`.
---------------------------------------------------------------------
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
5. 




---------------------------------------------------------------------
 Level 3 (Max grade 100%) This one is significantly more difficult than 1 and 2:
    - Create SSH keys on your local machine
    - Create a Droplet running Arch Linux using the `doctl` command-line tool.
    - Use `doctl` and cloud-init for every stage of setting up an Arch Linux droplet

   

