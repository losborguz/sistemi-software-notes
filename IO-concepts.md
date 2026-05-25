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

## 4. Stream di bytes

Il **canale di input a byte** è rappresentato dalla classe base **InputStream**. Il suo *costruttore apre lo stream*, il metodo *read legge uno o più byte* e *close chiude lo stream*. Inputstream è una **classe astratta**, dato che read non è definita al suo interno ma bensì dalle sue classi figlie.

Il **canale di output a byte** è invece rappresentato da **OutputStream**, il cui funzionamento è analogo ad inputStream: Il *costruttore apre lo stream*, *write* fa *scrivere uno o più byte*, *flush svuota il buffer* in uscita e *close chiude lo stream*. Anche OutputStream è **astratta**, write infatti andrà implementata dalle suo classi derivate.

### Input e output da file

La classe concreta **FileInputStream** estende InputStream e *implementa write per estrarre dati in binario da un file*. Il suo costruttore prende come parametro una *stringa contenente il percorso del file* da aprire o un *oggetto File* corrispondente al file e *lancia l'eccezione* a controllo obbligatorio `FileNotFoundException`. <br>Il metodo *read legge un byte alla volta*, restituendolo come *int compreso tra 0 e 255*. Se lo *stream è finito*, restituisce *-1*. Lancia l'eccezione `IOException` a controllo obbligatorio.

La classe **FileOutputStream** è la relativa *implementazione di OutputStream* per gestire l'*output di dati su un file*. Il suo funzionamento è analogo a FileInputStream, con write che scrive sul file un byte alla volta, codificato come intero da 0 a 255. In write è possibile poi *inserire un flag boolean* per specificare se *i dati devono essere appesi* o devono sovrascrivere.

### Adapter streams

Gli adapter stream sono classi che adattono uno stream già esistente per aggiungere funzionalità. La loro caratteristica in comune è il costruttore che accetta un InputStream o OutputStream.

Per l'input binario alcuni adapter notevoli sono:
- DataInputStream: implementa DataInput e definisce vari metodi per leggere valori corrispondenti ai tipi primitivi, utilizzando i wrapper (readInteger(), readFloat() ecc.), che lanciano l'espressione `EOFException` se lo stream termina in modo particolare.
- ObjectInputStream: implementa DataInput e ObjectInput e definisce il metodo readObject() per poter leggere un oggetto serializzabile. Mantiene anche i metodi di DataInputStream. Tutti i metodi lanciano l'espressione `EOFException` se lo stream termina in modo particolare.

Per l'output binario esistono gli analoghi adapter DataOutputStream e ObjectOutputStream, che seguono lo stesso principio di funzionamento di quelli per l'input.

#### Esempio: scrittura di dati su file
```java
import java.io.*;
public class Esempio1 {
    public static void main(String args[]){
        FileOutputStream fs = null;
    
        try {
            fs = new FileOutputStream("Prova.dat");
        }
        catch(IOException e){
            System.out.println("Apertura fallita");
        System.exit(1);
        }

        DataOutputStream os =
        new DataOutputStream(fs);
        float f1 = 3.1415F; char c1 = 'X';
        boolean b1 = true; double d1 = 1.4142;
        try {
            os.writeFloat(f1); os.writeBoolean(b1);
            os.writeDouble(d1); os.writeChar(c1);
            os.writeInt(12); os.close();
        } catch (IOException e){
            System.out.println("Scrittura fallita");
            System.exit(2);
        }
    }
}
```

## 5. Serializzazione di oggetti

