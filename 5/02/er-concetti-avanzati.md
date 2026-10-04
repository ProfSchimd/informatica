---
title: "ER: concetti avanzati"
---

## Relazioni ternarie
Fin'ora abbiamo visto unicamente *relazioni binarie*, cioè relazioni con due entità partecipanti, in certi casi il **grado** di una relazione (numero di entità partecipanti) è maggiore di due. Un caso piuttosto frequente è quello di relazioni che, naturalmente, coinvolgono tre entità, **relazioni ternarie**. Ovviamente, sono possibili relazioni con un numero maggiore di entità coinvolte, ma si presentano molto raramente.

:::{figure} /5/img/er-relazione-ternaria.svg
Relazione ternaria `Insegna` in cui le entità `Insegnante`, `Materia` e `Classe` sono coinvolte. Il vincolo di cardinalità `N:M:K` indica che un insegnante può insegnare più materie nella stessa classe, una materia può essere insegnata da più insegnanti nella stessa classe e una classe può avere più insegnanti per la stessa materia.
:::


Consideriamo la relazione ternaria `Insegna` il cui schema è presentato sopra ed a cui partecipano le tre entità `Insegnante`, `Classe` e `Materia`. Questa relazione descrive il fatto che gli insegnanti possono insegnare a diverse classi e possono insegnare materie diverse, perciò è necessario considerare la relazione ternaria per intero essendo importante sapere *quale/i  materia/e un dato insegnante insegna in una specifica classe*.

Una complicazione delle relazioni ternarie è la corretta assegnazione dei vincoli di cardinalità. Nell'esempio vediamo indicate `N`, `M` e `K` ad indicare che tutte le istanze di tutte e tre entità possono partecipare più volte alla relazione. Ovviamente sono possibili relazioni ternarie in cui le entità partecipino con cardinalità `1`.


Per Individuare i vincoli di cardinalità in una relazione ternaria, si deve ”isolare“ ciascuna entità e confrontarla con le altre due. Ad esempio, nella relazione `Insegna` abbiamo da individuare tre cardinalità.
* Per `Insegnante` dobbiamo rispondere alla seguente domanda: *una certa materia in una data classe, quanti insegnanti può avere?* Nel caso in cui la risposta sia *più di un insegnante,* indichiamo sul lato `Insegnante` della relazione `Insegna` `N` ad indicare questo fatto.
* Per `Materia` dobbiamo rispondere alla seguente domanda: *un certo insegnante in una data classe quante materie può insegnare?* Nel caso in cui la risposta sia *più di una materia,* indichiamo sul lato `Materia` della relazione `Insegna` `M` (per distinguerlo da `N`) ad indicare questo fatto.
* Infine, per `Classe` dobbiamo rispondere alla seguente domanda *Un dato insegnante in quante classi può insegnare una certa materia?* Nel caso in cui la risposta sia *più di una classe*, indichiamo sul lato `Classe` della relazione `Insegna` `K` (per distinguerlo da `N` e `M`) ad indicare questo fatto.

## Entità deboli
Un'**entità debole** è un'entità che non possiede un attributo chiave che identifica le istanze. Un'entità debole può esistere solo se in relazione con un'entità **forte** dalla quale ”eredita” la chiave. In questo modo è possibile, eventualmente utilizzando attributi “sottochiave”, definire una chiave per l'entità debole. La relazione che lega un'entità debole alla sua entità forte viene detta *relazione identificante*, tale relazione deve sempre esistere (non può esistere un'entità debole senza che essa sia collegata ad una forte) e la partecipazione a tale relazione dell'entità debole deve essere totale. Nel diagramma ER, le entità deboli sono indicate con un doppio bordo, come anche le corrispondenti relazioni identificanti.

:::{figure} /5/img/er-entita-debole.svg
La relazione identificante `Presenta` tra l'entità debole `Giustificazione` e la corrispondente entità forte `Studente`. Si noti che la chiave di `Giustificazione` è ottenuta combinando la chiave di `Studente` (entità forte) e l'attributo (sottochiave) `Giorno` dell'entità (debole) `Giustificazione`.
:::

Nell'esempio illustrato sopra, l'entità debole `Giustificazione` possiede l'attributo `Giorno` (nella figura altri attributi come `Motivazione`, `Firma` ed altri non sono indicati), ma non esiste un attributo chiave che da solo identifica un'istanza dell'entità. Per ottenere tale attributo chiave dobbiamo combinare la chiave parziale `Giorno` (sottolineata in tratteggio) con l'attributo chiave dell'entità forte (`Studente` con chiave `Codice`). Il collegamento di `Giustificazione` con `Studente` avviene mediante la relazione identificante `Presenta`.


:::{note} Osserva
Un'entità debole è diversa dall'entità forte che la identifica, la `Giustificazione` dell'esempio sopra, **non** è lo `Studente`. Per identificare una relazione gerarchica, cioè due entità in cui una è una specificazione di un'altra si usa il concetto di [specializzazione](#specializzazione) discusso nel prossimo paragrafo.
:::

(specializzazione)=
## Specializzazione
In certi casi è utile inserire nella schema concettuale indicazioni circa il fatto che alcune entità **specializzano** una specifica entità. Ad esempio, in una scuola ci possono essere insegnanti di laboratorio ed insegnanti di teoria.

Per indicare una specializzazione si crea un collegamento tra l'entità generica e tutte le sue specializzazione. Questo collegamento (vedi figura sotto) passa attraverso un nodo (il cerchio con `d` nella figura) che definisce se gli insiemi di specializzazione sono tra loro *disgiunti* (`d` nel nodo) o *sovrapposto* (`o` di *overlapping* nel nodo). Inoltre il collegamento ha una direzione, dall'entità generica a quelle specializzate, indicato da una freccia curvata a metà del collegamento (vedi figura sotto).

:::{figure} /5/img/er-specializzazione.svg
Esempio di specializzazione dell'entità `Insegnante` in `InsegnanteTeoria` e `InsegnanteLaboratorio`. In questo esempio la specializzazione è *disgiunta* (indicato dalla `d`) cioè non ci sono entità insegnante che sono sia `InsegnanteTeoria` sia `InsegnanteLaboratorio`.
:::

L'esempio sopra di specializzazione permette che un `Insegnante` possa non appartenere a nessuna delle specializzazione, se vogliamo indicare un vincolo di specializzazione possiamo esprimere questo usando una linea doppia (come per la partecipazione totale) tra l'entità generica ed il cerchio che indica la tipologia (`d` nell'esempio).

:::{figure} /5/img/er-specializzazione-totale.svg
Esempio di specializzazione dell'entità `Studente` in `StudenteInformatica` e `StudenteElettronica`. In questo esempio la specializzazione è obbligatoria (indicato con la linea doppia uscente da `Studente`) che significa che *tutti* gli studenti devono essere in almeno una delle specializzazioni (in questo essendo al tipologia disgiunta, ogni studente deve essere in esattamente una specializzazione).
:::

---

La specifica delle entità specializzanti descritta sopra fa parte di un insieme di costrutti noti come EER (*Extended ER*) e non è tanto utilizzata quanto le altre notazioni descritte in questa e nelle precedenti lezioni. Inoltre, l'utilizzo della specializzazione può rendere più difficoltosa la traduzione dello schema concettuale in uno schema logico relazionale. Per questo motivo è sempre meglio utilizzare con molta parsimonia questo tipo di descrizione che, pur essendo comodo per rappresentare alcuni aspetti della realtà di interesse, possono poi rivelarsi un problema in una fase successiva della progettazione.
