```
http://{IP_ADDRESS}/
```

There are some links on the homepage that create redirects. For example, when we go to the bottom of the site, we can see "Facebook, Twitter and Instagram" icons.
When you hover your mouse cursor over the icon or inspect the source code of the website, you will see the following.

```
<li><a href="index.php?page=redirect&site=facebook" class="icon fa-facebook"></a></li>
<li><a href="index.php?page=redirect&site=twitter" class="icon fa-twitter"></a></li>
<li><a href="index.php?page=redirect&site=instagram" class="icon fa-instagram"></a></li>
```

When we change the redirect address here.

For example, if we click on the Facebook icon, we get redirected to:

```
http://192.168.193.129/index.php?page=redirect&site=facebook
```

```
http://192.168.193.129/index.php?page=redirect&site=IMSOFKINGOOD
```

If you type this style or any url address you want, you will get a flag.


## Vulnerability: Unvalidated Redirects

This vulnerability exists because the application doesn't properly validate the "site" parameter in the redirect functionality. When a user clicks on a social media icon, the application takes the value of the "site" parameter and performs a redirect without validating or sanitizing the input.

### Why this is dangerous:

1. **Phishing attacks**: An attacker could craft a malicious URL that redirects users to a fake website that looks legitimate, potentially stealing credentials or personal information.

2. **Trust exploitation**: Since the redirect starts from a trusted domain (the website itself), users might be more likely to trust where they're being redirected.

### Best practices for prevention:

1. Implement a whitelist of allowed redirect destinations
2. Avoid using user-controllable input for redirect destinations
3. If redirects to external sites are necessary, use an intermediary confirmation page
4. Always validate and sanitize user input parameters used in redirects
