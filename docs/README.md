# LAMP vs Docker

Hosting a Laravel site in windows on a fresh Ubuntu 18.04 Server in VirtualBox.

## Setup

Installing docker is explained [here](./install-docker/).  
Hosting a simple site with docker is explained [here](./docker-simple-example/).  
Hosting a simple site and setting up a lamp server [here](./lamp-simple-example/).

The real test will be to set up a laravel site

1. How easy it is to set up
2. How easy it is to add a new site
3. How fast is is to add a new site

## Docker

### Devilbox

I will use [devilbox](http://devilbox.org/), a pre made LAMP stack to try and host my Laravel website.

Since I'm only using a 10GB virtual drive for my linux server, I can't start [all the containers](https://devilbox.readthedocs.io/en/latest/readings/available-container.html?highlight=container) it provides.
But for hosting a laravel website I'll only need httpd, php and mysql.

- `git clone https://github.com/cytopia/devilbox`
- `cd devilbox` `cp env-example .env`
- docker-compose up -d httpd php mysql
  - first time setup 4 min
  - once the images are loaded

### Installing my Laravel website

- Enter the php shell so we have access to composer etc `./shell.sh`
- Make a folder, enter the folder and git clone your project in there. Mine is called wedstrijd.
- Enter the laravel project and composer install. This took 18s65.
- To make a database we can surf to the ip of our server and just use phpmyadmin out of the box.
- The root folder is always /htdocs. So make a symbolic link from the public folder to htdos.
  `ln -s wedstrijd/public/ htdocs`
- To make the host we have to edit /etc/hosts on windows. The generated path is wedstrijd.loc. e.g. `192.168.0.110 wedstrijd.loc`

## LAMP

We will need to install all packages needed to host laravel ourselves.

### Composer

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '93b54496392c062774670ac18b134c3b3a95e5a5e5c8f1a9f115f203b75bf9a129d5daa8ba6a13e2cc8a1da0806388a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

To add it to our bin we use `mv composer.phar /usr/local/bin/composer`

- sudo apt install php-zip
- composer global require "laravel/installer"
