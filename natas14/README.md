# natas14

View the source page and we can find that this challenge is an upgraded version of Natas 12. In this challenge, the EXIF image type is checked, so we need to add a header to our php file and make it look like a JPEG file.

We can create a php file like this:

\0xFF\0xD8\0xFF\0xE0<?php echo exec('cat /etc/natas_webpass/natas14');?>

Note: The JPEG header above contains four characters in HEX format.

Upload this php using the similar way in Natas 12 and get the password.

Password: Lg96M10TdfaPyVBkJdjymbllQ5L6qdl1