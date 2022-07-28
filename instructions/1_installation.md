# Installation instructions

These instruction should guide you through how to set up
a Collective Access instance on a Nectar project from scratch.

The instructions are taken from several internet sources, including
Nectar tutorials and Collective Access installation instructions.

Please refer to mentioned links in case these instructions become deprecated.


## Creating the Collective Access instance

See https://manual.collectiveaccess.org/setup/install/Ubuntu_20.04.html#install-ubuntu-20-04
for more details.

```
$ sudo apt install -y apache2
$ sudo systemctl enable apache2.service
$ sudo systelctl start apache2.service
$ sudo apt install -y php libapache2-mod-php php-mbstring php-xmlrpc php-gd php-xml php-intl php-mysql php-cli php-zip php-curl php-posix php-dev php-pear php-redis php-gmagick php-gmp
```

Use `php -v` to check that everything is installed okay.

Edit `/etc/php/7.4/apache2/php.ini` to raise upload limits:
```
Memory_limit = 512M
Display_errors = On # in development, and Off in production
Post_max_size = 800M
Upload_max_filesize = 750M
```
```
$ sudo systemctl restart apache2.service
$ sudo apt install -y mysql-server
$ sudo systemctl start mysql
$ sudo systemctl enable mysql
$ sudo apt install -y ghostscript libgraphicsmagick1-dev libpoppler-dev poppler-utils dcraw redis-server ffmpeg libimage-exiftool-perl libreoffice mediainfo
cd /var/www/html
$ sudo git clone https://github.com/collectiveaccess/providence.git ca
$ sudo chown -R ubuntu:ubuntu ca
$ cd ca
$ git fetch --all --tags --prune
```

Check https://github.com/collectiveaccess/providence/releases for tagged versions
Or use `git tag --sort=committerdate -l` to see sorted order of tags

```
$ git checkout tags/X.Y.Z -b ca_X.Y.Z
$ cp setup.php-dist setup.php
```

Create a new VM user for MySQL to use
```
$ sudo useradd -mUs /bin/bash gmdb
$ sudo passwd gmdb
```

Don't forget to note down the password you set somewhere! Then set up MySQL:

```
$ sudo mysql -u root

CREATE DATABASE my_archive;
CREATE USER gmdb@localhost identified by 'my_password';
GRANT ALL on my_archive.* to gmdb@localhost;
exit;

$ sudo service mysql restart
```

In case of DB access issues try, and recreate user in MySQL again:
```
drop user gmdb@localhost;
flush privileges;
```

Edit the `setup.php` file (nano) to set up the following Collective Access settings:

```
define("__CA_DB_USER__", "gmdb");
define("__CA_DB_PASSWORD__", "my_password");
define("__CA_DB_DATABASE__", "my_archive");
define("__CA_STACKTRACE_ON_EXCEPTION__",true);
define("__CA_APP_DISPLAY_NAME__","Great Mercury Collective Access");
date_default_timezone_set('Pacific/Auckland');

```

```
$ sudo systemctl restart apache2.service
$ cd /var/www/html/ca
$ sudo chown -R www-data app/tmp app/log media vendor
$ sudo chmod -R 755 app/tmp app/log media vendor
```

Navigate to http://130.216.XXX.YYY/ca/ using local proxy (instructions to add)
and run the installer link. Add an email (used when email server configured).
Select the relevant profile (Default or project specific, see `../profiles` directory in
this repo).

Make a note of the USER/PASSWORD when done!


## Essential configuration for production

Configure apache to make Collective Access the default landing site:

```
$ cd /etc/apache2/sites-available
$ sudo cp 000-default.conf collective-access.conf
$ sudo nano collective-access.conf
    ServerName CollectiveAccess 
    DocumentRoot /var/www/html/ca
$ sudo a2ensite collective-access.conf
$ sudo a2dissite 000-default.conf
$ sudo apache2ctl configtest
$ sudo systemctl restart apache2
```

Follow these instructions to configure the VM to use a LetsEncrypt HTTPS cert:
https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-20-04 and 
https://tutorials.rc.nectar.org.au/dns/01-overview

```
$ sudo nano collective-access.conf
    ServerName ca.PROJECT.cloud.edu.au
$ sudo systemctl restart apache2
```
where PROJECT is the name of the Nectar Project.

Open http/https ports via Nectar security groups to allow cert challenge to
auto-refresh the HTTPS certificate.

Choose the following options while following the above instructions:

* Choose the ca.PROJECT.cloud.edu.au option
* Redirect all to https

Now create and attach a volume to the VM by following these Nectar instructions:
https://tutorials.rc.nectar.org.au/volume-storage/01-overview

and set up symlinks in the ~/ca/media folder to allow Collective Access to use
the newly attached storage:

```
$ cd /var/www/html/ca/media/
$ mkdir /gm_data/media/collectiveaccess/flv
$ mkdir /gm_data/media/collectiveaccess/images
$ mkdir /gm_data/media/collectiveaccess/mp3
$ mkdir /gm_data/media/collectiveaccess/quicktime
$ mkdir /gm_data/media/collectiveaccess/swf
$ mkdir /gm_data/media/collectiveaccess/tilepics
$ mkdir /gm_data/media/collectiveaccess/windowsmedia
$ mkdir /gm_data/media/collectiveaccess/workspace
$ sudo rm -r /var/www/html/ca/media/collectiveaccess
$ sudo ln -s /gm_data/media/collectiveaccess/ collectiveaccess
$ sudo chown -R www-data:www-data /gm_data/media/
$ sudo systemctl restart apache2.service
```