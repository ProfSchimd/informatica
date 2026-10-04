---
title: "ER: Cardinalità e Partecipazione"
---

Nella [lezione precedente](/5/02/er-intro.md) sono stati introdotti i concetti di *entità* e *relazione* e la loro rappresentazione nei diagrammi ER. Si è visto che le relazioni mettono in collegamento (*in relazione*) due o più entità, spesso è utile sapere anche la **cardinalità** di tale relazioni vale a dire quante *istanze* delle entità partecipano ad una data relazione. Questa lezione presenta i concetti di *vincolo di cardinalità* e *vincolo di partecipazione* ad una relazione.

(vincoli-di-cardinalita)=
##  Vincoli di cardinalità
I **vincoli di cardinalità** delle relazioni esprimono il *massimo* numero di volte una specifica istanza dell'entità partecipa alla relazione. Ad esempio, nella relazione `Composta` tra le entità `Studente` e `Classe` *ciascun studente appartiene al massimo ad una classe*, allo stesso tempo, ogni classe è composta da un numero qualsiasi di studenti. In questo caso parliamo di una relazione `1:N` (`1` a `N`).  Ovviamente esistono diversi tipi di vincoli di cardinalità che discutiamo sotto.

### Relazioni `1:1`
Quando il vincolo di cardinalità di una relazione impone che ogni *istanza* di ciascuna entità partecipi *al più* una volta, diciamo che la relazione è di tipo `1:1` che chiamiamo *uno ad uno*. Ad esempio nel database della scuola possiamo imporre che una classe abbia al massimo un coordinatore di classe e che un insegnante coordini al massimo una classe. 

```{figure} /5/img/er-cardinalita-11.svg
Relazione `Coordina` con vincolo di cardinalità `1:1` tra `Insegnante` e `Classe`. In questa relazione ogni insegnante può coordinare al massimo una classe ed ogni classe può essere coordinata da massimo un insegnante.
```

Una relazione `1:1` può essere facilmente rappresentata nel modello logico relazionale da un attributo che può essere messo in una delle due tabelle associate all'entità (possiamo avere l'attributo `coordinatore_di_classe` in `Classe` o l'attributo `coordina` in `Insegnante`).

### Relazioni `1:N`
Una relazione in cui il le istanze di un'entità hanno vincolo unitario mentre l'altra entità non imponga un limite di relazione `1:N`[^1], che chiamiamo *uno a `N`* o *uno a molti*. Ad esempio una classe è `Composta` da molti studente, ma ogni `Studente` appartiene ad una sola classe, la corrispondenze relazione (vedi figura sotto) è `1:N`. 

```{figure} /5/img/er-cardinalita-1N.svg
Relazione `Composta` con vincolo di cardinalità `1:N` tra `Classe` e `Studente`. La classe è composta da `N` studenti mentre ogni studente può appartenere a massimo una sola classe.
```

Una relazione `1:N` può essere rappresentata nel modello logico relazionale utilizzando un attributo nella tabella corrispondente all'entità la cui partecipazione è singola. Ad esempio, possiamo inserire in `Studente` l'attributo `class_di_appartenenza`, sarebbe più problematico inserire un attributo in `Classe` in quanto questo dovrebbe contenere tutti gli studenti della classe e non sarebbe, perciò, un attributo atomico.

### Relazioni `N:M`

Un relazione in cui entrambe le entità partecipanti non abbia vincolo sul massimo sul numero istanze partecipanti è una relazione `N:M` (*`N` a `M`* o *molti a molti*)[^2]. Ad esempio un `Insegnante` può insegnare più di una materia e la stessa materia può essere insegnate da più di un insegnante.  

```{figure} /5/img/er-cardinalita-NM.svg
Relazione `Insegna` con vincolo di cardinalità `N:M` tra `Insegnante` e `Materia`. Ogni insegnante può insegnare più materie ed una stessa materia può essere insegnata da più insegnanti.
```

