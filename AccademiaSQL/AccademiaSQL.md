# Accademia

## Tables

### Tabelle e Vincoli:

    Persona:id: Chiave primaria, intero positivo.
    nome: Stringa.
    cognome: Stringa.
    posizione: Enumerazione (Ricercatore, Professore Associato, Professore Ordinario).
    stipendio: Numero reale positivo.

    Progetto:id: Chiave primaria, intero positivo.
    nome: Stringa, unica. [1]
    inizio: Data.
    fine: Data, successiva a inizio. [1]
    budget: Numero reale positivo.

    WP (Work Package):progetto: Chiave esterna che fa riferimento a Progetto(id), intero positivo. [2]
    id: Chiave primaria (insieme a progetto), intero positivo.
    nome: Stringa.
    inizio: Data.
    fine: Data, successiva a inizio. [2]
    
    AttivitaProgetto:id: Chiave primaria, intero positivo.
    persona: Chiave esterna che fa riferimento a Persona(id), intero positivo. [2]
    progetto: Chiave esterna che fa riferimento a WP(progetto), intero positivo. [2]
    wp: Chiave esterna che fa riferimento a WP(id), intero positivo. [2]
    giorno: Data.
    tipo: Enumerazione (Ricerca e Sviluppo, Dimostrazione, Management, Altro). [3]
    oreDurata: Intero tra 0 e 8. [3]
    
    AttivitaNonProgettuale:id: Chiave primaria, intero positivo.
    persona: Chiave esterna che fa riferimento a Persona(id), intero positivo. [4]
    tipo: Enumerazione (Didattica, Ricerca, Missione, Incontro Dipartimentale, Incontro Accademico, Altro). [3]
    giorno: Data.
    oreDurata: Intero tra 0 e 8. [3]
    
    Assenza:id: Chiave primaria, intero positivo.
    persona: Chiave esterna che fa riferimento a Persona(id), intero positivo. [4]
    tipo: Enumerazione (Chiusura Universitaria, Maternita, Malattia). [3]
    giorno: Data.
    Vincolo di unicità su persona e giorno. [4]

Relazioni:

    Una Persona può partecipare a più Progetti tramite AttivitaProgetto.
    Un Progetto può avere più WP.
    Un WP può avere più AttivitaProgetto.
    Una Persona può svolgere più AttivitaNonProgettuale.
    Una Persona può avere più Assenze.

Note:

    I tipi di dato StringaM, Denaro e NumeroOre non sono definiti nei documenti. Si presume che siano tipi di dato personalizzati o alias per tipi di dato standard.
    Il diagramma UML mostra le relazioni tra le tabelle e i loro attributi, nonché i vincoli chiave e di unicità.
    Il vincolo "ennupla: inizio < fine" per le tabelle Progetto e WP significa che il valore del campo inizio deve essere inferiore al valore del campo fine. [1, 2]

Questo diagramma e la descrizione forniscono una rappresentazione visiva e testuale dello schema del database Accademia, facilitando la comprensione delle tabelle, dei loro attributi e delle relazioni tra di loro.

## Queries

### Quali sono i cognomi distinti di tutti gli strutturati?

``` SQL

    SELECT DISTINCT(p.Cognome)
    FROM Persona p,

```

### Quali sono i ricercatori?

``` SQL

    SELECT p.Nome, p.Cognome
    FROM Persona p
    WHERE p.Posizione = 'Ricercatore';
```

### Quali sono i professori associati i quali cognomi iniziano per V?

``` SQL
    
    SELECT p.Nome,p.Cognome
    FROM Persona p,
    WHERE p.Posizione = 'Professore Associato'AND p.Cognome LIKE '%V%'

```
### Quali sono i progetti terminati in data odierna?
``` SQL
    
    SELECT p.nome FROM Progetto p WHERE p.fine = CURRENT_DATE;
```

### Quali sono i progetti ordinati in ordine crescente per data d'inizio?

``` SQL
    
    SELECT nome,inizio,fine FROM Progetto ORDER BY inizio; 
```

### Quali sono i WorkPackage ordinati in modo crescente per nome?

``` SQL
    
    SELECT nome,progetto,inizio,fine FROM wp ORDER BY nome;

```

### Quali sono le cause di assenza distinte degli strutturati?

``` SQL

    SELECT DISTINCT persona.nome, persona.cognome,assenza.tipo FROM assenza,persona WHERE persona.id = assenza.persona;
```

### Quali sono le tipologie di attività progettuali distinte di tutti gli strutturati?

``` SQL

    SELECT DISTINCT p.nome, p.cognome, pr.nome,ap.tipo FROM attivitaprogetto ap, persona p, progetto pr WHERE ap.progetto = pr.id AND ap.persona = p.id;
```

### Quali sono i giorni distinti nei quali del personale ha effettuato attività non progettuali di tipo 'Didattica' (restituire in ordine crescente)

``` SQL

    SELECT DISTINCT p.nome,p.cognome, ap.giorno FROM attivitanonprogettuale ap ,persona p WHERE ap.persona = p.id  AND ap.tipo = 'Didattica' ORDER BY ap.giorno;
```
