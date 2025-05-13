```
http://{IP_ADDRESS}/?page=b7e44c7a40c5f80139f0a50f3650fb2bd8d00b0d24667c4c2ca32c88e13b758f
```

When we inspect the page, we find the following comments:

```html
<!--
Voila un peu de lecture :

Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.
-->

<!-- 
Fun right ?
source: loem.
Good bye  !!!!
-->
```

> You must come from : "https://www.nsa.gov/".

```html
<!--
Where does it come from?
Contrary to popular belief, Lorem Ipsum is not simply random text. It has roots in a piece of classical Latin literature from 45 BC, making it over 2000 years old. Richard McClintock, a Latin professor at Hampden-Sydney College in Virginia, looked up one of the more obscure Latin words, consectetur, from a Lorem Ipsum passage, and going through the cites of the word in classical literature, discovered the undoubtable source. Lorem Ipsum comes from sections 1.10.32 and 1.10.33 of "de Finibus Bonorum et Malorum" (The Extremes of Good and Evil) by Cicero, written in 45 BC. This book is a treatise on the theory of ethics, very popular during the Renaissance. The first line of Lorem Ipsum, "Lorem ipsum dolor sit amet..", comes from a line in section 1.10.32.

The standard chunk of Lorem Ipsum used since the 1500s is reproduced below for those interested. Sections 1.10.32 and 1.10.33 from "de Finibus Bonorum et Malorum" by Cicero are also reproduced in their exact original form, accompanied by English versions from the 1914 translation by H. Rackham.
-->
```

And this text:

> Let's use this browser : "ft_bornToSec". It will help you a lot.

<!--																																																																													
```
-->

## Vulnerability Analysis

### What is this vulnerability?

The application appears to control access based on client-controlled HTTP headers:

*   **Referer header**: Indicated by the comment "You must come from : `https://www.nsa.gov/`".
*   **User-Agent header**: Indicated by the text "Let's use this browser : `ft_bornToSec`".

### How to exploit this vulnerability

To bypass these checks, you need to craft an HTTP request that includes these specific headers with the required values. This can grant access to a hidden page, trigger specific behavior, or complete a challenge.

#### Using `curl`

Here's how you could do it using `curl` (a command-line tool for transferring data with URLs):

```bash
curl -H "Referer: https://www.nsa.gov/" \
     -H "User-Agent: ft_bornToSec" \
     "http://{IP_ADDRESS}/?page=b7e44c7a40c5f80139f0a50f3650fb2bd8d00b0d24667c4c2ca32c88e13b758f"
```

#### Other Methods

You can also use:

*   **Browser Developer Tools**: Most modern browsers allow you to modify and resend network requests. You can capture a request, edit the headers, and then resend it.
*   **Browser Extensions**: Extensions like "ModHeader" or "Modify Header Value" can be used.
*   **Scripting Languages**: Python (with libraries like `requests`), Node.js, or other languages can be used to send custom HTTP requests with any headers you need.

## Exploitation Steps

We used a browser extension to modify the headers as follows:

| URL                                                                                             | Header Name | Header Value           |
| :---------------------------------------------------------------------------------------------- | :---------- | :--------------------- |
| `http://192.168.193.129/?page=b7e44c7a40c5f80139f0a50f3650fb2bd8d00b0d24667c4c2ca32c88e13b758f` | User-Agent  | `ft_bornToSec`         |
| `http://192.168.193.129/?page=b7e44c7a40c5f80139f0a50f3650fb2bd8d00b0d24667c4c2ca32c88e13b758f` | Referer     | `https://www.nsa.gov/` |

Then we visited the website and refreshed the page (F5) to get the flag.