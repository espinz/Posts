---
layout: post
title:  Supermicro configuration
date: 2019-06-26 02:08:50
categories: [os, linux]
tags: [supermicro, server]
toc: true
published: true
---

## Create a Bootable USB

> From a working Linux distribution

Download an ISO image from [Debian](https://www.debian.org/)
debian-9.7.0-amd64-netinst.iso was used for this project

In terminal type:

    cd ~/Downloads # Or wherever you downloaded the iso image.
    dmesg | grep USB # Take note of the device path: /dev/sgb
    # Yours might be different.
    dd if=debian-9.7.0-amd64-netinst.iso of=/dev/sgb; sync
    sync # Once last time.

<!-- more -->
## Server Hardware

> Specifications

*   Motherboard - X7DBR
*   Processor - Xeon L5420 @ 2.50GHz; Socket LGA771
*   Memory - 32GB
*   Hard Disk - 4 x 1 TB
*   RAID Controller - 3ware Inc 9550SX SATA-II RAID PCI-X

## Setting up Server BIOS

> **BIOS Tip:** Press x to activate and deactivate; BIOS only allows up to 8 devices. deactivate one in order to activate another one. press x to delete entries and add boot usb boot from usb

Enter BIOS and enable USB booting. Use the BIOS tips above to complete this step.

## Setting up Server RAID

> Used a mirror raid setup

*   2TB mirror on /dev/sda
*   2TB mirror on /dev/sdb

## Installation

> Plug in USB; Power on; Follow installation prompts.

*   Debian GNU/Linux installer boot menu
*   Select Install
*   Select a language
*   Select your location
*   Configure the keyboard
*   Configure the network
    *   Hostname
    *   Domain name
*   Set up users and passwords
    *   Root password
    *   reenter password
    *   non-administrative account
        *   Full name
        *   Username
            *   password
            *   reenter password
    *   Configure the clock
    *   Partition disks
        *   Partitioning method
            *   Select: `Guided - use entire disk`
        *   Select disk to partition
            *   Select: `sda`
        *   Partitioning scheme
            *   Select: `All files in one partition (recommended for new users)`
        *   Finish partitioning and write changes to disk
        *   Write the changes to disks?
            *   Select: `Yes`
    *   Configure the package manager
    *   Scan another CD or DVD
        *   Select: `No`
    *   Debian archive mirror country
        *   Select: `your country`
    *   Debian archive mirror
        *   Select: `your choice: Choose one in your country`
    *   HTTP proxy information (blank for none)
    *   Configuring popularity-contest
        *   Participate in the package usage survey?
        *   Select: `your choice: Yes or No`
    *   Software selection
        *   Deselect: `Debian desktop environment`
        *   Dselect: `print server`
        *   Select: `... LXDE`
        *   Select: `SSH server`
        *   **Leave standard system utilities selected**
    *   Grub
        *   Select: /dev/sda
    *   Reboot

## Create User Accounts

> Log-in as root

**In console type:**

    adduser tux

Enter new Unix password
User information: Full name, Work number etc.

## System Packages

> Install vim backspace support, sudo, autossh, vnc, tmux, virt-manager, UFW

**In console type:**

    sudo apt install \
    vim-nox sudo autossh vnc4server \
    xvnc4viewer ssvnc tmux virt-manager ufw

## Edit Configuration Files

**In console type:**

    vi /etc/default/grub

**In vi type:**

    # this disables the graphical login - use startx to start xwindows
    # vi /etc/default/grub change to
    GRUB_CMDLINE_LINUX_DEFAULT="text"
    update-grub
    systemctl set-default multi-user.target

save and exit with :x

**In console type:**

    vi /etc/bin/upit

**In vi type:**

    #!/bin/bash
    # upit - here is the update script - also for future reference
    # to fix the repos
    #dpkg --configure -a
    #apt-get -f install
    # run the update upgrade
    apt-get check
    apt-get clean
    apt-get update
    apt-get -y upgrade
    # warning - will update kernels ...
    apt-get -y dist-upgrade
    apt-get -y autoclean
    apt-get -y autoremove
    lsb_release -a
    cat /proc/version

save and exit with :x

**In console type:**

    vi /etc/default/grub

**In vi edit:**

    GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200n8"

save and exit with :x

## System Configuration Commands

> Enable the getty for COM1

**In console type:**

    usermod -aG sudo,libvirt,netdev tux
    update-grub;
    systemctl enable serial-getty@ttyS0.service;
    systemctl start serial-getty@ttyS0.service;
    systemctl deamon-reload;
    systemctl enable serial-getty@ttyS0.service
    systemctl start serial-getty@ttyS0.service
    systemctl daemon-reload

## USB Serial on Windows

> On windows 10 used putty; already installed.

*   Putty on Windows 10
*   Open windows Device Manager
    *   Under ports; look for serial to USB
    *   **What is your COMM PORT?** Mine reads COM6.
*   Open Putty
    *   Under Connection Type; Click radio button Serial
    *   Set Baud speed to 115200; Click Open

**If everything is correct it will show the linux login prompt on the client.**

## USB Serial on Linux

*   Linux Notes for Serial

*   USB Serial F/F NM Serial USB

*   Connect USB to USB

    sudo chmod 777 /dev/ttyUSB0
    sudo screen /dev/ttyUSB0 115200
    dmesg | grep ttyUSB
    sudo chmod 777 /dev/ttyUSB0
    sudo chmod 777 /dev/ttyUSB1
    sudo screen /dev/ttyUSB0 115200

*   COM A
*   115200
*   RTS/CTS

*   On The Client Machine

*   **On the console type:**

    dmesg # to find usb ttyUSBx
    chmod 777 /dev/ttyUSBx
    screen /dev/ttyUSBx 115200

## Setting Up The Reverse Tunnel

**In console type:**

    vi /etc/systemd/system/rc-local.service

**In vi type:**

    [Unit]
    Description=/etc/rc.local
    ConditionPathExists=/etc/rc.local

    [Service]
    Type=forking
    ExecStart=/etc/rc.local start
    TimeoutSec=0
    StandardOutput=tty
    RemainAfterExit=yes
    SysVStartPriority=99

    [Install]
    WantedBy=multi-user.target

save and exit with :x

**In console type:**

    vi /etc/rc.local

**In vi type:**

    #!/bin/sh -e
    #sleep 30
    #tmux new-session -d -s "tunnel" "bash /home/tux/tunnel.sh"
    su - tux -c "autossh -N -f -R 2222:127.0.0.1:22 username@hostname"
    exit 0

save and exit with :x

**In console type:**

    sudo chmod +x /etc/rc.local

**In console type:**

    sudo systemctl enable rc-local.service

## Setting up VNC Server

> On server

**In console type:**

    vi ~/.vnc/startvnc

**In vi type:**

    #!/bin/bash
    xrdb $HOME/.Xresources
    xsetroot -solid grey
    ## Fix to make GNOME work
    export XKL_XMODMAP_DISABLE=1
    /etc/X11/Xsession
    lxterminal &
    /usr/bin/lxsession -s LXDE &

save and exit with :x

**In console type:**

    vi ~/.vnc/stopvnc

**In vi type:**

    vncserver -kill :1

save and exit with :x

**In console type:**

    chmod +x ~/.vnc/startvnc

**In console type:**

    xvncserver

Type in a password
Would you like to enter a view-only password (y/n)? `n`

## VNC as a Systemd Service

> [Thanks to spinxz](https://gist.github.com/spinxz/1692ff042a7cfd17583b)

**In console type:**

    sudo vi /etc/systemd/system/vncserver@.service

**In vi type:**

    # Vncserver service file for Debian or Ubuntu with systemd
    #
    #  Install vncserver and tools
    #  e.g. apt-get install tightvncserver autocutsel gksu
    #
    # 1\. Copy this file to /etc/systemd/system/vncserver@1.service
    # 2\. Edit User=
    #    e.g "User=paul"
    # 3\. Edit the vncserver parameters appropriately in the ExecStart= line!
    #    e.g. the -localhost option only allows connections from localhost (or via ssh tunnels)
    # 4\. Run `systemctl daemon-reload`
    # 5\. Run `systemctl enable vncserver@:<display>.service`
    #

    [Unit]
    Description=Remote desktop service (VNC)
    After=syslog.target network.target

    [Service]
    Type=forking
    User=<username>

    # Clean any existing files in /tmp/.X11-unix environment
    ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
    #ExecStart=/usr/bin/vncserver -geometry 1800x1000 -depth 16 -dpi 120 -alwaysshared -localhost %i
    ExecStart=/usr/bin/vncserver -xstartup /home/<username>/.vnc/startvnc
    ExecStop=/usr/bin/vncserver -kill %i

    [Install]
    WantedBy=multi-user.target

**In console type:**

    sudo systemctl daemon-reload
    sudo systemctl enable vncserver@1.service
    sudo systemctl start vncserver@1.service

## UFW

**In console type:**

    sudo ufw allow ssh
    sudo ufw allow www
    sudo ufw allow 443/tcp
    sudo ufw allow 5901/tcp
    sudo ufw allow 2222/tcp
    sudo ufw status
    sudo ufw enable

## Setting up SSH Jumps

> From a working Linux distribution

**In console type:**

    vi ~/.ssh/config

**In vi type:**

    Host firstHop
        Hostname x.x.x.x
        User username
        IdentityFile ~/.ssh/firstHop-id_rsa
    Host secondHop
        Hostname x.x.x.x
        ForwardX11 yes
        User username
        IdentityFile ~/.ssh/secondHop-id_rsa
        ProxyCommand ssh firstHop -W %h:%p
    Host thirdHop
        Hostname 127.0.0.1
        User username
        IdentityFile ~/.ssh/thirdHop-id_rsa
        ProxyCommand ssh secondHop -W %h:%p
        Port 2222
        LocalForward 5991 127.0.0.1:5901

save and exit with :x

    # make three keys firstHop, secondHop and thirdHop
    ssh-keygen -t rsa #firstHop
    cat firstHop-id_rsa.pub | ssh firstHop 'cat >> .ssh/authorized_keys'
    ssh firstHop; ssh-keygen -t rsa # secondHop
    cat secondHop-id_rsa.pub | ssh secondHop 'cat >> .ssh/authorized_keys'
    ssh secondHop; ssh-keygen -t rsa #thirdHop
    cat thirdHop-id_rsa.pub | ssh thirdHop 'cat >> .ssh/authorized_keys'

    Copy all private keys to your working Linux distro
    ssh-add ~/.ssh/{first,second,third}Hop{1,2,3}

## VNC

> From a working Linux distribution

**In console type:**

    ssh thirdHop

Enter secondHop password
Enter thirdHop password
Once connected
Open your VNC client
Type 127.0.0.1:5991

## KVM/LXC/LXD/QEMU

> Creating VM's - make sure BIOS is enabled for virtual machine

With LXDE available via VNC, go to the main menu.

Click `System Tools`
Click `Virtual Machine Manager`
Click `Create a new virtual machine`
Choose `Local install media (ISO image or CDROM)`
Click `Forward`
Click `Browse and Choose your ISO image`
Click `Forward`
Choose `CPU and Memory Allocation`
Click `Forward`
Choose `Storage Amount`
Click `Forward`
Click `Finish`

> Photo by Stef Westheim
