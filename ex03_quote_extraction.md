## Esercizio 3: estrarre tutte le citazioni e tutti gli autori dalla pagina web - Soluzione

```py
def parse(self, response):
    for quote in response.css("div.quote"):
        yield {
            "text": quote.css("span.text::text").get(),
            "author": quote.css("span small.author::text").get()
        }

        # extra 
        next_page = response.css('li.next a::attr("href")').get()
        if next_page is not None:
            yield response.follow(next_page, self.parse)
```

[Torna indietro](scrapy.md)