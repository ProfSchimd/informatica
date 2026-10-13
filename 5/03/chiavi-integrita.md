---
title: Chiavi e Integrità
---

## Chiavi di una relazione
### Superchiave

Nella [lezione precedente](relazioni-tuple.md) è stato definito il concetto di *relazione*, si è visto che **ogni tupla deve essere univocamente identificabile** e che **non ci possono essere due tuple identiche** (questo è conseguenza della definizione di relazione come *insieme di tuple*).

In altre parole, presa una relazione deve esistere un sottoinsieme di attributi (che potrebbero essere *tutti* gli attributi) per cui non esistono due tuple con gli stessi identici valori su tutti gli attributi di questo sottoinsieme. Chiamiamo un sottoinsieme con questa caratteristica una **superchiave** (**superkey**) per la relazione.

Consideriamo la seguente relazione `Studente`.

| Nome     | Cognome | Data Di Nascita |
|----------|---------|-----------------|
| Raimondo | Bianchi | 1946-01-01      |
| Edoardo  | Conte   | 1923-08-19      |
| Mario    | Bianchi | 1964-05-10      |
| Alice    | Rossi   | 1999-02-20      |
| Mario    | Rossi   | 1975-10-31      |
| Barbara  | Verdi   | 1982-04-01      |
| Carlo    | Rossi   | 1975-10-31      |

Si può vedere che nessuno degli attributi preso da solo è una superchiave, infatti ogni colonna contiene almeno due valori uguali. Il sottoinsieme di attributi `(Nome, Cognome)` rappresenta una superchiave per `Studente` poiché non esistono tuple con lo stesso nome e cognome.
Infine, la coppia di attributi `(Cognome, DataDiNascita)` contiene due tuple con gli stessi valori (`Mario Rossi` e `Carlo Rossi`), quindi non è una superchiave. Vale la pena notare che, essendo le tuple di una relazione uniche (altrimenti non si può nemmeno chiamare relazione), l'insieme di tutti gli attributi di una relazione è sempre una superchiave.

:::{note} Quiz
Esiste un'altra superchiave nell tabella sopra, quale?
:::

:::{prf:definition} Superchiave
Una **superchiave** (**superkey**) per la relazione $R$ è un sottoinsieme di attributi di $R$ tale che non esistono due tuple $t_1$ e $t_1$ in $R$ con lo stesso valore per tutti questi attributi.
:::

```{tip} Esercizio
Trovare tutte le superchiavi nella seguente relazione

| Id  | Nome            | Prezzo | Posto |
|-----|-----------------| -------|-------|
| 1   | iPhone 14       | 899    | 12-a  |
| 2   | iPad Air 2022   | 599    | 12-b  |
| 4   | MacBook Air 15' | 899    | 10-a  |
| 5   | Apple watch     | 499    | 3-c   |
| 9   | Mac Mini 256G   | 599    | 10-d  |
| 10  | iMac 27'        | 1599   | 10-g  |
| 11  | iMac 27'        | 1299   |`NULL` |
```

### Chiave candidata e chiave primaria
Nell'esempio discusso sopra tutti i seguenti insiemi di attributi sono *superchiave* per `Studente`:
* `(Congome, Nome)`
* `(Nome, DataDiNascita)`
* `(Cognome, Nome, DataDiNascita)`

Alcune di queste superchiavi sembrano migliori di altre, facciamo alcune osservazioni a riguardo.
* La terza chiave contiene 3 attributi (tutti), le altre ne contengono solo 2. Inoltre se noi togliamo l'attributo `DataDiNascita` da questa chiave otteniamo ancora una superchiave (lo stesso accade se togliamo l'attributo `Cognome`).
* Mentre è naturale identificare le persone per nome e cognome, più anomala è l'identificazione mediante nome e data di nascita.

La prima osservazione ci indica che la superchiave `(Nome, Cognome, DataDiNascita)` contiene informazione “ridondante” cioè che si può eliminare mantenendo una superchiave. Al contrario, le rimanenti due superchiavi non possono essere “tagliate” senza perdere la caratteristica di superchiave, una superchiave con questa proprietà la chiamiamo **chiave candidata** (**candidate key**).


:::{prf:definition} Chiave candidata
Una **chiave candidata** (**candidate key**) per una relazione $R$ è una superchiave in cui togliendo un qualsiasi attributo si perde la proprietà di superchive.
:::

Nell'esempio sopra abbiamo due chiavi candidate con lo stesso numero di attributi
* `(Nome, Cognome)`
* `(Nome, DataDiNascita)`

