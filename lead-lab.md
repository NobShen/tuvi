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
    
