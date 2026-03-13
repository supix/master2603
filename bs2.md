# Uso avanzato di Beautiful Soup

Creiamo nel nostro progetto un nuovo file HTML, copiando il seguente.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Educational registration form</title>
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.4.1/css/all.css" integrity="sha384-5sAR7xN1Nv6T6+dT2mhtzEpVJvfS3NScPQTrOxhwjIuvcA67KV2R5Jz6kr4abQsz" crossorigin="anonymous">
    <link href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700" rel="stylesheet">
    <style>
      html, body {
      min-height: 100%;
      }
      body, div, form, input, select, p { 
      padding: 0;
      margin: 0;
      outline: none;
      font-family: Roboto, Arial, sans-serif;
      font-size: 16px;
      color: #eee;
      }
      body {
      background: url("/uploads/media/default/0001/01/b5edc1bad4dc8c20291c8394527cb2c5b43ee13c.jpeg") no-repeat center;
      background-size: cover;
      }
      h1, h2 {
      text-transform: uppercase;
      font-weight: 400;
      }
      h2 {
      margin: 0 0 0 8px;
      }
      .main-block {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100%;
      padding: 25px;
      background: rgba(0, 0, 0, 0.5); 
      }
      .left-part, form {
      padding: 25px;
      }
      .left-part {
      text-align: center;
      }
      .fa-graduation-cap {
      font-size: 72px;
      }
      form {
      background: rgba(0, 0, 0, 0.7); 
      }
      .title {
      display: flex;
      align-items: center;
      margin-bottom: 20px;
      }
      .info {
      display: flex;
      flex-direction: column;
      }
      input, select {
      padding: 5px;
      margin-bottom: 30px;
      background: transparent;
      border: none;
      border-bottom: 1px solid #eee;
      }
      input::placeholder {
      color: #eee;
      }
      option:focus {
      border: none;
      }
      option {
      background: black; 
      border: none;
      }
      .checkbox input {
      margin: 0 10px 0 0;
      vertical-align: middle;
      }
      .checkbox a {
      color: #26a9e0;
      }
      .checkbox a:hover {
      color: #85d6de;
      }
      .btn-item, button {
      padding: 10px 5px;
      margin-top: 20px;
      border-radius: 5px; 
      border: none;
      background: #26a9e0; 
      text-decoration: none;
      font-size: 15px;
      font-weight: 400;
      color: #fff;
      }
      .btn-item {
      display: inline-block;
      margin: 20px 5px 0;
      }
      button {
      width: 100%;
      }
      button:hover, .btn-item:hover {
      background: #85d6de;
      }
      @media (min-width: 568px) {
      html, body {
      height: 100%;
      }
      .main-block {
      flex-direction: row;
      height: calc(100% - 50px);
      }
      .left-part, form {
      flex: 1;
      height: auto;
      }
      }
    </style>
  </head>
  <body>
    <div class="main-block">
      <div class="left-part">
        <i class="fas fa-graduation-cap"></i>
        <h1>Register to our courses</h1>
        <p>W3docs provides free learning materials for programming languages like HTML, CSS, Java Script, PHP etc.</p>
        <div class="btn-group">
          <a class="btn-item" href="https://www.w3docs.com/learn-html.html">Learn HTML</a>
          <a class="btn-item" href="https://www.w3docs.com/quiz/#">Select Quiz</a>
        </div>
        $2345
      </div>
      <form action="/">
        <div class="title">
          <i class="fas fa-pencil-alt"></i> 
          <h2>Register here</h2>
        </div>
        $123
        <div class="info">
          <input class="fname" type="text" name="name" placeholder="Full name">
          <input type="text" name="name" placeholder="Email">
          <input type="text" name="name" placeholder="Phone number">
          <input type="password" name="name" placeholder="Password">
          <select>
            <option value="course-type" selected>Course type*</option>
            <option value="short-courses">Short courses</option>
            <option value="featured-courses">Featured courses</option>
            <option value="undergraduate">Undergraduate</option>
            <option value="diploma">Diploma</option>
            <option value="certificate">Certificate</option>
            <option value="masters-degree">Masters degree</option>
            <option value="postgraduate" >Postgraduate</option>
          </select>
        </div>
        <div class="checkbox">
          <input type="checkbox" name="checkbox"><span>I agree to the <a href="https://www.w3docs.com/privacy-policy">Privacy Poalicy for W3Docs.</a></span>
        </div>
        <button type="submit" href="/">Submit</button>
      </form>
    </div>
  </body>
</html>
```

Proviamo ad aprire il file in un browser e analizziamone la struttura.

Carichiamo il file con Beautiful Soup, così come fatto con il precedente file HTML.

```py
from bs4 import BeautifulSoup

if __name__ == "__main__":
    with open("index2.html", "r") as f:
        doc = BeautifulSoup(f, "html.parser")
