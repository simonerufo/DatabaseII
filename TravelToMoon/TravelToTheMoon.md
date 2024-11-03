# Travel to the Moon

## Specifiche dei requisti

### Crociera

- codice
- data inizio $\wedge$ fine
- LunaDiMiele $\vee$ PerFamiglie (adatte ai bambini(?))
- le crociere LunaDiMiele possono essere:
  - tradizionali: numero di destinazioni romantiche $\ge$ destinazioni divertenti
  - alternative: numero di destinazioni romantiche $<$ destinazioni divertenti

### Nave: (associata a crociera)

- nome
- comfort
- num_passeggeri

### Prenotazione: (associata a crociera)

- istante di prenotazione
- numero di posti prenotati

### Destinazione: (associata a crociera)

- nome
- continente
- insieme di posti da vedere durante escursioni
- romantica $\wedge$ divertente

### Itinerario

- nome
- questo prevede una serie ordinata di destinazioni
- data ora arrivo e partenza (espresse come differenza con l'inizio della crociera)
- previsto da più di una crociera

### Escursione

- nome
- descrizione
- fascia oraria
- il sistema deve poter risalire ai posti da vedere in ogni singola destinazione        (usecase)

### Cliente: (prenota crociera)

- nome
- cognome
- età
- indirizzo
  
#### Funzionalità richieste del sistema

##### (1)  Ufficio prenotazioni Accetta/Rifiuta la prenotazione da parte del cliente

**Input** :

- Cliente
- Numero di posti
- Crociera

**Output**: bool

##### (2)  Ufficio marketing calcola l'età media dei clienti che hanno prenotato almeno una crociera esotica (continente $\not=$ Europa ) in un periodo dato

**Input** :

- Periodo (inizio,fine)

**Output**: Reale $\ge$ 0

##### (3)  Ufficio marketing calcola % delle destinazioni "Gettonate" in un certo periodo

**Nota: una destinazione è gettonata se: raggiunta da $\ge$ 10 crociere luna di miele $\vee$ $\ge$ 15 crociere per famiglie**

**Input** :

- Periodo (inizio,fine)

**Output**: (Destinazione,Reale $\ge$ 0) (0..*)

## Class diagram UML

![Alt text](ClassDiagram.jpeg)

## Specifica dei tipi di dato

### Crociera.COD

    Stringa formattata da standard

## Specifica delle Operazioni di classe

### Cliente.Eta(): Intero $\ge$ 0

  **pre-condizione**
  **post-condizione:**
  siano $i,n \space|\space Adesso(i) \wedge  Nascita(this,n) \wedge Anno(i) \wedge Anno(n)$
  $eta = i-n$
  $Result = eta $

### Nave.PostiDisponibili(): Intero $\ge$ 0

  **pre-condizione**
  **post-condizione:**
  
      Ricavo il numero di posti delle prenotazioni relative alla crociera che si riferisce a questa Nave
  
  $P = \set{np | \exists p,c \space PrenCroc(p,c) \wedge CrocNav(c,this) \wedge NumPosti(p,np)}$
  $sia \space T = pass \wedge NumPass(this,pass) $
  $Posti  = \sum{np \in P}$
  $Result = T - Posti$

### Destinazione.PostiDaVedere(): Escursione (0..*)

### LunaDiMiele.isTradizionale(): Bool

  **pre-condizione:**
  **post-condizione:**
  
    Raggruppo tutte le destinazioni romantiche e divertenti rispetto ad un itinerario
  $D = \{d \space|\space\exists i,t  \space\space CrIt(this,i) \wedge ItTappa(i,t) \wedge (TappaDest(t,d) \vee Arrivo(i,d) \vee Partenza(i,d)) \wedge isDivertente(d,True) \wedge isRomantica(d,False) \}$

  $R =\{r \space|\space\exists i,t  \space\space CrIt(this,i) \wedge ItTappa(i,t) \wedge (TappaDest(t,r) \vee Arrivo(i,r) \vee Partenza(i,r)) \wedge isDivertente(r,False) \wedge isRomantica(r,True) \}$

    Ritorno true se le destinazioni "Romantiche" sono >= delle "Divertenti" 
  $( |R| \ge |D|  \implies Result = True)\wedge ( |R| < |D|\implies Result = False)$

## Specifica dei Vincoli di classe


### [V.Prenotazione.Continuita]

$\forall c,c',p,i,i',n \space|\space ClPre(c,p) \wedge  PrenCroc(p,c) \wedge Nascita(c,n) \wedge Istante(p,i) \wedge Inizio(c,i') \implies n < i \wedge i<i'$

### [V.Prenotazione.NumeroPosti]

$\forall n,c,p,np,ps,p' \space|\space NumPosti(p,np)\wedge NumPass(n,ps)\wedge PrenCroc(p,c)\wedge CrocNav(c,n) \wedge PostiDisponibili(n,p') \implies np < ps \wedge p' > 0 $

### [V.Itinerario.ArrivoPartenza]

$\forall i,d,d' \space|\space Arrivo(i,d) \wedge Destinazione(i,d') \wedge d \not = d'$

### [V.Nave.CrocieraDisjointed]

