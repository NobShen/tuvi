to access GUI of a VM
    
    lxc console ubuntuVM --type vga
    systemctl enable NetworkManager
    cd /etc/sysconfig/network-scripts
    nmcli device wifi list
    nmcli device wifi connect "$SSID" password "$PASSWORD"
    nmcli --ask device wifi connect "Frontier6944"
    nmcli -p -f general,wifi-properties device show wlan0
    nmcli general permissions
    nmcli general logging
    nmcli g log level DEBUG domains CORE,ETHER,IP
    nmcli g log level INFO domains DEFAULT
    
    $ nmcli con add type bond ifname mybond0 mode active-backup
    $ nmcli con add type ethernet ifname eth1 master mybond0
    $ nmcli con add type ethernet ifname eth2 master mybond0
    
    $ nmcli con add type bridge con-name TowerBridge ifname TowerBridge
    $ nmcli con add type ethernet con-name br-slave-1 ifname ens3 master TowerBridge
    $ nmcli con add type ethernet con-name br-slave-2 ifname ens4 master TowerBridge
    $ nmcli con modify TowerBridge bridge.stp no
    
    $ nmcli con add con-name my-con-em1 ifname em1 type ethernet ip4 192.168.100.100/24 gw4 192.168.100.1 ip4 1.2.3.4 ip6 abbe::cafe
    $ nmcli con mod my-con-em1 ipv4.dns "8.8.8.8 8.8.4.4"
    $ nmcli con mod my-con-em1 +ipv4.dns 1.2.3.4
    $ nmcli con mod my-con-em1 ipv6.dns "2001:4860:4860::8888 2001:4860:4860::8844"
    $ nmcli -p con show my-con-em1

The first command adds an Ethernet connection profile named my-con-em1 that is bound to interface name em1. The profile is configured with static IP addresses. Three addresses are added, two IPv4 addresses and one IPv6. The first IP 192.168.100.100 has a prefix of 24 (netmask equivalent of 255.255.255.0). Gateway entry will become the default route if this profile is activated on em1 interface (and there is no connection with higher priority). The next two addresses do not specify a prefix, so a default prefix will be used, i.e. 32 for IPv4 and 128 for IPv6. The second, third and fourth commands modify DNS parameters of the new connection profile. The last con show command displays the profile so that all parameters can be reviewed.

https://developers.redhat.com/sites/default/files/blog/2018/10/bridge.png![image](https://user-images.githubusercontent.com/66322601/172082042-f138e97b-d069-4bbb-85ab-9dd8d92c0d36.png)