Abbiamo già osservato che la seconda scelta appare poco naturale come identificativo dell'entità `Studente`. Nel modello relazionale è necessario scegliere una superchiave come **chiave primaria** (**primary key**) che viene utilizzato dagli sviluppatori e dai DBMS per identificare univocamente una tupla all'interno di una relazione.

:::{prf:definition} Chiave primaria
La **chiave primaria** (**primary key**) per una relazione $R$ è una sua *chiave candidata* che viene scelta per fungere da identificativo. La scelta può essere fatta per vari motivi, ad esempio la superchiave ha un solo attributo, oppure la superchiave è "facile da cercare".
:::

```{attention} Attenzione
Una volta che si definisce la chiave primaria per una relazione, non è possibile avere due tuple con la stessa chiave primaria, di norma non è nemmeno possibile cambiare la chiave primaria. Se nel nostro esempio scegliamo come chiave primaria `(Nome, Cognome)`, allora non sarà possibile inserire tuple con lo stesso valore di nome e cognome.
```

```{attention} Importante
Quando si seleziona la chiave primaria non bisogna mai limitarsi a guardare le tuple presenti, ma si deve sempre pensare alla realtà che si intende rappresentare. Ad esempio, anche se nel nostro esempio `(Nome, Cognome)` è univoco, non si può affermare che in una scuola non ci saranno mai studenti con lo stesso nome e cognome. La scelta della chiave primaria, quindi, deve tenere conto di quali sono gli attributi che possono rappresentare un elemento di unicità. Nel caso dei cittadini residenti in Italia, il *codice fiscale* è un sequenza di lettere e numeri che deve essere unica per tutte le persone ed è spesso usato come chiave primaria in una anagrafica.[^1]
```

## Chiave esterna
Uno dei punti di forza del modello relazionale è la possibilità di “collegare” tra di loro le relazioni. Supponiamo di avere una relazione *classe* nel database della scuola. Per comodità immaginiamo che *classe* abbia una chiave primaria fatta di un singolo attributo che chiamiamo `IdClasse`. Per collegare `Studente` e `Classe` indicando che uno studente appartiene ad una certa classe, possiamo inserire l'attributo `ClasseFreqeuntata` nella relazione `Studente`. Questo attributo conterrà il valore di `IdClasse` per la classe frequentata, vediamo come apparirebbero le due relazioni con un esempio.

:::{table} Relazione `Studente`

| Nome     | Cognome | Data Di Nascita | IdClasse |
|----------|---------|-----------------|----------|
| Raimondo | Bianchi | 2004-01-01      | 7        |
| Edoardo  | Conte   | 2004-08-19      | 7        |
| Mario    | Bianchi | 2008-05-10      | 2        |
| Alice    | Rossi   | 2003-02-20      | 10       |
| Mario    | Rossi   | 2007-10-31      | 2        |
| Barbara  | Verdi   | 2007-04-01      | 2        |
| Carlo    | Rossi   | 2006-10-31      | 5        |
:::

:::{table} Relazione `Classe`
| Id  | Classe | Aula |
|-----|--------|------|
| 2   | 1A     | 2    |
| 3   | 2A     | 5    |
| 5   | 3A     | 3    |
| 7   | 5B     | 4    |
| 10  | 5A     | 9    |
:::

Le tabelle sopra sono collegate dagli attributi `IdClasse` della relazione `Studente` e `Id` della relazione `Classe`. Infatti nella colonna `IdClasse` si trovano i valori identificativi della classe di appartenenza dello studente. Ad esempio `Raimondo Bianchi` è della `5B` poiché il suo valore in `IdClasse` è `7` che corrisponde all'`Id` della classe `5B`. Anche `Edoardo Conte` è della `5B` mentre `Mario Bianchi` è della `1A` (valore `IdClasse` pari a `2`). 

Seguendo questi *riferimenti* è possibile ricostruire l'informazione completa e rispondere ad alcune domande (*query*) più complesse come la seguente: *in che aula si trova `Carlo Rossi`?* La risposta a questa domanda si ottiene in due passi:
1. si recupera l'informazione della classe di `Carlo Rossi` mediante il suo valore di `IdClasse`, in questo case tale valore è `5`;
2. si recupera l'informazione sull'aula per quella classe che ha il valore `Id` come quello di `Carlo Rossi`, in questo caso `5`, si scopre, quindi, che `5` (la classe `3A`) si trova nell'aula `3`, quindi *`Carlo Rossi` si trova nell'aula `3`.

In un certo senso l'operazione appena descritta utilizza il collegamento tra `Studente.IdClasse` e `Classe.Id` (utilizziamo la notazione compatta `R.A` per indicare l'attributo `A` della relazione `R`) per creare un'unica tupla “virtuale”   fatta nel seguente modo

