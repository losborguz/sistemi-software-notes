# **Strutture dati: il Collection Framework**

## 1. Cos'è un Collection Framework
Un collection framework è un'architettura logica globale e uniforme pensata per implementare le strutture dati in un linguaggio ad oggetti.

Questo framework è composto da:
- Interfacce: introducono i tipi di strutture dati in maniera ancora astratta e senza implementare nulla, insieme ad alcuni concetti di supporto necessari (es. le entità iterabili);
- Classi: prendono i concetti espressi dalle interfacce e li implementano fornendo versioni utilizzabili delle collezioni
- Librerie accessorie: contengono elementi utili nella realizzazione, come algoritmi o costanti statiche

L'organizzazione è volta a realizzare strutture **generali e parametriche**

### Similitudini e differenze tra linguaggi

Tutti i linguaggi ad oggetti offrono all'interno del loro collection framework **liste, set, mappe e iteratori**, ognuna parametrica rispetto ad un tipo generico. Gli array in questi linguaggi **NON FANNO PARTE DELLE COLLECTION**.

Varia da linguaggio a linguaggio invece la classificazione delle collezioni come **entità modificabili o immutabili**, la **restrizione dei tipi a soltanto gli oggetti** (in Java vengono esclusi i tipi primitivi), nonchè se la **parametrizzazione è limitata a compile-time** (Java, Kotlin, Scala) **o si estende anche a run-time** (C#).

Per quanto riguarda il **naming** delle classi e interfacce si è adottata questa convenzione: 

- **Nomi interfacce** --- concetti generici (Set, Map, List...)
- **Nomi classi** --- implementazioni più specifiche (TreeSet, HashMap, ArrayList...)

In Java il Collection Framework è organizzato come nel grafico sotto.
![diagramma organizzazione collection](images/collection-organization-java.png)

## 2. Le interfacce principali del JCF

Le interfacce chiave del Java Collection Framework sono le seguenti:
- **Collection\<T\>**: l'interfaccia genitore di Set e List, non specifica nessuna caratteristica particolare
- **Set\<T\>** (con **SortedSet\<T\>** e **NavigableSet\<T\>**): specifica un insieme di elementi (senza duplicati)
- **List\<T\>**: specifica una lista indicizzata di elementi
- **Map\<K, V\>** (con **SortedMap\<K, V\>** e **NavigableMap\<K, V\>**): specifica una tabella di associazioni chiave-valore
- **Queue\<T\>**: specifica una coda di elementi di qualsiasi tipo (FIFO, LIFO, ecc.)
- **Deque\<T\>**: specifica una coda circolare, le cui testa e coda sono collegate

L'aggettivo Sorted di SortedSet e SortedMap fa riferimento allo scorrimento della collection (per esempio nel foreach), che in questo caso implica anche l'ordinamento. L'aggettivo Navigable intende invece Set e Map navigabili in entrambi i sensi (sx - dx e viceversa).

### Iterable\<T\>

L'interfaccia Iterable è la madre di tutte le collection. Le classi che la implementano indicano la capacità di inserire loro istanze all'interno di cicli for-each per poter iterare sugli elementi contenuti. Il bisogno è nato dato che molte strutture dati non sono indicizzate come gli array, e si perdeva quindi la possibilità di scorrere gli elementi.

### Collection\<T\>

L'interfaccia Collection\<T\>, figlia di Iterable, introduce l'idea di collezione di elementi, senza ulteriori specifiche. L'interfaccia di accesso definita è altrettanto generale, composta da metodi come i seguenti:
- **add()**: aggiunge un elemento alla collezione
- **remove()**: rimuovere un elemento dalla collezione
- **contains()**: verificare se un elemento è nella collezione
- **isEmpty()**: verificare se la collezione non contiene elementi
- **toArray()**: fornire un array che contenga gli elementi della collezione
- **size()**: fornire il numero di elementi contenuti nella collezione
- **equals(that)**: verificare se due collezioni sono uguali

e altri.

### Set\<T\> e SortedSet\<T\>

Set estende Collection e introduce il concetto di insieme (senza duplicati). L'interfaccia di accesso è simile, tuttavia add() e i costruttori devono accertarsi di non inserire/avere duplicati all'interno del Set e equals adotta il significato matematico di uguaglianza insiemistica (∀x ∈ S1, x ∈ S2 e viceversa).

SortedSet estende a sua volta Set e aggiunge il concetto di ordinamento fra gli elementi, che devono necessariamente estendere Comparable\<T\> e quindi implementare compareTo(). Questa collection aggiunge metodi che sfruttano l'ordinamento degli elementi, come first(), last() o subSet().

### List\<T\>

List anch'essa estende Collection, ma introduce il concetto di sequenza indicizzata che ammette duplicati, molto simile ad un array. La navigazione della lista avviene seguendo la sequenza di inserimento dei valori. 

Anche qua l'interfaccia di accesso è simile, tuttavia add() aggiunge un elemento in fondo alla lista, equals effettua il confronto a due a due e il nuovo metodo get(index) permettere di accedere direttamente ad una posizione della lista.

### Queue\<T\> e Deque\<T\>

Queue\<T\> introduce il concetto di coda di elementi, da cui deriva il concetto di testa della coda. Non esiste il concetto di posizione, si può solo accedere all'elemento in testa alla serie. In base al tipo di coda poi inserimenti e rimozioni saranno applicati all'inizio o alla fine della collection.

Deque\<T\> specializza ulteriormente Queue, introducendo il concetto di coda doppia e permettendo quindi di aggiungere/rimuovere elementi da entrambe le estremità.

### Map\<K, V\>

L'interfaccia Map non deriva da Collection, in quanto questa struttura è una **tabella bidimensionale** e che quindi non può derivare da una serie monodimensionale di valori.

La tabella è composta da **associazioni chiave-valore**. In altre parole ogni riga è composta da un valore associato ad un identificatore detto chiave, che è univoco. 

L'accesso ai valori non è più sequenziale, scorrendo tutti i valori della struttura, ma bensì **casuale**, **accedendo al valore direttamente tramite la sua chiave**. Ciò è reso possibile da funzioni matematiche (funzioni hash) che creano una corrispondenza tra chiavi e valori in modo che, data una chiave, viene restituita la posizione dell'elemento in tabella.

L'interfaccia di accesso alla collection prevede i seguenti metodi:
- **put()**: permette di inserire una nuova riga nella tabella
- **get()**: permette di accedere ad una riga specificando la chiave
- **containsKey()**: ricerca nella tabella se è presente la chiave specificata
- **containsValue()**: ricerca nella tabella se è presente il valore specificato


