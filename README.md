# VM clustering using lxd
- Install Ubuntu server 22.04 & ignore lid behavior:  sudo nano /etc/systemd/logind.conf && systemctl restart systemd-logind.service
- Use http://192.168.254.254/cgi-bin/home.ha to find out the ip address of the server

- LXD method:
        
  LXD already preinstalled on Ubuntu 22.04 server - check by: 
  
        multipass2@multipass2:~$ sudo lxd
        ERROR  [2022-06-07T03:18:58Z] Failed to start the daemon                    err="LXD is already running"
        Error: LXD is already running
   
        multipass2@multipass2:~$ ip a
        1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
            link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
            inet 127.0.0.1/8 scope host lo
               valid_lft forever preferred_lft forever
            inet6 ::1/128 scope host 
               valid_lft forever preferred_lft forever
        2: enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
            link/ether 5c:f9:dd:4d:1c:26 brd ff:ff:ff:ff:ff:ff
            inet 192.168.254.144/24 metric 100 brd 192.168.254.255 scope global dynamic enp3s0
               valid_lft 14132sec preferred_lft 14132sec
            inet6 fe80::5ef9:ddff:fe4d:1c26/64 scope link 
               valid_lft forever preferred_lft forever
        3: wlp2s0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
            link/ether 84:a6:c8:13:32:60 brd ff:ff:ff:ff:ff:ff
        
  You'll not be able to create a VM until lxd init
  
        multipass2@multipass2:~$ lxc launch ubuntu:20.04 --network=enp3s0
        Creating the instance
        Error: Failed instance creation: Failed creating instance record: Failed initialising instance: Invalid devices: Failed detecting root disk device: No root device could be found
        multipass2@multipass2:~$ 

  So lxd init
        
        multipass2@multipass2:~$ sudo lxd init
        [sudo] password for multipass2: 
        Would you like to use LXD clustering? (yes/no) [default=no]: 
        Do you want to configure a new storage pool? (yes/no) [default=yes]: 
        Name of the new storage pool [default=default]: 
        Name of the storage backend to use (btrfs, dir, lvm, zfs, ceph) [default=zfs]: dir
        Would you like to connect to a MAAS server? (yes/no) [default=no]: 
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

  Check ip addr will show lxdbr0 created:
        
        multipass2@multipass2:~$ ip addr
        1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
            link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
            inet 127.0.0.1/8 scope host lo
               valid_lft forever preferred_lft forever
            inet6 ::1/128 scope host 
               valid_lft forever preferred_lft forever
        2: enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
            link/ether 5c:f9:dd:4d:1c:26 brd ff:ff:ff:ff:ff:ff
            inet 192.168.254.144/24 metric 100 brd 192.168.254.255 scope global dynamic enp3s0
               valid_lft 13286sec preferred_lft 13286sec
            inet6 fe80::5ef9:ddff:fe4d:1c26/64 scope link 
               valid_lft forever preferred_lft forever
        3: wlp2s0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
            link/ether 84:a6:c8:13:32:60 brd ff:ff:ff:ff:ff:ff
        4: lxdbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default qlen 1000
            link/ether 00:16:3e:57:73:3f brd ff:ff:ff:ff:ff:ff
            inet 10.31.131.1/24 scope global lxdbr0
               valid_lft forever preferred_lft forever

  After lxd init, you can lxc launch:
        
        multipass2@multipass2:~$ lxc launch ubuntu:20.04 --network=enp3s0
        Creating the instance
        Instance name is: elegant-drake           
        Starting elegant-drake

  Check that the VM is on your network:
  
        multipass2@multipass2:~$ lxc list
        +---------------+---------+------------------------+------+-----------+-----------+
        |     NAME      |  STATE  |          IPV4          | IPV6 |   TYPE    | SNAPSHOTS |
        +---------------+---------+------------------------+------+-----------+-----------+
        | elegant-drake | RUNNING | 192.168.254.176 (eth0) |      | CONTAINER | 0         |
        +---------------+---------+------------------------+------+-----------+-----------+


        No need to install NetworkManager (only needed to use the nmcli commands)
        Install network-manager to use nmtui
        do we need to run lxd init?
        lxc launch ubuntu:20.04 --network=enp0s25
  
  In case you just want a private VM then launch without --network=enp0s25
  
        multipass2@multipass2:~$ lxc launch ubuntu:20.04
        Creating the instance
        Instance name is: expert-perch            
        Starting expert-perch
        multipass2@multipass2:~$ lxc list
        +---------------+---------+------------------------+------+-----------+-----------+
        |     NAME      |  STATE  |          IPV4          | IPV6 |   TYPE    | SNAPSHOTS |
        +---------------+---------+------------------------+------+-----------+-----------+
        | elegant-drake | RUNNING | 192.168.254.176 (eth0) |      | CONTAINER | 0         |
        +---------------+---------+------------------------+------+-----------+-----------+
        | expert-perch  | RUNNING | 10.31.131.170 (eth0)   |      | CONTAINER | 0         |
        +---------------+---------+------------------------+------+-----------+-----------+

  THIS IS ALL YOU NEED TO CREATE VM's WITH PHYSICAL IP ADDR
  
  THE REST OF THIS PAGE IS FOR REFERENCE ONLY - DON'T USE IT

