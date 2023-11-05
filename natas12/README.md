# natas12

View source page and find that the data stored in our cookie is “encrypted” by xoring with a censored string $key. The default original data is json encoded array array("showpassword"=>"no", "bgcolor"=>"#ffffff");.

Thus, we could xor the original string with the encrypted text to get the $key:

```php
<?php
$originalString = json_encode(array( “showpassword”=>”no”, “bgcolor”=>”#ffffff”));
$cookieString = base64_decode(‘ClVLIh4ASCsCBE8lAxMacFMZV2hdVVotEhhUJQNVAmhSEV4sFxFeaAw=’);
echo $originalString ^ $cookieString;
```

The result is qw8Jqw8Jqw8Jqw8Jqw8Jqw8Jqw8Jqw8Jqw8Jqw8Jq, which implies that the $key should be qw8J (repeated during the xor_encrypt function).

Since we’ve got the $key, we can build our own data setting showpassword to yes:

```php
<?php
$key = ‘qw8J’;
$newString = json_encode(array( “showpassword”=>”yes”, “bgcolor”=>”#ffffff”));
$cookieData = ”;
for ($i = 0; $i < strlen($newString); $i++) {
$cookieData .= $key[$i % strlen($key)] ^ $newString[$i];
}
echo base64_encode($cookieData);
```

The result is ClVLIh4ASCsCBE8lAxMacFMOXTlTWxooFhRXJh4FGnBTVF4sFxFeLFMK. Alter the data in cookie with this string and reload the page to get the password.

Password: EDXp0pS26wLKHZy1rDBPUZk0RKfLGIR3