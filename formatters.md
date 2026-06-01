# I formattatori e la gestione dell'internazionalizzazione

- [I formattatori e la gestione dell'internazionalizzazione](#i-formattatori-e-la-gestione-dellinternazionalizzazione)
  - [1. La cultura locale in Java](#1-la-cultura-locale-in-java)
  - [2. Formattatori numerici](#2-formattatori-numerici)
    - [Parsing di stringhe](#parsing-di-stringhe)


## 1. La cultura locale in Java

Ogni cultura ha le sue convenzioni per formattare valori particolari, come importi in valute, percentuali, numeri decimali, date e orari. In Java una cultura locale è intesa come l'unione tra la zona geografica (il paese) e la lingua parlata ed è rappresentata da una stringa del tipo it_IT, en_GB, en_US:
- sigla di due lettere minuscole che rappresenta la lingua
- _
- sigla di due lettere maiuscole che rappresenta il paese

Questa rappresentazione è trasportata dalla classe `java.util.Locale`. Al suo interno vi sono:
-  varie costanti statiche che rappresentano i maggiori paesi (`Locale.FRANCE`) e maggiori lingue (`Locale.GERMAN`) 
- i metodi factory `of()` e `forLanguageTag()` per rappresentare le culture assenti controllando la correttezza delle stringhe passate.
- i metodi statici `getDefault` e `getAvailableLocales` per restituire la cultura locale di sistema e l'elenco di tutte le culture disponibili.
- i getter `getCountry()` e `getLanguage()` per restituire le stringhe di paese e lingua.

---

## 2. Formattatori numerici

La classe `java.text.NumberFormat` contiene tre formattatori numerici che formattano numeri, importi monetari e percentuali.  
La costruzione avviene tramite i metodi factory `getNumberInstance()`, `getCurrencyInstance()` e `getPercentInstance()`, tutti configurabili specificando la cultura locale tramite Locale e tramite altri metodi di configurazione (es. `setMaximumFractionDigits(int)` per impostare il numero massimo di cifre decimali).  
Una volta ottenuto il formatter tramite `format(x)` inserendo il numero che si vuole formattare si ottiene una stringa che rispetta le regole della cultura locale. 

### Parsing di stringhe

I formattatori numerici come tutti gli altri oltre a formattare hanno anche la capacità di estrarre da una stringa formattata in un certo modo un dato numerico. Tramite il metodo `parse(String)` è possibile inserire una stringa (che deve essere correttamente formattata secondo la stessa cultura del formatter) e ottenere un oggetto della classe Number, la classe padre dei wrapper per i tipi primitivi numerici, contenente il numero estratto e convertibile a sua volta nei tipi primitivi tramite i metodi `intValue()`, `floatValue()` e `doubleValue()`.