# Beautiful soup

Beautiful Soup è una libreria Python utile a estrarre dati da contenuti rappresentati in linguaggio HTML e XML.

La documentazione ufficiale di Beautiful Soup è a questo link: https://www.crummy.com/software/BeautifulSoup/bs4/doc/

Per mostrarne le potenzialità e le modalità d'uso, creiamo un progetto Python. Si consiglia l'uso dell'ambiente di sviluppo _VS Code_.

## Creazione di un ambiente virtuale

Utilizzando la console, creiamo un ambiente virtuale Python, per es.: `bs4env`.

```sh
python -m venv bs4env
```

e attiviamolo (se siamo in Windows, col seguente comando).

```sh
.\bs4env\Scripts\activate.bat
```

## Installazione

Sempre utilizzando la console, installiamo la libreria beautiful soup.

```sh
pip install beautifulsoup4
```

## Uso di Beautiful Soup

Creiamo un progetto Python con un file principale, per es.: `main.py`.Ecco il codice.

```py
if __name__ == "__main__":
    print("ciao")
```

Eseguiamolo per verificare che tutto funzioni.

### Importazione della libreria Beautiful Soup

Importiamo la libreria Beautiful Soup nel codice.

```py
from bs4 import BeautifulSoup

if __name__ == "__main__":
    print("ciao")
```

Ancora una volta, l'esecuzione del codice dovrebbe andare a buon fine.

### Importazione di un file HTML statico

Importiamo nel nostro progetto il seguente contenuto HTML, e salviamolo all'interno del file `index.html` posizionato nella stessa cartella del file `main.py`.

```html
<HTML>
<HEAD>
<TITLE>Your Title Here</TITLE>
</HEAD>
<BODY BGCOLOR="FFFFFF">
<CENTER><IMG SRC="clouds.jpg" ALIGN="BOTTOM"></CENTER>
<HR>

<a href="http://somegreatsite.com">Link Name</a>

is a link to another nifty site

<H1>This is a Header</H1>
<H2>This is a Medium Header</H2>

Send me mail at <a href="mailto:support@yourcompany.com">
support@yourcompany.com</a>.

<P> This is a new paragraph!

    <P> <B color="red">This is a new paragraph!</B>
<BR> <B><I>This is a new sentence without a paragraph break, in bold italics.</I></B>
<HR>
</BODY>
</HTML>
```

### Parsing di un file HTML

Ora proviamo a usare Beautiful Soup per fare il parsing del file e stamparlo a video.

```py
from bs4 import BeautifulSoup

if __name__ == "__main__":
    with open("index.html", "r") as f:
        doc = BeautifulSoup(f, "html.parser")

    print(doc)
```

Per verificare come il file sia stato effettivamente processato e trasformato in un file HTML, usiamo questa nuova versione della riga per la stampa del documento.

```py
    print(doc.prettify())
```

Come si può notare, il file è stato processato dalla libreria e correttamente reindentato.

### Estrazione di una sezione del file HTML

Vediamo ora come è possibile estrarre una specifica sezione di un documento. Per esempio il frammento del documento contenuto in uno specifico _tag_, il tag `TITLE`.

```py
tag = doc.title
print(tag)
```

In questo modo la prima occorrenza del tag `TITLE` viene estratta e assegnata alla variabile `tag`, fornendo il seguente output.

```html
<title>Your Title Here</title>
```

### Estrazione del contenuto interno a un tag HTML

Per ottenere il solo testo contenuto all'interno del tag `TITLE` si usa questa sintassi.

```py
tag = doc.title
print(tag.string)
```

ottenendo il seguente output.

```html
Your Title Here
```

### Modifica di un documento

Un documento caricato può anche essere modificato. Ecco un esempio.

```py
tag = doc.title
tag.string = "Hello!"
print(tag.string)
```

Ecco il risultato.

```html
<title>Hello!</title>
```

La stampa dell'intero documento mostra che anche in questo caso il tag `TITLE` risulta modificato.

```py
tag = doc.title
tag.string = "Hello!"
print(doc)
```

N.B.: non viene modificato il file sul disco, ma solo la sua rappresentazione in memoria risultante dal parsing del documento da parte della libreria Beautiful Soup.

### Una seconda modalità di accesso a una sezione

Una seconda modalità per accedere a un tag in un documento è rappresentata dall'uso del metodo `find()`.

```py
tag = doc.find("a")
print(tag)
```

L'output è il seguente.

```html
<a href="http://somegreatsite.com">Link Name</a>
```

Anche in questo caso, viene intercettata e restituita la _prima occorrenza_ del tag richiesto.

Qualora si vogliano acquisire tutte le occorrenze di un certo tag presenti nel file, si utilizza invece il metodo `find_all()`.

```py
tags = doc.find_all("a")
print(tags)
```

L'output è il seguente.

```html
[
  <a href="http://somegreatsite.com">Link Name</a>,
  <a href="mailto:support@yourcompany.com">support@yourcompany.com</a>
]
```

