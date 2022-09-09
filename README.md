# Neppixel's Linux Server Help
This is a mishmash of things to remember when creating linux servers, and just shit for me to remember in general reguarding linux.

```
npm init -y && npm i --save-dev node@16 && npm config set prefix=$(pwd)/node_modules/node && export PATH=$(pwd)/node_modules/node/bin:$PATH
```

# Table of contents
  - [General Ubuntu Server jazz](#general-ubuntu-server-jazz)
    - [Renaming hostname](#renaming-hostname)
    - [Coloring the console](#coloring-the-console)
    - [Add Aliases](#add-aliases)
    - [Run a startup script at boot (non root)](#run-a-startup-script-at-boot-non-root)
    - [Run a startup script at boot (ROOT)](#run-a-startup-script-at-boot-root)
    - [Auto mounting hard drive](#auto-mounting-hard-drive)
    - [NGINX](#nginx)
      - [Install NGINX](#install-nginx)
      - [Configuring NGINX](#configuring-nginx)
      - [Restart NGINX](#restart-nginx)
    - [Installing MYSQL](#installing-mysql)
    - [Create SAMBA share](#create-samba-share)
      - [Installation](#installation)
      - [Fixing user shit](#fixing-user-shit)
      - [Adding a share](#adding-a-share)
      - [Restarting samba](#restarting-samba)
    - [Screen Utilities](#screen-utilities)
      - [Starting up a screen with a name and have it run a program](#starting-up-a-screen-with-a-name-and-have-it-run-a-program)
      - [Renaming screen](#renaming-screen)
      - [Disconnect from screen](#disconnect-from-screen)
      - [Terminate Screen](#terminate-screen)
    - [Programs to remember](#programs-to-remember)
  - [Debian Specific Jazz](#debian-specific-jazz)
    - [Installation Steps](#installation-steps)
      - [Fast package](#fast-package)
      - [Web Server Option](#web-server-option)
      - [After successful login for the first time](#after-successful-login-for-the-first-time)
    - [Fix/Add sudo command](#fixadd-sudo-command)
    - [Remove apache2](#remove-apache2)
    - [Installed UFW](#installed-ufw)
    - [Enabling SSH](#enabling-ssh)

## General Ubuntu Server jazz
These are common things I do when I create a ubuntu server

### Renaming hostname
1) `hostnamectl set-hostname new-host-name`
2) `sudo nano /etc/hosts` 2nd line needs to be changed to `new-host-name`

### Coloring the console
1) `sudo nano ~/.bashrc`
2) Uncomment: `force_color_prompt=yes`
3) Restart your terminal

### Add Aliases
1) `sudo nano ~/.bashrc`
2) Scroll down to bottom
3) Add `alias cls='clear'` <-- This will make `cls` run the `clear` command 

### Run a startup script at boot (non root)
1) Navigate to home
2) Create startup script file: `nano startup-script.sh`
3) Make it executable: `//TODO`
4) Open Cron: `crontab -e`
5) Add `@reboot /home/user/startup-script.sh` to the end of the file
6) Exit & test!

### Run a startup script at boot (ROOT)
1) Navigate to home
2) Create startup script file: `nano startup-script-root.sh`
3) Make it executable: `//TODO`
4) Open Cron: `sudo crontab -e`
5) Add `@reboot /home/user/startup-script-root.sh` to the end of the file
6) Exit & test!

### Auto mounting hard drive
//TODO

### NGINX
//TODO

#### Install NGINX
//TODO

#### Configuring NGINX
//TODO

#### Restart NGINX
1) `sudo systemctl restart nginx`
2) `sudo systemctl status nginx`

### Installing MYSQL
//TODO

### Create SAMBA share
Instructions how to do everything with SAMBA and get it working with Windows.

#### Installation
1) `sudo apt-get purge samba samba-common`
2) `sudo rm -rf /etc/samba/ /etc/default/samba`
3) `sudo apt-get install samba`
   
#### Fixing user shit
Had some weird issues with users, so we create a share user, the share user will own all the files created on the samba share
1) `adduser --system shareuser`
2) `chown -R shareuser /path/to/share`

#### Adding a share
1) `sudo nano /etc/samba/smb.conf`
2) Template:
```toml
[myshare] # Share name that appears on other computers. No spaces
comment = My Amazing Share # A viewable comment describing what the share is
path = /path/to/share # Absolute path to file share
writeable = yes # We want to be able to write to the share
browseable = yes # We want to browse the file share
public = yes # Don't remember but this fixed weird shit
create mask = 0777 # Make every file executable read and writable. Probs not the best idea but like it works...
directory mask = 0777 # Make every folder executable read and writable. Probs not the best idea but like it works...
force user = shareuser # Force the user to be the share user we created above
```
3) Example:
```toml
[photos]
comment = Family photos
path = /home/eric/photos
writeable = yes
browseable = yes
public = yes
create mask = 0777
directory mask = 0777
force user = shareuser
```
#### Restarting samba
1) `sudo systemctl restart smbd`

### Screen Utilities
Useful screen commands

#### Starting up a screen with a name and have it run a program
`screen -S 'Screen-Name' -dm bash -c "cd /home/user/directory/; java -jar Program.jar"`

#### Renaming screen
1) `CTRL + A`
2) `:sessionname name`

#### Disconnect from screen
`CTRL + D`

#### Terminate Screen
`CTRL + C`

### Programs to remember
1) screen -> Run things in the background
2) htop -> Better TOP command, view programs, kill them. **Must run as SUDO to kill things**
3) lolcat -> Rainbowify text

## Debian Specific Jazz
Specific weird quirks that Ubuntu normally would do for us, but Debian doesn't

### Installation Steps

#### Fast package
When prompted to select a mirror, choose `mirrors.edge.kernel.org` because its fast.

#### Web Server Option
Needed to install networking. Installs apache2, we need to uninstall to afterard.

#### After successful login for the first time
1) `su -`
2) `apt-get update && apt-get upgrade -y`
3) After this you can use apt and not apt get

### Fix/Add sudo command
Sudo isn't installed by default.
1) `apt install sudo`
2) `nano /etc/sudoers`
3) `Replace %sudo with username`

### Remove apache2
1) `sudo apt remove apache2`
2) `sudo apt autoremove -y`

### Installed UFW
1) `sudo apt install ufw`
2) `sudo ufw allow ssh`
3) `sudo ufw allow http`
4) `sudo ufw allow https`
5) `sudo ufw allow ftp`

### Enabling SSH
1) `sudo systemctl enable ssh`
2) `sudo systemctl start ssh`
