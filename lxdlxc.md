Install lxd bridged network

    sudo apt install snapd
    snap install lxd
    more /etc/group | grep lxd
    sudo usermod -aG lxd $USER

Log out and log back in

    lxc remote list
    lxd init
      - LXD clustering - NO
      - Storage pool - YES
      - Name of storage pool - default
      - Name of storage backend - dir
      - Connect to MAAS server - NO
      - New local network bridge - YES
      - Name of new bridge - lxdbr0
      - IPv4 address - auto
      - IPv6 address - none
      - LXD server available over network - YES
      - Address to bind LXD - ALL
      - Port to bind LXD to - default=8443
      - Trust password - drupalpass
      - Cached images updated automatically - YES
      - Print YAML - NO

Now launch a new VM

    lxc launch ubuntu:22.04 ubuntuVM
    lxc exec ubuntuVM bash
    sudo apt install apache2
    exit
    sudo lxc config device add ubuntuVM myport80 proxy listen=tcp:0.0.0.0:80 connect=tcp:127.0.0.1:80
    lxc launch ubuntu:22.04 //launch a container - it's very light because it shares the same kernel
    lxc launch ubuntu:22.04 --vm //launch a vm - it's much bigger than container because it has its own kernel
    sudo nmcli conn up "Bridge connection 1"
    
Reboot system then
    
    lxc stop ubuntVM
    lxc config device add ubuntuVM eth0 nic nictype=bridged parent=bridge0 name=eth0
    

    
lxc remote list
lxd init
  - LXD clustering - NO
  - Storage pool - YES
  - Name of storage pool - default
  - Name of storage backend - dir
  - Connect to MAAS server - NO
  - New local network bridge - YES
  - Name of new bridge - lxdbr0
  - IPv4 address - auto
  - IPv6 address - none
  - LXD server available over network - YES
  - Address to bind LXD - ALL
  - Port to bind LXD to - default=8443
  - Trust password - drupalpass
  - Cached images updated automatically - YES
  - Print YAML - NO
