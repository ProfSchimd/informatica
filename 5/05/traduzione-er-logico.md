# Traduzione diagramma ER in schema relazionale

In questa lezione verrà affrontato la traduzione di un diagramma ER in uno schema relazionale. Oltre a fornire uno schema per il database relazionale risultante dalla traduzione, verranno anche presentate le query SQL per la creazione di ciascuna tabella dello schema.

```{attention} Attenzione
Alcune delle query SQL presentate richiedono che altre tabelle siano già state create. Di conseguenza, non è garantito che l'ordine con cui le query sono qui presentato sia il corretto ordine di esecuzione. La presentazione qui è fatta a scopo didattico e segue un ordine utile allo sviluppo dell'argomento.
```

Utilizzeremo come diagramma ER da tradurre quello riportato nella figura sottostante che rappresenta un'espansione di quanto sviluppato per DB scuola più volte affrontato durante le precedenti lezioni.

```{figure} /5/img//ER-db-scuola.svg
:label: db-scuola-er
```

## Da entità a tabella

Durante il primo passo della traduzione **trasformiamo ciascuna entità in una tabella**, gli attributi dell'entità diventano attributi della relazione e l'attributo chiave diventa la *chiave primaria* della nuova relazione. 

```{hint} Scelta della chiave primaria
Quando si crea la tabella per una data entità si può utilizzare l'insieme degli attributi chiave come chiave primaria della tabella. In certi casi, tuttavia, può essere conveniente creare una chiave primaria appositamente per la tabella creata (vedi [esempio sotto](#ex-traduzione-classe)).
```

Se l'entità prevede attributi composti, non potendo una colonna essere un attributo composto abbiamo due possibilità.
1. Inseriamo una colonna per ciascuna parte che compone l'attributo composto.
2. Inseriamo un unico attributo che verrà trattato come atomico.

:::{prf:example} Traduzione dell'entità `Studente`
:label: traduzione-studente
L'entità studente possiede quattro attributo di cui uno composta per il quale decidiamo di inserire un unico attributo `DataDiNascita`.[^1]

```{figure} /5/img/studente-relazione.svg
:label: tabella-studente
La traduzione in tabella della relazione `Studente` dallo schema [ER](#db-scuola-er).
```

