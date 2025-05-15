## 🎯 Client-Side Select Manipulation Vulnerability

### 🧩 Vulnerable Code

    The following HTML code presents a drop-down list (`<select>`) that submits the form automatically when an option is selected:

```html
    <select name="valeur" onchange="javascript:this.form.submit();">
    <option value="1">1</option>
    <option value="2">2</option>
    ...
    <option value="10">10</option>
    </select>

```
### ⚠️ Vulnerability Description
    This implementation assumes that users will only select valid options (1 to 10). However, since this is purely client-side, an attacker can easily modify the HTML in the browser (using DevTools) and change the option values.

    
    For example:
```html
    <option value="1000">1</option>
```


    Now, when the user selects this option, the form sends valeur=1000 instead of a value between 1–10.

    If the server does not validate this value on the backend, it may lead to:

    Privilege escalation

    Score manipulation

    Unexpected behavior or logic bypass

    Flag leakage in CTF environments

### 🛡️ How to Prevent It
    Server-side validation is essential.

    Never trust input from the client.

    Verify that valeur is within the expected range (1 to 10).