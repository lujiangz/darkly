```
http://{IP_ADDRESS}/?page=
```

    Testing for vulnerability:
    - First attempt: `?page=../etc/passwd` → Result: "WTF?" alert
    - Continued attempts: Added more `../` sequences to traverse higher in directory structure
    - Success: After multiple directory traversals, we found the working payload

## Successful Exploit
```
http://192.168.193.129/?page=../../../../../../../etc/passwd
```
This URL successfully retrieved the flag.

## Vulnerability Explanation
The application fails to properly sanitize user input in the `page` parameter. By using `../` sequences (which mean "go up one directory" in filesystem navigation), an attacker can escape the intended directory and access sensitive system files.

In this case, we accessed `/etc/passwd`, a critical system file that contains user account information on Unix-based systems. The need for multiple `../` sequences indicates the application attempts some protection but fails to implement it correctly.

## Security Impact
This vulnerability could allow attackers to:
- Read sensitive configuration files
- Access credentials and other confidential information
- Potentially gain further system access
