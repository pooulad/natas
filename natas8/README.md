# natas8

Path traversal.

This page doesn’t validate the input page, and therefore is vulnerable to path traversal attack.

Remember that the introduction of Natas Wargame says All passwords are also stored in /etc/natas_webpass/ and named as /etc/natas_webpass/natasX. Thus, we can try to set the param page as /etc/natas_webpass/natas8 and find the password.

Note: Input /etc/natas_webpass/natas7 to find the password for this page itself. Other numbers would fail due to privilege limitation.

Password: DBfUBfqQG69KvJvJ1iAbMoIpwSNQ9bWe