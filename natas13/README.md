# natas13

View source page and find that the extension of the file uploaded is extracted from $_POST["filename"]. Thus, upon uploading a file, we can set the filename as any string ending with .php, and upload a php file.

In the php file, we can execute the following code:
echo exec("cat /etc/natas_webpass/natas13");

After we upload the php file, we can open that page and get the password.

Password: jmLTY0qiPZBbaKc9341cqPQZBJv7MQbY