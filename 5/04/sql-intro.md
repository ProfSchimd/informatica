# Linguaggio SQL

Il progenitore di SQL fu sviluppato, nella metà degli anni 70, da Donald D. Chamberlin e Raymond F. Boyce presso IBM, il linguaggio venne chiamato *SEQUEL* (Structured English QUEry Language), nome che viene spesso utilizzato anche per l'attuale SQL (che andrebbe correttamente pronunciato *es-chiu-el*) il cui acronimo (ottenuto eliminando le vocali da SEQUEL) è stato successivamente associato a *Structured Query Language*.

SQL ha avuto una forte espansione, specialmente per opera di IBM, tra la fine degli anni 70 e l'inizio degli anni 80. Questo successo fu anche dovuto al rilascio da parte di Oracle (all'epoca *Relational Software, Inc.*) del suo DBMS relazionale che comprendeva pieno supporto di SQL.

Dal 1986, l'ANSI e l'ISO hanno adottato e promosso lo standard SQL che oggi è ufficialmente giunto alla versione 2023.

## DDL e DQL
La gestione di un database relazionale è un'operazione molto complessa che comprende numerose istruzioni. Per facilità di descrizione le istruzioni vengono suddivise in base all'effetto che queste hanno sulla base di dati ed in base al risultato che producono. In particolare noi ci occuperemo
* di *Data Definition Language (DDL)* che indica l'insieme delle operazioni che modificano lo schema di un database e
* di *Data Query Language (DQL)* che indica l'insieme delle operazioni per recuperare (senza modificare) dati da un database.

```{note} Nota
La suddivisione tra DDL e DQL non è la più esaustiva, volendo andare più in dettaglio oltre a DQL e DDL si possono anche identificare
* *Data Manipulation Language (DML)* per la modifica dei record e
* *Data Control Language (DCL)* per la gestione della base di dati.

In queste lezioni includeremo DML e DCL nell'insieme di istruzioni che abbiamo denominato DDL.
```