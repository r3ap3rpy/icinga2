### Setting up icinga director

We have a total of 3 dependencies which need to be satisfied in order to start using the director.

First we install the director module itself.

## Director

We need a DB where the module will live.

``` bash
mysql -u root -p -e "CREATE DATABASE director CHARACTER SET 'utf8';
   CREATE USER director@localhost IDENTIFIED BY 'password';
   GRANT ALL ON director.* TO director@localhost;"
```

On the web UI we need to add a new resource.

You need to create a database resource.

Then we can install the module with GIT

``` bash
ICINGAWEB_MODULEPATH="/usr/share/icingaweb2/modules"
REPO_URL="https://github.com/icinga/icingaweb2-module-director"
TARGET_DIR="${ICINGAWEB_MODULEPATH}/director"
MODULE_VERSION="1.7.1"
git clone "${REPO_URL}" "${TARGET_DIR}" --branch v${MODULE_VERSION}
```

Finally enable it.

``` bash
icingacli module enable director
```

## IPL 

This is one dependency module, we install it with git.

``` bash 
MODULE_NAME=ipl
MODULE_VERSION=v0.4.0
REPO="https://github.com/Icinga/icingaweb2-module-${MODULE_NAME}"
MODULES_PATH="/usr/share/icingaweb2/modules"
git clone ${REPO} "${MODULES_PATH}/${MODULE_NAME}" --branch "${MODULE_VERSION}"
```

Then enable the module.

``` bash
icingacli module enable ipl
```

## Incubator

Another dependency module. We install it with git.

``` bash
MODULE_NAME=incubatorÖÜ
REPO="https://github.com/Icinga/icingaweb2-module-${MODULE_NAME}"
MODULES_PATH="/usr/share/icingaweb2/modules"
git clone ${REPO} "${MODULES_PATH}/${MODULE_NAME}" --branch "${MODULE_VERSION}"
```

Then enable it.

``` bash
icingacli module enable incubator
```

## Reactbundle

Our final dependency.

``` bash
MODULE_NAME=reactbundle
MODULE_VERSION=v0.7.0
REPO="https://github.com/Icinga/icingaweb2-module-${MODULE_NAME}"
MODULES_PATH="/usr/share/icingaweb2/modules"
git clone ${REPO} "${MODULES_PATH}/${MODULE_NAME}" --branch "${MODULE_VERSION}"
```

This also needs to be enabled.

``` bash
icingacli module enable reactbundle
```

## Config

Icinga director needs a fresh start and you need to either manually configure the hosts and checks and stuff, or use the API.

In order to add a new host:
 - We need to define our command.
 - We need to define a hostgroup.
 - Create a host template.
 - Add the new host.