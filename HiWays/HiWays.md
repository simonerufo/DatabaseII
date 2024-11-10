## Class diagram UML
![Alt text](src/ClassDiagramHiWays.jpeg)
### Specifica dei tipi di classe
#### Codice
Stringa alfanumerica formattata come da standard
#### Posizione
(KM:Reale >= 0, Localita: str)
#### Modalità Pagamento
Enumerazione = {"Contanti", "Carta di Credito", "Bancomat", "ViaCard"}
#### Targa
Stringa formattata come da standard
#### Tipo Veicolo
Enumerazione = {Tipo veicolo | Tipo Veicolo $\in$ Tipi Veicoli}
#### Codice fiscale
Stringa formattata come da standard
#### IBAN
Stringa formattata come da standard
#### Mese-Anno
DataOra(M,A)
## Vincoli di classe

### [V.Autostrada.Località]
*le Località collegate ad un autostrada devono essere diverse*

$\forall l,l',a \space|\space Autostrada(a) \wedge Localita(l) \wedge Localita(l') \wedge AutLoc(a,l) \wedge AutLoc(a,l') \implies l \not = l'$
### [V.Telepass.ClienteUnivoco]
*il telepass è univocamente legato ad un cliente abbonato*

$\forall t,t',c,c',a  \space|\space Telepass(t) \wedge Telepass(t') \wedge Cliente(c) \wedge Cliente(c') \wedge Abbonamento(a) \wedge Abbonamento(a') \wedge ClAb(c,a) \wedge ClAb(c',a') \wedge AbTel(a,t) \wedge AbTel(a',t') \implies t = t' \wedge c = c' \wedge a \not = a'$
### [V.ModalitàPagamento.Telepass]
*se la modalità di pagamento non è specificata, allora è telepass*
$\forall mp,t \space|\space ModalitaPagamento(mp) \wedge Tipo(mp,t) \wedge t = NULL \implies  \exists tp \space|\space Telepass(tp)$
### [V.TagliandoVirtuale.Telepass]
*se ho un TagliandoVirtuale allora è il cliente associato al veicolo  e paga con telepass*
$\forall t,cl,v,cs,mp \space|\space TagliandoVirtuale(t) \wedge Cliente(c) \wedge Veicolo(v)  \wedge Casello(cs) \wedge ClVeic(cl,v) \wedge TagVeic(t,v) \wedge Passaggio(t,cs) \implies CasPag(cs,mp) \wedge Telepass(mp)$

### [V.Casello.VolumeMax]
*il numero di veicoli al casello deve essere inferiore o uguale al VolumeMax*

### [V.Tagliando.CaselloEntrataUscita]
*il tagliando è associato ad un casello in entrata e ad uno in uscita e questi sono diversi e l'istante di entrata < l'istante di uscita*
$\forall t,i,i',ce,cu,pe,pu \space|\space Tagliando(t) \wedge Passaggio(t,ce) \wedge Passaggio(t,cu) \wedge Istante(t,ce,i) \wedge Istante(t,cu,i') \wedge Posizione(ce,pe) \wedge Posizione(cu,pu) \implies ce \not = cu \wedge i < i' \wedge pe < pu$
### [V.Posizione.Localita]
*La località deve essere una delle due associate all'autostrada*

### [V.Tutor.Continuità]
*L'istante del tutor deve essere compreso tra l'entrata e l'uscita del veicolo nel casello (istante entrata casello < istante passaggio tra i tutor < uscita casello*
$\forall v,t,tt,ce,cu,i,i',i'',e,u,p,p' \space|\space Veicolo(v) \wedge TagVeic(t,v) \wedge TutVe(tt,v) \wedge Passaggio(t,ce) \wedge Passaggio(t,cu) \wedge Posizione(ce,p) \wedge Posizione(cu,p') \wedge Istante(t,ce,i) \wedge Istante(t,cu,i') \wedge Istante(tt,v,i'') \wedge Entrata(tt,e) \wedge Uscita(tt,u) \implies p \le e \wedge u \le p' \wedge i \le i'' \wedge i'' \le i'$
### [V.Tutor.EntrataUscita]
$\forall t,e,u \space|\space Tutor(t) \wedge Entrata(t,e) \wedge Uscita(t,u) \implies e < u$
### [V.Veicolo.Disjoint]
*due veicoli  diversi non possono passare nello stesso casello nello stesso momento*
$\forall v,v',cs,t,t' \space|\space TagVeic(t,v) \wedge TagVeic(t',v') \wedge Passaggio(cs,t) \wedge Passaggio(cs,t') \wedge Istante(cs,t,i) \wedge Istante(cs,t',i') \wedge v \not = v' \wedge t \not t' \implies \not \exists t \space|\space i = t \wedge i' = t$

### [V.Tutor.Disjoint]
*due veicoli diversi non possono passare nello stesso tutor nello stesso istante*

## Use-Cases

## Ristrutturazione

## Traduzione

## SQL Queries
