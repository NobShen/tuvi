# create tuvi.com website 
- Install Ubuntu server 22.04 & ignore lid behavior:  sudo nano /etc/systemd/logind.conf && systemctl restart systemd-logind.service
- Install multipass:  sudo snap install multipass && sudo snap info multipass
-   To remove:  sudo snap remove multipass
- Create a Docker instance:

        The official instructions for installing Docker on Ubuntu are at:

        (docs.docker.com) https://docs.docker.com/engine/install/ubuntu/ -- Install Docker-CE on Ubuntu
        (docs.docker.com) https://docs.docker.com/engine/install/linux-postinstall/ -- Run post-install steps for Linux

-   sudo nano init-instance.sh
        
        NM=$1
        multipass launch --name ${NM} focal
        multipass transfer setup-instance.sh ${NM}:/home/ubuntu/setup-instance.sh
        multipass exec ${NM} -- sh -x /home/ubuntu/setup-instance.sh

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
- Install Ubuntu desktop on your server:  sudo apt-get install --no-install-recommends ubuntu-desktop
- Remove Ubuntu desktop when you're done:  sudo apt purge ubuntu-desktop -y && sudo apt autoremove -y && sudo apt autoclean
- https://techsparx.com/software-development/docker/swarm/multipass.html
- https://gist.github.com/ynott/f4bdc89b940522f2a0e4b32790ddb731 //shows how to connect VM to network
