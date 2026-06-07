# JavaFX: guida all'utilizzo dei componenti

## 1. Layout panes

Sono i contenitori di javafx, hanno quindi la funzione di contenere al loro interno i singoli componenti dell'interfaccia, disponendoli in modalità differenti in base a quale si sceglie. I più importanti sono HBox, VBox, Border Pane, Stack Pane, Grid Pane e Flow Pane

- **FlowPane**: elementi disposti in un flusso, regolati in base all'altezza o alla larghezza a seconda del tipo flusso (orizzontale o verticale)
- **HBox**: elementi disposti uno di fianco all'altro. L'altezza del pannello è uguale all'altezza del componente più alto
- **Vbox**: elementi disposti uno sotto l'altro. La larghezza del pannello è uguale alla larghezza del componente più largo
- **BorderPane**: pannello diviso in cinque parti tob, bottom, left, right e center
- **StackPane**: elementi disposti uno sopra l'altro come in uno stack. L'inserimento è sempre in testa alla pila
- **TilePane**: pannello composto da "tiles" di dimensione uguale, ognuna ospita un componente
- **GridPane**: pannello a griglia di dimensioni variabili, modificate all'inserimento di nuovi elementi

### HBox
```java
TextField textField = new TextField();       
Button playButton = new Button("Play");       
Button stopButton = new Button("stop");

HBox hbox = new HBox();
hbox.setSpacing(10); //set spaziatura tra gli elementi
hbox.getChildren().addAll(textField, playButton, stopButton); //aggiunta degli elementi al pannello

//set margini per i singoli elementi
hbox.setMargin(textField, new Insets(20, 20, 20, 20)); 
hbox.setMargin(playButton, new Insets(20, 20, 20, 20)); 
hbox.setMargin(stopButton, new Insets(20, 20, 20, 20));  
```

### BorderPane
```java
BorderPane bPane = new BorderPane();   

bPane.setTop(new TextField("Top")); 
bPane.setBottom(new TextField("Bottom")); 
bPane.setLeft(new TextField("Left")); 
bPane.setRight(new TextField("Right")); 
bPane.setCenter(new TextField("Center"));
```

### GridPane
```java
Text text1 = new Text("Email");       
Text text2 = new Text("Password"); 
TextField textField1 = new TextField();       
TextField textField2 = new TextField();  
Button button1 = new Button("Submit"); 
Button button2 = new Button("Clear");  

GridPane gridPane = new GridPane();    
gridPane.setMinSize(400, 200);  //set dimensioni minime del pannello
gridPane.setPadding(new Insets(10, 10, 10, 10));  //set padding 
gridPane.setVgap(5); //set gap tra le righe
gridPane.setHgap(5); //set gap tra le colonne
gridPane.setAlignment(Pos.CENTER); //set allineamento del pannello rispetto alla scena

//inserimento dei componenti
gridPane.add(text1, 0, 0); 
gridPane.add(textField1, 1, 0); 
gridPane.add(text2, 0, 1);       
gridPane.add(textField2, 1, 1); 
gridPane.add(button1, 0, 2); 
gridPane.add(button2, 1, 2);  
```

### TilePane
```java
Button[] buttons = new Button[] { 
    new Button("SunDay"), 
    new Button("MonDay"), 
    new Button("TuesDay"), 
    new Button("WednesDay"), 
    new Button("ThursDay"), 
    new Button("FriDay"), 
    new Button("SaturDay")  
};

TilePane tilePane = new TilePane();   
tilePane.setPrefColumns(4); //set numero di tiles per riga (dette colonne)
tilePane.getChildren().addAll(buttons);
```

---
## 2. Label
 
### Descrizione
`Label` è un componente di sola visualizzazione utilizzato per mostrare testo statico o un'icona nell'interfaccia. Non è interattivo di per sé, ma può essere associato a un altro controllo (es. un `TextField`) come etichetta descrittiva tramite `setLabelFor()`.
 
### Costruzione
```java
// Label con solo testo
Label label = new Label("Nome utente:");
 
// Label con testo e icona
ImageView icon = new ImageView(new Image("icona.png"));
Label label = new Label("Attenzione", icon);
 
// Label vuota (testo impostato dopo)
Label label = new Label();
label.setText("Ciao!");
```
 
