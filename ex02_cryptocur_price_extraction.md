# Esercizio 2: recupero delle informazioni sulle cryptovalute - Soluzione

```py
from bs4 import BeautifulSoup
import requests

url = "https://coinmarketcap.com"
result = requests.get(url).text
doc = BeautifulSoup(result, "html.parser")

tbody = doc.tbody
trs = tbody.contents

prices = {}

for tr in trs[:15]:
    name, price = tr.contents[2:4]
    fixed_name = name.p.string
    fixed_price = price.span.string

    prices[fixed_name] = fixed_price

print(prices)
```