$\forall n,c,c',i,i',f,f' \space|\space CrocNav(c,n)\wedge CrocNav(c',n) \wedge c \not = c' \wedge Inizio(c,i) \wedge Inizio(c',i') \wedge Fine(c,f) \wedge Fine(c',f') \implies \not \exist t \space|\space (i \le t \wedge t \le f) \wedge (i' \le t \wedge t \le f')$

### [V.Crociera.Tappe]

per ogni tappa dell'itinerario, deve esserci un ordine (le tappe sono comprese tra inizio e fine crociera)
$\forall i,t,c,i',i'',f,f' \space|\space CrocIt(c,i) \wedge ItTappa(i,t) \wedge Inizio(c,i') \wedge Fine(c,f) \wedge Inizio(t,i'') \wedge Fine(t,f') \implies i' \le i \wedge f' \le f $

### [V.Crociera.InizioFine]

$\forall c,i,f \space|\space CrocieraTerm(c) \implies i < f$

### [V.Tappa.Escursione]

per ogni escursione relativa alla destinazione, deve esserci un ordine (le escursioni sono comprese tra inizio e fine della tappa)
$\forall i,t,d,e,i',i'',f,f' \space|\space ItTappa(i,t) \wedge TappaDest(t,d) \wedge DestEsc(d,e) \wedge Inizio(t,i') \wedge Fine(t,f) \wedge Inizio(e,i'') \wedge Fine(e,f') \implies i' \le i'' \wedge f' \le f $

### [V.Tappa.Disjointed]

### [V.Destinazione.Disjointed]

### [V.Tappa.InizioFine]

### [V.Escursione.InizioFine]

### [V.Cliente.Continuita]

## Use-cases

### [1] Prenotazione(c: Cliente, p: Prenotazione, c': Crociera): bool

  **pre-condizioni:**
  $¬ CrocieraTerm(c') \wedge PrenCroc(c',p) \wedge ClPre(c,p)$
  **post-condizione**
  $NumPosti(p) = postiPrenotati$
  $\forall n \space|\space CrocNav(c',n) \wedge Nave.PostiDisponibili(n)=postiRimanenti$ 
  $ (postiRimanenti - postiPrenotati \ge 0 \implies Result = True) \wedge (postiRimanenti - postiPrenotati \le 0 \implies Result = False) $

### [2] CrocieraEsotica(i: DataOra, f: DataOra):Reale $\ge$ 0

  **pre-condizione:**

  $i < f$

  **post-condizione:**
  
  $C = \set{c | \exist it,d,t,con,nome \space CrocieraTerm(c) \wedge CrocIt(c,it) \wedge (Arrivo(it,d) \vee Partenza(it,d) \vee ( ItTappa(it,t) \wedge TappaDest(t,d))) \wedge ContDest(con,d) \wedge Nome(con,nome) \wedge nome \not = Europa}$
  
  $CL_{eta} = \set{eta \space|\space \exist c,p,c',i' Cliente(c) \wedge Eta(c)=eta \wedge ClPre(c,p) \wedge PrenCroc(p,c') \wedge c' \in C \wedge i \le i' \wedge i' \le f}$
  
  $Eta = \sum eta \in CL_{eta}$
  
  $Result = Eta/|CL_{eta}|$

### [3.1] DestinazioneGettonata(d: Destinazione, i: DataOra, f: Dataora):bool

  **pre-condizione:**
  
  $i \le f$

  **post-condizione:**

  $LDM = \set{c \space|\space \exist it,t,c,i',f' \space\space CrocIt(c,it) \wedge  ItTappa(it,t) \wedge TappaDest(t,d) \wedge LunaDiMiele(c) \wedge Inizio(c,i') \wedge Fine(c,f') \wedge i \le i' \wedge f' \le f}$

  $F = \set{c \space|\space \exist it,t,c \space\space CrocIt(c,it) \wedge  ItTappa(it,t) \wedge TappaDest(t,d) \wedge PerFamiglie(c) \wedge Inizio(c,i') \wedge Fine(c,f') \wedge i \le i' \wedge f' \le f}$

  $(|F| >= 15 \vee |LDM| >= 10 \implies Result = True) \space\wedge\space $
  $(|F| < 15 \vee |LDM| < 10 \implies Result = False)$

### [3.2] CrociereItinerario(it: Itinerario, i: DataOra, f: DataOra):Crociera (0..*)
  
  **pre-condizione:**

  $i \le f$

  **post-condizione:**

  $C = \set{c | \exist i',f' \space \space CrocIt(c,it) \wedge Inizio(c,i') \wedge Fine(c,f') \wedge i \le i' \wedge f' \le f}$
  
  $Result = C$

### [3] Gettonate(i: DataOra, f: DataOra):(Destinazione, Reale$\ge$0)(0..*)

  **pre-condizione:**
  
  $i \le f$
  
  **post-condizione:**

  $D = \set{(d,|{c | CrocieraItinerario(c,it,i,f)}|) | \exist c,it,t \space DestinazioneGettonata(d,i,f )= True \wedge TappaDest(d,t) \wedge ItTappa(it,t) \wedge }$

  $Tot = \sum c \in D$

  
  $Result = \set{(d,p) | d,c \in D \wedge p = c * 100 / Tot}$