### Modifiche di base
```java
// Impostare/ottenere il testo
label.setText("Nuovo testo");
String testo = label.getText();
 
// Font e dimensione
label.setFont(Font.font("Arial", FontWeight.BOLD, 16));
 
// Colore del testo
label.setTextFill(Color.DARKBLUE);
 
// Stile CSS inline
label.setStyle("-fx-font-size: 14px; -fx-text-fill: #2c3e50;");
 
// Dimensioni preferite
label.setPrefWidth(200);
 
// A capo automatico (wrapping) quando il testo è troppo lungo
label.setWrapText(true);
 
// Allineamento del testo
label.setAlignment(Pos.CENTER);
label.setTextAlignment(TextAlignment.CENTER);
 
// Associare la label a un controllo (accessibilità + tasto mnemonico)
TextField campo = new TextField();
label.setLabelFor(campo);
 
// Tooltip
label.setTooltip(new Tooltip("Inserisci il tuo nome"));
```
 
---
 
## 3. ImageView
 
### Descrizione
`ImageView` è un nodo JavaFX che visualizza un'immagine nella scena. Lavora insieme alla classe `Image` (che carica e decodifica l'immagine) e `ImageView` (che la mostra e ne controlla dimensioni e trasformazioni).
 
### La classe `Image`
`Image` rappresenta l'immagine in memoria e si occupa del caricamento. Va creata separatamente da `ImageView`.
 
```java
// Da path assoluto su disco
Image img = new Image("file:///C:/utente/foto.png");
 
// Da URL
Image img = new Image("https://esempio.com/immagine.jpg");
 
// Con dimensioni di caricamento ridotte (ottimizza la memoria)
Image img = new Image("/immagini/foto.png", 200, 150, true, true);
//                                           ^     ^    ^     ^
//                                       width height preserveRatio smooth
```
 
### Costruzione
```java
// ImageView vuoto (immagine impostata dopo)
ImageView imageView = new ImageView();
imageView.setImage(new Image("https://esempio.com/immagine.jpg"));
 
// ImageView con immagine diretta
ImageView imageView = new ImageView(new Image("https://esempio.com/immagine.jpg"));
 
// Da URL diretto (scorciatoia)
ImageView imageView = new ImageView("https://esempio.com/immagine.jpg");
```
 
### Modifiche di base
```java
// Impostare una nuova immagine
imageView.setImage(nuovaImmagine);
 
// Dimensioni di visualizzazione
imageView.setFitWidth(200);
imageView.setFitHeight(150);
 
// Mantenere le proporzioni originali
imageView.setPreserveRatio(true);
 
// Interpolazione smooth (immagine più nitida se ridimensionata)
imageView.setSmooth(true);
 
// Rotazione
imageView.setRotate(45);
 
// Opacità
imageView.setOpacity(0.8);
```

---

## 4. TextField

### Descrizione
`TextField` è un campo di input a riga singola che consente all'utente di inserire e modificare testo. È il componente di base per raccogliere stringhe brevi come nomi, email o numeri.

### Costruzione
```java
// Campo vuoto
TextField textField = new TextField();

// Campo con testo iniziale
TextField textField = new TextField("Testo iniziale");
```

### Modifiche di base
```java
// Impostare/ottenere il testo
textField.setText("Nuovo testo");
String testo = textField.getText();

// Testo suggerito (placeholder)
textField.setPromptText("Inserisci il tuo nome...");

// Larghezza preferita
textField.setPrefWidth(200);

// Rendere non modificabile
textField.setEditable(false);

// Stile CSS inline
textField.setStyle("-fx-font-size: 14px; -fx-border-color: #3498db;");
```

### Gestione degli eventi
```java
// Evento al cambio del testo (ad ogni carattere)
textField.textProperty().addListener((observable, oldValue, newValue) -> {
    System.out.println("Testo cambiato: " + newValue);
});

// Evento alla pressione di Invio
textField.setOnAction(event -> {
    System.out.println("Invio premuto: " + textField.getText());
});
```

---

## 5. TextArea

### Descrizione
`TextArea` è un campo di input multiriga che permette l'inserimento di testi lunghi, come commenti, descrizioni o note. A differenza di `TextField`, supporta l'accapo automatico e la barra di scorrimento.

### Costruzione
```java
// Area vuota
TextArea textArea = new TextArea();

// Area con testo iniziale
TextArea textArea = new TextArea("Testo di esempio\nSeconda riga");
```

### Modifiche di base
```java
// Impostare/ottenere il testo
textArea.setText("Nuovo contenuto");
String contenuto = textArea.getText();

// Testo suggerito
textArea.setPromptText("Scrivi qui...");

// Dimensioni preferite
textArea.setPrefWidth(300);
textArea.setPrefHeight(150);

// Numero di righe/colonne visibili
textArea.setPrefRowCount(5);
textArea.setPrefColumnCount(30);

// Abilita/disabilita il ritorno a capo automatico
textArea.setWrapText(true);

// Rendere non modificabile
textArea.setEditable(false);

// Stile CSS inline
textArea.setStyle("-fx-font-family: 'Courier New'; -fx-font-size: 13px;");
```

