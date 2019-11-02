### The api usage.

In order to complete this guide you either disable firewalld and not configure the IPtables or do the opposite.

If you want to disable the firewalld you do this.

``` bash
systemctl stop firewalld
```

Then you can issue your python calls.

``` python
import requests
from requests.auth import HTTPBasicAuth

authentication = HTTPBasicAuth('icingaweb2','Wijsn8Z9eRs5E25d')

requests.get(url = "https://centosa:5665/v1",verify = False, auth = authentication).text

requests.get(url = "https://centosa:5665/v1/status",verify = False, auth = authentication).text

requests.get(url = "https://centosa:5665/v1/objects/hosts",verify = False, auth = authentication).text
```

The first one checks if your authentication is working, the second gives you a status of your API, the third one gives you a list of configured hosts.

You need to keep in mind the CRUD concept to successfully script against the API itself.