

## web 2

yum install https://packages.icinga.com/epel/icinga-rpm-release-7-latest.noarch.rpm

yum install epel-release

yum install centos-release-scl -y 

yum install icingaweb2 icingacli -y 

yum install httpd

systemctl start httpd.service
systemctl enable httpd.service

firewall-cmd --add-service=http
firewall-cmd --permanent --add-service=http

systemctl start rh-php71-php-fpm.service
systemctl enable rh-php71-php-fpm.service

yum install rh-php71-php-mysqlnd

yum -y install http://mirror.centos.org/centos/7/sclo/x86_64/sclo/sclo-php71/sclo-php71-php-pecl-imagick-3.4.3-2.el7.x86_64.rpm
systemctl restart httpd
systemctl restart rh-php71-php-fpm

chcon -R -t httpd_sys_rw_content_t /etc/icingaweb2/





icingacli setup token create



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