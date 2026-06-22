# European Trip Optimization

Progetto realizzato per il corso di **Metodi Decisionali per l’Analisi Economica**.

## Autori

* Giovanni Giacomo Cerasoli
* Lorenzo Del Corso
* Alessandro Ricchebono

## Obiettivo

Il progetto affronta un problema di ottimizzazione relativo alla pianificazione di un viaggio in alcune delle principali città europee.

L’obiettivo è individuare un itinerario che minimizzi il costo totale degli spostamenti, rispettando vincoli sul numero di tappe, sulla città di partenza e arrivo e sul tempo complessivo di viaggio.

Le città considerate sono:

```r id="7dpm66"
Genova
Marsiglia
Parigi
Barcellona
Madrid
Vienna
Amsterdam
Bruxelles
```

Genova è stata imposta come città obbligatoria di partenza e di arrivo.

## Dati raccolti

Per ogni coppia di città sono stati raccolti costi e tempi di viaggio relativi a:

* aereo;
* treno;
* pullman.

I dati sono stati raccolti dai siti:

```r id="l8z0g3"
thetrainline.com
flixbus.it
skyscanner.it
```

Per ogni mezzo è stata selezionata la soluzione migliore disponibile in termini di prezzo e durata, utilizzando come riferimento il 1° marzo 2023.

Sono state costruite sei matrici:

```r id="w4plof"
3 matrici dei costi
3 matrici dei tempi
```

I dati non sono stati considerati simmetrici, poiché costo e durata di un viaggio possono variare in base alla direzione dello spostamento.

## Modello matematico

Il problema è stato formulato come un modello di programmazione lineare binaria.

Le variabili decisionali indicano se viene selezionato uno spostamento tra due città con uno specifico mezzo di trasporto:

```r id="3bw86q"
p_ij = 1 se il viaggio da i a j avviene in pullman
t_ij = 1 se il viaggio da i a j avviene in treno
a_ij = 1 se il viaggio da i a j avviene in aereo
```

Il modello utilizza:

```r id="az1h8r"
192 variabili binarie
```

La funzione obiettivo minimizza il costo totale del viaggio:

```r id="a6d3am"
min z = Σ(c_pij * p_ij + c_tij * t_ij + c_aij * a_ij)
```

## Vincoli principali

Il modello include i seguenti vincoli:

* tempo totale di viaggio non superiore a 800 minuti;
* selezione di cinque città, inclusa Genova;
* partenza e ritorno obbligatori a Genova;
* nessun viaggio dalla città verso sé stessa;
* al più una partenza da ogni città;
* conservazione del flusso: se si arriva in una città, da quella città si deve anche partire;
* eliminazione di cicli inversi tra due città.

Il vincolo temporale principale è:

```r id="axobn8"
Σ(t_pij * p_ij + t_tij * t_ij + t_aij * a_ij) <= 800
```

## Implementazione

Il modello è stato implementato e risolto tramite:

```r id="p99up5"
Microsoft Excel Solver
```

## Soluzione base

La soluzione ottima individua un costo minimo pari a:

```r id="vawn3q"
157 €
```

con un tempo totale di viaggio pari a:

```r id="y9v6lr"
789 minuti
```

L’itinerario ottenuto è:

```r id="zg3a4f"
Genova -> Barcellona -> Bruxelles -> Madrid -> Marsiglia -> Genova
```

Tutti gli spostamenti avvengono in aereo, ad eccezione del tratto:

```r id="zkvb5t"
Marsiglia -> Genova
```

effettuato in pullman.

## Analisi di sensitività manuale

Essendo un problema binario, non è stato possibile utilizzare direttamente il report di sensitività di Excel Solver.

È stata quindi svolta un’analisi manuale sui vincoli temporali.

La soluzione iniziale presenta uno slack di:

```r id="uvnqr1"
11 minuti
```

La soluzione rimane invariata fino a un tempo massimo disponibile di 822 minuti.

A 823 minuti, il costo scende a:

```r id="nqpzzk"
152 €
```

mentre riducendo il tempo massimo a 788 minuti cambia l’itinerario e il costo aumenta leggermente.

## Scenari alternativi

Sono stati analizzati quattro scenari aggiuntivi.

### Utilizzo obbligatorio di tutti i mezzi

È stato imposto l’utilizzo di almeno un aereo, un treno e un pullman.

```r id="0uypg7"
Costo minimo = 159 €
Tempo totale = 706 minuti
```

Itinerario:

```r id="gmm5gi"
Genova -> Barcellona -> Parigi -> Bruxelles -> Marsiglia -> Genova
```

### Passaggio obbligatorio per Vienna

Aggiungendo il vincolo di visitare Vienna, si ottiene:

```r id="wapk55"
Costo minimo = 163 €
Tempo totale = 754 minuti
```

Itinerario:

```r id="le2xrs"
Genova -> Barcellona -> Vienna -> Bruxelles -> Marsiglia -> Genova
```

### Durata massima per ogni singolo spostamento

Imponendo che ogni viaggio duri al massimo tre ore:

```r id="2a0vcu"
Costo minimo = 264 €
Tempo totale = 713 minuti
```

In questo scenario entra Amsterdam nell’itinerario, mentre Marsiglia non viene più selezionata.

### Minimizzazione del tempo con budget massimo

Modificando la funzione obiettivo e imponendo un costo massimo di 200 €:

```r id="xwryof"
Tempo minimo = 683 minuti
Costo totale = 194 €
```

L’analisi mostra come un aumento moderato del budget permetta una riduzione significativa del tempo complessivo di viaggio.

## Conclusioni

Il progetto mostra come un problema apparentemente semplice di organizzazione di un viaggio possa essere modellato come un problema di programmazione matematica con vincoli multipli.

I risultati evidenziano un compromesso tra costo, durata complessiva e flessibilità dell’itinerario:

* la soluzione base minimizza il costo con 157 €;
* l’introduzione di vincoli aggiuntivi aumenta il costo, ma in alcuni casi riduce il tempo complessivo;
* il limite massimo sulla durata di ogni spostamento è il vincolo più oneroso;
* Barcellona e Marsiglia risultano le città più frequentemente presenti nelle soluzioni;
* Amsterdam e Vienna risultano meno competitive, salvo vincoli specifici.

Il modello può essere facilmente esteso includendo ulteriori vincoli, come budget differenziati, preferenze del viaggiatore, numero massimo di voli o permanenza minima in ciascuna città.

