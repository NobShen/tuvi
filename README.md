# create tuvi.com website 
- Install Ubuntu server 22.04 & ignore lid behavior:  sudo nano /etc/systemd/logind.conf && systemctl restart systemd-logind.service
- Install multipass:  sudo snap install multipass && sudo snap info multipass
-   To remove:  sudo snap remove multipass
- Create a new ubuntu 20.04 vm: sudo multipass launch --name drupalVM 20.04
- Get inside drupalVM:  sudo multipass shell drupalVM
- Initialize Docker on drupalVM:  
- The steps required to setup Docker Swarm on Ubuntu 20.04 on Multipass are:

Initialize an Ubuntu 20.04 instance on Multipass
Inside the instance execute the steps to initialize Docker on Ubuntu 20.04
Inside the first such instance run docker swarm init
Inside subsequent instances, run the command causing the instance to join the swarm
The official instructions for installing Docker on Ubuntu are at:

(docs.docker.com) https://docs.docker.com/engine/install/ubuntu/ -- Install Docker-CE on Ubuntu
(docs.docker.com) https://docs.docker.com/engine/install/linux-postinstall/ -- Run post-install steps for Linux
- Now install lamp on foo
-   sudo apt install tasksel
-   sudo tasksel install lamp-server
- Install individual components if preferred
-   sudo apt install apache2
-   sudo apt install mysql-server
-   sudo apt install php libapache2-mod-php php-mysql
-   sudo apt install php-cli unzip php-dom php-xml php-curl php-gd php-json php-cgi
- Install Composer to install Drupal.  Follow instructions here: https://getcomposer.org/download/
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
    php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
    php composer-setup.php
    php -r "unlink('composer-setup.php');"
    sudo mv composer.phar /usr/local/bin/composer
- Create a Drupal project using Composer
-   composer create-project drupal/recommended-project askVN //create a project in ~/askVN
-   or a specific Drupal:  composer create-project drupal-composer/drupal-project:9.x-dev askVN --no-interaction
- If you're using a docker based workflow and do not have composer, you can create Drupal as follow:
-   docker run --rm -i --tty -v $PWD:/app composer create-project drupal/recommended-project my_site_name_dir --ignore-platform-reqs
- Now run docker hello-world to verify docker is installed correctly:  sudo multipass exec docker docker run hello-world
-   sudo multipass docker run hello-world //Hello from Docker!

Miscellaneous:
- Install Ubuntu desktop on your server:  sudo apt-get install --no-install-recommends ubuntu-desktop
- Remove Ubuntu desktop when you're done:  sudo apt purge ubuntu-desktop -y && sudo apt autoremove -y && sudo apt autoclean
- https://techsparx.com/software-development/docker/swarm/multipass.html
- https://gist.github.com/ynott/f4bdc89b940522f2a0e4b32790ddb731 //shows how to connect VM to network
