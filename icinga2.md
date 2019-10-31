yum install https://packages.icinga.com/epel/icinga-rpm-release-7-latest.noarch.rpm
yum install epel-release


yum install icinga2 -y 
systemctl enable icinga2
systemctl start icinga2

icinga2 feature list


yum install nagios-plugins-all -y 

yum install icinga2-selinux

systemctl restart icinga2
yum install vim-icinga2 vim -y
vim ~/.vimrc
syntax on

yum install mariadb-server mariadb
yum install icinga2-ido-mysql

systemctl enable mariadb
systemctl start mariadb
mysql_secure_installation

yum install icinga2-ido-mysql


mysql -u root -p

CREATE DATABASE icinga;
GRANT SELECT, INSERT, UPDATE, DELETE, DROP, CREATE VIEW, INDEX, EXECUTE ON icinga.* TO 'icinga'@'localhost' IDENTIFIED BY 'icinga';
quit

mysql -u root -p icinga < /usr/share/icinga2-ido-mysql/schema/mysql.sql

icinga2 feature enable ido-mysql
systemctl restart icinga2
