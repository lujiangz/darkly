```
http://{IP_ADDRESS}/index.php?page=recover
```

## Discovery

After clicking the Sign-in button, we selected "Forgot my password". Notably, it didn't request any identifying information (username, email).

Investigating the page source revealed:

```html
<input type="hidden" name="mail" value="webmaster@borntosec.com" maxlength="15">
```

The form contained a hidden email field pre-populated with the administrator email address.

> By modifying this hidden field to our own email address and submitting the form, we could hijack the password reset process.

## Vulnerability Analysis

This implementation contains a critical security flaw in the password recovery mechanism. The target email address is:
- Hardcoded in the HTML
- Sent as a hidden field
- Easily modifiable by any client

## Impact

An attacker can:
- Intercept password reset functionality intended for administrators
- Redirect sensitive information to their own email address
- Potentially gain unauthorized access to the admin account

## Proper Security Implementation

To prevent this vulnerability:
- Never trust client-side data
- Validate user identity before initiating password resets
- Store sensitive information server-side in sessions
- Implement proper authentication checks
- Use CSRF tokens to protect form submissions
- Require verified user information before sending recovery emails