### Gestione degli eventi
```java
// Evento al cambio del testo
textArea.textProperty().addListener((observable, oldValue, newValue) -> myHandler(newValue));

// Focus perso (utile per validare l'input)
textArea.focusedProperty().addListener((obs, wasFocused, isNowFocused) -> {
    if (!isNowFocused) System.out.println("Testo inserito: " + textArea.getText());
});
```

---

## 6. Button

### Descrizione
`Button` è un pulsante cliccabile che esegue un'azione al click. È il componente di interazione più comune in JavaFX.

### Costruzione
```java
// Pulsante con testo
Button button = new Button("Clicca qui");

// Pulsante con icona
ImageView icon = new ImageView(new Image("icona.png"));
Button button = new Button("Salva", icon);
```

### Modifiche di base
```java
// Impostare il testo
button.setText("Nuovo testo");

// Dimensioni
button.setPrefWidth(120);
button.setPrefHeight(40);

// Disabilitare il pulsante
button.setDisable(true);

// Pulsante di default (risponde a Invio) e di annullamento (risponde a Esc)
button.setDefaultButton(true);
button.setCancelButton(true);

// Stile CSS inline
button.setStyle("-fx-background-color: #3498db; -fx-text-fill: white; -fx-font-size: 14px;");

// Tooltip
button.setTooltip(new Tooltip("Clicca per salvare"));
```

### Gestione degli eventi
```java
// Click del mouse
button.setOnAction(event -> {
    System.out.println("Pulsante cliccato!");
});
```

---

## 7. ComboBox

### Descrizione
`ComboBox<T>` è un menù a tendina che mostra un elenco di opzioni tra cui l'utente può sceglierne una. Supporta sia valori predefiniti che input libero (in modalità editabile).

### Costruzione
```java
// ComboBox con lista di stringhe
ComboBox<String> comboBox = new ComboBox<>();
comboBox.getItems().addAll("Opzione 1", "Opzione 2", "Opzione 3");

// Oppure con ObservableList
ObservableList<String> opzioni = FXCollections.observableArrayList("A", "B", "C");
ComboBox<String> comboBox = new ComboBox<>(opzioni);
```

### Modifiche di base
```java
// Selezionare un valore di default
comboBox.setValue("Opzione 1");
// oppure per indice
comboBox.getSelectionModel().select(0);

// Testo suggerito quando nulla è selezionato
comboBox.setPromptText("Scegli...");

// Rendere editabile (l'utente può scrivere un valore personalizzato)
comboBox.setEditable(true);

// Ottenere il valore selezionato
String selezionato = comboBox.getValue();

// Aggiungere o rimuovere elementi
comboBox.getItems().add("Nuova opzione");
comboBox.getItems().remove("Opzione 2");

// Larghezza
comboBox.setPrefWidth(180);
```

### Gestione degli eventi
```java
// Cambio di selezione
comboBox.setOnAction(event -> {
    System.out.println("Selezionato: " + comboBox.getValue());
});

// Tramite listener sulla proprietà
comboBox.valueProperty().addListener((obs, oldVal, newVal) -> {
    System.out.println("Da " + oldVal + " a " + newVal);
});
```

---

## 8. ColorPicker

### Descrizione
`ColorPicker` è un selettore di colore che apre una finestra grafica per scegliere un colore dalla tavolozza o inserendo un valore esadecimale. Restituisce un oggetto `Color`.

### Costruzione
```java
// Con colore iniziale di default
ColorPicker colorPicker = new ColorPicker();

// Con colore iniziale specifico
ColorPicker colorPicker = new ColorPicker(Color.BLUE);
```

### Modifiche di base
```java
// Impostare un colore
colorPicker.setValue(Color.RED);

// Ottenere il colore selezionato
Color colore = colorPicker.getValue();

// Ottenere il valore esadecimale
String hex = String.format("#%02X%02X%02X",
    (int)(colore.getRed() * 255),
    (int)(colore.getGreen() * 255),
    (int)(colore.getBlue() * 255));

// Stile del pulsante
colorPicker.setStyle("-fx-color-label-visible: false;"); // nasconde l'etichetta del colore
```

### Gestione degli eventi
```java
// Colore cambiato
colorPicker.setOnAction(event -> {
    Color c = colorPicker.getValue();
    System.out.println("Colore scelto: R=" + c.getRed() + " G=" + c.getGreen() + " B=" + c.getBlue());
});

// Tramite listener
colorPicker.valueProperty().addListener((obs, oldColor, newColor) -> {
    System.out.println("Nuovo colore: " + newColor);
});
```

