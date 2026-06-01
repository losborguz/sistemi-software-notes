# La gestione del tempo, il package `java.time`

- [La gestione del tempo, il package `java.time`](#la-gestione-del-tempo-il-package-javatime)
  - [1. Tipi di rappresentazioni del tempo](#1-tipi-di-rappresentazioni-del-tempo)
  - [2. Il package `java.time`](#2-il-package-javatime)
  - [3. Date e orari locali](#3-date-e-orari-locali)
    - [Aritmetica delle date](#aritmetica-delle-date)
    - [Aritmetica degli orari](#aritmetica-degli-orari)
  - [4. Period e Instant](#4-period-e-instant)
      - [Aritmetica dei periodi](#aritmetica-dei-periodi)
    - [Instant](#instant)
  - [5. Duration](#5-duration)
  - [6. Date e orari assoluti](#6-date-e-orari-assoluti)


## 1. Tipi di rappresentazioni del tempo

Quando indichiamo un concetto relativo al tempo, distinguiamo prima di tutto tra uno **assoluto** e uno **relativo**. Un concetto relativo può essere una **data, un'orario o una combinazione di entrambi** ma sempre **riferiti al luogo in cui si trova chi li esprime**.  
Un concetto assoluto è un **concetto relativo** del quale però viene indicato anche il luogo in cui bisogna considerarlo: il **fuso orario**, per date e orari.

Inoltre date e orari non sono le uniche cose che usiamo. Periodi di tempo, durate e calendari sono rappresentazioni del tempo che quotidianamente vengono usate.

---

## 2. Il package `java.time`

In Java tutti questi concetti vengono modellati da opportune classi contenute nel package `java.time`. I punti cardine del funzionamento delle classi nel package sono l'immutabilità degli oggetti creati (un valore non può essere modificato dopo la sua creazione) e la costruzione sempre indiretta degli oggetti tramite metodi factory.  
Le classi principali del package sono:
- LocalDate: data relativa (giorno/mese/anno)
- LocalTime: orario relativo (ore/minuti/secondi)
- LocalDateTime: data + orario relativo
- Period: durata relativa (in giorni, mesi, minuti ecc.)
- Instant: un instante di tempo sulla linea espresso in nanosecondi e relativo all'UTC
- Duration: una durata in nanosecondi tra due istanti di tempo
- OffsetDateTime: data + orario + offset in ore dall'UTC
- ZonedDateTime: data + orario + fuso orario

Gli enumerativi DayOfWeek e Month rappresentano invece i giorni della settimana e i mesi dell'anno. Tramite il metodo `getValue()` viene restituito il corrispondente valore numerico del giorno o del mese, partendo da 1 e non da 0 (in quel caso si può usare `ordinal()`).  
Tramite il metodo statico `of()` invece si può ottenenere la costante enumerativa dato un valore intero compreso nel range 1-7 o 1-12.

---

## 3. Date e orari locali

```java
//Costruttori
LocalDate xmas2020 = LocalDate.of(2020, 12, 25); //tre parametri int
LocalDate xmas2016 = LocalDate.of(2016, Month.DECEMBER, 25); //mese come costante di Month
LocalTime noon = LocalTime.of(12, 0);
LocalDateTime xmas2020noon = LocalDateTime.of(xmas2020,noon); //un oggetto LocalDate e uno LocalTime

LocalDateTime now = LocalDateTime.now(); //restituisce la data e ora attuale prendendola dal sistema

//Getters base
int year = xmas2020.getYear(); 
Month month = xmas2020.getMonth();
int dayOfMonth = xmas2020.getDayOfMonth();

int hour = noon.getHour();
int minute = noon.getMinute();
int second = noon.getSecond();
int nanosecond = noon.getNano();

//Alcuni getters avanzati
int dayOfYear = xmas2020.getDayOfYear();
DayOfWeek dayOfWeek = xmas2020.getDayOfWeek();
boolean isLeapYear = xmas2020.isLeapYear();
```

### Aritmetica delle date
- *plusDays, plusWeeks, plusMonths, plusYears*: chiedono un long che specifica quanti giorni/settimane/mesi/anni aggiungere e **restituiscono un nuovo oggetto LocalDate** modificato
- *minusDays, minusWeeks, minusMonths, minusYears*: uguale a plus* ma la quantità viene sottratta
- *withDayOfMonth, withDayOfYear, withMonth, withYear*: chiedono un intero che specifica il nuovo giorno del mese/giorno dell'anno/mese/anno e **restituiscono un nuovo oggetto** con il valore sostituito.

```java
LocalDate xmas2016 = LocalDate.of(2016, Month.DECEMBER, 25);
LocalDate xmas2020 = xmas2016.plusYears(4);
LocalDate xmas2018 = xmas2016.withYear(2018);
```

### Aritmetica degli orari

Come per le date anche per gli orari ci sono i metodi plus*, minus* e with*, che lavorano allo stesso modo di quelli per le LocalDate.

```java
LocalTime noon = LocalTime.of(12,0);
LocalTime whatTime = noon.plusMinutes(10).minusHours(13).withSeconds(10);
```

---

## 4. Period e Instant

La classe Period modella un lasso di tempo misurato in anni, mesi, settimane e giorni volutamente primo di un concetto preciso di durata data la variabilità degli elementi al suo interno, come i mesi.

```java
//Costruttori of* e between
Period monthlySubscription = Period.ofMonths(1);

LocalDate start = LocalDate.of(2025, Month.JUNE, 15);
Period holiday = Period.between(start, start.plusWeeks(1));

//Getters
int days = holiday.getDays();
int months = holiday.getMonths();
int years = holiday.getYears();

//Modifica della durata
Period quarterlySubscription = montlySubscription.plusMonths(2);
Period shorterHoliday = holiday.minusDays(3);
```

#### Aritmetica dei periodi

I metodi addTo e subtractFrom permettono di ricavarsi nuove date partendo da una di riferimento passata come argomento e sommandogli/sottraendogli il periodo sul quale si richiama il metodo. I metodi lavorano con tutti i tipi di oggetti temporali, restituendo quindi un contenitore generico: Temporal. E' necessario quindi un cast esplicito alla classe specifica.

```java
Period p1 = Period.ofMonths(2).plusDays(3);

LocalDate d1 = (LocalDate)p1.addTo(xmas2017);
LocalDate d2 = (LocalDate)p1.subtractFrom(xmas2017);
```

### Instant

Instant rappresenta un punto sula linea assoluta del tempo, codificato come il numero di nanosecondi trascorsi dalla mezzanotte dell'1 gennaio 1970, ora di Greenwitch.  
Un oggetto può essere costruito specificando manualmente in numero di nanosecondi o tramite il metodo `now()` che inserisce l'istante attuale. Si possono istanziare nuovi oggetti modificando il contenuto di uno già esistente tramite `plus*()` e `minus*()`, si possono comparare due oggetti tramite `isBefore(other)`, `isAfter(other)` e `equals(other)`, tramite i getter `get*()` si possono ottenere varie rappresentazioni alternative dell'istante.

---

## 5. Duration

Duration rappresenta un lasso di tempo tra due istanti misurato in nanosecondi.  

La costruzione avviene anche qua tramite i metodi factory `of` e derivati (`ofDays`, `ofHours`, ecc., che accettano valori interi) e anche il metodo `between` con il quale si costruisce la durata tra due oggetti temporali (che quindi implementano l'interfaccia Temporal).

Tramite i metodi `plus*`, `minus*` e `with*` si istanziano nuove Duration il cui valore è la modifica di quello dell'oggetto sul quale è stato richiamato il metodo. Questi metodi possono essere applicati in sequenza secondo quella che prende il nome di **fluent interface**.
```java
Duration dx = Duration.ofDays(1).plusHours(3).minusMinutes(4).minusSeconds(10);
```

I metodi toDays(), toHours(), toMinutes() e gli altri to*() estraggono dalla duration il numero totale di unità di misura indicata nel nome del metodo, **escludendo però la quantità di tempo espressa in unità minori di quella considerata**. In altre parole se si vogliono estrarre le ore da una durata verranno considerati **i giorni e le ore**, mentre i minuti e le grandezze più piccole saranno escluse.
```
Durata di 1 giorno + 2 ore + 55 minuti + 50 secondi
- toHours = 26
- toMinutes = 26*60+55 = 1615
- toMillis = (1615*60+50)*1000 = 96950 * 1000
```

---

## 6. Date e orari assoluti

Le date e gli orari assoluti permettono di rispondere a domande come:
- che ore sono adesso a New York?
- quando iniziano le lezioni del 2° ciclo a Bologna nel 2026?
- a che ora parte il volo EJ842?

Tutte domande che richiedono di tenere in considerazione non solo il tempo, ma anche il luogo relativo all'evento richiesto. Il volo EJ842 partirà da una città che avrà un suo fuso orario, da tenere in considerazione.

Le due classi `OffsetDateTime` e `ZonedDateTime` permettono di rappresentare date e orari assoluti indicando o la distanza in ore dall'UTC o il relativo fuso orario. 
```java
LocalDateTime inizio = LocalDateTime.of( 2026, 2, 17, 9, 0);
OffsetDateTime offsInizio = OffsetDateTime.of(inizio, ZoneOffset.ofHours(1));
ZonedDateTime zInizio = ZonedDateTime.of( inizio, ZoneId.of("CET"));1
```