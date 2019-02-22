# Template Docky Odoo com módulos de localização brasileira.

##  Instalação ODOO+DOCKY no UBUNTU 18.04 LTS BIONIC

### Preparando o servidor

Acesse o SHELL do servidor e execute os seguintes passos:

```
apt-get update
sudo apt-get install byobu
```

Crie o usuário que executar o serviço

```
adduser erp
sudo visudo
```

Adicione a linha abaixo no arquivo visudo (veja https://phpraxis.wordpress.com/2016/09/27/enable-sudo-without-password-in-ubuntudebian/)

```
erp      ALL=(ALL) NOPASSWD:ALL
```


Troque seu usurário para o usuário ERP e use o byobu

```
su erp
byobu
```

#### Instalação Docker CE
Instale o Docker CE na estação onde ser instalado o serviço (Veja https://docs.docker.com/install/linux/docker-ce/ubuntu/)

```
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
```

Verifique se o Docker está operacional:

```
sudo docker run hello-world
```

Adicione o usuário ERP no grupo do Docker

```
sudo usermod -aG docker $USER
```

saia do byobu com

**CTRL+D**

faça logout da sessão do usuário erp com

**CTRL+D**

-> você deve ser root agora!
faça o login no usuário erp

```
su erp
byobu
```

Agora você deve conseguir executar o docker como o usuário do erp. Verifique com::

```
docker run hello-world
```

O resultado deve ser algo parecido com isso:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
#### Instale o ak eo Docky Akretion devops tools:

```
sudo apt-get install python3-pip
sudo pip3 install git+https://github.com/akretion/ak.git@master --upgrade
sudo pip3 install git+https://github.com/akretion/docky.git@master --upgrade
```

Clone o Docky-Odoo v11.0 template para criar seu primeiro projeto Docky

```
git clone -b 11.0 https://github.com/Escodoo/docky-odoo-template docky-odoo-v11-1

cd docky-odoo-v11-1/odoo
```

Corrija o AK para rodar em Python3, veja: https://github.com/akretion/ak/issues/55

Altere o arquivo *usr/local/lib/python3.6/dist-packages/ak/ak_build.py* e substitua `python` por `python3` na linha 5.

```
ak build
cd ..
docky build
```

Entre no contêiner Docky e execute o serviço Odoo:

```
docky run
odoo -d db -i crm
```

### Setup Nginx
configure o Nginx para acessar de fora do servidor:
abrir uma nova aba byobu usando SHIFT + F2
você pode então alternar a guia usando SHIFT + F3 e SHIFT + F4

```
sudo apt-get install nginx
```

#### Configurar algumas configurações nginx padrão para o seu contêiner Docky

```
sudo wget https://gist.githubusercontent.com/rvalyi/10f0770a49b626f40a2c1910374dc70d/raw/457baa90cb0321d95af14437013286a36ba85f5c/nginx-odoo -O /etc/nginx/sites-enabled/erp

sudo systemctl reload nginx
```

VOCÊ PODE AGORA ACESSAR SEU SERVIDOR ODOO AO NAVEGAR NO IP DO SEU SERVIDOR
USANDO HTTP (NÃO HTTPS, PORT IS 80)

AVISO: a senha para o banco de dados db é admin/admin

#### PARA USO DE PRODUÇÃO HTTPS E USE O docky up em vez !!

```
sudo /etc/init.d/nginx reload
```