---

## 9. DatePicker

### Descrizione
`DatePicker` è un selettore di data che mostra un calendario a comparsa per scegliere un giorno. Restituisce un oggetto `LocalDate`.

### Costruzione
```java
// Senza data iniziale
DatePicker datePicker = new DatePicker();

// Con data iniziale
DatePicker datePicker = new DatePicker(LocalDate.now());
```

### Modifiche di base
```java
// Impostare una data
datePicker.setValue(LocalDate.of(2025, 6, 15));

// Ottenere la data selezionata
LocalDate data = datePicker.getValue();

// Formato di visualizzazione personalizzato
datePicker.setConverter(new StringConverter<LocalDate>() {
    DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd/MM/yyyy");

    @Override
    public String toString(LocalDate date) {
        return date != null ? formatter.format(date) : "";
    }

    @Override
    public LocalDate fromString(String string) {
        return (string != null && !string.isEmpty()) ? LocalDate.parse(string, formatter) : null;
    }
});

// Rendere editabile il campo di testo
datePicker.setEditable(false);

// Testo suggerito
datePicker.setPromptText("gg/mm/aaaa");
```

### Gestione degli eventi
```java
// Data cambiata
datePicker.setOnAction(event -> {
    System.out.println("Data selezionata: " + datePicker.getValue());
});

// Tramite listener
datePicker.valueProperty().addListener((obs, oldDate, newDate) -> {
    System.out.println("Nuova data: " + newDate);
});
```

---

## 10. Spinner

### Descrizione
`Spinner<T>` è un componente che permette di selezionare un valore incrementandolo o decrementandolo tramite frecce. Supporta valori interi, decimali e liste di oggetti.

### Costruzione
```java
// Spinner intero: min, max, valore iniziale
Spinner<Integer> spinner = new Spinner<>(0, 100, 50);

// Spinner decimale: min, max, valore iniziale, passo
Spinner<Double> spinner = new Spinner<>(0.0, 10.0, 5.0, 0.5);

// Spinner con lista di valori
SpinnerValueFactory<String> factory =
    new SpinnerValueFactory.ListSpinnerValueFactory<>(
        FXCollections.observableArrayList("Lun", "Mar", "Mer", "Gio", "Ven")
    );
Spinner<String> spinner = new Spinner<>(factory);
```

### Modifiche di base
```java
// Rendere editabile il campo di testo
spinner.setEditable(true);

// Ottenere il valore corrente
Integer valore = spinner.getValue();

// Modificare il valore programmaticamente
spinner.getValueFactory().setValue(75);

// Larghezza
spinner.setPrefWidth(100);

// Wrapping (torna al minimo dopo il massimo)
SpinnerValueFactory.IntegerSpinnerValueFactory factory =
    (SpinnerValueFactory.IntegerSpinnerValueFactory) spinner.getValueFactory();
factory.setWrapAround(true);
```

### Gestione degli eventi
```java
// Valore cambiato
spinner.valueProperty().addListener((obs, oldValue, newValue) -> {
    System.out.println("Nuovo valore: " + newValue);
});
```

---

## 11. Slider

### Descrizione
`Slider` è una barra scorrevole che permette di selezionare un valore numerico all'interno di un intervallo trascinando un cursore. È ideale per impostazioni come volume, luminosità o zoom.

### Costruzione
```java
// min, max, valore iniziale
Slider slider = new Slider(0, 100, 50);
```

### Modifiche di base
```java
// Impostare/ottenere il valore
slider.setValue(75);
double valore = slider.getValue();

// Orientamento (orizzontale o verticale)
slider.setOrientation(Orientation.VERTICAL);

// Mostrare i tick (indicatori e etichette)
slider.setShowTickMarks(true);
slider.setShowTickLabels(true);
slider.setMajorTickUnit(25);   // tick principale ogni 25
slider.setMinorTickCount(4);   // 4 tick minori tra i principali

// Snap to tick (si ferma ai tick)
slider.setSnapToTicks(true);

// Dimensioni
slider.setPrefWidth(250);
```

### Gestione degli eventi
```java
// Valore cambiato (si aggiorna anche durante il trascinamento)
slider.valueProperty().addListener((obs, oldVal, newVal) -> {
    System.out.println("Valore: " + newVal.intValue());
});

// Solo quando l'utente smette di trascinare
slider.valueChangingProperty().addListener((obs, wasChanging, isChanging) -> {
    if (!isChanging) System.out.println("Valore finale: " + slider.getValue());
});
```

---

## 12. ProgressBar

