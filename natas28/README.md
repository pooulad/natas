# natas28

Two solutions:

    Keep inserting data with username “natas28” and a password, as the database is cleared in 5 minutes, the insertion would succeed by chance. Then query with the username and password to get the real password.
    (Better way) Insert data with username (“natas28” + more than 64-7 times of space + “whatever”), and a password. Then query with “natas28” and the password. Then find the real password. (Source: http://r4stl1n.github.io/2014/12/30/OverTheWire-Natas25-28.html)

Password: JWwR438wkgTsNKBbcJoowyysdM82YjeF