### Adding monitored instance

This guide shows you how you can add monitored instances.

Namely our infrastructure consists of 2 monitored hosts and an icinga2 instance.

What we need to make sure is to have a working name resolution.

I have edited the /etc/hosts file and added the three servers with their names and IP-s.

``` bash
192.168.56.2 centosa
192.168.56.3 centosb
192.168.56.5 2019A
```

Then we need to reconfigure the /etc/icinga2/conf.d/hosts.conf file and add these entries.

``` bash
object Host "centosb" {
        import "generic-host"
	address = "centosb"
	vars.os = "Linux"
}

object Host "2019A" {
	import "generic-host"
	address = "2019A"
	vars.os = "Windows"
}
```

These changes were reflected on the Web UI after I issued the command.

``` bash
systemctl restart icinga2
```