### Descrizione
`ProgressBar` mostra il progresso di un'operazione come una barra orizzontale riempita. Il valore va da `0.0` (0%) a `1.0` (100%). Con valore `-1` mostra un'animazione indeterminata.

### Costruzione
```java
// Barra al 50%
ProgressBar progressBar = new ProgressBar(0.5);

// Barra indeterminata (animata)
ProgressBar progressBar = new ProgressBar(ProgressBar.INDETERMINATE_PROGRESS);
// oppure
ProgressBar progressBar = new ProgressBar(-1);
```

### Modifiche di base
```java
// Impostare il progresso (0.0 - 1.0)
progressBar.setProgress(0.75);

// Ottenere il progresso
double progresso = progressBar.getProgress();

// Dimensioni
progressBar.setPrefWidth(300);
progressBar.setPrefHeight(20);

// Stile (colore della barra)
progressBar.setStyle("-fx-accent: #27ae60;");
```

### Gestione degli eventi
```java
// Monitorare il cambio di progresso
progressBar.progressProperty().addListener((obs, oldVal, newVal) -> {
    if (newVal.doubleValue() >= 1.0) System.out.println("Completato!");
});

// Associare a un Task in background
Task<Void> task = new Task<>() {
    @Override protected Void call() throws Exception {
        for (int i = 0; i <= 100; i++) {
            updateProgress(i, 100);
            Thread.sleep(50);
        }
        return null;
    }
};
progressBar.progressProperty().bind(task.progressProperty());
new Thread(task).start();
```

---

## 13. ProgressIndicator

### Descrizione
`ProgressIndicator` visualizza il progresso sotto forma di cerchio (o arco) invece di una barra orizzontale. Come `ProgressBar`, supporta la modalità determinata (0.0–1.0) e quella indeterminata.

### Costruzione
```java
// Indicatore al 60%
ProgressIndicator indicator = new ProgressIndicator(0.6);

// Indicatore indeterminato (ruota)
ProgressIndicator indicator = new ProgressIndicator();
// valore di default è -1 (indeterminato)
```

### Modifiche di base
```java
// Impostare il progresso
indicator.setProgress(0.9);

// Dimensioni
indicator.setPrefSize(80, 80);

// Stile CSS
indicator.setStyle("-fx-progress-color: #e74c3c;");
```

### Gestione degli eventi
```java
// Monitorare il completamento
indicator.progressProperty().addListener((obs, oldVal, newVal) -> {
    if (newVal.doubleValue() >= 1.0) System.out.println("Operazione completata!");
});

// Associare a un Task (come ProgressBar)
progressIndicator.progressProperty().bind(task.progressProperty());
```

---

## 14. FileChooser

### Descrizione
`FileChooser` non è un nodo JavaFX (non si aggiunge alla scena), ma una finestra di dialogo di sistema per aprire o salvare file. Va istanziato e mostrato in risposta a un'azione dell'utente.

### Costruzione
```java
FileChooser fileChooser = new FileChooser();
```

### Modifiche di base
```java
// Titolo della finestra
fileChooser.setTitle("Seleziona un file");

// Directory iniziale
fileChooser.setInitialDirectory(new File(System.getProperty("user.home")));

// Nome file iniziale (per il salvataggio)
fileChooser.setInitialFileName("documento.txt");

// Filtri per estensione
fileChooser.getExtensionFilters().addAll(
    new FileChooser.ExtensionFilter("File di testo", "*.txt"),
    new FileChooser.ExtensionFilter("Immagini", "*.png", "*.jpg", "*.gif"),
    new FileChooser.ExtensionFilter("Tutti i file", "*.*")
);
```

### Utilizzo (apertura e salvataggio)
```java
// Aprire UN file
File file = fileChooser.showOpenDialog(primaryStage);
if (file != null) System.out.println("Aperto: " + file.getAbsolutePath());

// Aprire PIÙ file
List<File> files = fileChooser.showOpenMultipleDialog(primaryStage);

// Salvare un file
File file = fileChooser.showSaveDialog(primaryStage);
if (file != null) System.out.println("Salvato in: " + file.getAbsolutePath());
```

> **Nota**: `FileChooser` non ha gestione di eventi tradizionale; il risultato è restituito direttamente dal metodo `show*Dialog()`.

---

## 15. CheckBox

### Descrizione
`CheckBox` è una casella di spunta con tre stati possibili: **selezionato**, **deselezionato** e **indeterminato** (il terzo stato va abilitato esplicitamente). È usato per scelte binarie o multiple indipendenti.

