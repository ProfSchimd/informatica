# Progettazione concettuale

La fase di *progettazione concettuale* permette di identificare nell'analisi dei requisiti i *concetti* fondamentali che devono essere presenti all'interno della base di dati. Il termine concetto è spesso sostituito da *entità* e *relazione* essendo il Diagramma ER lo strumento pressoché universale per la fase di progettazione concettuale.

Per comprendere meglio la fase di progettazione concettuale vediamo un esempio analizzando un estratto di una possibile analisi dei requisiti per un registro elettronico. Nell'esempio sono state evidenziate alcune parole importanti che rappresentano informazione da mantenere nella base di dati.

Si deve permettere l'inserimento di un **voto** per ogni **materia** nella **pagella** finale, i voti possibili sono solo **voti interi** compresi tra 1 e 10 (non 6.5 o 7/8) o il **valore NC** che indica assenza di valutazione. Non è possibile procedere alla fase successiva dello **scrutinio** se tutti gli studenti non hanno una valutazione per tutte le materie previste.

In fase di progettazione concettuale è opportuno prevedere che vi sia un costrutto (entità o relazione) per ciascuno degli elementi sopra evidenziati, in caso contrario il rischio è di non aderire alle specifiche (si immagini, ad esempio, la mancanza del voto NC oppure la mancanza del concetto di pagella).
