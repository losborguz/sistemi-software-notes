# Lambda expression

## 1. Introduzione

Da sempre nei linguaggi di programmazione abbiamo avuto una chiara distinzione tra codice e dati, tanto da memorizzarli alcune volte in aree di memoria ben distinte. Con il passare del tempo però ci si è iniziati a chiedere se questa fosse effettivamente la rappresentazione corretta.  
Alternativamente infatti si può vedere una funzione come un oggetto, che al suo interno conserva il suo nome, i suoi argomenti e il suo codice e che ha la possibilità di mettersi in esecuzione. In questo modo la funzione può essere assegnata a variabili e passata come parametro ad altre funzioni, creando così molti nuovi scenari a volte molto utili.

Nasce così il concetto di oggetto-funzione o **lambda expression**, un tipo particolare di espressione con una sua struttura definita.
```java
f = x -> 2*x+1;
g = x -> 2*Math.sin(x/2);
h = (x,y) -> Math.sqrt(x*x + y*y);
```
Questa nuova categoria di oggetti rappresenta funzioni senza un nome esplicito, caratterizzate quindi dai parametri in ingresso e dal corpo della funzione (il codice).

---

## 2. Tipi di lambda expressions

In informatica come in matematica le lambda expression sono distinte in base al **dominio, codominio e numero degli argomenti**. Per esprimere questi raggruppamenti Java adotta un approccio nominale, dando ossia un nome ad ogni tipo possibile (serie di nomi standard + alcuni specifici per i tipi primitivi più usati come int, double e long). In particolare:

- `XXXOperator`: dominio e codominio sono dello stesso tipo
- `Function`: dominio e codominio sono di tipi diversi
- `UnaryOperator`: operatori a singolo argomento
- `BinaryOperator`: operatori a due argomenti
- `Function`: funzioni a singolo argomento
- `BiFunction`: funzioni a due argomenti

Nomi specifici come `IntUnaryOperator`, `DoubleFunction`, `LongBinaryOperator` e altri rappresentano casi molto specifici perchè parecchio usati.

---

## 3. Chiamata delle funzioni

Se si volesse seguire la notazione matematica al 100%, la chiamata delle lambda non dovrebbe cambiare rispetto ai metodi tradizionali. Purtroppo in Java questo non è possibile, in quanto per motivi di progettazione del nuovo costrutto si è deciso di dotare questi oggetti di un metodo `applyXXX()` (il nome varia in base al tipo di lambda da apply() per Function ad applyAsInt() per i casi più specifici) a cui vengono passati i valori da utilizzare.

```java
//Chiamata metodi tradizionali
System.out.println(f(5));
System.out.println(g(Math.PI/2));
System.out.println(h(3,4));

//Dichiarazione e chiamata lambda
IntUnaryOperator f = x -> 2*x+1
DoubleUnaryOperator g = x -> 2*Math.sin(x/2);
DoubleBinaryOperator h = (x,y) -> Math.sqrt(x*x + y*y);

System.out.println(f.applyAsInt(5));
System.out.println(g.applyAsDouble(Math.PI/2));
System.out.println(h.applyAsDouble(3,4));
```

---

## 4. Esempi di utilizzo

Come detto prima, l'utilità delle lambda expression si ottiene quando le si utilizza per passare come argomento ad una funzione un pezzo di codice, rendendola personalizzabile al bisogno.  
In questo modo per esempio possiamo costruire una calcolatrice estremamente flessibile che permette di realizzare qualsiasi operazione si voglia passando al metodo di calcolo sia i dati che l'operazione stessa.

```java
public static double calc(DoubleUnaryOperator op, double arg) {
    return op.applyAsDouble(arg);
}

DoubleUnaryOperator f1 = x -> 2*Math.sin(x/2);
DoubleUnaryOperator f2 = x -> Math.sqrt(x);
DoubleUnaryOperator f3 = x -> (x+3)/(x-2);

System.out.println(calc(f1, Math.PI));
System.out.println(calc(f2, 1.0/9.0));
System.out.println(calc(f3, 3.0));
```

`calc()` stesso può diventare una lambda expression, ma questa variante diventa più difficile da leggere e oltretutto non si riesce in questo modo a nascondere apply() dentro al metodo statico, in quanto va richiamato per far eseguire il calcolo.

```java
//Method reference
System.out.println(calc(Math::sin, Math.PI));
System.out.println(calc(Math::sqrt, 1.0/9.0));

/*
Il riferimento a metodo è una scorciatoia che a volte può essere molto utile. Consente di richiamare il metodo con la sintassi sopra senza includere né () nè argomenti. E' quindi molto più snella. 
*/
```