Il comando SQL per creare la [tabella studente](#tabella-studente) è il seguente
```sql
CREATE TABLE Studente (
	CF CHAR(16) PRIMARY KEY, /* Codice Fiscale ha 16 caratteri */
	Nome VARCHAR(128),
	Cognome VARCHAR(128),
	DataDiNascita DATE
);
```
:::

:::{prf:example} Traduzione dell'entità `Classe`
:label: ex-traduzione-classe

In questo esempio traduciamo in tabella l'entità `Classe`. Anziché utilizzare `Livello` e `Sezione` per comporre la chiave primaria, si è deciso di inserire un nuovo attributo `IDClasse` che funge da identificativo e chiave primaria della tabella `Classe`.

```{figure} /5/img/classe-relazione.svg
:label: tabella-classe

La traduzione in tabella della relazione `Classe` dallo schema [ER](#db-scuola-er).
```

```sql
CREATE TABLE Classe (
	IDClasse INT PRIMARY KEY,
	Livello INTEGER,
	Sezione VARCHAR(8)
)
```
:::

### Entità deboli
Le entità deboli sono entità che, per essere definite, devono “appoggiarsi” ad un'entità forte, dalla quale prendono in prestito la chiave. Normalmente, se in un diagramma ER è presente un'entità debole questa ha diversi attributi associati, quando si traduce nello schema relazionale, tale entità diventa una tabella la cui chiave primaria è formata dalla chiave primaria dell'entità forte a supporto ed eventuali attributi che sono chiave parziale.

:::{prf:example} Traduzione dell'entità `Giustificazione`
L'entità debole `Giustificazione` è supportata dall'entità (forte) `Studente`. Inoltre `Giustificazione` ha l'attributo `Giorno` che è chiave parziale. Questo significa che una giustificazione è identificata dalla combinazione di studente e giorno (in questo database uno studente non può avere più giustificazione con lo stesso giorno, ma può avere più giustificazioni purché in giorni diversi). L'attributo della relazione `Presenta`, poiché si tratta di una relazione `1-N` ([vedi sotto](#relazione-1-N)) verrà inserita nella tabella `Giustificazione`.

```{figure} /5/img/tabelle-studente-giustificazione.svg

```

Il codice SQL che permette di creare la tabella `Giustificazione` della figura è presentato di seguito. Si noti la sintassi per definire una chiave primaria con due attributi.

```sql
CREATE TABLE Giustificazione(
	Studente CHAR(16),
	Giorno DATE,
	Stato INT,
	Firma VARCHAR(128),
	PRIMARY KEY (Studente, Giorno),
	FOREIGN KEY (Studente) REFERENCES Studente(CF)
)
```
:::

Nel caso in cui la relazione definente (quella che collega un'entità debole all'entità forte di supporto) sia di tipo `1-1`, è possibile inserire tutti gli attributi dell'entità debole e della relazione definente come attributi dell'entità forte.

### Specializzazioni
La traduzione di una specializzazione è un processo che dipende dal tipo (*overlap* o *disjoint*), dalla numerosità dell'entità specializzazione e dalla differenza in quanto ad attributi delle specializzazioni. Ci sono due possibilità quando si traduce una specializzazione.
1. Ogni entità specializzante viene tradotta in una tabella con gli stessi attributi della tabella generica più gli attributi della specializzazione;
2. La specializzazione diventa un attributo della tabella corrispondente all'entità generica e tutti gli attributi di tutte le entità specializzanti diventano attributi della stessa tabella.

Sono possibili anche altre traduzioni, ma le due sopra menzionate sono le più comunemente utilizzate.

Il criterio con cui scegliere se creare un'entità per ogni specializzazione (soluzione 1.) o inserire tutto tra gli attributi della entità generica (soluzione 2.) dipende da quanto diverse sono tra loro le entità specializzanti. Ad esempio se ogni entità specializzante contiene un solo attributo e il numero di queste entità è relativamente basso (poche unità), può aver senso utilizzare un'unica tabella mettendo tutti gli attributi. Quando qualche attributo non è significativo il suo valore sarà lasciato vuoto o impostato a `NULL`. Ovviamente, se gli attributi sono molti e molto diversi o le entità specializzanti sono tante, nella tabella finale, il rischio è che molti valori siano `NULL`, sprecando spazio e rendendo più complicata la gestione dell'entità.

:::{prf:example} La specializzazione di `Valutazione`
La relazione `Valutazione` nel [diagramma ER delle scuola](#db-scuola-er) presenta una specializzazione in `Voto` e `Giudizio`, la prima comprende un valore numerico che indica il voto (ad esempio in decimi) mentre la seconda comprende un commento testuale. Notiamo che la specializzazione è di tipo *overlap* ed è quindi possibile che una valutazione sia contemporaneamente un voto ed un giudizio. Avendo solo due entità specializzanti ciascuna con un solo attributo ed essendo la specializzazione di tipo overlap, una buona scelta è tradurre le tre relazioni `Valutazione`, `Voto` e `Giudizio` in un'unica tabella con i due attributi `Voto` e `Commento` (oltre agli attributi della relazione `Valutazione`). Potrà succedere che uno di questi due attributi sia di tipo `NULL`, ma si tratterebbe di un singolo valore, facilmente gestibile. Inoltre con questa soluzione possiamo facilmente implementare la sovrapposizione, basta avere entrambi i valori non `NULL`.

```{figure} /5/img/classe-valutazione.svg
Traduzione della specializzazione di `Valutazione` in `Voto` e `Giudizio` in un'unica tabella `Valutazione` che contiene tutti gli attributi di tutte le tre relazioni.
```

Il codice SQL per creare la tabella `Valutazione` sopra rappresentate è presentato nel seguente frammento di codice.s

```sql
CREATE TABLE Valutazione(
	Id INT PRIMARY KEY,
	Data DATE NOT NULL,
	Voto NUMERIC,
	Commento TEXT
)
```

:::

## Tradurre le relazioni
Svolta la traduzione delle entità in tabelle, si passa a riportare nello schema logico-relazionale le relazioni presenti nel diagramma ER. Tratteremo i vari i casi di relazioni in base alla cardinalità.

```{note} Nota
Durante la traduzione delle relazioni, può accadere che le tabelle create in corrispondenza delle entità debbano essere modificate, tipicamente per aggiungere attributi *chiavi esterne* o altri attributi provenienti da relazioni.
```

### Relazioni `1-1`
Una relazione `1-1` indica che c'è una corrispondenza unica tra due istanze delle entità partecipanti. Ad esempio la classe `2A` e il docente `Mario Rossi` sono in corrispondenza uno-a-uno nella relazione `Coordina`. In altre parole se conosco la classe so chi è il coordinatore e se conosco il nome del coordinatore so che classe coordina (è possibile che un docente non coordini alcuna classe).

Pensandoci, si vede che `Coordinatore` avrebbe potuto essere un attributo dell'entità `Classe`, tuttavia essendo `Coordinatore` un'istanza dell'entità `Insegnante` ed non potendo avere attributi che sono di un tipo entità (se non altro sarebbero attributi composti), `Coordina` è stata rappresentata come una relazione. Questo suggerisce che nella traduzione di una relazione `1-1`, non è necessario creare una nuova tabella, ma possiamo aggiungere una *chiave esterna* in una delle due tabelle entità, tale chiave esterna riferisce la chiave primaria dell'altra entità. Nel nostro esempio abbiamo scelto di inserire `Coordinatore` nella tabella `Classe`, questo attributo sarà chiave esterna rispetto a `Insegnante`, come si vede nella figura sotto.

:::{prf:example} La relazione `Coordina`

Per quanto detto sopra, traduciamo la relazione `Coordina` in un attributo `Coordinatore` che aggiungiamo alla tabella `Classe`.[^2] Tale attributo, inoltre sarà *chiave esterna* che farà riferimento alla chiave primaria di `Insegnante`. Graficamente indichiamo la chiave esterna con una freccia che esce dall'attributo `Coordinatore` e punta alla chiave primaria (`CF`) della tabella `Insegnante`.

```{figure} /5/img/tabelle-classe-insegnante.svg
La tabella `Classe` riferisce con l'attributo `Coordinatore` (l'identificativo di) chi coordina la classe stessa.
```

Per creare la tabella `Classe` con l'attributo `Coordinatore` chiave esterna si può utilizzare il seguente codice SQL.

```sql
CREATE TABLE Classe (
	IDClasse INT PRIMARY KEY,
	Livello INTEGER,
	Sezione VARCHAR(8),
	Coordinatore CHAR(16),
	FOREIGN KEY (Coordinatore) REFERENCES Insegnante(CF)
)
```

```{note} Nota
Rispetto all'[esempio sopra](#ex-traduzione-classe) abbiamo aggiunto l'attributo `Coordinatore` che deve essere **dello stesso tipo** dell'attributo `CF` della tabella insegnante. Inoltre abbiamo inserito il *vincolo di chiave esterna*
> ```sql
> FOREIGN KEY (Coordinatore) REFERENCES Insegnante(CF)
> ```
che ci permette di rendere esplicito il collegamento tra `Coordinatore` e `Insegnante.CF`.[^3]
```

Ovviamente affinché questa query funzioni, deve essere già stata definita la tabella `Insegnante`. Il codice SQL per tale definizione è riportato di seguito.

```sql
CREATE TABLE Insegnante (
 CF CHAR(16) PRIMARY KEY,
 Nome VARCHAR(128),
 Cognome VARCHAR(128),
 DataDiNascita DATE
);
```

:::

(relazione-1-N)=
### Relazioni `1-N`
Una relazione `1-N` può essere tradotta nello schema relazionale in modo simile alle relazioni `1-1`, la differenza è che non abbiamo scelta sulla tabella che dovrà ospitare la chiave esterna in quanto solo un lato della relazione ha cardinalità `1`. Ad esempio uno studente può appartenere ad una sola classe e quindi la relazione `Composta` del [diagramma ER di esempio](#db-scuola-er) si può tradurre inserendo in `Studente` la chiave esterna della classe di appartenenza.

Se la relazione presenta egli attributi, questi devono necessariamente essere inseriti nella stessa tabella in cui si è inserita la chiave esterna. Ad esempio l'attributo `Iscrizione` della relazione `Composta` (che rappresenta la data di iscrizione dello studente alla classe), deve essere inserito nella tabella `Studente`, non avrebbe senso inserirlo nella tabella `Classe` dato che ogni studente della classe può avere una data di iscrizione diversa.

:::{prf:example} La relazione `Composta`
La traduzione della relazione `Composta` utilizza una chiave esterna nella tabella `Studente` che riferisce la chiave primaria di `Classe`. Nella tabella `Studente` è stato anche inserito l'attributo `Iscrizione` della relazione `Composta`.

```{figure} /5/img/tabelle-studente-classe.svg
Le tabelle `Classe` e `Studente`, quest'ultima contiene l'attributo `Classe` chiave esterna che fa riferimento alla chiave di `Classe`.
```

Il codice SQL per creare la tabella `Studente` contenente i due attributi `Classe` e `Iscrizione` (oltre a quelli dell'entità `Studente`) è presentato di seguito.
```sql
CREATE TABLE Studente (
	CF CHAR(16) PRIMARY KEY,
	Nome VARCHAR(128),
	Cognome VARCHAR(128),
	DataDiNascita DATE,
	Classe INT NOT NULL,
	Iscrizione DATE NOT NULL,
	FOREIGN KEY (Classe) REFERENCES Classe(IdClasse)
);
```
:::

### Relazioni `N-M`
Nelle relazioni molti-a-molti, nessuno dei due lati della relazione ha cardinalità `1`, di conseguenza non è possibile tradurre la relazione semplicemente aggiungendo una chiave esterna in una delle due tabelle (come per le relazioni `1-1` e `1-N`).

Per questo tipo di relazioni, l'unica strada percorribile è creare una tabella che rappresenti la relazione stessa. Questa tabella, che chiamiamo *tabella di collegamento* o *tabella di relazione*, conterrà due chiavi esterne, una per ciascuna delle entità collegate. Se l'entità con chiave `x` è in relazione con l'entità di chiave `y`, allora `(x,y)` sarà presente nella tabella di collegamento.

Nel caso in cui la relazione comprendesse anche degli attributi, questi finirebbero, ovviamente, nella tabella di collegamento.

:::{prf:example} La relazione `Occupa`
Prendiamo dal nostro [digramma ER di esempio](#db-scuola-er) la relazione `Occupa` che collega `Classe` con `Aula`. La relazione è di cardinalità `N-M` in quanto una classe può occupare più aulee ed un'aula può essere occupata da più classi. La traduzione in schema relazionale prevede, quindi, la creazione di una tabelle, che chiamiamo `Occupa` che le chiavi esterne che riferiscono le due chiavi primarie delle entità `Classe` e `Aula`. Riportiamo di seguito la rappresentazione grafica di tale tabella.

```{figure} /5/img/tabelle-classe-occupa-aula.svg
```

```sql
CREATE TABLE Occupa (
	Classe INT,
	Aula INT,
	FOREIGN KEY (Classe) REFERENCES Classe(IdClasse),
	FOREIGN KEY (Aula) REFERENCES Aula(Numero)
)
```

Si noti che in questa tabella ci sono due chiavi esterne, ciascuna delle quali deve essere definita con un'apposita clausola `FOREIGN KEY`.

Per una corretta esecuzione  del codice sopra, è necessario che siano già presente le tabelle `Classe` (vedi sopra) e `Aula`. Quest'ultima può essere creata utilizzando il seguente codice SQL

```sql
CREATE TABLE Aula (
	Numero INT PRIMARY KEY,
	Capienza INT,
	Posizione Varchar(64)
)
```
:::

```{note} Tabella di collegamento
È possibile utilizzare una tabella di collegamento anche nel caso di relazioni `1-1` e `1-N`. Questo approccio è consigliato se la relazione contiene molti attributi che, se non si usasse una tabella di collegamento, andrebbero inseriti insieme alla chiave esterna, nella tabella di una delle due entità. Lo svantaggio di avere questi attributi risiede nella gestione della tabella le cui righe potrebbero avere molti valori `NULL`. In generale, è sempre sconsigliato mescolare nella stessa tabella concetti diversi.
```

### Relazioni ternarie
La traduzione di relazioni ternarie segue regole simili alla traduzione di relazioni binarie. 
Distinguiamo due case:
1. solo un lato della relazione ha cardinalità molti, gli altri due hanno cardinalità uno e
2. almeno due lati della relazione hanno cardinalità molti.

Nel primo caso si può scegliere di inserire in una delle due entità con cardinalità `1` le chiavi esterne verso le altre due entità.

Nel secondo caso si procede creando una tabella di collegamento che conterrà tutte e tre le chiavi esterne che riferiscono le tre entità coinvolte nella relazione.

:::{prf:example} La relazione `Insegna`
Nel [diagramma ER della scuola](#db-scuola-er) ci sono due relazioni ternarie `N-M-K`, in questo esempio di occupiamo di tradurre nello schema relazionale la relazione `Insegna` che mette in relazione `Insegnante`, `Materia` e `Classe`.

```{figure} /5/img/tabelle-relazione-insegna.svg
La traduzione nello schema logico della relazione ternaria `Insegna`.
```

La creazione della tabella `Insegna` può essere svolta dalla seguente istruzione SQL.
```sql
CREATE TABLE Insegna(
	Insegnante CHAR(16),
	Materia INT,
	Classe INT,
	FOREIGN KEY (Insegnante) REFERENCES Insegnante(CF),
	FOREIGN KEY (Materia) REFERENCES Materia(Codice),
	FOREIGN KEY (Classe) REFERENCES Classe(IdClasse) 
)
```

Per funzionare, la query sopra necessita che le tabelle `Insegnante`, `Classe` e `Materia` siano definite. Il codice per la creazione della tabella `Materia` è riportato di seguito.

```sql
CREATE TABLE Materia(
	Codice INT PRIMARY KEY,
	Nome VARCHAR(128),
	OreSettimanali INT
)
```
:::

[^1]: Questa scelta si giustifica anche dal fatto che in SQL esiste il tipo `DATE` che ci permette di trattare attributi di tipo data in modo efficiente fornendo anche operazioni basilari tra date (ad esempio confronto).

[^2]: È interessante notare che il nome dato all'attributo (`Coordinatore`) esprime *colui che compie l'azione di `Coordinare`* (il nome che ha la relazione del diagramma ER). 

[^3]: Abbiamo usato la sintassi `Tabella.Attributo` per fare riferimento all'attributo `CF` della tabella `Insegnante`. Si nota, tuttavia, che nella dichiarazione del vincolo su usa la notazione `Tabella(Attributi)`, nell'esempio `Insegnante(CF)`, questo ci permette di inserire referenze multi-attributo.