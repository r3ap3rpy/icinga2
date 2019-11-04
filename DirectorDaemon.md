### THe director needs it's daemon.

In order for the director to work it needs it's daemon setup.

We do this by the following.

We need to create a user which is going to run the daemon.

``` bash
useradd -r -g icingaweb2 -d /var/lib/icingadirector -s /bin/false icingadirector
install -d -o icingadirector -g icingaweb2 -m 0750 /var/lib/icingadirector
```

We need to create a unit file so the daemon is managable by SystemD, this file comes with the director we just need to copy it.

We copy the following file: /usr/share/icingaweb2/modules/director/contrib/systemd/icinga-director.service 
To: /etc/systemd/system/

The unit file definition looks like this.

``` bash
[Unit]
Description=Icinga Director - Monitoring Configuration
Documentation=https://icinga.com/docs/director/latest/
Wants=network.target

[Service]
EnvironmentFile=-/etc/default/icinga-director
EnvironmentFile=-/etc/sysconfig/icinga-director
ExecStart=/usr/bin/icingacli director daemon run
ExecReload=/bin/kill -HUP ${MAINPID}
User=icingadirector
SyslogIdentifier=icingadirector
Type=notify

NotifyAccess=main
WatchdogSec=10
RestartSec=30
Restart=always

[Install]
WantedBy=multi-user.target
```

This service has a dependency which needs to be installed.

``` bash
yum install rh-php71-php-posix
```

Then we need to reload the php-service.

``` bash
systemctl restart rh-php71-php-fpm.service
```

Finally since there is a new unit file we need to reload the daemon.

``` bash 
systemctl daemon-reload
```

Then enable and start the service!

``` bash
systemctl enable icinga-director.service
systemctl start icinga-director.service
```