# natas31

SQL injection via quote() with list argument

As this and this said, the quote() takes list argument and parse the second item as an option parameter to indicate the type of first item. If the type is non-string, it will return the value of first without any quoting. With the help of CGI that passes two URL parameters as a list argument, we can put the injection string as the first parameter and 4 (to indicate the type as SQL_INTEGER as the second parameter. SQL DATA TYPE CODES

An example of POST request body that works:

```bash
username=abcd&password=’abcd’ or 1=1&password=2
```

(2 refers to SQL_NUMERIC)

Password: hay7aecuungiuKaezuathuk9biin0pu1
