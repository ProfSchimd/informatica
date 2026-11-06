---
title: "WHERE e ORDER BY"
---

## Clausola `WHERE`

La clausola `WHERE` si usa nell'istruzioni `SELECT` per *filtrare le righe* di una tabella sulla base della condizione specificata nella clausola. Questo filtraggio consente di recuperare solo le tuple che soddisfano dei criteri di interesse. 

::::{grid} 2

:::{grid-item}
```sql
SELECT first_name, last_name
FROM Student
WHERE last_name = 'Bianchi';
```
:::

:::{grid-item}
| first_name | last_name |
|------------|-----------|
| Raimondo   | Bianchi   |
| Mario      | Bianchi   |
:::
::::
### Operatori booleani

Nella clausola `WHERE`, è possibile utilizzare gli operatori booleani `AND`, `OR` e `NOT` per combinare diverse condizioni.

```sql
-- Utilizzo di AND per combinare condizioni
SELECT first_name, last_name
FROM Student
WHERE first_name = 'Mario' AND last_name = 'Bianchi';

-- Utilizzo di OR per combinare condizioni
SELECT first_name, last_name
FROM Student
WHERE first_name = 'Mario' OR last_name = 'Bianchi';

-- Utilizzo di NOT per escludere righe che soddisfano una condizione
SELECT first_name, last_name
FROM Student
WHERE NOT first_name = 'Mario';
```

I risultati per questi tre esempi sono riportati qui sotto.

::::{grid} 3 3 3 3 
:::{grid-item}

`AND`
| first_name | last_name |
|------------|-----------|
| Mario      | Bianchi   |

:::
:::{grid-item}

`OR`
| first_name | last_name |
|------------|-----------|
| Raimondo   | Bianchi   |
| Mario      | Bianchi   |
| Mario      | Rossi     |

:::
:::{grid-item}

`NOT` 
| first_name | last_name |
|------------|-----------|
| Raimondo   | Bianchi   | 
| Edoardo    | Conte     |
| Alice      | Rossi     |
| ...        | ...       |

:::
::::

Lo stesso risultato di `NOT` si può utilizzare mediante l'operatore *diverso da* che in SQL si indica con `<>`.
```sql
SELECT first_name, last_name
FROM Student
WHERE  first_name <> 'Mario';
```

Ovviamente nella clausola `WHERE` si possono utilizzare condizione su valori numerici.

```sql
SELECT first_name, last_name
FROM Teacher
WHERE  teacher_id > 2
```

Gli operatori di confronto disponibili sono quelli usuali: `=`, `<>`, `>`. `<`, `<=`, `>=`.

### Condizioni su stringhe `LIKE`

Gli operatori `=` e `<>` eventualmente combinati con operatori booleani, permettono di creare condizioni su stringhe relativamente semplici. Ad esempio, non è possibile indicare una query del tipo *tutti gli studenti con iniziale del cognome `V`*. Lo standard SQL prevede un operatore di confronto tra stringhe più potente chiamato `LIKE`.

 L'operatore `LIKE` consente di cercare stringhe che corrispondono a un determinato *pattern* utilizzando dei *caratteri jolly*: 
 * `%` che rappresenta un numero imprecisato di caratteri e
 * `_` che rappresenta un singolo carattere.

Vediamo alcuni esempi di utilizzo

```sql
-- Studenti con il cognome che inizia con la V
SELECT first_name, last_name
FROM Student
WHERE last_name LIKE 'V%';
```

```sql
-- Studenti che si chiamano Mario, Maria, Marii, ...
SELECT first_name, last_name
FROM Student
WHERE first_name LIKE 'Mari_';
```

```sql
-- Studenti il cui nome contiene 'ele'
SELECT first_name, last_name
FROM Student
WHERE first_name LIKE '%el%';
```

In questo esempio, la query restituirà tutte le righe in cui il nome inizia con "Ma".

```{note} Osserva
Utilizzando l'operatore `LIKE` è possibile definire condizioni molto complesse, tuttavia esiste uno strumento ancora più potente e molto utilizzato denominato [*espressione regolare*](https://en.wikipedia.org/wiki/Regular_expression) (*regular expression*). Lo standard SQL non definisce un vero e proprio meccanismo per l'utilizzo delle espressioni regolari, tuttavia molti DBMS hanno aggiunto funzioni *custom* al loro linguaggio SQL. Ad esempio in MariaDB è disponibile la funzione [`REGEXP`](https://mariadb.com/kb/en/regexp/). Sebbene le espressioni regolari siano inizialmente difficili da utilizzare, padroneggiandole in maniera adeguata si possono risolvere numerosi problemi di *pattern matching* (cioè confronto tra stringhe).
```

