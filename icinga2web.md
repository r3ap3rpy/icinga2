### Installing the web ui

This part is also tricky a bit but only because of configuration steps.

If you install it on other node than the engine itself you may need to modify the config steps and install these packages for start.

You either "sudo su -" or prefix the commands with sudo.

``` bash
yum install https://packages.icinga.com/epel/icinga-rpm-release-7-latest.noarch.rpm
yum install epel-release
``` 

We need to install the SCL package.

``` bash
yum install centos-release-scl -y 
``` 

Then we can install the web and cli components.

``` bash
yum install icingaweb2 icingacli -y 
``` 

We also need a webserver installed, and started and enabled.

``` bash
yum install httpd
systemctl start httpd.service
systemctl enable httpd.service
``` 

We need to allow **http** service through the firewall permanently.

``` bash
firewall-cmd --permanent --add-service=http
``` 

Icinga needs a newer version of php for which the packages are installed as dependency we just need to enable and start the service.

``` bash
systemctl start rh-php71-php-fpm.service
systemctl enable rh-php71-php-fpm.service
```

There are some dependencies which we need to handle, for example imagick which allows you to generate pdf reports.
After the install the php and httpd service begs for a restart.

``` bash
yum install http://mirror.centos.org/centos/7/sclo/x86_64/sclo/sclo-php71/sclo-php71-php-pecl-imagick-3.4.3-2.el7.x86_64.rpm -y 
systemctl restart httpd
systemctl restart rh-php71-php-fpm
```

We need to change the context of the icingaweb2 root to prevent the disabling of selinux.

``` bash
chcon -R -t httpd_sys_rw_content_t /etc/icingaweb2/
```

We should now create a token for the setup.

``` bash
icingacli setup token create
```

If you forget it or close the terminal you can check the token with this command.

``` bash
icingacli setup token show
``` 

Now we can navigate to the browser and complete our setup. The url is "http://yourhostname/icingaweb2/setup".

We insert the token.

![Drag Racing](pics/token.png)

sudo vi /etc/icinga2/features-available/ido-mysql.conf
/**
 * The db_ido_mysql library implements IDO functionality
 * for MySQL.
 */
library "db_ido_mysql"
object IdoMysqlConnection "ido-mysql" {
  user = "icinga"
  password = "icinga"
  host = "localhost"
  database = "icinga"
}



#### api CONFIG

vim /etc/icinga2/conf.d/api-users.conf

object ApiUser "icingaweb2" {
  password = "Wijsn8Z9eRs5E25d"
  permissions = [ "status/query", "actions/*", "objects/modify/*", "objects/query/*" ]
}

systemctl restart icinga2