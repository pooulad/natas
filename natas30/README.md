# natas30

Perl RCE via open(). Note this is not a traditional command injection.

In perl open() function, the filename can be interpreted as a command. The pipe character | can be used to “trigger” this mode. Reference.

The file parameter in this page can be injected and we can get RCE using stuff like |id%00 or |id%26%26false%26%26.

Note that the string natas will be sanitized by this page, so we need to use some tweaks here. One possible solution:

```bash
http://natas29.natas.labs.overthewire.org/index.pl?file=|cat /etc/na“tas_webpass/na“tas30%00
```

Password: wie9iexae0Daihohv8vuu3cei9wahf0e