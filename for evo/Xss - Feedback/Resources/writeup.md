```
http://{IP_ADDRESS}/index.php?page=feedback
```

When we analyze the page;

```html
<form method="post" name="guestform" onsubmit="return validate_form(this)">
    <table width="50%" border="0" cellpadding="2" cellspacing="1">

        <tbody><tr style="background-color:transparent;border:none;">
            <td width="110">Name *</td>
            <td> <input name="txtName" type="text" size="30" maxlength="10"> </td>
        </tr>

        <tr style="background-color:transparent;border:none;">
            <td width="110">Message *</td>
            <td> <textarea name="mtxtMessage" cols="50" rows="3" maxlength="50"></textarea> </td>
        </tr>

        <tr style="background-color:transparent;border:none;">
            <td width="110">&nbsp;</td>
            <td><input name="btnSign" type="Submit" value="Sign Guestbook" onclick="return checkForm();"></td>
        </tr>

    </tbody></table>

</form>
```

When we look at the source code side, we see that the **checkForm** function is not there. Additionally, when we came to the Console, we also tested with checkForm() and saw that this function was not present. We already knew that there was an XSS vulnerability on the site because we had seen it while doing my first tests on the member side. However, since the flag is on this page, we will perform XSS here.

Simply, we write **<script>alert(1)</script>** in the name part. We get the flag.

---

## How to fix XSS vulnerability

1. **Sanitize User Input**
    - Use functions like `strip_tags()` and `htmlspecialchars()` in PHP to remove or escape dangerous input.
2. **Validate Input**
    - Implement server-side and client-side validation for all fields.
3. **Use HttpOnly Cookies**
    - Set cookies with the `HttpOnly` flag to prevent JS access.
4. **Content Security Policy (CSP)**
    - Use a CSP header to restrict executable sources.
5. **Modern Frameworks**
    - Use frameworks with built-in XSS protections.
6. **Web Application Firewall (WAF)**
    - Deploy a WAF to block common XSS attack patterns.