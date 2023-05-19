# OS Installation

## Assumptions

- You have an ubuntu 22.04.1 LTS image on a USB drive for installation
- You have booted the system into the ubuntu installer

## 1. Install Ubuntu

You may have some regional variations to the following. These settings are what was used for a US English install with the system located on the west coast of the United States.

- **Select Language**: English

- **Installer Update**: Update to the new installer

  - Only applicable if you're online

- **Keyboard Configuration**: English (US)

  - Same selection for both layout and variant

- **Type of Install**: Ubuntu Server

  - You may want to add third-party drivers depending on your specific hardware selection.

- **Network Connections**: Ensure you have an IP address

  - If you have DHCP on your network one will be assigned automatically at which point you should probably put a reservation in on this address to make it static
  - If you don't have DHCP on your network, you'll want to manually assign the network info to the correct adapter here before proceeding.
  - Don't enable/assign any more network interfaces than you need absolutely need to, minimize the footprint of your device as much as possible.

- **Configure Proxy**: < blank >

  - Most connections do not require a proxy so this is left blank.

- **Configure Ubuntu archive mirror**: < No Change >

  - Most systems will connect to the public ubuntu mirrors, so no change is required. If you have a special mirror, change it here.

- **Storage Configuration 1/2**: Use an entire disk & set up this disk as an LVM group

  - Storage configurations are as infinite as outer space. For simplicity it can be easiest to use the entire disk and then also enable LVM for future growth potential. Change this as you need to.

- **Storage Configuration 2/2**: Set the max size for ubuntu-lv

  - This page will look different depending on the selections made in part 1 of 2. If you choose similar to above in this guide, you will probably want to edit the "ubuntu-lv" to match the size of "ubuntu-vg" so that you are allocating all your storage as available and don't need to extend the volume after the installation completes.

- **Profile Setup**:

  - Enter your values, be sure to choose a highly secure password. We'll enable other forms of authentication beyond just a password shortly after install, but you will still need this password for sudo usage.
    - Your name:
    - Your server's name:
    - Pick a username:
    - Choose a password:
    - Confirm your password:

- **Ubuntu Pro**: Enable it if you have/want it

  - Ubuntu pro is certainly not a requirement, but if you want to use it for something specific go for it.

- **SSH Setup**: Install OpenSSH Server & Import SSH identity from GitHub & GitHub username

  - You can choose to import your keys from GitHub if you have them there - remember this is only your public key that is saved to GitHub so it is still secure, you'll need your private key to login to the server with. Of course you can opt to login via password if you choose. For this guide we will pull keys from GH, if you don't do this, you'll have to disable password based SSH logins later on, this guide won't fully cover that, but will call out when you should perform this change.

- **Featured Server Snaps**: Do not select anything here

  - Everything you need will be installed later, if you select things from here you're likely to create conflicts later on.

- **Reboot the system**