Come si vede, il metodo `find_all()` restituisce un array contenente tutte le occorrenze. E questo accade anche quando l'occorrenza individuata è unica. Pertanto, qualora si volgia nuovamente ottenere la sola prima occorrenza, lo si fa attraverso l'operatore di indicizzazione dell'array.

```py
tag = doc.find_all("a")[0]
print(tag)
```

che restituisce di nuovo

```html
<a href="http://somegreatsite.com">Link Name</a>
```

### Acquisizione di tags innestati

Con il metodo `find_all()` acquisiamo tutti i tags `<p>` contenuti nel documento.

```py
tags = doc.find_all("p")
print(tags)
```

L'output è il seguente.

```html
[
  <p> This is a new paragraph!
    <p><b color="red">This is a new paragraph!</b>
      <br/>
      <b>
        <i>This is a new sentence without a paragraph break, in bold italics.</i>
      </b>
      <hr/>
    </p>
  </p>, 
  <p>
    <b color="red">This is a new paragraph!</b>
    <br/>
    <b>
      <i>This is a new sentence without a paragraph break, in bold italics.</i>
    </b>
    <hr/>
  </p>
]
```

Nell'array sono presenti due occorrenze del tag `<p>`, ciascuna contenente a sua volta HTML strutturato all'interno.

Se vogliamo accedere ai tag `<b>` interni alla prima occorrenza del tag `<p>` usiamo il seguente codice.

```py
ptag = doc.find_all("p")[0]
btags = ptag.find_all("b")
print(btags)
```

col seguente risultato

```html
[
  <b color="red">This is a new paragraph!</b>,
  <b><i>This is a new sentence without a paragraph break, in bold italics.</i></b>
]
```

### Accedere agli attributi di un tag

Una volta acquisito un tag, è possibile visualizzare i valori dei suoi attributi. Acquisiamo il tag `img`.

```py
tag = doc.find("img")
print(tag)
```

Otteniamo il seguente output.

```html
<img align="BOTTOM" src="clouds.jpg"/>
```

L'oggetto tag si comporta come un dizionario, le cui chiavi sono i nomi degli attributi. Pertanto è indicizzabile mediante l'operatore `[]`.

```py
print(tag["align"])
print(tag["src"])
```

### Modificare gli attributi di un tag

Anche gli attributi di un tag possono essere modificati nel modo consueto.

```py
tag = doc.find("img")
tag["align"] = "TOP"
tag["src"] = "newImg.jpg"
print(tag)
```

Otteniamo il seguente output.

```html
<img align="TOP" src="newImg.jpg"/>
```

E possibile anche aggiungere nuovi tag a quelli già esistenti.

```py
tag = doc.find("img")
tag["new-tag"] = "new-tag-value"
print(tag)
```

che dà il seguente output

```html
<img align="BOTTOM" new-tag="new-value" src="clouds.jpg"/>
```

### Accedere a contenuti in rete

Quanto eseguito su un file HTML locale può essere similmente eseguito direttamente su contenuti presenti in rete. A questo scopo è necessario installare una libreria che consente di acquisire contenuti remoti: la libreria `requests`. Digitiamo in console il seguente comando `pip`.

```sh
pip install requests
```

Importiamo la libreria nel codice Python.

```py
import requests
```

Ora possiamo scaricare un contenuto HTML direttamente dal web. Useremo il sito http://example.com.

Proviamo a visualizzarlo [nel browser](http://example.com) e a visualizzarne il relativo codice HTML (tasto destro + _Visualizza sorgente pagina_)

Scarichiamo con uno script la pagina dell'articolo.

```py
url = "http://example.com"

result = requests.get(url)
print(result.text)
```

### Elaborare i contenuti in rete con Beautiful Soup

Ora che abbiamo il contenuto della pagina, analizziamolo con Beautiful Soup. Modifichiamo il codice precedente come segue.

```py
url = "http://example.com"

result = requests.get(url)
doc = BeautifulSoup(result.text, "html.parser")
print(doc.prettify())
```

L'esecuzione del codice, di nuovo, mostrerà l'HTML della pagina. Questa volta però il contenuto sarà stato elaborato e mostrato in una forma correttamente indentata.

### Esercizio: estrarre il prezzo di un articolo da un sito di e-commerce

Vogliamo ora scrivere del codice utile a estrarre il prezzo dell'articolo dalla pagina di dettaglio di un articolo su un sito di e-commerce.

URL sito e-commerce: https://www.newegg.ca
URL dettaglio articolo: https://www.newegg.ca/asus-tuf-gaming-tuf-rtx3080ti-o12g-gaming-geforce-rtx-3080-ti-12gb-graphics-card-triple-fans/p/N82E16814126509

Attraverso le URL fornite, analizzare il sito e comprenderne a grandi linee le caratteristiche.

Non esiste un unico modo di risolvere questo esercizio. Si rediga lo script che restituisce il prezzo dell'articolo nella modalità ritenuta più semplice.

Alla fine dell'esercizio, confrontare la propria soluzione con [questa](ex01_price_extraction.md).

[Vai alla prossima lezione](bs2.md)