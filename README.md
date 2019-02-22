# Template Docky Odoo com módulos de localização brasileira.

Primeiro clone este projeto

git clone https://github.com/Escodoo/docky-odoo-template.git

Define your repository as virtual environement

Then go to https://github.com/akretion/ak/wiki

# ODOO+DOCKY INSTALATION ON UBUNTU 18.04 LTS BIONIC

## SSH as root
apt-get update

## Add erp user:
adduser erp
sudo visudo

## add line:
erp      ALL=(ALL) NOPASSWD:ALL
# see https://phpraxis.wordpress.com/2016/09/27/enable-sudo-without-password-in-ubuntudebian/

## switch to erp user and use byobu
su erp
sudo apt-get install byobu
byobu


## install Docker CE ( see https://docs.docker.com/install/linux/docker-ce/ubuntu/ )
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

## ensure it worked:
sudo docker run hello-world

## add the erp user to the docker group
sudo usermod -aG docker $USER

## log out from byobu with
CTRL+D

## log out form the erp user session with
CTRL+D

## -> you should be root now!
### log in a the erp user
su erp
byobu

## you should now be able to run docker as the erp user. Check it with:
docker run hello-world

## install ak and Docky Akretion devops tools:
sudo apt-get install python3-pip
sudo pip3 install git+https://github.com/akretion/ak.git@master --upgrade
sudo pip3 install git+https://github.com/akretion/docky.git@master --upgrade

## clone the Docky v 12.0 template to create your 1st Docky project!
git clone https://github.com/Escodoo/docky-odoo-template docky-odoo1
cd docky-odoo1/odoo

--- fix the ak command for Python3, see: https://github.com/akretion/ak/issues/55

ak build
cd ..
docky build

## enter the Docky container and run the Odoo service:
docky run
odoo -d db -i crm

# Setup Nginx
set up Nginx to access from outside of the serveur:
open a new byobu tab using SHIFT+F2
you can then switch tab using SHIFT+F3 and SHIFT+F4

sudo apt-get install nginx

## set up some default nginx config for your Docky container
sudo wget https://gist.githubusercontent.com/rvalyi/10f0770a49b626f40a2c1910374dc70d/raw/457baa90cb0321d95af14437013286a36ba85f5c/nginx-odoo -O /etc/nginx/sites-enabled/erp

## reload nginx

YOU CAN NOW ACCESS YOUR ODOO SERVER BY BROWSING THE IP OF YOUR SERVER
USING HTTP (NOT HTTPS, PORT IS 80)

WARNING: password for the db database is admin/admin

# FOR PRODUCTION USE HTTPS AND USE THE docky up instead!!
sudo /etc/init.d/nginx reload
