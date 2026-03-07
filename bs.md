# Beautiful soup

Beautiful Soup è una libreria Python utile a estrarre dati da contenuti rappresentati in linguaggio HTML e XML.

La documentazione ufficiale di Beautiful Soup è a questo link: https://www.crummy.com/software/BeautifulSoup/bs4/doc/

## Creazione di un ambiente virtuale

Utilizzando la console, creiamo un ambiente virtuale Python, per es.: `bs4env`.

```
python -m venv bs4env
```

e attiviamolo (se siamo in Windows, col seguente comando).

```
.\bs4env\Scripts\activate.bat
```

## Installazione

Sempre utilizzando la console, installiamo la libreria beautiful soup.

```
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
<CENTER><IMG SRC="clouds.jpg" ALIGN="BOTTOM"> </CENTER>
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
