A bridge is nothing but a device which joins two local networks into one network. It works at the data link layer, i.e., layer 2 of the OSI model. Network bridge often used with virtualization and other software. Disabling NetworkManager for a simple bridge especially on Linux Laptop/desktop doesn’t make any sense. The nmcli tool can create Persistent bridge configuration without editing any files. 

This page shows how to create a bridge interface using the Network Manager command line tool called nmcli.

First, a bridge can't be created over wi-fi per the IEEE 802.11 standard.  See below:

    Note that a bridge cannot be established over Wi-Fi networks operating in Ad-Hoc or Infrastructure modes. 
    This is due to the IEEE 802.11 standard that specifies the use of 3-address frames in Wi-Fi for the efficient use of airtime.

Get info about the current connection:
    
    nmcli con show
Add a new bridge:
    
    nmcli con add type bridge ifname br0
Create a slave interface:
    
    nmcli con add type bridge-slave ifname eno1 master br0
Turn on br0:
    
    nmcli con up br0
    //to verify:
    nmcli con show
    mcli connection show --active

    sudo nmcli con add ifname br0 type bridge con-name br0
    sudo nmcli con add type bridge-slave ifname eno1 master br0
    nmcli connection show
****