| Nome     | Cognome | Data Di Nascita | IdClasse | Id  | Classe | Aula |
|----------|---------|-----------------|----------|-----|--------|------|
| Carlo    | Rossi   | 2006-10-31      | 5        | 5   | 3A     | 3    |


Vedremo [l'operazione di Join](algebra-relazionale.md#operazione-di-join) viene utilizzata nell'[algebra relazione](algebra-relazionale.md) e nel [linguaggio SQL](/5/04/sql-join.md) per generare tuple di questo tipo che possono essere poi utilizzate per rispondere a query, anche complesse, che richiedono di incrociare i dati di varie relazioni.

I valori utilizzati per collegare due relazioni come sopra devono determinare univocamente quali tuple vengono collegate. L'attributo `IdClasse` di `Studente` deve definire esattamente l'unica classe a cui lo studente appartiene. Per questo motivo si usa come attributo *riferito* (`Classe.Id` nel nostro esempio) la *chiave primaria* della relazione riferita, l'attributo `Studente.IdClasse` viene quindi chiamato **chiave esterna** (**foreign key**).

### Integrità referenziale
Per collegare due relazione mediante chiave esterna basta aggiungere un attributo (es. `IdClasse`) alla relazione *referente* (es. `Studente`) che sia dello stesso tipo (es. `int`) della chiave primaria (es. `Id`) della relazione *riferita* (es. `Classe`). Questo, tuttavia, non garantisce automaticamente che ogni valore di `Studente.IdClasse` abbia un corrispondente in `Classe.Id`. 

Se la relazione `Studente` contenesse anche la seguente tupla

| Nome     | Cognome | Data Di Nascita | IdClasse |
|----------|---------|-----------------|----------|
| Raimondo | Bianchi | 2004-01-01      | 11       |


ci sarebbe un problema in quanto non esiste nella relazione `Classe` una tupla il cui valore di `Id` è `12`. Questo crea un problema nel momento in cui si cerca di collegare le due tabelle poiché questa specifica tupla non avrebbe il corrispettivo. Per evitare problemi nelle operazioni di `JOIN` (tipo quella mostrata sopra) in cui le relazioni vengono collegate mediante chiavi esterne, si impone che **ogni valore della chiave esterna sia presente nella chiave primaria a cui fa riferimento oppure il valore sia `NULL`**, questo vincolo lo chiamiamo **integrità referenziale** (cioè il riferimento è “integro” nel senso che è valido o è inesistente - `NULL`). L'integrità referenziale in un DB relazionale è garantita dal DBMS stesso, in altre parole se si cerca di modificare (inserire, aggiornare o cancellare) una qualche tupla che viola l'integrità referenziale, il DBMS si rifiuta di eseguire l'operazione e genera un errore.


Possiamo quindi dare la seguente definizione di *chiave esterna* nella quale viene indicato anche il vincolo di integrità referenziale come parte del requisito di uno stato valido della base di dati.

:::{prf:definition} Chiave esterna
Una **chiave esterna** della relazione $R$ è un sottoinsieme di attributi di $R$ che è chiave primaria di una relazione $Q$ (non è necessario che $R$ e $Q$ siano relazioni diverse). Affinché gli attributi di $R$ siano una valida chiave esterna deve essere vero che:
* i valori ammessi siano dello stesso tipo dei corrispondenti attributi chiave di $Q$,
* per ogni stato valido di del database una tupla di $Q$ deve contenere chiavi esterne di $Q$ i cui valori sono presenti in $R$ o sono `NULL`, chiamiamo questo **vincolo di integrità referenziale**.
:::

```{important} Importante
Dal momento che la chiave esterna si riferisce ad una chiave primaria, la chiave esterna è composta da più attributi quando la chiave primaria che viene riferita è composta da più attributi.
```

```{tip} Esercizio
1. Definire una relazione `Professore` che ha come attributi: *nome*, *cognome* e *coordinatore*, quest'ultimo deve essere una chiave esterna per la chiave primaria della relazione `Classe`.
2. Aggiungere almeno 4 tuple alla relazione professore dove almeno un professore è coordinatore è almeno uno non lo è.
3. Descrivi la procedura attraverso la quale è possibile conoscere il coordinatore di una data classe (vedi l'esempio sopra per trovare l'aula di un dato studente).
```

```{tip} Esercizio
Indica come si può ottenere lo stesso risultato dell'esercizio precedente inserendo una chiave esterna in `Classe` anziché in `Professore`.
```

[^1]: Con anagrafica si intende un elenco di persone con informazioni associati. Ad esempio un'azienda può avere un'anagrafica clienti e un'anagrafica fornitori.


