# I/O in Java: concetti base

## 1. Introduzione
Java gestisce sin dalla sua nascita il trasferimento di dati in input e output tramite stream, canali unidirezionali che possono essere collegati a qualsiasi dispositivo e che alla base trasportano singoli bytes. Sugli stream di byte poi si appoggiano quelli di caratteri, realizzati perchè largamente utilizzati e quindi parecchio comodi. <br> I punti a favore di questa architettura sono proprio il fatto che il trasferimento può essere realizzato tra dispositivi di qualsiasi tipo (anche tramite rete) per via dell'assenza di necessità di avere informazioni sui soggetti. Inoltre in questo modo si ha anche la possibilità di creare canali più evoluti appoggiandosi su quelli esistenti tramite un pattern a cipolla. <br> L'unico punto a sfavore è la difficoltà di configurazione del sistema quando si vanno a creare applicazioni da console e non grafiche. Per questo le versioni successive, soprattutto Java25 con la classe IO e i suoi metodi print() e println(), si sono proposte di porre rimedio.

## 2. Rappresentare file e directory: File & Path

File è una entità di `java.io` che modella in Java il concetto di file e di directory. Il costruttore prende in argomento il nome del file o directory, che possono anche non esistere al momento della creazione, ma essere create successivamente. La classe offre metodi per operare su files e directory, nonchè per controllare l'esistenza degli stessi e i permessi in lettura, scrittura ed esecuzione. In alcune costanti poi sono memorizzati i separatori per file e percorsi:
```java
File.separator //separatore per un file nei path (\ in Windows, / in MacOS e Unix)

File.pathSeparator //separatore per path differenti (: in MacOS, Unix e nei JAR, ; in Windows)
```
A partire da Java 7 File è stata sostituita con Path. File infatti rappresentava un approccio al problema troppo basico e poco scalabile.<br> Path è un'interfaccia di `java.nio.file` che come File modella un percorso (file o directory) che può essere anche inesistente, fornendo metodi per operarci e recuperarne informazioni, risentendo dei separatori relativi al sistema. A differenza di File non ha costruttori interni, rimpiazzati dai metodi della factory *Paths*. I metodi toFile e toPath garantiscono l'interoperabilità tra il nuovo e il vecchio metodo.

Per costruire un Path il metodo principale è Paths.get(), che accetta sia una singola stringa che più parametri rappresentanti ognuno un pezzo del percorso.
```java
Path p1 = Paths.get("Documents\\text\\file.txt");
Path p1 = Paths.get("Documents", "text", "file.txt");

//Con tutta una serie di metodi si può poi operare sul path
p1.getFileName() -> file.txt
p1 -> Documents\text\file.txt
p1.getNameCount() -> il numero strati attraversati: 3
p1.getParent() -> il padre del file: text

e altri
```
Più Path possono essere **concatenati** con il metodo p1.resolve(p2). In questo caso il metodo fallisce se si provano a concatenare un percorso relativo e uno assoluto o similari. <br> Più Path possono essere anche **relativizzati** uno rispetto all'altro, tramite p1.relativize(p2) che crea il Path relativo che da p1 va a p2.

## 3. `java.io`: le basi

Come detto prima alla base dell'IO c'è lo stream di byte, canale monodirezionale capace di trasferire uno o più byte in sequenza. Oltre a quello poi è implementato anche lo stream di caratteri, ottimizzato per l'IO appunti di caratteri.

Le classi astratte alla base del package sono 4: **InputStream** e **OutputStream** per gli stream di byte, **Reader** e **Writer** per gli stream di caratteri.

![Diagramma classi base IO](images/IO-base-classes.png)

Per l'approccio che si è seguito queste classi sono fondamentali perchè costituiscono il nucleo della "cipolla" che si crea inglobando implementazioni di stream sempre più efficienti e complessi, che possono eventualmente rappresentare anche categorie di dispositivi particolari, come la rete.