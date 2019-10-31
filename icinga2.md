### Installing the engine

In order to prepare for the configuration we need to install some prerequisites like icinga and epel repositories

You should either "sudo su -" or append the "sudo" prefix to get these installed.

``` bash
yum install https://packages.icinga.com/epel/icinga-rpm-release-7-latest.noarch.rpm -y 
yum install epel-release -y
```

After this we are able to install, start and enable icinga.

``` bash
yum install icinga2 -y 
systemctl enable icinga2
systemctl start icinga2
```

We can list the enabled and disabled features with this command.

``` bash
icinga2 feature list
```

Now we need to install the plugins to allow monitoring of remote resources.

``` bash
yum install nagios-plugins-all -y 
``` 

The SE linux package is needed to work properly and we need to restart the instance.

``` bash
yum install icinga2-selinux -y
systemctl restart icinga2
``` 

Optionally you can have some form of syntax highlighting in your favourite editor, mine is **vim**.

``` bash
yum install vim-icinga2 vim -y
vim ~/.vimrc
syntax on
``` 

Now let's instal the mariadb, or you can opt for the postgres version if you want. We also need the IDO module.

``` bash
yum install mariadb-server mariadb -y 
yum install icinga2-ido-mysql -y 
```

Once the install is complete we can enable and start the DB.

``` bash
systemctl enable mariadb
systemctl start mariadb
``` 

Then we securely configure the DB. I set the password to "Start!123", and leave the rest to the default.

``` bash
mysql_secure_installation
``` 

Now we create our table with the user.

``` bash
mysql -u root -p

CREATE DATABASE icinga;
GRANT SELECT, INSERT, UPDATE, DELETE, DROP, CREATE VIEW, INDEX, EXECUTE ON icinga.* TO 'icinga'@'localhost' IDENTIFIED BY 'icinga';
quit

``` 

Now we apply the schema to our **icinga** database.

``` bash
mysql -u root -p icinga < /usr/share/icinga2-ido-mysql/schema/mysql.sql
``` 

Finally we enable the IDO module and restart icinga2.

``` bash
icinga2 feature enable ido-mysql
systemctl restart icinga2
``` 