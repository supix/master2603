# Scrapy

Scrapy è un framework free e open-source scritto in Python. Originariamente progettato per il web scraping, può essere utilizzato anche per accedere a API o come web crawler.

L'architettura di Scrapy è basata su dei componenti denominati _spider_, che sono crawler autocontenuti creati specificando l'insieme di istruzioni che devono eseguire. Benché gli intenti di Scrapy e Beutiful Soup, Scrapy è una soluzione più ingegnerizzata e scalabile. Per questo risulta adatta a eseguire attività di web scraping di grandi dimensioni.

La documentazione ufficiale di Scrapy è disponibile su https://docs.scrapy.org

## Installazione

Per installare Scrapy nel virtual environment già configurato e attivato, digitare il seguente comando in console.

```sh
pip install Scrapy
```

oppure, alternativamente:

```sh
python -m pip install Scrapy
```

Spostiamoci nella cartella in cui sono stati copiati i binary del prodotto Scrapy a valle del comando di installazione. Al posto di `{venv}` sostituire il nome del virtual environment Python utilizzato.

```sh
cd ./{venv}/Scripts
```

Per verificare che Scrapy è correttamente installato, digitiamo il comando `.\scrapy` in console.

Vedremo qualcosa del tipo seguente.

```sh
Scrapy 2.14.2 - active project: bookscraper

Usage:
  scrapy <command> [options] [args]

Available commands:
  bench         Run quick benchmark test
  check         Check spider contracts
  crawl         Run a spider
  edit          Edit spider
  fetch         Fetch a URL using the Scrapy downloader
  genspider     Generate new spider using pre-defined templates
  list          List available spiders
  parse         Parse URL (using its spider) and print the results
  runspider     Run a self-contained spider (without creating a project)
  settings      Get settings values
  shell         Interactive scraping console
  startproject  Create new project
  version       Print Scrapy version
  view          Open URL in browser, as seen by Scrapy

Use "scrapy <command> -h" to see more info about a command
```

## Creazione di un progetto

Per creare un progetto Scrapy si usa il seguente comando.

```sh
scrapy startproject mytutorial
```

Dopo l'esecuzione del comando, una nuova cartella dal nome `mytutorial` verrà automaticamente creata nel nostro progetto. Possiamo navigarla per visualizzarne il contenuto.

Entriamo nella cartella `mytutorial`

Il sito su cui lavoreremo è https://quotes.toscrape.com/

Visualizziamolo nel browser e analizziamone a grandi linee la struttura.

Creiamo ora uno scraper con il seguente comando lanciato nella console.

```sh
cd mytutorial
scrapy genspider Quote http://quotes.toscrape.com/
```

Il nuovo file `quote.py` verrà creato nella cartella `spiders`.

Con il comando `scrapy list` è possibile visualizzare gli spiders esistenti.

Analizziamo il file `quote.py`.

```py
import scrapy


class QuoteSpider(scrapy.Spider):
    name = "Quote"
    allowed_domains = ["quotes.toscrape.com"]
    start_urls = ["http://quotes.toscrape.com/"]

    def parse(self, response):
        pass
```

Gli attributi `name`, `allowed_domains` e `start_urls` hanno dei nomi convenzionali che configurano il comportamento dello spider. Anche il metodo `parse(self, response)` viene automaticamente invocato dal framework Scrapy e non deve essere direttamente chiamato dal programmatore.

Proviamo a implementare questo metodo con una semplice istruzione.

```py
    def parse(self, response):
        yield { "response": response }
```

Per eseguire lo spider si digita il seguente comando sulla console.

```sh
scrapy crawl Quote -o output.json
```

Viene creato un file `output.json` nella root del progetto.

```json
[
  {"response": "<HtmlResponse 200 http://quotes.toscrape.com/>"}
]
```

Possiamo anche restituire altre proprietà dell'oggetto `response`.

```py
    def parse(self, response):
        yield { 
            "url": response.url,
            "status": response.status,
        }
```

Dopo l'esecuzione dello spider il file `output.json` appare come di seguito mostrato.

```json
[
{"response": "<HtmlResponse 200 http://quotes.toscrape.com/>"}
][
{"url": "http://quotes.toscrape.com/", "status": 200}
]
```

Ogni esecuzione pertanto _accoda_ l'output nel file.

## Uso della shell Scrapy

Entriamo ora nella shella Scrapy con il comando `scrapy shell`.

In questa shell digitiamo il comando seguente.

```sh
>>> fetch("http://quotes.toscrape.com/")
```

Questo comando produce l'accesso al contenuto web da parte di Scrapy e la sua memorizzazione locale. E' ora possibile interrogare velocemente le proprietà dell'oggetto `response`, digitando per esempio i seguenti comandi nella Scrapy shell.

```py
>>> response
<200 http://quotes.toscrape.com/>
>>> response.url
'http://quotes.toscrape.com/'
>>> response.status
200
>>> response.body
b'<!DOCTYPE html>\n<html lang="en">\n<head>\n\t<meta charset="UTF-8">\n\t<title>Quotes to Scrape</title>...
...
```

## Accesso ai tag dell'HTML

Se si vuole accedere a specifici tag presenti nel body dell'HTML si utilizza la seguente sintassi.

```py
>>> response.css("a").get()
>>> response.css("a").getall()
```

La prima forma consente di accedere alla prima occorrenza del tag specificato. La seconda forma restituisce un array contenente tutte le occorrenze presenti nel HTML.

Il risultato della prima invocazione è il seguente.

```py
'<a href="/" style="text-decoration: none">Quotes to Scrape</a>'
```

Nel caso si voglia accedere al testo del link (o comunque al testo contenuto all'interno del tag) si usa la seguente sintassi.

```py
>>> response.css("a::text").get()
'Quotes to Scrape'
```

Per accedere a un attributo di un tag, per esempio l'attributo `href` del precedente tag `a`, si opera come segue.

```py
>>> response.css("a::attr(href)").get()
'/'
```

E' possibile anche accedere a tutti i valori degli attributi, contemporaneamente.

```py
>>> response.css("a::attr(href)").getall()
['/', '/login', '/author/Albert-Einstein', '/tag/change/page/1/', '/tag/deep-thoughts/page/1/', '/tag/thinking/page/1/', '/tag/world/page/1/', '/author/J-K-Rowling', '/tag/abilities/page/1/', '/tag/choices/page/1/', '/author/Albert-Einstein', '/tag/inspirational/page/1/', '/tag/life/page/1/', '/tag/live/page/1/', '/tag/miracle/page/1/', '/tag/miracles/page/1/', '/author/Jane-Austen', '/tag/aliteracy/page/1/', '/tag/books/page/1/', '/tag/classic/page/1/', '/tag/humor/page/1/', '/author/Marilyn-Monroe', '/tag/be-yourself/page/1/', '/tag/inspirational/page/1/', '/author/Albert-Einstein', '/tag/adulthood/page/1/', '/tag/success/page/1/', '/tag/value/page/1/', '/author/Andre-Gide', '/tag/life/page/1/', '/tag/love/page/1/', '/author/Thomas-A-Edison', '/tag/edison/page/1/', '/tag/failure/page/1/', '/tag/inspirational/page/1/', '/tag/paraphrased/page/1/', '/author/Eleanor-Roosevelt', '/tag/misattributed-eleanor-roosevelt/page/1/', '/author/Steve-Martin', '/tag/humor/page/1/', '/tag/obvious/page/1/', '/tag/simile/page/1/', '/page/2/', '/tag/love/', '/tag/inspirational/', '/tag/life/', '/tag/humor/', '/tag/books/', '/tag/reading/', '/tag/friendship/', '/tag/friends/', '/tag/truth/', '/tag/simile/', 'https://www.goodreads.com/quotes', 'https://www.zyte.com']
```

## Utilizzare il metodo `parse()` per recuperare i dati

Fino a ora abbiamo recuperato dati dal web attraverso l'uso della Scrapy shell. Questo è utile quando si vuole sperimentare l'uso di comandi per individuare l'esatto contenuto da recuperare. Una volta conclusa questa fase, i comandi devono essere inseriti nella classe spider, così da eseguirle in modalità automatizzata, e non interattiva.

Ecco un esempio di metodo `parse()` per recuperare tutti i tag `a` e i relativi attributi `href` dalla pagina.

```py
def parse(self, response):
    for link_href in response.css("a::attr(href)"):
        yield {
            "href": link_href.get()
        }
```

Questa versione del metodo `parse()` può essere eseguita mediante il seguente comando a console.

```sg
scrapy crawl Quote -o output.json
```

L'esecuzione produce il file `output.json` che contiene quanto richiesto.

```py
[
{"href": "/"},
{"href": "/login"},
{"href": "/author/Albert-Einstein"},
{"href": "/tag/change/page/1/"},
{"href": "/tag/deep-thoughts/page/1/"},
{"href": "/tag/thinking/page/1/"},
{"href": "/tag/world/page/1/"},
{"href": "/author/J-K-Rowling"},
{"href": "/tag/abilities/page/1/"},
...
]
```

## Filtrare i tag per classe

E' possibile associare una classe a un tag, come nell'esempio seguente.

```html
<small class="author" itemprop="author">Albert Einstein</small>
```

Il questo caso, al tag `small` è associata la classe `author`.

Per selezionare i tag a cui è associata questa classe, e prelevarne poi il testo contenuto (che è il nome dell'autore), la sintassi che si utilizza è la seguente.

```sh
>>> response.css("small.author::text").getall()
```

Il risultato è il seguente.

```py
['Albert Einstein', 'J.K. Rowling', 'Albert Einstein', 'Jane Austen', 'Marilyn Monroe', 'Albert Einstein', 'André Gide', 'Thomas A. Edison', 'Eleanor Roosevelt', 'Steve Martin']
```

## Filtrare i tag da estrarre in base ad attributi

Vogliamo ora estrarre solo un sottoinsieme dei tag `a` e dei relativi attributi `href`. Quelli di nostro interesse sono quelli che conducono a un autore.

Dall'HTML possiamo notare che i tag che conducono a un autore, tra tutti quelli esistenti, sono quelli che iniziano per `/author/`.

In questo caso la sintassi da utilizzare è la seguente.

```sh
>>> response.css("a[href^='/author/']::attr(href)").getall()
```

L'uso delle parentesi quadre `[]` serve proprio a indicare un filtro sui risultati individuati dal selettore `a` e indica _tutti i selettori_ `a` _il cui attributo_ `href` _inizia per_ `/author/`.

## Esercizio 3: estrarre tutte le citazioni e tutti gli autori dalla pagina web

Si chiede di scrivere un metodo `parse()` capace di estrarre tutti i testi delle citazioni e i relativi autori dalla pagina web. Restituire il risultato nella forma di una lista di dizionari.

Consultare la [documentazione di Scrapy](https://docs.scrapy.org) qualora vi sia la necessità di utilizzare funzionalità non ancora mostrate.

Fai [click qui](ex03_quote_extraction.md) per verificare la tua soluzione.