### Costruzione
```java
// CheckBox con etichetta
CheckBox checkBox = new CheckBox("Accetto i termini");

// CheckBox selezionato di default
CheckBox checkBox = new CheckBox("Newsletter");
checkBox.setSelected(true);
```

### Modifiche di base
```java
// Impostare/ottenere lo stato
checkBox.setSelected(true);
boolean isSelezionato = checkBox.isSelected();

// Abilitare il terzo stato (indeterminato)
checkBox.setAllowIndeterminate(true);
checkBox.setIndeterminate(true);
boolean isIndeterminato = checkBox.isIndeterminate();

// Disabilitare
checkBox.setDisable(true);

// Stile
checkBox.setStyle("-fx-font-size: 14px;");
```

### Gestione degli eventi
```java
// Cambio di stato
checkBox.setOnAction(event -> {
    System.out.println("Selezionato: " + checkBox.isSelected());
});

// Tramite listener sulla proprietà
checkBox.selectedProperty().addListener((obs, wasSelected, isSelected) -> {
    System.out.println("Cambiato a: " + isSelected);
});
```

---

## 16. ToggleButton

### Descrizione
`ToggleButton` è un pulsante con due stati: **premuto** (selezionato) e **non premuto**. A differenza di un `Button` normale, mantiene il suo stato fino a un nuovo click. Può essere raggruppato con altri `ToggleButton` tramite `ToggleGroup`.

### Costruzione
```java
ToggleButton toggleButton = new ToggleButton("Grassetto");
```

### Modifiche di base
```java
// Impostare lo stato
toggleButton.setSelected(true);

// Ottenere lo stato
boolean isPremuto = toggleButton.isSelected();

// Aggiungere a un gruppo (uno solo attivo per volta)
ToggleGroup group = new ToggleGroup();
ToggleButton btn1 = new ToggleButton("Vista lista");
ToggleButton btn2 = new ToggleButton("Vista griglia");
btn1.setToggleGroup(group);
btn2.setToggleGroup(group);
btn1.setSelected(true); // selezione iniziale

// Stile
toggleButton.setStyle("-fx-selected: -fx-accent;");
```

### Gestione degli eventi
```java
// Cambio di stato
toggleButton.setOnAction(event -> {
    System.out.println("Stato: " + (toggleButton.isSelected() ? "attivo" : "inattivo"));
});

// Listener sulla selezione
toggleButton.selectedProperty().addListener((obs, wasSelected, isSelected) -> {
    System.out.println("Toggle: " + isSelected);
});

// Cambio nel gruppo
group.selectedToggleProperty().addListener((obs, oldToggle, newToggle) -> {
    if (newToggle != null)
        System.out.println("Attivo: " + ((ToggleButton) newToggle).getText());
});
```

---

## 17. RadioButton

### Descrizione
`RadioButton` è un pulsante di selezione circolare usato per scelte mutuamente esclusive. Va sempre usato insieme a un `ToggleGroup` per garantire che un solo `RadioButton` alla volta sia selezionato all'interno del gruppo.

### Costruzione
```java
ToggleGroup group = new ToggleGroup();

RadioButton rb1 = new RadioButton("Opzione A");
RadioButton rb2 = new RadioButton("Opzione B");
RadioButton rb3 = new RadioButton("Opzione C");

rb1.setToggleGroup(group);
rb2.setToggleGroup(group);
rb3.setToggleGroup(group);

rb1.setSelected(true); // selezione iniziale
```

### Modifiche di base
```java
// Impostare/ottenere lo stato
rb2.setSelected(true);
boolean isSelezionato = rb1.isSelected();

// Ottenere il RadioButton selezionato dal gruppo
RadioButton selezionato = (RadioButton) group.getSelectedToggle();
System.out.println("Scelto: " + selezionato.getText());

// Disabilitare un'opzione
rb3.setDisable(true);
```

### Gestione degli eventi
```java
// Cambio di selezione nel gruppo
group.selectedToggleProperty().addListener((obs, oldToggle, newToggle) -> {
    if (newToggle != null) {
        RadioButton rb = (RadioButton) newToggle;
        System.out.println("Selezionato: " + rb.getText());
    }
});

// Listener su un singolo RadioButton
rb1.selectedProperty().addListener((obs, wasSelected, isSelected) -> {
    if (isSelected) System.out.println("Opzione A attivata");
});
```

---

## 18. ListView

### Descrizione
`ListView<T>` mostra una lista scorrevole di elementi selezionabili. Supporta selezione singola o multipla e può essere personalizzata con celle grafiche personalizzate.

