# python 爬虫

## requests

```python
import requests
url = "https://www.baidu.com"
kw = input("search words : ")
param = {
    "wd" : kw
}

headers = {
    "User-Agent" : "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.0.0 Safari/537.36"
}

resp = requests.get(url=url, params=param, headers=headers)
# resp = requests.get(url=url)

with open("test.html", 'w', encoding='utf-8') as fp:
    fp.write((resp.text))

print(resp.text)
```
