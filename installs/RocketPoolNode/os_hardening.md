# OS Installation

## Assumptions

- There are A LOT of different practices for hardening an installation, the following is somewhat easy and fairly secure, of course there is more you can do, and there is less you can do. Hardening is a trade-off between convenience and security so choose your level of comfort.
- It's assumed you had ubuntu setup the basics of SSH and import some keys in the previous step. If you did not, there are a lot of good guides on setting up SSH and adding keys to it on the web, a simple google search should get you there and you can resume this after that.
- Open 2x SSH sessions into your machine. One of these sessions will be a backup in case you need to reset anything, the other will be where we actually make changes. Don't close the second session until you finish the OS hardening section.

## Secure SSH Access

- Make some changes to the `/etc/ssh/sshd_config` file:

  1. `Port 22` -> to a port you prefer
  2. Uncomment `#AuthorizedKeysFile` line
  3. Remove the `.ssh/authorized_keys2` option from the `AuthorizedKeysFile` directive
      1. If you're on Ubuntu 22.04, you really only need the `.ssh/authorized_keys` entry for this line, but be sure the file exists and has your public keys in it before removing the `.ssh/authorized_keys2` file from the list
  4. Change `#PasswordAuthentication yes` to `PasswordAuthentication no`
  5. Change `#PermitRootLogin prohibit-password` to `PermitRootLogin no`
  6. Save and exit the file

- Restart the SSH daemon with the following command:

     ```text
     sudo systemctl restart sshd
     ```

## Add MFA to your SSH logins

- Install the mfa module on your server with the following command:

  ```text
  sudo apt install libpam-google-authenticator
  ```

- Make some changes to the `/etc/pam.d/sshd` config file:

  1. Change `@include common-auth` to `#@include common-auth`
  2. Add the following to the top of the file:

      ```text
      # Enable Google Authenticator
      auth required pam_google_authenticator.so
      ```

  3. Save and exit the file

- Make some changes to the `/etc/ssh/sshd_config` file:

  1. Change `KbdInteractiveAuthentication no` to `KbdInteractiveAuthentication yes`
  2. Add the following to the last line of the file:

      ```text
      AuthenticationMethods publickey,keyboard-interactive:pam
      ```

  3. Save and exit the file

- Setup MFA by running the command `google-authenticator`

  1. Do you want authentication tokens to be time-based: Yes
  2. Scan the barcode in your MFA app of choice and verify it by entering a code into the command line.
  3. Backup your secret key and emergency scratch codes somewhere secure.
  4. Update the .google_authenticator file: Yes
  5. Disallow multiple uses of a token: Yes
  6. Permit longer time skew: No
  7. Enable rate-limiting: Yes

- Once again, restart the SSH daemon with the following command:

     ```text
     sudo systemctl restart sshd
     ```

## Enable Automatic Security Updates

- Run the following command to install the unattended upgrades functionality (this may already be installed)

  ```text
  sudo apt install unattended-upgrades update-notifier-common
  ```

- Add the following lines to the `/etc/apt/apt.conf.d/20auto-upgrades` file (you can alter these to suit your needs):

    ```text
    APT::Periodic::AutocleanInterval "7";
    Unattended-Upgrade::Remove-Unused-Dependencies "true";
    Unattended-Upgrade::Remove-New-Unused-Dependencies "true";
    
    # This is the most important choice: auto-reboot.
    # This should be fine since Rocketpool auto-starts on reboot.
    Unattended-Upgrade::Automatic-Reboot "true";
    Unattended-Upgrade::Automatic-Reboot-Time "02:00";
    ```

- Restart the unattended-upgrades service with the following command:

    ```text
    sudo systemctl restart unattended-upgrades
    ```

## Enable a Firewall

- Disable connections unless explicitly allowed:

    ```text
    sudo ufw default deny incoming comment 'Deny all incoming traffic'
    ```

- Allow ssh:

    ```text
    sudo ufw allow "< your ssh port # >/tcp" comment 'Allow SSH'
    ```

- Allow ethereum execution client traffic:

    ```text
    sudo ufw allow 30303/tcp comment 'Execution client port, standardized by Rocket Pool'
    sudo ufw allow 30303/udp comment 'Execution client port, standardized by Rocket Pool'
    ```

- Allow ethereum consensus client traffic:

    ```text
    sudo ufw allow 9001/tcp comment 'Consensus client port, standardized by Rocket Pool'
    sudo ufw allow 9001/udp comment 'Consensus client port, standardized by Rocket Pool'
    ```

- Enable the firewall and get the status of the firewall to check the rules you've implemented look correct

    ```text
    sudo ufw enable
    sudo ufw status
    ```

- Check you can create a new SSH session to the box using the new port, the MFA, and the firewall in place. If you can, then you can abandon your other older session at this point as your connectivity is working.

## Enable fail2ban

- Install fail2ban

    ```text
    sudo apt install fail2ban
    ```

- Create a file with the following lines at `/etc/fail2ban/jail.d/ssh.local`:

    ```text
    [sshd]
    enabled = true
    banaction = ufw
    port = 22
    filter = sshd
    logpath = %(sshd_log)s
    maxretry = 5
    ```

- Restart the fail2ban service with the command:

    ```text
    sudo systemctl restart fail2ban
    ```

- Strictly speaking you don't need to, but it can be helpful to reboot your system at this point and ensure you can still login correctly.
