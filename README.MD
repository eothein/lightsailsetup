# Define Virtual Host Configuration on bitnami
To configure your application to use a virtual host, make these changes:

- Delete the following line in the `/opt/bitnami/apache2/conf/bitnami/bitnami-apps-prefix.conf` file:

```
Include "/opt/bitnami/apps/myapp/conf/httpd-prefix.conf"
```

create new app directory

```
mkdir /home/bitnami/apps/appname
mkdir /home/bitnami/apps/appname/conf
sudo chown -R bitnami:daemon appname/
```

Add a new link in the `/opt/bitnami/apache2/conf/bitnami/bitnami-apps-vhosts.conf` file:

```
nano /opt/bitnami/apache2/conf/bitnami/bitnami-apps-vhosts.conf
Include "/opt/bitnami/apps/appname/conf/httpd-vhosts.conf"
```

## Create http-vhosts.conf file 

```{xml }
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        Options Indexes FollowSymLinks MultiViews
        ServerAdmin jens.buysse@gmail.com
        ServerName appname.e-quality.be
        ServerAlias www.appname.e-quality.be
        DocumentRoot "/home/bitnami/appname/public_html"

        <Directory "/home/bitnami/appname/public_html">
        Order allow,deny
        Allow from all
        Require all granted
        AllowOverride All
        </Directory>
    ErrorLog "/home/bitnami/appname/appname-error_log"
    CustomLog "/home/bitnami/appname/appname-access_log" common
</VirtualHost>
```
Add this as `/opt/bitnami/apps/appname/conf/httpd-vhosts.conf`

## Installeer Luya

Zie [setup guide](https://luya.io/guide/install).

## Setup virtual host locally

```
sudo nano /etc/apache2/extra/httpd-vhosts.conf
```

XML code voor virtual host

```
<VirtualHost *:80>
        Options Indexes FollowSymLinks MultiViews
        ServerAdmin jens.buysse@gmail.com
        ServerName preventic.test
        ServerAlias www.preventic.test
        DocumentRoot "/Users/eothein/equality/projects/preventic/preventic-site/public_html"

        <Directory "/Users/eothein/equality/projects/preventic/preventic-site/public_html">
        Order allow,deny
        Allow from all
        Require all granted
        AllowOverride All
        </Directory>
    ErrorLog "/private/var/log/apache2/preventic.test-error_log"
    CustomLog "/private/var/log/apache2/preventic.test-access_log" common
</VirtualHost>
```

Add to host file

```
mate /etc/hosts
127.0.0.1	preventic.test
```

## Setup project with git

```
git remote add origin git@github.com:eothein/preventic.git
git remove -v
```

To add the luya project to git add the following directories

```
git add assets/*.php composer.json configs/*.php messages/*.php public_html/* resources/* tests/*.php views/*.php
git add public_html/.htaccess
git add views/cmslayouts/main.json
git add README.md
git add CHANGELOG.md
git add .gitignore
``

## Generating a ssh key 
```
ssh-keygen -t rsa -b 4096 -C "jens.buysse@gmail.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
pbcopy < ~/.ssh/id_rsa.pub
```


## Setup mysql workbench connection to server

Login op de server op bitnami

```
mysql -u root -p
CREATE USER 'jens'@'127.0.0.1' IDENTIFIED BY 'pwd';
GRANT ALL PRIVILEGES ON *.* TO 'jens'@'127.0.0.1';
FLUSH PRIVILIGES; 
```
Paswoord: `cat ./bitnami_credentials`

Nog niet gevonden hoe Workbench kan connecteren.

## Luya installeren op server

Composer directory toevoegen

```
sudo chown -R bitnami /opt/bitnami/php/composer
sudo chown -R bitnami /opt/bitnami/php/composer
composer global require "fxp/composer-asset-plugin:~1.4"
composer install
```



