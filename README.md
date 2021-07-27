# Debian Server Set Up

## Setup SSH

```
sudo apt-get update ; \
sudo apt-get install -y vim mosh htop git curl wget unzip zip gcc build-essential make
```

Configure SSH:

```
sudo vim /etc/ssh/sshd_config
    AllowUsers farid
    PermitRootLogin no
    PasswordAuthentication no
    
systemctl restart sshd
service sshd restart
/etc/init.d/ssh restart
    
sudo adduser farid
usermod -aG sudo farid
su - farid

mkdir ~/.ssh
vim ~/.ssh/authorized_keys 

ssh newuser@server_address
```

## ZSH

```
sudo apt-get install -y zsh
```

## Docker
```
sudo apt install apt-transport-https ca-certificates curl gnupg2 software-properties-common
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"

sudo apt update

sudo apt install docker-ce
sudo usermod -aG docker ${USER}


sudo usermod -aG   docker   $USER
sudo    newgrp   docker

id -nG
```

Docker-compose:
```
sudo curl -L https://github.com/docker/compose/releases/download/1.25.3/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```


## Apache

Install Apache, create `code` directory in user home directory:

```
sudo apt install -y apache2 apache2-utils
cd
mkdir code
cd code
mkdir newproject
sudo chown www-data:www-data /home/www/code/ -R
```

Now install PHP:

```
sudo apt install -y php7.0 libapache2-mod-php7.0 php7.0-mysql php-common php7.0-cli php7.0-common php7.0-json php7.0-opcache php7.0-readline php7.0-mbstring php7.0-xml php7.0-gd
```

Enable `mod_php` in Apache:

```
sudo a2enmod php7.0
sudo systemctl restart apache2
```

Configure Apache:

```
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/newproject.conf

sudo vim /etc/apache2/sites-available/newproject.conf
```

Change or append lines:

```
        DocumentRoot /home/www/code/newproject
        ServerName newproject.your-site.ru
```

Enable site config in Apache, enable `mod_rewrite`:

```
sudo a2ensite newproject.conf

sudo a2enmod rewrite
```

Configure Apache for `.htaccess` files:

```
sudo vim /etc/apache2/apache2.conf
```

Change lines:

```
<Directory />
    Options FollowSymLinks
    AllowOverride All
    Order allow,deny
    Allow from all
</Directory>
```

Restart Apache:

```
sudo service apache2 restart
```


