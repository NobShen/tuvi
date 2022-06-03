# create tuvi.com website 
- Install Ubuntu server 22.04 & ignore lid behavior:  sudo nano /etc/systemd/logind.conf && systemctl restart systemd-logind.service
- Use http://192.168.254.254/cgi-bin/home.ha to find out the ip address of the server
- Install multipass:  sudo snap install multipass && sudo snap info multipass
-   To remove:  sudo snap remove multipass
- Use libvirt driver:
        
        sudo apt install libvirt-daemon-system
        // connect the libvirt interface/plug
        snap connect multipass:libvirt
        // Then, to switch the Multipass driver to libvirt, you'll need to stop all the instances first
        multipass stop --all
        // now tell Multipass to use libvirt
        sudo multipass set local.driver=libvirt
        sudo multipass list

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
- Get inside drupalVM:  
        
        sudo multipass shell drupalVM
        docker run hello-world //Verify Docker is installed correctly:  
  
  Since you do not have composer, you can create Drupal as follow:
  
        docker run --rm -i --tty -v $PWD:/app composer create-project drupal/recommended-project my_site_name_dir --ignore-platform-reqs
  
  You can verify that Drupal is installed by: curl 

Miscellaneous:
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
