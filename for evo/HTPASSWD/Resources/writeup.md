```
http://{IP_ADDRESS}/robots.txt
```
```
http://{IP_ADDRESS}/whatever/
```

### What is robots.txt?

robots.txt is a simple text file located in the root directory of a website that is used to instruct search engine bots (like Googlebot, Bingbot) which pages they can access or cannot access on the site.

### Contents of robots.txt
```
User-agent: *
Disallow: /whatever
Disallow: /.hidden
```

### What is .htpasswd?

.htpasswd file is used for basic authentication on web servers. It contains a username and password. Passwords are typically stored in a hashed format. To access a webpage or directory, the correct username and password must be provided.

### Contents of .htpasswd

| Username | Password Hash |
|----------|---------------|
| root     | 437394baff5aa33daa618be47b75cb49 |

We have a root user and a password.


```
 ~  hashid 437394baff5aa33daa618be47b75cb49                                    ok | took 40s | system node
Analyzing '437394baff5aa33daa618be47b75cb49'
[+] MD2
[+] MD5
[+] MD4
[+] Double MD5
[+] LM
[+] RIPEMD-128
[+] Haval-128
[+] Tiger-128
[+] Skein-256(128)
[+] Skein-512(128)
[+] Lotus Notes/Domino 5
[+] Skype
[+] Snefru-128
[+] NTLM
[+] Domain Cached Credentials
[+] Domain Cached Credentials 2
[+] DNSSEC(NSEC3)
[+] RAdmin v2.x
```

```
https://www.dcode.fr/cipher-identifier
```

You can use hashid or the site recommended by the school's snowcrash project. We can see that our password is hashed with md5.
We will decrypt the password using any md5 decrypter.

```
https://crackstation.net/
```

We choose this website.

| Hash | Type | Result |
|------|------|--------|
| 437394baff5aa33daa618be47b75cb49 | md5 | qwerty123@ |

We decrypted the password. Where will we use it?
We scanned our server with dirb.

```
GENERATED WORDS: 4612

---- Scanning URL: http://192.168.193.129/ ----
==> DIRECTORY: http://192.168.193.129/admin/
==> DIRECTORY: http://192.168.193.129/audio/
==> DIRECTORY: http://192.168.193.129/css/
==> DIRECTORY: http://192.168.193.129/errors/
+ http://192.168.193.129/favicon.ico (CODE:200|SIZE:1406)
==> DIRECTORY: http://192.168.193.129/fonts/
==> DIRECTORY: http://192.168.193.129/images/
==> DIRECTORY: http://192.168.193.129/includes/
+ http://192.168.193.129/index.php (CODE:200|SIZE:6892)
==> DIRECTORY: http://192.168.193.129/js/
+ http://192.168.193.129/robots.txt (CODE:200|SIZE:53)
==> DIRECTORY: http://192.168.193.129/whatever/
```

**==> DIRECTORY: http://192.168.193.129/admin/**

Let's go there and log in with:

```
username: root
password: qwerty123@
```

After logging in, we get our flag.
If we hadn't gotten the desired result from Dirb, we would have searched for the admin panel through other methods.


## Why this vulnerability occurs and its solution

Since robots.txt is a publicly accessible file, its content can be seen by anyone. The web application provides information to bots about directories and pages that might be confidential. This allows attackers to discover hidden content.

- Cause of Vulnerability: Directories like /whatever/ and /.hidden have been added to the robots.txt file, but they were assumed to be hidden. However, anyone can access these directories, and when their existence is known, attackers can explore them.

- In the .htpasswd file, usernames and passwords are stored in hashed form. However, MD5 is used as the encryption algorithm. MD5 is an insecure algorithm nowadays, and there are many online tools available to crack these hashes. This makes it easier for attackers to decrypt passwords.

## Solution

- It should be remembered that robots.txt is only valid for search engines. Directories with confidential content should not be specified in this file. The content of this file should be configured appropriately only for search engines. Adding hidden directories and files to the robots.txt file should be prevented. Only authorized users should be allowed to access such directories. If you want to prevent bots from learning about special directories, they should be carefully reviewed before being added to the robots.txt file.

- Access to the .htpasswd file should only be given to authorized personnel. This file should be configured correctly and should only be accessible by the server.
- Proper access control should be configured on the web server, and external access to the .htpasswd file should be prevented. Additionally, IP-based access controls and authentication should be added for hidden directories using server configurations.