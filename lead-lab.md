Edit lxd init YAML file:

    Would you like to use LXD clustering? (yes/no) [default=no]: 
    Do you want to configure a new storage pool? (yes/no) [default=yes]: 
    Name of the new storage pool [default=default]: 
    Name of the storage backend to use (btrfs, dir, lvm, zfs, ceph) [default=zfs]: dir
    Would you like to connect to a MAAS server? (yes/no) [default=no]: no
    Would you like to create a new local network bridge? (yes/no) [default=yes]: 
    What should the new bridge be called? [default=lxdbr0]: 
    What IPv4 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]: 
    What IPv6 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]: none
    Would you like the LXD server to be available over the network? (yes/no) [default=no]: yes
    Address to bind LXD to (not including port) [default=all]: 
    Port to bind LXD to [default=8443]: 
    Trust password for new clients: 
    Again: 
    Would you like stale cached images to be updated automatically? (yes/no) [default=yes]: 
    Would you like a YAML "lxd init" preseed to be printed? (yes/no) [default=no]: yes
    config:
      core.https_address: '[::]:8443'
      core.trust_password: drupalpass
    networks:
    - config:
        ipv4.address: auto
        ipv6.address: none
      description: ""
      name: lxdbr0
      type: ""
      project: default
    storage_pools:
    - config: {}
      description: ""
      name: default
      driver: dir
    profiles:
    - config: {}
      description: ""
      devices:
        eth0:
          name: eth0
          network: lxdbr0
          type: nic
        root:
          path: /
          pool: default
          type: disk
      name: default
    projects: []
    cluster: null
    
After lxd init, this shows up in ip addr:

    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host 
           valid_lft forever preferred_lft forever
    2: enp0s25: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether e8:9d:87:6f:c8:30 brd ff:ff:ff:ff:ff:ff
        inet 192.168.254.135/24 metric 100 brd 192.168.254.255 scope global dynamic enp0s25
           valid_lft 9199sec preferred_lft 9199sec
        inet6 fe80::ea9d:87ff:fe6f:c830/64 scope link 
           valid_lft forever preferred_lft forever
    3: wlp5s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
        link/ether 88:53:2e:57:3b:63 brd ff:ff:ff:ff:ff:ff
        inet 192.168.254.157/24 metric 600 brd 192.168.254.255 scope global dynamic wlp5s0
           valid_lft 2057sec preferred_lft 2057sec
    4: lxdbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default qlen 1000
        link/ether 00:16:3e:80:21:13 brd ff:ff:ff:ff:ff:ff
        inet 10.220.236.1/24 scope global lxdbr0
           valid_lft forever preferred_lft forever
           
 You can see lxdbr0 created but it has no physical connection.  It has an internal IP addr of 10.220.236.1.
 
    