### Confronto con `NULL`
Per effettuare il confronto con il valore `NULL`, se previsto, non si può utilizzare l'operatore di confronto `=`, ma l'operatore `IS`.

```sql
SELECT * 
FROM Student
WHERE birth_date IS NULL;
```

allo stesso modo per selezionare valori non `NULL` si utilizza `IS NOT`

```sql
SELECT * 
FROM Student
WHERE birth_date IS NOT NULL;
```

## Ordinamento con `ORDER BY`

La clausola `ORDER BY` permette di ordinare i risultati di una query (con o senza `WHERE`) in base a una o più colonne specificate. Ad esempio:

::::{grid} 2 2 2 2
:::{grid-item}
```sql
-- Ordina gli studenti per cognome
-- in ordine alfabetico crescente
SELECT last_name, first_name
FROM Student
ORDER BY last_name ASC;
```
:::

:::{grid-item}
| last_name | first_name |
|-----------|------------|
| Bianchi   | Raimondo   |
| Bianchi   | Mario      |
| Conte     | Edoardo    |
| ...       | ...        |
:::
::::

Ovviamente è anche possibile ordinare secondo l'ordine decrescente utilizzando `DESC` anziché `ASC` (il quale è anche l'ordinamento di default, se non ne viene specificato uno).

::::{grid} 2 2 2 2 
:::{grid-item}
Nel caso di elementi uguali, l'ordine del risultato è quello della tabella, nel nostro esempio `Bianchi Raimondo` viene prima di `Bianchi Mario`. Tuttavia è possibile indicare una seconda colonna da utilizzare nell'ordinamento.

```sql
SELECT last_name, first_name
FROM Student
ORDER BY last_name, first_name ASC;
```

:::

:::{grid-item}
| last_name | first_name |
|-----------|------------|
| Bianchi   | Mario      |
| Bianchi   | Raimondo   |
| Conte     | Edoardo    |
| ...       | ...        |
In questo esempio, `Bianchi Mario` viene prima di `Bianchi Raimondo` perché `Mario` precede alfabeticamente `Raimondo`.
:::
::::

## Domande ed Esercizi
### Domande
* Che differenza c'è tra `SELECT` e `WHERE`?
* Come si può selezionare le righe che hanno un valore numero non-negativo?
* Cosa succede se la condizione della clausola `WHERE` non è soddisfatta da nessuna tupla?
* Scrivi una condizione che sia sempre vera ed una condizione che sia sempre falsa.

### Esercizi

Gli esercizi proposti di seguito sono stati pensati per essere eseguiti sul Database "Scuola" utilizzato negli esempi di queste lezioni. Per avere un riscontro, su può utilizzare [questo file](https://github.com/ProfSchimd/teaching-material/blob/main/inf/5/INF.5.04/database/db_school_big.sql) per creare e popolare un database di dimensioni (numero di tuple) adeguate.

1. Visualizzare cognome, nome e email per tutti gli studenti con nome `Alice`.
2. Visualizzare l'identificativo e la mail di tutti gli studenti con cognome `Rossi`, ma non diverso da `Mario`.
3. Visualizzare l'email con dominio `gmail.com` o `email.it`, ordinando il risultato in ordine alfabetico di cognome e nome.
4. Selezionare tutti gli studenti che sono nati nel `2004`.
5. Mostrare cognome e email di tutti gli studenti ordinandoli per data di registrazione decrescente.
6. Mostrare gli studenti che sono nati nel mese di febbraio o nel mese di marzo di qualsiasi anno.
7. Visualizzare tutti gli studenti che all'interno del proprio cognome contengono i caratteri `cont` ignorando maiuscole e minuscole (ad esempio `Conte`).

Nello svolgere gli esercizi sopra si tenga conte dei seguenti suggerimenti.
* Dal momento che il tipo data è un tipo stringa, `LIKE` può essere usato anche sulle date.
* Per confronti *case-insensitive* usare `LOWER` o `UPPER`.
* Ricordarsi il funzionamento degli operatori `AND` ed `OR` con le loro principali differenze.