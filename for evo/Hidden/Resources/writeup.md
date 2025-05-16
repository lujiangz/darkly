```
http://{IP_ADDRESS}/robots.txt
```
```
http://{IP_ADDRESS}/.hidden/
```

>When we open the URL, we see 26 randomly named folders + a README file.
>
>Reading the README, we encounter the message **"Tu veux de l'aide ? Moi aussi !"** which means "Do you want help? Me too!"
>
>As we explore these randomly named folders, we notice that each subfolder contains a README file with different content.
>
>Therefore, our goal is to scan through all README files to find a keyword like "my flag is" to obtain the flag.


```
#!/usr/bin/env python3
import requests
from bs4 import BeautifulSoup
import re

base_url = "http://{IP_ADDRESS}/.hidden/"
visited = set()
flagged = []

session = requests.Session()
session.mount("http://", requests.adapters.HTTPAdapter(max_retries=3))

def scan(url):
    if url in visited:
        return
    visited.add(url)

    try:
        print(f"Taranıyor: {url}")
        r = session.get(url, timeout=5)
        if r.status_code != 200:
            return

        if url.endswith("README"):
            matches = [line for line in r.text.splitlines() if "flag" in line.lower()]
            if matches:
                print(f"\n✅ FLAG HERE: {url}")
                print("📦 CONTENT:\n" + "\n".join(matches) + "\n")
                flagged.append((url, matches))

        for a in BeautifulSoup(r.text, 'html.parser').find_all('a'):
            href = a.get('href')
            if href and not href.startswith('?') and href != '../':
                next_url = url.rstrip('/') + '/' + href
                if next_url.startswith(base_url):
                    scan(next_url)
    except Exception as e:
        print(f"Error: {url} - {str(e)[:100]}")

if __name__ == "__main__":
    print("🔍 start searching...\n")
    scan(base_url)

    print(f"\n🎯 {len(flagged)} find 'flag' in README.")
    with open("flagged_readmes.txt", "w", encoding="utf-8") as f:
        for url, lines in flagged:
            f.write(f"URL: {url}\nFLAG LINES:\n{chr(10).join(lines)}\n{'-'*50}\n")
```

>This script can be described as a simple scraper that scans all folders, including all subfolders, and stops when it finds a URL containing "flag" within the content of any file named "README".


```
URL: http://192.168.193.129/.hidden/whtccjokayshttvxycsvykxcfm/igeemtxnvexvxezqwntmzjltkt/lmpanswobhwcozdqixbowvbrhw/README
FLAG LINES:
Hey, here is your flag : d5eec3ec36cf80dce44a896f961c1831a05526ec215693c8f2c39543497d4466
--------------------------------------------------
```