- Install multipass:  sudo snap install multipass && sudo snap info multipass
-   To remove:  sudo snap remove multipass

- lxd is pre-installed with Ubuntu 22.04
- Install NetworkManager as renderer
        
        sudo apt install network-manager -y

  And modify netplan to use NetworkManager as renderer:
  
        echo $'network:\n  version: 2\n. renderer: NetworkManager' > /etc/netplan/00-installer-config.yaml
        
- Setup multipass network configuration:  make sure lxd driver is used: 
        
        sudo multipass set local.driver=lxd        
        sudo snap restart multipass.multipassd
        
- Show multipass networks:  
        
        sudo multipass networks
        Name    Type      Description
        enp0s25  ethernet  Ethernet device
        mpbr0    bridge    Network bridge for Multipass
  
- Reboot for the change in netplan to take effect

- Then create a new VM using the enp0s25 Ethernet device
        
        sudo multiplan launch //without any option will create VM with 1p ipv4 10.77.97.130
        sudo multipass launch --network=mpbr0 //this option will create VM with 2 ip 10.77.97.154 & 10.77.97.96 (mpbr0 ?) so 2 ip connections
        sudo multipass laucnh --network=enp0s25 //this option will 1st create a bridge device br-enp0s25 with 2 slaves br-enp0s25-child and tapxxxx
        then create VM with ip 
        if you delete br-enp0s25 then the ip addr is available for the host
        
        multipass launch --network=enp0s25

        Multipass needs to create a bridge to connect to enp1s0.
        Do you want to continue (yes/no)? yes
        Creating vm....

- At this point, the ip addr of the host is removed (?) from enp0s25 (only ipv6 remain why?)
- And a physical ip addr is given to the new VM
- 
        
        multipass list

        Name                    State             IPv4             Image
        sinewy-kookaburra       Running           10.86.127.216    Ubuntu 20.04 LTS
                                                  192.168.254.35
- Launching another VM will get another physical ip addr

So we'll need to find out how to preserve the host ip addr.
This procedure is from this url:  https://www.rootisgod.com/2022/Using-Multipass-Like-a-Personal-Cloud-Service/
We may want to try the following:

- Bridge to physical network:  sudo multipass launch --network enp3s0 --network name=mpbr0,mode=manual
- Bridge multipass network to physical network:  sudo multipass launch --network en0 --network name=bridge0,mode=manual
- If failed, check:  sudo lxd -d -v init
- Create a Docker instance:

        The official instructions for installing Docker on Ubuntu are at:

        (docs.docker.com) https://docs.docker.com/engine/install/ubuntu/ -- Install Docker-CE on Ubuntu
        (docs.docker.com) https://docs.docker.com/engine/install/linux-postinstall/ -- Run post-install steps for Linux

