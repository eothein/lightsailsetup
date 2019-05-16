# Define Virtual Host Configuration
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

