# Il modello relazionale
Una base di dati può essere descritta in diversi modi all'uso che si deve della descrizione. Durante la progettazione di una base di dati, si usano tre *livelli* di descrizione:
1. livello **concettuale** in cui la parte importante sono i *concetti* rappresentati nella base di dati,
2. livello **logico** in cui viene descritto il modo in cui i dati sono organizzati,
3. livello **fisico** in cui si descrive come i dati sono memorizzati nei supporti fisici (es. SSD, dischi, ...).

Ognuno di questi livelli può essere descritto da un **modello** per cui si parla perciò di modello concettuale, modello logico e modello fisico.

In una [precedente unità](/5/02/concettuale.md) si è discusso modello concettuale chiamato **diagramma entità-relazione**.

In questa unità si discute il **modello relazionale**, ancora oggi il modello logico più utilizzato e di maggiore successo. Nel modello relazionale l'informazione viene organizzata in *relazioni* che possono essere paragonate a tabelle costituite da colonne e righe. 
Il modello relazionale ha visto un consolidamento decennale ed è stato studiato sotto diversi aspetti. Particolarmente importante nei database relazionali è il *linguaggio SQL* che rappresenta lo strumento per creare, gestire e modificare base di dati relazionali. Il linguaggio SQL è fortemente basato su una teoria matematica nota come *algebra relazionale* che rappresenta un'estensione della teoria insiemistica per operare su relazioni. Il Linguaggio SQL sarà oggetto di un successivo modulo, in questo modulo, dedicato al modello logico relazionale discuteremo:
- i [concetti di relazione e tupla](relazioni-tuple.md);
- i [concetti di chiave, referenza e integrità](chiavi-integrita.md) e
- le [basi dell'algebra relazionale](algebra-relazionale.md).

```{figure} /5/img/map-modello-relazionale.png
```