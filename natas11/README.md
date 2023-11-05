# natas11

Comparing to Natas 9, this challenge filters the key characters to command injection. However, we could take advantage of the grep command to print the password.

Note that the command line grep -i <word> <file 1> <file 2> ... <file n> will print every line in each file that contains the . Thus, we could compile such command and go through the 26 letters and their capitalized format to find a ‘matched letter’ to the password.

For this challenge, we could input c /etc/natas_webpass/natas11 and print the password out.

Password: U82q5TCMMQ9xuFoI3dYX61s7OZD9JKoK