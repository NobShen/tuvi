How to Create a Bridged Network for LXD Containers

Create a Netplan Bridge
The first thing you must do is create a bridge using Netplan. This is done on the host computer. Netplan uses configuration files found in /etc/netplan. We’re going to edit the existing file and add the bridge to that. Before you touch that file, you’ll want to back it up.

Find out the name of the Netplan file with the command:

ls /etc/netplan/

You’ll see something like 00-installer-config.yaml listed. Back that file up with the command:

      sudo cp /etc/netplan/00-installer-config.yaml /etc/netplan/00-installer-config.yaml

Now that you have your Netplan file backed up, let’s open it for editing with the command:

      sudo nano /etc/netplan/00-installer-config.yaml

That file will look something like:


