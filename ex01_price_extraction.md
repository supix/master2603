# Esercizio: estrarre il prezzo di un articolo da una pagina web

Scarichiamo la pagina dell'articolo.

```py
url = "https://www.newegg.ca/asus-tuf-gaming-tuf-rtx3080ti-o12g-gaming-geforce-rtx-3080-ti-12gb-graphics-card-triple-fans/p/N82E16814126509"

result = requests.get(url)
print(result.text)
```

Eseguendo questo codice si ottiene un lungo output in formato HTML, che rappresenta la pagina dell'articolo.

N.B.: il sito in questione consente di avere accesso all'HTML attraverso l'uso di uno script. Non tutti i siti lo consentono, avendo delle _bot protection_. Quando si utilizzano strumenti automatici di analisi del web, bisogna fare attenzione a non violare policy e a rispettare eventuali direttive richieste dal produttore del sito. Per esempio, alcune di queste direttive sono espressamente contenute in un file dal titolo `robots.txt` presente nella radice del sito web, ed è il primo file che un web crawler dovrebbe analizzare per guidare la scansione.

### Elaborare i contenuti in rete con Beautiful Soup

Ora che abbiamo il contenuto della pagina, analizziamolo con Beautiful Soup. Modifichiamo il codice precedente come segue.

```py
url = "https://www.newegg.ca/asus-tuf-gaming-tuf-rtx3080ti-o12g-gaming-geforce-rtx-3080-ti-12gb-graphics-card-triple-fans/p/N82E16814126509"

result = requests.get(url)
doc = BeautifulSoup(result.text, "html.parser")
print(doc.prettify())
```

L'esecuzione del codice, di nuovo, mostrerà l'HTML della pagina. Questa volta però il contenuto sarà stato elaborato e mostrato in una forma correttamente indentata.

### Come estrarre il prezzo dell'articolo

Vogliamo ora scrivere del codice utile a estrarre il prezzo dell'articolo dalla pagina.

Un primo modo di risolvere il problema consiste nel cercare il simbolo del dollaro nella pagina e recuperare il costo immediatamente successivo. Eccone il codice.

```py
prices = doc.find_all(string="$")
print(prices)
```

L'esecuzione di questo codice produrrà in output qualcosa di simile al contenuto seguente.

```py
['$']
```

Abbiamo trovato un'occorrenza del simbolo del dollaro. Ma ciò non è sufficiente, poiché il prezzo non è visibile in questo modo.

Possiamo usare la proprietà `parent` per accedere al tag in cui il simbolo del dollaro è contenuto.

```py
dollar_sign = doc.find_all(string="$")[0]
parent = dollar_sign.parent
print(parent)
```

Ecco l'output.

```html
<div class="price-current"><span class="price-current-label"></span>$<strong>2,260</strong><sup>.88</sup></div>
```

Per accedere puntualmente al prezzo, pertanto, può essere possibile usare il seguente codice.

```py
dollar_sign = doc.find_all(string="$")[0]
parent = dollar_sign.parent
price = parent.find("strong").string.replace(",", "")
print(price)
```

L'esecuzione di questo codice stampa esattamente il prezzo, che può essere convertito in un intero e inserito in un database.

[Torna indietro](bs.md)