Serializzare un oggetto significa *salvarlo in una rappresentazione binaria*, deserializzarlo invece significa *ricostruirlo partendo da questa rappresentazione*. In molti casi è utile e necessario poter salvare interi oggetti per poi successivamente ricostruirli, tuttavia occorre prestare attenzione alle informazioni contenute da questi oggetti perchè dal momento che vengono inseriti nello stream perdono il controllo della JVM e ciò non è sempre opportuno.
Per poter marcare una classe come serializzabile, questa deve implementare l'interfaccia **Serializable**, un'interfaccia marker che denota solamente la possibilità per gli oggetti della classe che la implementa di essere serializzati e deserializzati. Inoltre in questo caso è buona pratica aggiungere alla classe un numero di versione: il SerialVersionUID. Questa variabile **private, static e final** è un indicatore univoco della versione della classe , che deve essere modificato ogni volta che si cambia la sua struttura.  
Quando viene riletto un file di oggetti serializzati viene confrontato in automatico il numero di versione dell'oggetto con quello della sua classe e se sono diversi viene lanciata una `InvalidClassException`.  
Se non dichiarato ne viene creato uno di default calcolato in base ad alcuni dettagli della classe. Tuttavia compilatori diversi potrebbero controllare dettagli diversi e ciò potrebbe portare a eccezioni tirate anche quando non ce n'era bisogno.

```java
public class 2DPoint implements Serializable {
    private float x, y;
    private static final SerialVersionUID = 1;

    //tutti i metodi della classe
}
```

### Metodi per serializzare e deserializzare

Nella pratica la scrittura e la lettura di oggetti si applica tramite i due metodi **writeObject()** e **readObject()**. Il primo serializza un oggetto in binario, lanciando un'eccezione se il metodo non implementa Serializable. Il secondo lo ricostruisce, restituendo formalmente un Object e richiedendo quindi un cast alla classe specifica da parte del programmatore.

```java
2DPoint p = new 2DPoint(3.2F, 1.5F);

try (FileOutputStream f = new FileOutputStream("data.bin")) {
    ObjectOutputStream os = new ObjectOutputStream(f);
    os.writeObject(p);

} catch (IOException e) {
    ...
}

2DPoint p2 = null;
try (FineInputStream f = new FineInputStream("data.bin")) {
    ObjectInputStream os = new ObjectInputStream(f);
    p2 = (2DPoint) os.readObject(p);

} catch (IOException | ClassNotFoundException e) {
    ...
}
```

**Tip**: quando si vuole salvare una Collection di oggetti, conviene serializzare l'intera collezione piuttosto che ogni singolo oggetto presente, in questo modo la rilettura diventa più facile.

```java
public static void salvaPunti(String filename, Punto2D[] punti){
    try {
        FileOutputStream f = new FileOutputStream(filename);
        ObjectOutputStream os = new ObjectOutputStream(f);
        os.writeObject(punti);
        os.flush(); // non necessaria prima della close
        os.close();
    }
        catch (IOException e){
        System.err.println("Errore in fase di salvataggio dati");
    }
}

public static Punto2D[] leggiPunti(String filename){
    Punto2D[] punti = null;
    try {
        FileInputStream f = new FileInputStream(filename);
        ObjectInputStream is = new ObjectInputStream(f);
        punti = (Punto2D[]) is.readObject();
        is.close();
    }
    catch (IOException | ClassNotFoundException e){
        System.err.println("Errore in fase di rilettura dati");
    }
    return punti;
}
```

## 6. La stream factory

A partire da Java 7 con la classe **Files** la creazione diretta di stream tramite new ... viene affiancata da un insieme di factory methods per la creazione indiretta. Questi metodi statici hanno signature uniforme new*NomeStream* e come argomenti, oltre all'oggetto Path del file da aprire , hanno un set di OpenOption per indicare le varie opzioni di apertura.
```java
newInputStream(Path p, OpenOption... options) //i tre punti ... indicano che possono esserci più elementi di quel tipo.
newOutputStream(Path p, OpenOption... options) 
```

## 7. Stream di testo

### Casi d'uso dell'input di testo

Leggere testo da un file è raramente un'operazione a sè stante, molto più spesso è inclusa in un processo più grande di estrazione di informazioni da un file e di creazione di oggetti con quelle informazioni. Le fasi sono le seguenti:
- Si leggono dal file di testo righe intere di dati, che solitamente corrispondono ad un oggetto di una specifica classe. Questi dati sono strutturati in un modo definito per rappresentare le informazioni.
- Si analizzano le righe lette e le si spezzettano in varie porzioni corrispondenti alle singole informazioni, come il nome di una linea ferroviaria, le sue fermate ecc.
- Si usano le informazioni estratte dalle stringhe per costruire oggetti che permettono di rappresentare le entità descritte nel file in maniera più completa.
