---
layout: post
title:  "Set up LAMP in Ubuntu"
date:   2014-08-29 14:34:25
categories: "Front End Development"
tags: ubuntu, apache, lamp
published: true
image: 
---

# Install Apache

{% highlight bash %}
sudo apt-get update
sudo apt-get install apache2
{% endhighlight %}

Open browser, and enter server's ip address, if it displays "It works!", means everything's good 

#Install MySQL

{% highlight bash %}
sudo apt-get install mysql-server libapache2-mod-auth-mysql php5-mysql
sudo mysql_install_db
sudo /usr/bin/mysql_secure_installation
{% endhighlight %}

Remember to enter password for root. Enter `Y` for all options.

#Install PHP

{% highlight bash %}
sudo apt-get install php5 libapache2-mod-php5 php5-mcrypt
{% endhighlight %}

## Change directory index file
{% highlight bash %}
sudo vim /etc/apache2/mods-enabled/dir.conf
{% endhighlight %}

Add `index.php` to the beginning of index files:
{% highlight text %}
<IfModule mod_dir.c>
  DirectoryIndex index.php index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
{% endhighlight %}


#Configure Apache Permision

## adduser to www-data group

{% highlight bash %}
sudo adduser username www-data
{% endhighlight %}

## Change folder ownership and set permission
{% highlight bash %}
sudo chown -R username:username /var/www/html
sudo chmod 775 -R /var/www/html
{% endhighlight %}


#Add virtual hosts

## Create new config file

{% highlight bash %}
sudo cp /etc/apache2/sites-available/default /etc/apache2/sites-available/mysite.com
sudo vim /etc/apache2/sites-available/mysite.com
{% endhighlight %}

Modify `ServerName` to `mysite.com`  
Modify `<VirtualHost *:80>` info ..
Modify `DocumentRoot` to `DocumentRoot /var/www/html`

#Activate Host
{% highlight bash %}
sudo a2ensite mysite.com
{% endhighlight %}

#Restart Apache 
{% highlight bash %}
sudo service apache2 restart
{% endhighlight %}