Una relazione `N:M` può essere tradotta nel modello logico relazionale utilizzando una *tabella di collegamento* nella quale ogni istanza della relazione rappresenta una riga. Questa tabella avrà chiavi esterne verso le tabelle di entità e l'insieme di queste chiavi esterne costituirà la chiave primaria della tabella di collegamento stessa.


## Vincoli di partecipazione
Certe entità possono esistere unicamente in virtù della loro partecipazione ad una relazione, ad esempio uno `Studente` deve essere iscritto ad una `Classe` perché possa essere presente nel database della scuola. 

In generale è importante sapere il *numero minimo* di relazioni a cui un entità deve partecipare, nel caso della relazione tra `Classe` e `Studente` questo numero minimo è 1, indicando che ogni studente deve appartenere ad *almeno una* classe. Come visto [sopra](#vincoli-di-cardinalita), abbiamo anche un vincolo sul numero massimo di classi (anche questo pari a 1); questi due vincoli insieme ci dicono che uno studente deve essere iscritto ad *esattamente* una classe. 

:::{prf:definition} Partecipazione Totale
Si chiama **partecipazione totale** il vincolo per cui un'istanza di un'entità deve partecipare ad almeno una volta alla relazione. Nell'esempio sopra, `Studente` ha un vincolo di partecipazione totale con la relazione `Composta`. 
:::

:::{prf:definition} Partecipazione Parziale
Si chiama **partecipazione parziale** il vincolo per cui un'istanza di un'entità può o meno partecipare alla relazione. Ad esempio l'entità `Insegnante` ha un vincolo di partecipazione parziale con la relazione `Insegna` collegata all'entità `Materia`.
:::

Nel diagramma ER, la partecipazione totale è indicata utilizzando una doppia linea tra l'entità e la relazione a cui essa partecipa in modo totale. Una linea singola, in questo notazione, indica una partecipazione parziale. Sotto possiamo vedere come modificare la relazione `Composta` aggiungendo il vincolo di partecipazione totale tra `Studente` e `Composta`. Il fatto che il collegamento tra `Classe` e`Composta` sia indicato con una singola linea indica che può esistere un'entità `Classe` anche senza che questa partecipi alla relazione `Composta` (cioè una classe senza studenti).

```{figure} /5/img/er-partecipazione.svg
Indicazione della *partecipazione totale* dell'entità `Studente` alla relazione `Composta` con l'entità `Classe`.
```

Per determinare se un'entità ha una partecipazione totale in un relazione basta rispondere alla seguente domanda: *È obbligatorio che ogni istanza dell'entità partecipi alla relazione?*. Vediamo questo “trucco” in azione con degli esempi.
* Ogni `Studente` deve essere `Iscritto` ad una `Classe`? Si, allora la partecipazione è totale.
* Ogni `Insegnante` deve `Insegnare` in una `Classe`? No, allora la partecipazione è parziale.
* Ogni `Cliente` deve `Acquistare` qualche prodotto? No (clienti appena iscritti), allora la partecipazione è parziale.
* Ogni `Album` deve essere stato `Pubblicato` da un `Artista`?. Si , allora la partecipazione è totale.

---

:::{attention} Attenzione 
I vincoli di cardinalità e di partecipazione sono delle proprietà dell'entità e non della singola istanza. Per capire meglio conviene pensare a tali vincoli come presenti *per ogni istanza* di un'entità. Ad esempio il fatto che il vincolo di cardinalità singola su `Studente` indica che *ogni studente* deve avere al massimo una classe mentre il vincolo di partecipazione dice che *ogni studente* deve avere almeno una classe. In altre parole quando imponiamo un vincolo ad un'entità stiamo, in realtà, imponendo in vincolo su ciascuna istanza dell'entità.
:::

[^1]: Raramente si usa anche `N:1` nel caso in cui si voglia esplicitare quale tra prima e la seconda entità riceve il vincolo `N` e quale il vincolo `1`.
[^2]: A volte si usa anche `N:N`, ma questo non significa che il numero di istanze partecipanti deve essere lo stesso per entrambe le entità, è sempre preferibile usare la notazione `N:M`.