### Costruzione
```java
// Con lista di stringhe
ListView<String> listView = new ListView<>();
listView.getItems().addAll("Elemento 1", "Elemento 2", "Elemento 3");

// Con ObservableList
ObservableList<String> items = FXCollections.observableArrayList("A", "B", "C");
ListView<String> listView = new ListView<>(items);
```

### Modifiche di base
```java
// Aggiungere/rimuovere elementi
listView.getItems().add("Nuovo elemento");
listView.getItems().remove("A");

// Selezionare un elemento
listView.getSelectionModel().select(0);         // per indice
listView.getSelectionModel().select("Elemento 1"); // per valore

// Ottenere l'elemento selezionato
String selezionato = listView.getSelectionModel().getSelectedItem();

// Orientamento verticale/orizzontale
listView.setOrientation(Orientation.HORIZONTAL);

// Dimensioni
listView.setPrefHeight(200);
listView.setPrefWidth(150);

// Disabilitare
listView.setDisable(true);
```

### Selezione multipla
```java
// Abilitare la selezione multipla
listView.getSelectionModel().setSelectionMode(SelectionMode.MULTIPLE);

// Selezionare più elementi programmaticamente
listView.getSelectionModel().selectIndices(0, 2, 4);
listView.getSelectionModel().selectAll();

// Ottenere tutti gli elementi selezionati
ObservableList<String> selezionati = listView.getSelectionModel().getSelectedItems();
selezionati.forEach(System.out::println);
```

### Gestione degli eventi
```java
// Cambio di selezione (singola)
listView.getSelectionModel().selectedItemProperty().addListener((obs, oldVal, newVal) -> {
    System.out.println("Selezionato: " + newVal);
});

// Cambio di selezione (multipla)
listView.getSelectionModel().getSelectedItems().addListener(
    (ListChangeListener<String>) change -> {
        System.out.println("Selezione aggiornata: " + change.getList());
    }
);

// Doppio click su un elemento
listView.setOnMouseClicked(event -> {
    if (event.getClickCount() == 2 && !listView.getSelectionModel().isEmpty()) {
        System.out.println("Doppio click su: " + listView.getSelectionModel().getSelectedItem());
    }
});
```

---

## 19. BarChart

### Descrizione
`BarChart` visualizza dati comparativi tramite barre verticali (o orizzontali). Ogni serie di dati è una collezione di coppie chiave-valore, dove la chiave è la categoria sull'asse X e il valore è la misura sull'asse Y.

### Costruzione
```java
// Definire gli assi
CategoryAxis xAxis = new CategoryAxis();
xAxis.setLabel("Mese");

NumberAxis yAxis = new NumberAxis();
yAxis.setLabel("Vendite");

// Creare il grafico
BarChart<String, Number> barChart = new BarChart<>(xAxis, yAxis);
barChart.setTitle("Vendite mensili");

// Creare una serie di dati
XYChart.Series<String, Number> serie = new XYChart.Series<>();
serie.setName("Prodotto A");
serie.getData().add(new XYChart.Data<>("Gen", 150));
serie.getData().add(new XYChart.Data<>("Feb", 200));
serie.getData().add(new XYChart.Data<>("Mar", 180));

// Aggiungere la serie al grafico
barChart.getData().add(serie);

// Aggiungere più serie
XYChart.Series<String, Number> serie2 = new XYChart.Series<>();
serie2.setName("Prodotto B");
serie2.getData().add(new XYChart.Data<>("Gen", 90));
serie2.getData().add(new XYChart.Data<>("Feb", 130));
serie2.getData().add(new XYChart.Data<>("Mar", 110));
barChart.getData().addAll(serie, serie2);
```

### Modifiche di base
```java
// Dimensioni
barChart.setPrefSize(600, 400);

// Nascondere la legenda
barChart.setLegendVisible(false);

// Animazione
barChart.setAnimated(false);

// Spaziatura tra le barre
barChart.setBarGap(5);
barChart.setCategoryGap(20);

// Gap della categoria
xAxis.setCategories(FXCollections.observableArrayList("Gen", "Feb", "Mar"));
```

### Gestione degli eventi
```java
// Click su una barra
for (XYChart.Data<String, Number> dato : serie.getData()) {
    dato.getNode().setOnMouseClicked(event -> {
        System.out.println("Cliccato: " + dato.getXValue() + " = " + dato.getYValue());
    });
}

// Hover su una barra
for (XYChart.Data<String, Number> dato : serie.getData()) {
    dato.getNode().setOnMouseEntered(event ->
        dato.getNode().setStyle("-fx-bar-fill: #e74c3c;"));
    dato.getNode().setOnMouseExited(event ->
        dato.getNode().setStyle(""));
}
```

---

## 20. BubbleChart

