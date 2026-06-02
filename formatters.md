# I formattatori e la gestione dell'internazionalizzazione

- [I formattatori e la gestione dell'internazionalizzazione](#i-formattatori-e-la-gestione-dellinternazionalizzazione)
  - [1. La cultura locale in Java](#1-la-cultura-locale-in-java)
  - [2. Formattatori numerici](#2-formattatori-numerici)
    - [Parsing di stringhe](#parsing-di-stringhe)
  - [3. Formattatori di date e orari](#3-formattatori-di-date-e-orari)
    - [Costruzione](#costruzione)
      - [Costruzione con pattern personalizzato](#costruzione-con-pattern-personalizzato)
    - [Utilizzo](#utilizzo)
    - [Formattazione di date e orari assoluti](#formattazione-di-date-e-orari-assoluti)
    - [Parsing di date e orari](#parsing-di-date-e-orari)

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

---

## 3. Formattatori di date e orari

All'interno del package `java.time.format` è persente la classe **DateTimeFormatter**, che si occupa di formattare stringhe contenenti valori di tempo secondo il formato indicato dalla cultura locale applicata.

### Costruzione

La costruzione di questi formatter, come per quelli numerici, avviene tramite metodi factory. Tramite i tre metodi statici `ofLocalizedDate()`, `ofLocalizedTime()` e `ofLocalizedDateTime()` è possibile creare il formatter per date, orari o entrambi scegliendo tra 4 possibili stili per la data e 2 per l'orario.  
Gli stili sono inseriti tramite l'enum FormatStyle, composto da:
- SHORT: gg/mm/aaaa e hh:mm
- MEDIUM: gg * sigla del mese a 3 lettere * aaaa e hh:mm:ss
- LONG: gg * mese * aaaa
- FULL: * giorno * gg * mese * aaaa

```java
DateTimeFormatter formatterShort =
    DateTimeFormatter.ofLocalizedDate(FormatStyle.SHORT); // gg/mm/aaaa
DateTimeFormatter formatterMedium =
    DateTimeFormatter.ofLocalizedTime(FormatStyle.MEDIUM);// hh:mm:ss
DateTimeFormatter formatterLong =
    DateTimeFormatter.ofLocalizedDate(FormatStyle.LONG).withLocale(Locale.FRANCE); // gg *mese* aaaa con il mese in francese
DateTimeFormatter formatterFull =
    DateTimeFormatter.ofLocalizedDateTime(FormatStyle.FULL, FormatStyle.MEDIUM); // *giorno* gg *mese* aaaa hh:mm:ss
```

#### Costruzione con pattern personalizzato

Alternativamente si può utilizzare il metodo ofPattern, che costruisce il formatter seguendo il formato specificato nella stringa passata come parametro accompagnata dal Locale che si vuole utilizzare.  
Nella tabella sono elencati i simboli più utili per la creazione di pattern personalizzati.

| Simbolo | Significato | Esempio output |
|---------|-------------|----------------|
| `yyyy` | Anno a 4 cifre | `2026` |
| `yy` | Anno a 2 cifre | `26` |
| `MM` | Mese numerico (2 cifre) | `06` |
| `MMM` | Mese abbreviato | `giu` / `Jun` |
| `MMMM` | Mese per esteso | `giugno` / `June` |
| `dd` | Giorno del mese (2 cifre) | `01` |
| `d` | Giorno del mese (senza zero) | `1` |
| `HH` | Ora in formato 24h | `14` |
| `hh` | Ora in formato 12h | `02` |
| `mm` | Minuti | `30` |
| `ss` | Secondi | `45` |
| `SSS` | Millisecondi | `123` |
| `a` | AM/PM | `PM` |
| `EEE` | Giorno settimana abbreviato | `lun` / `Mon` |
| `EEEE` | Giorno settimana per esteso | `lunedì` / `Monday` |
| `z` | Timezone abbreviato | `CET` |
| `Z` | Offset timezone | `+0100` |

```java
// Data e ora
DateTimeFormatter f2 = DateTimeFormatter.ofPattern("dd/MM/yy HH:mm:ss");
// → "01/06/26 14:30:45"

// Con il giorno della settimana
DateTimeFormatter f3 = DateTimeFormatter.ofPattern("EEEE dd MMMM yyyy", Locale.ITALIAN);
// → "lunedì 01 giugno 2026"
```

### Utilizzo

La formattazione può avvenire nelle due modalità mostrate sotto

```java
DateTimeFormatter formatterShort = DateTimeFormatter.ofLocalizedDate(FormatStyle.SHORT); // gg/mm/aaaa

LocalDate now = LocalDate.now();
System.out.println(formatterShort.format(now)); //si chiama la format del formatter e gli si passa la data
System.out.println(now.format(formatterShort)); //si chiama la format della data e si passa il formattatore 
                                                //(vale anche per LocalTime e LocalDateTime)
```

### Formattazione di date e orari assoluti

Tramite i formatter `ofLocalizedDate`, `ofLocalizedTime` e `ofLocalizedDateTime` è possibile formattare anche oggetti del tipo `OffsetDateTime` e `ZonedDateTime` che rappresentano date e orari assoluti (altro non sono che una LocalDateTime a cui è stata aggiunta una specifica del fuso orario).  
La rappresentazione della data ovviamente non cambia da oggetti LocalDateTime e LocalDate, mentre per l'orario bisogna distinguere tra `OffsetDateTime`, la cui rappresentazione non cambia rispetto agli orari locali, e `ZonedDateTime`, per la quale vengono inseriti due nuovi formati **LONG** e **FULL** che aggiungono la specifica come acronimo o come nome completo del fuso orario.

```
19:27                                      | → FormatStyle.SHORT
19:27:13                                   | → FormatStyle.MEDIUM
19:27:13 CET                               | → FormatStyle.LONG
19:27:13 Ora standard dell'Europa centrale | → FormatStyle.FULL
```

Sono poi presenti all'interno di DateTimeFormatter alcuni formattatori preimpostati che seguono lo standand ISO nelle sue varianti. Questi sono particolarmente utilizzati nelle comunicazioni macchina-macchina.
```java
LocalDateTime now = LocalDateTime.now();
OffsetDateTime odt = OffsetDateTime.of(now, ZoneOffset.ofHours(1));
ZonedDateTime zdt = ZonedDateTime.of(now, ZoneId.of("CET"));

System.out.println(DateTimeFormatter.ISO_OFFSET_DATE_TIME.format(odt)); //2026-06-02T09:24:15.983582586+01:00
System.out.println(zdt.format(DateTimeFormatter.ISO_ZONED_DATE_TIME)); //2026-06-02T09:24:15.983582586+02:00[CET]
```

### Parsing di date e orari

Come per quelli numerici i formattatori hanno anche il compito di convertire stringhe formattate in oggetti contenenti il valore della stringa. Per date e orari il modo più comodo è utilizzare il metodo parse di LocalDate, LocalTime e tutte le altre classi inserendo come parametri la stringa da convertire e il formatter da utilizzare per convertirla.

```java
DateTimeFormatter formatter = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.MEDIUM, FormatStyle.MEDIUM);
LocalDateTime dateTime = LocalDateTime.parse("03 Giu 2026 12:34:19", formatter);
```