-   sudo nano init-instance.sh
        
        NM=$1
        sudo multipass launch --name ${NM} focal
        sudo multipass transfer setup-instance.sh ${NM}:/home/ubuntu/setup-instance.sh
        sudo multipass exec ${NM} -- sh -x /home/ubuntu/setup-instance.sh

-   sudo nano setup-instance.sh
        
        sudo apt-get update
        sudo apt-get upgrade -y
        sudo apt-get -y install \
            apt-transport-https \
            ca-certificates \
            curl \
            gnupg-agent \
            software-properties-common

        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

        sudo apt-key fingerprint 0EBFCD88

        sudo add-apt-repository \
           "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

        sudo apt-get update
        sudo apt-get upgrade -y
        sudo apt-get install -y docker-ce docker-ce-cli containerd.io
        sudo groupadd docker
        sudo usermod -aG docker ubuntu
        sudo systemctl enable docker
        
- Execute ./init-instance.sh drupalVM //to initialize and setup new Docker instance named: drupalVM
- Don't create this way if you want to use Docker:  sudo multipass launch --name drupalVM 20.04
- Get inside drupalVM:  
        
        sudo multipass shell drupalVM
        docker run hello-world //Verify Docker is installed correctly:  
  
  Since you do not have composer, you can create Drupal as follow:
  
        docker run --rm -i --tty -v $PWD:/app composer create-project drupal/recommended-project my_site_name_dir --ignore-platform-reqs

  Or you can use an image here:
  
        docker run -i -t -p 80:80 ricardoamaro/drupal9
  
  You can verify that Drupal is installed by: curl 

- Another way to install Drupal on drupalVM:  
        
        sudo apt install tasksel
        sudo tasksel install lamp-server
        
  Install individual components if preferred

        sudo apt install apache2
        sudo apt install mysql-server
        sudo apt install php libapache2-mod-php php-mysql
        sudo apt install php-cli unzip php-dom php-xml php-curl php-gd php-json php-cgi

  Install Composer to install Drupal.  Follow instructions here: https://getcomposer.org/download/
    
        php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
        php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
        php composer-setup.php
        php -r "unlink('composer-setup.php');"
        
        sudo mv composer.phar /usr/local/bin/composer

  Create a Drupal project using Composer
  
        composer create-project drupal/recommended-project askVN //create a project in ~/askVN
  
  or a specific Drupal:  
        
        composer create-project drupal-composer/drupal-project:9.x-dev askVN --no-interaction

Miscellaneous:
- When to use libvirt driver:
        
        sudo multipass set local.driver=libvirt

- Install Ubuntu desktop on your server:  
        
        sudo apt-get install --no-install-recommends ubuntu-desktop
        sudo apt purge ubuntu-desktop -y && sudo apt autoremove -y && sudo apt autoclean

- Multipass swarm install: https://techsparx.com/software-development/docker/swarm/multipass.html
- Connect VM to network:  https://gist.github.com/ynott/f4bdc89b940522f2a0e4b32790ddb731
- multipass networks command | Multipass documentation

        https://multipass.run/docs/networks-command/19542

Additional network interfaces | Multipass documentation

        https://multipass.run/docs/additional-networks

multipass networks command - Multipass / Documentation - Ubuntu Community Hub

        https://discourse.ubuntu.com/t/multipass-networks-command/19542

        "networks failed: LXD object not found" after sudo snap connect multipass:lxd lxd 
        canonical/multipass#2139

How to Create a Bridged Network for LXD Containers – The New Stack

        https://thenewstack.io/how-to-create-a-bridged-network-for-lxd-containers/