```

Come già fatto in precedenza, estraiamo tutti i tag `option` dal documento.

```py
tags = doc.find_all('option')
print(tags)
```

## Estrazione di elementi in base a un insieme di tags

E' possibile estrarre più elementi del contenuto HTML in un unico comando, attraverso la seguente sintassi.

```py
tags = doc.find_all(['p', 'a'])
print(tags)
```

## Estrazione in base a condizioni multiple

E' anche possibile cercare un tag specificando una serie di condizioni, che devono essere tutte contemporaneamente verificate, come nel seguente codice.

```py
tags = doc.find_all('option', text = "Undergraduate")
print(tags)
```

Che fornisce il seguente output.

```html
[<option value="undergraduate">Undergraduate</option>]
```

A titolo d'esempio, estendiamo le condizioni inserendone una aggiuntiva che verifica il valore di un attributo.

```py
tags = doc.find_all('option', text = "Undergraduate", value = "undergraduate")
print(tags)
```

Il valore restituito è simile al precedente. Modificando l'ultima condizione, viene invece restituito un risultato vuoto.

## Accesso a un tag HTML attraverso la classe CSS

E' possibile estrarre i tag che sono associati a una certa classe CSS. La sintassi è la seguente.

```py
tags = doc.find_all(class_="fname")
print(tags)
```

L'esecuzione del codice restituisce il seguente output.

```html
[<input class="fname" name="name" placeholder="Full name" type="text"/>]
```

Si noti come si utilizza il parametro `class_` e non `class`. L'ultimo identificatore è infatti una parola chiave riservata del linguaggio Python.

## Accesso a un contenuto mediante regular expression (regExp)

Per poter introdurre espressioni regolari nel codice Python bisona includere la libreria `re`.

```py
import re
```

Avendo ora la possibilità di effettuare una ricerca attraverso una espressione regolare, il codice per individuare un prezzo, che è il numero seguente il simbolo del dollaro ($), diventa come il seguente.

```py
tags = doc.find_all(text=re.compile('\\$.*'))
```

Eseguendo questa riga di codice si ottiene il seguente output.

```py
['\n        $2345\n      ', '\n        $123\n        ']
```

I prezzi possono quindi essere isolati mediante il seguente codice.

```py
tags = doc.find_all(text=re.compile('\\$.*'))
for tag in tags:
  print(tag.strip())
```

ottenendo il seguente output

```txt
$2345
$123
```

## Limitare il numero di risultati in una ricerca

Per limitare il numero di risultati di una ricerca, si può usare il parametro `limit`. Immaginiamo per esempio di voler limitare la precedente ricerca a un solo risultato. Scriveremo quanto segue.

```py
tags = doc.find_all(text=re.compile('\\$.*'), limit=1)
for tag in tags:
  print(tag.strip())
```

ottenendo il seguente output

```txt
$2345
```

## Salvare un documento modificato

Con Beautiful Soup è possibile salvare un documento dopo aver apportato delle modifiche.

Iniziamo con il caricare il documento e modificarlo. Proviamo a cambiare, per esempio, l'attributo placeholder di tutte le caselle di testo presenti nel form HTML.

```py
tags = doc.find_all("input", type="text")

for tag in tags:
    tag['placeholder'] = "I changed you!"
```

Il documento modificato può essere salvato con il seguente codice.

```py
with open("changed.html", "w") as file:
    file.write(str(doc))
```

L'esecuzione di questo file produrrà la creazione di un nuovo file HTML, dal nome `changed.html`. Aprendo questo file nel browser si noterà come il placeholder è stato impostato al valore modificato.

## Navigare il Document Object Model (DOM)

Per mostrare la navigazione di un DOM HTML, useremo il seguente portale web: https://coinmarketcap.com

Visualizziamo nel browser la home page e analizziamo velocemente la struttura del relativo HTML.

Come si può notare il contenuto principale è all'interno di un tag `tbody` (contrazione di table body), che contiene un elenco di tag `tr`, ognuno rappresentativo di una riga della tabella.

I codice per recuperare le righe della tabella può pertanto essere il seguente.

```py
from bs4 import BeautifulSoup
import requests

url = "https://coinmarketcap.com"
result = requests.get(url).text
doc = BeautifulSoup(result, "html.parser")

tbody = doc.tbody
trs = tbody.contents
print(trs[0])
```

Un metodo alternativo di prelevare le righe della tabella consiste nell'usare il metodo `next_sibling`, come mostrato di seguito.

```py
first_tr = trs[0]
second_tr = first_tr.next_sibling

print(second_tr)
```

Come si intuisce, il `parent` di un qualsiasi tag `tr` è nuovamente il tag `tbody`.

```py
print(trs[0].parent.name)
print(trs[1].parent.name)
```

il cui output è

```html
tbody
tbody
```

## Esercizio: recupero delle informazioni sulle cryptovalute

Recuperare tutte le coppie (crypto-currency, price) dal HTML.

Fai [click qui](ex02_cryptocur_price_extraction.md) per vedere una possibile soluzione.