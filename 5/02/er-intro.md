# Diagramma Entità-Relazione (ER)

Questa lezione introduce il **Diagramma Entità-Relazione (ER)** per la *progettazione concettuale di un database* utilizzando un semplice esempio di *analisi dei requisiti*. Verranno discussi i concetti di [entità](#entity) e [relazione](#relation) che saranno approfonditi in lezioni successive.

```{tip} Esempio: “Estratto di un'analisi dei requisiti”
Si deve creare un *registro elettronico* per la gestione delle classi all'interno di una scuola superiore. Ogni classe è composta di studenti i quali sono identificati per mezzo del loro codice fiscale. Ogni classe ha un nome (ad esempio `1A`) suddiviso in sezione e livello. Oltre al codice fiscale, agli studenti sono associati nome, cognome, data di nascita, indirizzo di casa e numero di telefono dello studente (opzionale).
```

(entity)=
## Entità e attributi
La progettazione concettuale individua i *concetti* della realtà di interesse da rappresentare nel database. Nel diagramma ER questi concetti diventano **entità** e **relazioni**. Le entità si individuano identificando gli oggetti (non necessariamente fisici) che vanno descritti nel database.

Nel nostro esempio possiamo individuare i seguenti elementi da inserire nel database:
* `classe` è l'entità che rappresenta un insieme di studenti, a tale entità è anche associato un `nome`,
* `studente` è l'entità che rappresenta gli *studenti* della scuola, dai requisiti si legge che si identifica mediante il `codice fiscale` e che va mantenuta un'anagrafica con diversi dati riguardanti lo studente.

Un ulteriore concetto da considerare è quello di *appartenenza* di uno studente ad una classe che, tuttavia, esprime una *relazione* tra le due *entità* `classe` e `studente`.


```{figure} /5/img/er-single-entity.svg
Entità `Studente` con i corrispondenti attributi. Si noti che `CF` (codice fiscale) è *attributo chiave* (sottolineato). Inoltre, l'attributo `Data di Nascita` è composta dagli attributi `Giorno`, `Mese` e `Anno`.
```
Nell'immagine sopra vediamo la rappresentazione grafica dell'entità `Studente`.

* Ogni entità di un diagramma ER è rappresentata da un rettangolo contenente il nome dell'entità.
* Un entità può avere degli attributi che la descrivono e che sono rappresentati da ovali collegati all'entità.
* Uno o più **attributi chiave** identificano l'entità (una *chiave primaria*) e sono indicati sottolineandone il nome.

```{note} Nota
Gli attributi non contengono indicazione sul tipo o sul dominio, ma solo il nome e l'entità collegata. Inoltre gli attributi non hanno un ordine particolare, quindi vanno considerati un *insieme*, questa è una differenza con il modello logico relazionale nel quale una tabella possiede degli attributi che hanno un ordine specifico.
```


:::{tip}
Nell'identificare le entità in un'analisi dei requisiti si può ricorrere a qualche trucchetto.
* Di norma, le entità si esprimono come sostantivi (meglio al singolare): *classe*, *studente*, *voto*, ... Spesso i verbi hanno un ruolo di collegamento tra entità e sono meglio descritti come [relazioni](#relation) in un diagramma ER.
* Gli elementi che sono ulteriormente caratterizzati da dettagli (attributi) sono candidati ad essere entità. Ad esempio allo studente si sono associati nome, cognome, ... 
:::

(attributi-composti)=
### Attributi composti

Nell'immagine sopra l'entità `Studente` contiene un attributo `Data di Nascita` che è *composto* di più parti. Le parti che compongono l'attributo composto (`Giorno`, `Mese` e `Anno`, nell'esempio) vengono rappresentate come un attributo ciascuno connessi all'attributo composto anziché all'entità (o alla relazione) che li contiene.


```{attention} Attenzione
Nel modello relazionale gli attributi devono essere atomici e non possono essere composti. Nella progettazione concettuale, quindi nello schema ER, questo non è necessario. Se un attributo è composto, come `Data di Nascita` per l'entità `Studente`, esso va rappresentato come tale. Nella fase di traduzione da modello concettuale a modello logico relazionale si dovrà rendere atomici gli attributi composti.
```

### L'entità `Classe`

Per completare il nostro esempio introduciamo una seconda entità che chiamiamo `Classe`. Questa entità rappresenta il *gruppo classe* (ad esempio la `2A`), il suo schema ER è presentato sotto.
```{figure} /5/img/entity-classe.svg
L'entità `Classe` contenente due attributi `Id` e `Nome`, questo ultimo attributo composto.
```

(relation)=
## Relazione
In una realtà di interesse le entità non sono elementi isolati, ma sono tra lore collegate da **relazioni**. Nella progettazione concettuale si devono individuare e rendere esplicite le relazioni significative per il database. Nel diagramma ER una relazione si indica con un rombo collegato a due o più relazioni (tipicamente due o tre, mai di più).

```{figure} /5/img/relazione-esempio.svg
La relazione `Comprende`
```

La figura sopra mostra la relazione `Comprende` che mette in collegamento le entità `Classe` e `Studente` dalle quali sono stati tolti gli attributi per rendere l'immagine più chiara e leggibile.


```{attention} Attenzione
Nel ricercare entità e relazioni dello schema concettuale va sempre tenuto presente che:
* le entità non sono **mai collegate direttamente**, ma tramite relazioni;
* le **relazioni non sono mai collegate** tra di loro;
* Mentre possono esistere entità non collegate a relazioni, **non si possono avere relazioni collegate a nessuna o a solo una entità** (cosa metterebbe in relazione in questo caso?).
```

```{note} Osserva
Nell'identificare le relazione aiuta a ricercare i *verbi* che collegano due o più entità già individuate. Ad esempio nel testo sopra si legge *Ogni classe è composta di studenti ...* in cui le due entità `classe` e `studente` sono legate (messe in relazione) dal verbo *è composta*.
```

### Attributi di una relazione

```{figure} /5/img/relazione-con-attributo.svg
La relazione `Composta` con l'attributo `Iscrizione`.
```

Sopra vediamo la stessa relazione `composta` in cui è stato aggiunto un attributo che indica la *data di iscrizione* di uno studente alla classe. Come si vede l'attributo, indicato con un ovale come tutti gli attributi, è collegato alla relazione simile a quanto accade con gli attributi di entità.


Ovviamente gli attributi di una relazione possono essere composti, la descrizione è simile a quanto descritto [sopra](#attributi-composti) per gli attributi composti di un'entità.