### Descrizione
`BubbleChart` visualizza dati tridimensionali: la posizione X e Y di ogni bolla rappresenta due variabili, mentre il **raggio** della bolla rappresenta una terza variabile. Usa `XYChart.Data<Number, Number>` con un extra value per il raggio.

### Costruzione
```java
// Definire gli assi
NumberAxis xAxis = new NumberAxis(0, 100, 10);
xAxis.setLabel("Asse X");

NumberAxis yAxis = new NumberAxis(0, 100, 10);
yAxis.setLabel("Asse Y");

// Creare il grafico
BubbleChart<Number, Number> bubbleChart = new BubbleChart<>(xAxis, yAxis);
bubbleChart.setTitle("Analisi dati");

// Creare una serie
XYChart.Series<Number, Number> serie = new XYChart.Series<>();
serie.setName("Dataset 1");

// Aggiungere bolle: Data(x, y, raggio)
serie.getData().add(new XYChart.Data<>(20, 30, 15));
serie.getData().add(new XYChart.Data<>(50, 60, 25));
serie.getData().add(new XYChart.Data<>(80, 40, 10));

bubbleChart.getData().add(serie);
```

### Modifiche di base
```java
// Dimensioni
bubbleChart.setPrefSize(600, 400);

// Nascondere la legenda
bubbleChart.setLegendVisible(true);

// Disabilitare l'animazione
bubbleChart.setAnimated(false);

// Stile delle bolle via CSS
bubbleChart.setStyle(".chart-bubble { -fx-bubble-fill: rgba(52, 152, 219, 0.6); }");
```

### Gestione degli eventi
```java
// Click su una bolla
for (XYChart.Data<Number, Number> dato : serie.getData()) {
    dato.getNode().setOnMouseClicked(event -> {
        System.out.printf("Bolla cliccata: X=%.1f, Y=%.1f%n",
            dato.getXValue().doubleValue(),
            dato.getYValue().doubleValue());
    });
}
```

---

## 21. PieChart

### Descrizione
`PieChart` visualizza proporzioni di dati come fette di un cerchio. Ogni fetta corrisponde a un `PieChart.Data` con un nome e un valore numerico; la dimensione della fetta è proporzionale al valore rispetto al totale.

### Costruzione
```java
// Creare i dati
ObservableList<PieChart.Data> dati = FXCollections.observableArrayList(
    new PieChart.Data("Java", 35),
    new PieChart.Data("Python", 30),
    new PieChart.Data("JavaScript", 20),
    new PieChart.Data("Altro", 15)
);

// Creare il grafico
PieChart pieChart = new PieChart(dati);
pieChart.setTitle("Linguaggi di programmazione");
```

### Modifiche di base
```java
// Dimensioni
pieChart.setPrefSize(400, 400);

// Mostrare/nascondere le etichette
pieChart.setLabelsVisible(true);

// Distanza delle etichette dal bordo
pieChart.setLabelLineLength(20);

// Grafico ad anello (donut)
pieChart.setStyle(".chart-pie { -fx-pie-color: transparent; }"); // non standard, meglio via CSS

// Esplodere una fetta
dati.get(0).getNode().setStyle("-fx-pie-color: #3498db; -fx-background-insets: 0;");
// esplosione tramite traslazione
PieChart.Data fettaEspansa = dati.get(0);

// Abilitare/disabilitare la legenda
pieChart.setLegendVisible(true);
pieChart.setLegendSide(Side.RIGHT);

// Angolo iniziale del grafico
pieChart.setStartAngle(90);

// Senso orario
pieChart.setClockwise(true);

// Aggiungere/rimuovere dati dinamicamente
pieChart.getData().add(new PieChart.Data("Kotlin", 10));
pieChart.getData().remove(0);
```

### Gestione degli eventi
```java
// Click su una fetta
for (PieChart.Data dato : pieChart.getData()) {
    dato.getNode().setOnMouseClicked(event -> {
        System.out.println("Fetta: " + dato.getName() + " = " + dato.getPieValue());
    });
}

// Hover su una fetta (evidenziazione)
for (PieChart.Data dato : pieChart.getData()) {
    dato.getNode().setOnMouseEntered(event -> {
        dato.getNode().setStyle("-fx-opacity: 0.7;");
    });
    dato.getNode().setOnMouseExited(event -> {
        dato.getNode().setStyle("-fx-opacity: 1.0;");
    });
}

// Cambio di valore programmatico
dato.setPieValue(45); // aggiorna automaticamente il grafico
```

---

*Guida generata per JavaFX 17+. Per approfondimenti consultare la [documentazione ufficiale JavaFX](https://openjfx.io/javadoc/17/).*