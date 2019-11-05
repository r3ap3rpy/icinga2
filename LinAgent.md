### Setting up the linux agent

In order to setup the agent we need to generate a ticket on our master.

``` bash
icinga2 pki ticket --cn 'centosb' --salt whatever
```

Then on our icinga2 client where the agent is setup we need to first setup the reposistories that allow icinga2 to be installed.

``` bash
yum install https://packages.icinga.com/epel/icinga-rpm-release-7-latest.noarch.rpm -y 
yum install epel-release -y
```

Then we can install and enable and finally start icinga2.

``` bash
yum install icinga2 -y
systemctl start icinga2 
systemctl enable icinga2
```

Once this is done we need to run the node wizard.

``` bash
icinga2 node wizard
```

After answering the obvious questions we should restart the service.

``` bash
systemctl restart icinga2
```

On our master we need to add these minimum to the hosts.conf under the /etc/icinga2/conf.d/

``` bash
object Zone "centosb" {
  endpoints = [ "centosb" ]
  parent = "centosa"
}
object Endpoint "centosb" {
   host = "192.168.56.3"
}
object Host "centosb" {
   import "generic-host"
   address = "192.168.56.3"
vars.client_endpoint = "centosb" }
```

Then we need to restart icinga2 and see on the web ui that the new agent is communicating.

``` bash
systemctl restart icinga2
```