### Where the things at 

This guide tells you about a simple addition of two extra hosts.

One is linux the other is  windows.

We need to edit the /etc/hosts file to provide name resolution.

``` bash 
192.168.56.2 centosa
192.168.56.3 centosb
192.168.56.5 2019A
```

Then we need to edit the /etc/icinga2/conf.d/hosts.conf file to add the new hosts.

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
