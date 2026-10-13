---
title: Relazioni e Tuple
---

## Il modello relazionale
La maggior parte dei database attualmente in utilizzo sono basati sul modello logico dei *database relazionali*. Anche se oggi altri tipi di database chiamato *NoSQL* (Not Only SQL) sono popolari, la conoscenza approfondita del modello relazionale e delle tecniche di progettazione di database relazionali, sono ancora competenze fondamentali per uno sviluppatore (specialmente, ma non esclusivamente, per sviluppatori backend).

Il modello relazionale si basa sul concetto di *relazione* che verrà presentato nella [prossima sezione](#relazione-e-tupla) insieme al concetto di tupla. Un database è normalmente composto da un insieme di relazioni, tale insieme viene detto **schema** della base di dati. 

Questa lezione introduce i concetti fondamentali dei database relazionali, in lezioni successive tratteremo:
* [le chiavi e i vincoli di uno schema relazionale](chiavi-integrita.md);
* [algebra relazionale](algebra-relazionale.md).

In moduli successivi affronteremo SQL il linguaggio per creare e gestire database relazionali. Conoscenza adeguata di SQL è spesso requisito per posizioni di lavorative in ambito di sviluppo software.

```{important} Importante
Avere una comprensione ”teorica” del modello relazionale permette di capire meglio come utilizzare sul campo strumenti come SQL. È quindi importante acquisire il materiale presentato in questo modulo cosicché nei moduli successivi ci si possa pienamente concentrare sull'utilizzo del linguaggio SQL e sulla progettazione di database relazionali.
```

(relazione-e-tupla)=
## Relazione e Tupla
Il modello relazionale prevede che le entità della realtà da descrivere siano tradotti in **relazioni**. Una relazione può essere vista come una tabella fatta di colonne e riga a cui è associato un *nome* che rappresenta il *nome della relazione*. Ogni colonna corrisponde ad un *attributo* ed ogni riga ad una *istanza* detta anche **tupla**. A differenza di una tabella Excel, una relazione deve avere un *dominio* ben specificato per ogni colonna (in Excel la cella `A1` può contenere un numero mentre la cella `A2` una stringa). Il numero di attributi di una relazione prende il nome di *grado* della relazione.

```{danger} Attenzione
Il concetto di relazione del modello relazionale **non coincide** con il concetto di relazione del modello Entità-Relazione. Per ridurre al minimo la confusione, useremo spesso il termine *tabella* per indicare le relazioni del modello relazionale. Tuttavia il concetto matematico di relazione non coincide con il concetto di tabella.
```

Introduciamo ora una prima definizione di *schema di relazione*, è importante notare che questa definizione riguarda lo *schema* cioè il livello intensionale: come è fatta mentre **non riguarda** lo *stato* cioè il livello estensionale: cosa contiene. 

:::{prf:definition} Schema di Relazione
Uno **schema di relazione** è definito da un *nome* e da una lista di *attributi* $A_1,...,A_n$ il numero $n$ di attributi si dice **grado** dello schema. Ad ogni singolo attributo $A_i$ è associato un *dominio*.
:::

:::{prf:example}
Consideriamo l'entità *studente* che nella base di dati è descritto da *nome*, *cognome* e *data di nascita*. Nel modello relazionale utilizzeremo una relazione/tabella chiamata `Studente` con tre attributi/colonne: `Nome`, `Cognome` e `DataDiNascita`. Il Dominio dei primi due attributi sarà quello delle stringhe di possibili nomi, mentre il terzo attributo avrà come dominio tutte le possibili date, ad esempio nel formato `AAAA-MM-GG`.

| Nome     | Cognome | DataDiNascita |
|----------|---------|---------------|
|   ....   |   ...   |      ...      |

:::


Mentre lo schema di una relazione rappresenta come è fatta la relazione e come sono fatte le istanze (righe della tabella), il contenuto rappresenta lo *stato* della relazione e si descrive indicando le *tuple* (righe) che compongono la relazione. Ogni tupla è una *sequenza ordinata di $n$ valori*, analizziamo bene questa definizione.
* Sequenza significa che i valori sono in successione (uno dopo l'altro), esiste perciò un primo, un secondo, un ultimo valore. La tupla `(Mario,Rossi,1970-07-23)` ha come primo valore `Mario`, come secondo valore `Rossi` e come terzo e ultimo valore `1970-07-23`.
* Ordinata significa che l'ordine dei valori conta. La tupla `(Mario,Rossi,1970-07-23)` è diversa dalla tupla `(Rossi,Mario,1970-07-23)`.
* $n$ rappresenta il numero di valori che è anche il grado della relazione a cui la tupla si riferisce.
* Valori indica che una tupla contiene “qualcosa” che deve essere preso dai domini corrispondenti. La tupla `(Musk,XÆA-12,2020-05-04)`, ad esempio, contiene dei valori non validi per i domini associati a causa del carattere `Æ` che normalmente non può essere utilizzato per nomi o cognomi.

:::{prf:example}
Riprendendo l'esempio precedente, aggiungiamo alla relazione `Studente`  due *tuple* una per lo studente *Edoardo Conte* ed una per lo studente *Raimondo Bianchi*.

| Nome     | Cognome | DataDiNascita |
|----------|---------|---------------|
| Edoardo  | Conte   | 1923-08-19    |
| Raimondo | Bianchi | 1946-01-01    |

Le due tuple possono essere indicate anche nel seguente modo:
* `(Edoardo, Conte, 1923-08-19)` 
* `(Raimondo, Bianchi, 1946-01-01)`

È importante notare che, essendo le tuple sequenze *ordinate*, non si possono inserire “a piacere” i valori queste sono tuple non valide per lo schema `Studente`.
* `(1923-08-19, Edoardo, Conte)` 
* `(Bianchi, Raimondo, 1946-01-01)`
* `(Bianchi, Raimondo)`

L'ultimo esempio mostra come una tupla valida debba indicare *tutti* i valori degli attributi. Vedremo che in alcuni casi è possibile indicarne solo alcuni, ma questo è possibile **solo se sono indicati valori di default da inserire se non dati esplicitamente**. Anche in presenza di valori di default, tuttavia, va esplicitato quali valori sono indicati esplicitamente. Ad esempio la seguente tupla `(Giacomo, 2010-01-02)` può indicare che va preso il cognome o il nome di default. Supponendo che `NULL` sia valido valore per nome e cognome, la tupla può indicare uno dei due seguenti casi:
* `(Giacomo, NULL, 2010-01-02)`
* `(NULL, Giacomo, 2010-01-02)`

L'unica soluzione è indicare esplicitamente se `Giacomo` riguarda il primo (nome) o il secondo (cognome) attributo.
:::

Possiamo quindi dare la seguente definizione di tupla.

:::{prf:definition} Tupla
Una **tupla** per uno schema di relazione è una lista ordinata di valori $t_1,\ldots,t_n$ dove ogni valore $t_i$ deve essere un valore del dominio di $A_i$ il quale può o meno contenere il valore `NULL`.
:::


```{note} Osserva
Il termine *tupla* può suonare strano, si tratta infatti di una italianizzazione del termine inglese *tuple* il quale indica una sequenza di $n$ valori. L'origine del termine inglese è semplice: dopo i termini pair, triple e quadruple (coppie, terne e quaterne), per indicare una sequenza di $n$ valori si può scrivere $n$-tuple (ad esempio 5-tuple, 9-tuple). L'unica cosa a cui bisogna fare attenzione quando il termine inglese *tuple* viene tradotto in italiano è al singolare plurale:
* singolare: l'inglese *tuple* diventa *tupla* in italiano,
* plurale: l'inglese *tuples* diventa *tuple* in italiano.

In pratica la parola *tuple* è singolare in italiano e plurale in inglese.
```

Ora che abbiamo viso cosa è uno schema di una relazione e cosa sono le tuple, possiamo definire il concetto di relazione

:::{prf:definition} Relazione
Una **relazione** per uno *schema di relazione* è un insieme di $m$ *tuple* ognuna delle quali deve essere *univoca* e deve contenere un valore valido (compreso nel dominio) per ogni attributo.
:::

Due osservazioni importanti rispetto alla definizione appena proposta.

Una relazione è un **insieme** di tuple, gli insiemi sono oggetti **non ordinati** e perciò in una relazione l'ordine delle tuple non ha alcuna importanza. Le due seguenti tabelle rappresentano esattamente la stessa relazione.

| Nome     | Cognome | DataDiNascita |
|----------|---------|---------------|
| Edoardo  | Conte   | 1923-08-19    |
| Raimondo | Bianchi | 1946-01-01    |

| Nome     | Cognome | DataDiNascita |
|----------|---------|---------------|
| Raimondo | Bianchi | 1946-01-01    |
| Edoardo  | Conte   | 1923-08-19    |

Ogni tupla deve essere **univoca** cioè non possono esserci “doppioni”. La seguente tabella **non** rappresenta una relazione, data la presenza di righe uguali.


| Nome     | Cognome | DataDiNascita |
|----------|---------|---------------|
| Raimondo | Bianchi | 1946-01-01    |
| Edoardo  | Conte   | 1923-08-19    |
| Raimondo | Bianchi | 1946-01-01    |

Tuttavia la seguente tabella è una valida relazione

| Id | Nome     | Cognome | DataDiNascita |
|----|----------|---------|---------------|
| 1  | Raimondo | Bianchi | 1946-01-01    |
| 2  | Edoardo  | Conte   | 1923-08-19    |
| 3  | Raimondo | Bianchi | 1946-01-01    |

in quanto la colonna `Id` differenzia quelle con i rimanenti attributi uguali.

### Vincoli e stati validi
Lo **stato** di una relazione (righe della tabella) rappresenta la situazione del mondo reale che si sta cercando di rappresentare nella base di dati. Tuttavia, non tutte le combinazioni di valori degli attributi sono realtà possibile. Ad esempio uno studente non può avere una data di nascita nel futuro e nemmeno una data troppo nel passato. Diremo che i valori hanno dei **vincoli** che vanno rispettati, altrimenti lo stato *non è valido*. Queste tuple sono ovviamente non valide per una base di dati di una scuola.
* `(Renato, Carta, 1596-03-31)`
* `(Luisa, Turati, 2975-07-29)`

```{attention} Attenzione
In ogni istante l'intera base di dati deve memorizzare unicamente *stati validi* altrimenti le informazioni recuperati da esso potrebbero causare malfunzionamenti dell'applicazione (ad esempio non accettare l'iscrizione di uno studente se la data è sbagliata). È compito dello sviluppatore garantire che la base di dati sia sempre in uno stato valido. Casi in cui questo non sia garantito vanno considerati bug dell'applicazione. La gravità di questi bug dipende dagli effetti che possono avere sulle applicazioni che si appoggiano alla base di dati.
```

## Database relazionale
Abbiamo visto come una relazione rappresenta un'entità (ad esempio l'entità *studente*). Tuttavia, una base di dati è normalmente composta di varie entità (*studenti*, *professori*, *materie*, ...), per questo motivo un **database relazionale** di norma contiene diverse relazioni (anche centinaia nei casi più complessi) che sono tra loro collegate.

:::{prf:example}
Vediamo in questo esempio un piccolo database per la memorizzazioni di informazioni relative ad una scuola. Il DB descriverà informazioni su: studenti, professori, classi e materie e dovrà indicare quali studenti appartengono a quale classe e quali professori insegnano quali materia in quali classi.

Come prima cosa individuiamo le *entità* che vogliamo considerare nel nostro DB elencano le caratteristiche (*attributi*) che ci interessano.
* **Studente**: *nome*, *cognome*, *data di nascita*, *classe*
* **Professore**: *nome*, *cognome*, *materia*
* **Materia**: *nome*, *ore settimanali*, *ore laboratorio*, *elenco insegnanti*
* **Classe**: *nome*, *aula*, *elenco studenti*

Consideriamo ora la relazione `Classe`, mentre gli attributi `Nome` e `Aula` sono semplici da definire, l'attributo `ElencoStudenti` presenta un problema. Se, ad esempio, la classe `1A` ha 25 studenti e la classe `2A` ne ha 21, come facciamo a mettere questo in una tabella che deve avere un numero prestabilito di colonne? Una possibile soluzione è creare una tabella che abbia più colonne di quelle che servono (ad esempio 40) e memorizzare gli studenti uno per colonna lasciando dello spazio vuoto per le colonne in eccesso. Questa soluzione ha diversi problemi, ad esempio lo spreco di memoria perché anche un valore vuoto (o `NULL`) occupa spazio. Vedremo nelle prossime lezione che la soluzione passa attraverso l'attributo `Classe` della relazione studente. Infatti possiamo ottenere l'elenco degli studenti della `2A` elencando le tuple in `Studente` che hanno valore `2A` nell'attributo `Classe`. Questo meccanismo prende il nome di *chiave esterna* e verrà trattato in dettaglio in una [prossima lezione](chiavi-integrita.md).

Un ragionamento simile a quello fatto per *elenco studenti* dell'entità *classe* può essere fatto per *elenco insegnanti* dell'entità *materia*.
:::

```{tip} Esercizio
Definire le relazioni per le seguenti entità, indicando, per ogni attributo, il tipo e il dominio (ad esempio: tipo *intero* dominio *valori tra 1 e 5*).
* **Prodotto**: *codice*, *nome*, *descrizione*, *prezzo*, *quantità in magazzino*
* **Fornitore**: *codice fornitore*, *nome*, *indirizzo*, *telefono*
* **Ordine**: *data ordine*, *prodotto*, *fornitore*, *quantità*

Spiegare le scelte effettuate, specialmente per quanto riguarda l'entità *ordine*.
```



