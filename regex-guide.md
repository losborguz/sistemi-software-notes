# Regular Expression in Java

- [Regular Expression in Java](#regular-expression-in-java)
  - [Cos'è una Regular Expression?](#cosè-una-regular-expression)
  - [A cosa servono?](#a-cosa-servono)
  - [Elementi di una Regex](#elementi-di-una-regex)
    - [1. Caratteri Letterali](#1-caratteri-letterali)
    - [2. Il Punto `.`](#2-il-punto-)
    - [3. Ancore: `^` e `$`](#3-ancore--e-)
    - [4. Classi di Caratteri `[...]`](#4-classi-di-caratteri-)
    - [5. Classi di Caratteri Predefinite (`\d`, `\w`, `\s`)](#5-classi-di-caratteri-predefinite-d-w-s)
    - [6. Quantificatori](#6-quantificatori)
      - [Quantificatori greedy vs lazy](#quantificatori-greedy-vs-lazy)
    - [7. Gruppi `(...)`](#7-gruppi-)
      - [Gruppi non catturanti `(?:...)`](#gruppi-non-catturanti-)
    - [8. Alternanza `|`](#8-alternanza-)
    - [9. Escape `\`](#9-escape-)
    - [10. Lookahead e Lookbehind (Asserzioni Zero-Width)](#10-lookahead-e-lookbehind-asserzioni-zero-width)
  - [Esempio Completo: Validazione Email](#esempio-completo-validazione-email)
  - [Consigli](#consigli)


## Cos'è una Regular Expression?

Una **regular expression** (o *regex*) è una sequenza di caratteri che definisce un **pattern di ricerca**. Si tratta di un mini-linguaggio formale usato per descrivere insiemi di stringhe secondo regole precise.

Le regex sono uno strumento potentissimo e versatile: permettono di cercare, validare, estrarre e trasformare testo in modo molto più flessibile rispetto ai normali confronti tra stringhe.

---

## A cosa servono?

Le regular expression trovano impiego in moltissimi contesti pratici:

- **Validazione dell'input**: verificare che un'email, un numero di telefono o un codice postale siano nel formato corretto.
- **Ricerca nel testo**: trovare tutte le occorrenze di un pattern in un documento.
- **Sostituzione**: rimpiazzare parti di una stringa con un'altra (es. censurare parole, riformattare date).
- **Estrazione di dati**: recuperare gruppi specifici di informazioni da un testo strutturato (es. log di sistema, HTML grezzo, CSV).
- **Splitting di stringhe**: dividere una stringa in base a separatori complessi.

---

## Elementi di una Regex

### 1. Caratteri Letterali

Il tipo più semplice di pattern: corrispondono esattamente al carattere scritto.

```
ciao   →  trova la stringa "ciao"
java   →  trova la stringa "java"
```

```java
"Hello World".matches("Hello World"); // true
```

---

### 2. Il Punto `.`

Corrisponde a **qualsiasi carattere singolo**, eccetto il newline `\n`.

```
c.sa   →  trova "casa", "cosa", "c3sa", ecc.
```

```java
"casa".matches("c.sa"); // true
"c sa".matches("c.sa"); // true
"csa".matches("c.sa");  // false (manca un carattere)
```

---

### 3. Ancore: `^` e `$`

- `^` indica l'**inizio** della stringa.
- `$` indica la **fine** della stringa.

```
^Ciao     →  la stringa deve iniziare con "Ciao"
mondo$    →  la stringa deve finire con "mondo"
^Ciao$    →  la stringa deve essere esattamente "Ciao"
```

```java
"Ciao mondo".matches("^Ciao.*");  // true
"Ciao mondo".matches(".*mondo$"); // true
"Ciao mondo".matches("^mondo$");  // false
```

---

### 4. Classi di Caratteri `[...]`

Definiscono un **insieme** di caratteri accettabili in una posizione.

| Sintassi | Significato |
|---|---|
| `[aeiou]` | Una qualsiasi vocale |
| `[a-z]` | Una lettera minuscola dalla a alla z |
| `[A-Z]` | Una lettera maiuscola |
| `[0-9]` | Una cifra |
| `[a-zA-Z0-9]` | Lettera o cifra |
| `[^aeiou]` | Qualsiasi carattere che **non** sia una vocale |

```java
"gatto".matches("[a-z]+");  // true
"Gatto".matches("[a-z]+");  // false (la G è maiuscola)
"abc3".matches("[^0-9]+");  // false (contiene una cifra)
```

---

### 5. Classi di Caratteri Predefinite (`\d`, `\w`, `\s`)

Java supporta una serie di shortcut molto comodi:

| Sequenza | Significato | Equivalente |
|---|---|---|
| `\d` | Cifra numerica | `[0-9]` |
| `\D` | Non cifra | `[^0-9]` |
| `\w` | Carattere "parola" (lettera, cifra, underscore) | `[a-zA-Z0-9_]` |
| `\W` | Non carattere parola | `[^a-zA-Z0-9_]` |
| `\s` | Spazio bianco (spazio, tab, newline) | `[ \t\r\n\f]` |
| `\S` | Non spazio bianco | `[^ \t\r\n\f]` |

> **Attenzione in Java**: il backslash deve essere raddoppiato nelle stringhe Java: `\d` diventa `"\\d"`.

```java
"12345".matches("\\d+");      // true
"hello_1".matches("\\w+");    // true
"hello world".matches("\\S+"); // false (contiene uno spazio)
```

---

### 6. Quantificatori

Specificano **quante volte** deve comparire l'elemento precedente.

| Quantificatore | Significato |
|---|---|
| `?` | Zero o una volta (opzionale) |
| `*` | Zero o più volte |
| `+` | Una o più volte |
| `{n}` | Esattamente n volte |
| `{n,}` | Almeno n volte |
| `{n,m}` | Da n a m volte |

```java
"colour".matches("colou?r");   // true  (la 'u' è opzionale)
"color".matches("colou?r");    // true
"colo".matches("colo.*");      // true  (* = zero o più caratteri)
"abc".matches("[a-z]{3}");     // true  (esattamente 3 lettere)
"ab".matches("[a-z]{3}");      // false
"abcde".matches("[a-z]{3,5}"); // true  (da 3 a 5 lettere)
```

#### Quantificatori greedy vs lazy

Per default i quantificatori sono **greedy** (cercano di comprendere il più grande numero di caratteri possibile nel raggruppamento). Aggiungendo `?` diventano **lazy** (raggruppano soltanto il minimo numero di caratteri che rispetta la condizione):

```
.*    →  greedy: consuma tutto il possibile
.*?   →  lazy: consuma il minimo necessario
```

```java
String testo = "<b>ciao</b> e <b>mondo</b>";

// Greedy: il pattern .*  si espande il più possibile,
// quindi matches() restituisce true solo se l'intera stringa
// va dalla prima <b> all'ultima </b>
testo.matches(".*<b>.*</b>.*");   // true (match sull'intera stringa)

// Lazy: per verificare che esistano due tag distinti possiamo
// controllare che la stringa contenga almeno due aperture <b>
testo.matches(".*<b>.*</b>.*<b>.*</b>.*"); // true
```

> **Nota**: `String.matches()` confronta sempre l'intera stringa, quindi la differenza greedy/lazy è rilevante soprattutto quando si usa `Matcher.find()` per cercare occorrenze multiple all'interno di un testo più lungo.

Questa differenza rende possibile regolare il livello di precisione con cui si estraggono i token, andando a prendere stringhe più o meno lunghe a seconda del tipo di quantificatore usato.

---

### 7. Gruppi `(...)`

Le parentesi tonde **raggruppano** parte del pattern e permettono di:
- Catturare sottostringhe.
- Applicare quantificatori a un gruppo intero.
- Fare riferimento a gruppi nelle sostituzioni.

```java
String testo = "Data: 2024-07-15";

// Verifichiamo che la stringa contenga una data nel formato YYYY-MM-DD
boolean valida = testo.matches(".*\\d{4}-\\d{2}-\\d{2}.*"); // true

// Estraiamo le tre parti separando sul trattino,
// dopo aver isolato la sola data con replaceAll
String soloData = testo.replaceAll(".*?(\\d{4}-\\d{2}-\\d{2}).*", "$1");
String[] parti = soloData.split("-");

System.out.println("Anno:   " + parti[0]); // 2024
System.out.println("Mese:   " + parti[1]); // 07
System.out.println("Giorno: " + parti[2]); // 15
```

#### Gruppi non catturanti `(?:...)`
 
Se si vuole raggruppare senza catturare (per motivi di performance o chiarezza):
 
```java
// Verifica che la stringa sia un URL che inizia con http o https
"https://example.com".matches("(?:http|https)://\\S+"); // true
"ftp://example.com".matches("(?:http|https)://\\S+");   // false
```
 
---

### 8. Alternanza `|`

L'operatore `|` funziona come un **OR logico**.

```
gatto|cane   →  trova "gatto" oppure "cane"
```

```java
"gatto".matches("gatto|cane"); // true
"cane".matches("gatto|cane");  // true
"topo".matches("gatto|cane");  // false
```

Combinato con i gruppi:

```java
"http://example.com".matches("(?:http|https)://.*"); // true
```

---

### 9. Escape `\`

Per cercare un carattere che ha significato speciale nella regex (come `.`, `*`, `+`, `?`, `(`, `)`, `[`, `]`, `^`, `$`, `|`, `\`), occorre **eseguire l'escape** con `\`. In Java il backslash va raddoppiato:

```java
"3.14".matches("\\d+\\.\\d+");   // true  (\. = punto letterale)
"3X14".matches("\\d+\\.\\d+");   // false
"(test)".matches("\\(test\\)");  // true
```

---

### 10. Lookahead e Lookbehind (Asserzioni Zero-Width)

Permettono di controllare il contesto **senza consumare caratteri**. Questo significa che il token che viene estratto non comprenderà i caratteri compresi nel lookahead o lookbehind.

| Sintassi | Tipo | Significato |
|---|---|---|
| `(?=...)` | Lookahead positivo | Seguito da... |
| `(?!...)` | Lookahead negativo | Non seguito da... |
| `(?<=...)` | Lookbehind positivo | Preceduto da... |
| `(?<!...)` | Lookbehind negativo | Non preceduto da... |

```java
// Verifica che la stringa contenga almeno un numero seguito da "€"
"Prezzo: 99€, sconto: 10".matches(".*\\d+(?=€).*"); // true
"Prezzo: 99$, sconto: 10".matches(".*\\d+(?=€).*"); // false

// Estraiamo i soli valori numerici seguiti da "€"
// usando split per separare tutto ciò che non è "numero€"
String testo = "Prezzo: 99€, sconto: 10";
String[] token = testo.split("[^0-9€]+"); // ["", "99€", "10"]
for (String t : token) {
    if (t.matches("\\d+€")) {
        System.out.println(t.replace("€", "")); // 99
    }
}
```

---

## Esempio Completo: Validazione Email

```java
public class ValidazioneEmail {

    public static boolean isEmailValida(String email) {
        return email.matches("^[\\w.+-]+@[\\w-]+\\.[a-zA-Z]{2,}$");
    }

    public static void main(String[] args) {
        System.out.println(isEmailValida("mario.rossi@example.com")); // true
        System.out.println(isEmailValida("mario@.com"));              // false
        System.out.println(isEmailValida("@example.com"));            // false
        System.out.println(isEmailValida("utente+tag@mail.org"));     // true
    }
}
```

## Consigli

Quando si vuole validare un campo di testo libero che termini con un certo carattere separatore, conviene crearsi una classe con tutti i caratteri tranne quel separatore (SE NON PRESENTE ANCHE COME TESTO NORMALE).
```text
[^\t]+ valida tutti i caratteri fino alla prossima tabulazione
(ANALISI MATEMATICA T-1		) viene validata
```
