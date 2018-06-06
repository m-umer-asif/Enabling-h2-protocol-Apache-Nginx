# Enabling h2 (Http/2) protocol for Apache and Nginx

* Note: h2 only works with HTTPS enabled. For a website that is not on https, letsencript can be used.

* Its would be a good practice to install apache from the following repo, as this repo contains the latest version of apache or nginx which has support for h2.

### For apache
```bash
$ sudo add-apt-repository ppa:ondrej/apache2
```

```bash
$ sudo apt-key update
```

```bash
$ sudo apt-get update
```

### For nginx 
```bash
$ sudo add-apt-repository ppa:ondrej/nginx
```

```bash
$ sudo apt-key update
```

```bash
$ sudo apt-get update
```

### To enable https use the following links.

* [For Ubuntu 14.04 and nginx](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-14-04)

* [For Ubuntu 16.04 and nginx](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04)

* [For Ubuntu 14.04 and Apache](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-14-04)

* [For Ubuntu 16.04 and Apache](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-16-04)

# Apache

### Updating apache2

* Apache 2.4.27, HTTP/2 not supported in prefork

```bash
$ sudo apt-get install software-properties-common
```

```bash
$ sudo add-apt-repository ppa:ondrej/apache2\
```

```bash
$ sudo apt-key update
```

```bash
$ sudo apt-get update
```

```bash
$ sudo apt-get upgrade
```

```bash
$ sudo apt-get --only-upgrade install apache2 -y
```

* here you will be prompted two times like :

```bash
*** apache2.conf (Y/I/N/O/D/Z) [default=N] ?
```

* press Y both the times and proceed.


```bash
$ sudo a2enmod http2
```

* Update the host file in 
```bash
$ nano /etc/apache2/sites-available/000-default-le-ssl.conf
```

	<VirtualHost *:443> 
	[For HTTPS Support]
	Protocols h2 http/1.1

	[For HTTP Support]
	Protocols h2c http/1.1

```bash
$ sudo service apache2 restart
```

### Updating PHP

```bash
$ sudo add-apt-repository ppa:ondrej/php
```

```bash
$ sudo apt-get install -y language-pack-en-base
```

```bash
$ sudo LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php
```

```bash
$ sudo apt-get update
```

```bash
$ sudo apt-get install php7.2
```

```bash
$ sudo apachectl stop
```

```bash
$ sudo apt-get install php7.2-fpm
```

```bash
$ sudo a2enmod proxy_fcgi setenvif
```

```bash
$ sudo a2enconf php7.2-fpm
```

```bash
$ sudo a2dismod php7.2
```

```bash
$ sudo a2dismod mpm_prefork
```

```bash
$ sudo a2enmod mpm_event
```

```bash
$ sudo apachectl start
```

#### Troubleshooting
* If you have some problem with


```bash
$ sudo a2dismod php7.3
```


* just try this:

```bash
$ sudo a2dismod php7.0
```

```bash
$ sudo a2dismod php7.1
```

```bash
$ sudo service apache2 restart
```

### Updating MYSQL
```bash
$ sudo apt-get install php-mysqlnd
```
* from output of the above command choose explicit version of mysql to be installed.


```bash
sudo service apache2 restart
```

#### To check for error go to 
```bash
$ sudo nano /var/log/apache2/error.log
```
* And see what are the errors.


# Nginx

### Updating Nginx

* (skip these steps if you have already updated nginx)
```bash
$ sudo add-apt-repository ppa:ondrej/nginx
```

```bash
$ sudo apt-key update
```

```bash
$ sudo apt-get update
```

```bash
$ sudo apt-get install nginx
```

```bash
$ sudo nginx -v 
```
* Should give output something like this

```bash
nginx version: nginx/1.10.0 (Ubuntu)
```

### Changing the Listening Port and Enabling HTTP/2

* Open the configuration file
```bash
$ sudo nano /etc/nginx/sites-available/default
```

* Add http2 as indicated


    listen 443 ssl http2 default_server;
    listen [::]:443 ssl http2 default_server;

* Save and close the file, then run

```bash
$ sudo nginx -t
```

* This should display the following
```bash
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

* Restart nginx service

```bash
sudo service nginx restart
```

## References:

* https://serverfault.com/questions/867454/http2-enabled-but-not-supported
* https://askubuntu.com/questions/979323/http-2-not-working-in-ubuntu-16-04-with-apache2-how-can-i-debug-this
* https://www.2daygeek.com/how-to-enable-http2-0-support-on-apache-web-server/2/#
* https://stackoverflow.com/questions/37103927/why-does-apache2-module-http2-not-exist-on-ubuntu-16-04
* https://serverfault.com/questions/515417/error-start-apache-php-value
* https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-with-http-2-support-on-ubuntu-16-04
