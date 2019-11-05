### Windows agent setup

In order to install the windows agent we need to download the appropriate package from [here](http://packages.icinga.com/windows/)

![DBRoot](/pics/iciwin.PNG)

After this is done you need to copy it to the windows machine and start the installer.

If you cannot download it add the url to the trusted sites

![Trusted](/pics/icitrust.PNG)

The installer will ask you for a ticket which you need to generate on the master with this command.

``` bash
icinga2 pki ticket --cn 'centosb' --salt whatever
```

On the windows node follow these during install.

Select a default folder.

![Folder](/pics/icifolder.PNG)

Run the wizard.

![Folder](/pics/iciwiz.PNG)

Select these settings.

![Folder](/pics/iciwiz2.PNG)

Once this is done you will see the new node in your web ui!