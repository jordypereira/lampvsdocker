# Simple site in LAMP

- `sudo apt install lamp-server^`
  or
- `sudo apt install apache2`
- `sudo apt install mysql-server`
- `sudo apt install php7.2 libapache2-mod-php7.2 php-mysql`
- `sudo apt install php-curl php-json php-cgi`

## Change VHost

- `sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/tester.int.conf`

```bash
<Directory /var/www/html/tester.int/public_html>
        Require all granted
</Directory>

<VirtualHost *:80>
  ServerName tester.int
  ServerAlias www.tester.int
  ServerAdmin testerman@localhost
  DocumentRoot /var/www/html/tester.int/public_html
  ErrorLog /var/www/html/tester.int/logs/tester.error.log
  CustomLog /var/www/html/tester.int/logs/tester.access.log combined
</VirtualHost>
```

- `sudo mkdir -p /var/www/html/tester.int/{public_html,logs}`

- `sudo a2ensite tester.int`
- `sudo a2dissite 000-default.conf`
- `sudo systemctl reload apache2`
- `sudo echo "<?php phpinfo() ?>" > /var/www/html/tester.int/public_html/index.php

- Add to windows host file `192.168.0.109 tester.int`

Now tester.int will load our simple page.
