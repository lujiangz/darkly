```
http://{IP_ADDRESS}/
```

When we just inspect and check the cookies;

```I_am_admin:68934a3e9455fa72420237eb05902327```

As soon as we looked, we understood that it was md5, but let's check anyway.

```
 ~  hashid 68934a3e9455fa72420237eb05902327

Analyzing '68934a3e9455fa72420237eb05902327'
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
https://www.dcode.fr/md5-hash
```
Let's decrypt and analyze it.

```
68934a3e9455fa72420237eb05902327 = false
```

Let's simply write **true** and hash it with md5.

```
b326b5062b2f0e69046810717534cb09 = true
```

And we press F5 and get our flag.

---

# Possible problems

1. Access restricted administration panels and functionality
2. Modify site content or configuration
3. View sensitive user data
4. Execute privileged operations intended only for administrators
5. Potentially gain further access to backend systems

# Solution

### 1. Implement Server-Side Session Management (Recommended)

The most secure way to handle user roles and permissions is on the server-side.

*   **Authentication:** When a user successfully authenticates as an administrator, the server should create a session for that user.
*   **Session State:** Store the admin status (e.g., `isAdmin: true`) within the server-side session data, associated with a unique, cryptographically strong, and random session ID.
*   **Session Cookie:** Send this session ID to the client as an HTTP-only, secure cookie. This cookie should not directly contain the admin status, only the identifier for the server-side session.
*   **Authorization:** On subsequent requests, the server uses the session ID from the cookie to retrieve the user's session data and verify their admin status. The `I_am_admin` cookie should be completely removed.

**Why this is better:**
*   The admin status is managed and verified entirely on the server, which is a trusted environment.
*   The client only holds an opaque session identifier, which is difficult to guess or forge if generated correctly.
*   It prevents attackers from simply changing a value in their browser to gain admin rights.

### 2. Use Cryptographically Signed Cookies (If Server-Side Sessions Are Not Feasible)

If you absolutely must store a flag like this in a cookie (which is generally discouraged for authorization), it **must** be protected against tampering. Hashing alone is insufficient, as demonstrated.

*   **Signing:** Instead of just hashing "true" or "false", the server should create a cookie that includes the value and a cryptographic signature (e.g., using HMAC-SHA256) generated with a secret key known only to the server.
    *   Example: `cookie_value = "is_admin=true"`
    *   `signature = HMAC_SHA256(cookie_value, server_secret_key)`
    *   The cookie sent to the client might look like: `admin_data="is_admin=true"; admin_signature="<signature_value>"`
*   **Verification:** On each request, the server must recompute the signature for the received `admin_data` using its secret key and compare it with the received `admin_signature`. If they don't match, or if the signature is missing, the cookie is considered tampered, and access should be denied.

**Important Considerations for Signed Cookies:**
*   **Secret Key Management:** The secret key must be strong, kept confidential, and rotated periodically.
*   **Algorithm Strength:** Use strong, modern cryptographic algorithms.
*   **Replay Attacks:** This method doesn't inherently protect against replay attacks if the cookie value itself doesn't change (e.g., if a valid admin cookie is stolen and reused). Session management handles this better with session expiration.
*   **Confidentiality:** Signing provides integrity and authenticity, but not confidentiality. If the cookie data is sensitive, it should also be encrypted.

### Key Security Principles to Apply:

*   **Never Trust Client-Side Input for Authorization:** All authorization decisions must be made and enforced on the server-side.
*   **Defense in Depth:** Implement multiple layers of security.
*   **Principle of Least Privilege:** Users should only have the permissions necessary to perform their tasks.