# Modellizzare la struttura di un database per memorizzare tutti i dati riguardanti una università:

sono presenti diversi `Dipartimenti` (es.: Lettere e Filosofia, Matematica, Ingegneria ecc.);
ogni `Dipartimento` offre `più Corsi di Laurea` (es.: Civiltà e Letterature Classiche, Informatica, Ingegneria Elettronica ecc..)
`ogni Corso di Laurea` prevede `diversi Corsi` (es.: Letteratura Latina, Sistemi Operativi 1, Analisi Matematica 2 ecc.);
`ogni Corso` può essere tenuto da `diversi Insegnanti`;
`ogni Corso` prevede` più appelli d'Esame`;
`ogni Studente` è iscritto ad `un solo Corso di Laurea`;
`ogni Studente` può iscriversi a `più appelli di Esame`;
per `ogni appello d'Esame` a cui lo Studente ha partecipato, è necessario `memorizzare il voto ottenuto`, anche se non sufficiente.

# Pensiamo a quali entità (tabelle) creare per il nostro database e cerchiamo poi di stabilirne le relazioni. Infine, andiamo a definire le colonne e i tipi di dato di ogni tabella.

# Utilizzare https://www.drawio.com/ per la creazione dello schema.
# Esportare quindi il diagramma in png, caricarlo in un file html e pushare tutto nella repo.

# Estrapoliamo le informazioni necessarie per tenerle sotto controllo:

## Dopo l'analisi sappiamo che le informazioni principali sono le seguenti:

1) ci sono dei `Dipartimenti` (probabile table: Dipartimenti, one to many)

2) ogni `Dipartimento` offre più `Corsi di Laurea` (probabile table: Corsi_di_laurea, one to many)
# A questo punto partendo da questi dati, mi verrebbe da creare una table Dipartimenti ed una Corsi_di_laurea, dove in Corsi_di_laurea la foreign key sarà dipartimenti_id, confermando il one to many

3) Ogni `corso di laurea` ha `diversi corsi` (probabile table: Corso, one to many)
# Con l'aggiunta di questo elemento, seguirei la linea guida di sopra, creando la table Corso, dove la foreign key sarà corso_di_laurea_id

4) ogni `corso` può avere più `insegnanti` (probabile table: insegnanti, many to many)
# Analizzando questo ulteriore elemento, le vie da optare sono due, posso ipotizzare che i corsi possano avere più insegnanti, ma che lo stesso insegnante a sua volta possa avere più corsi da gestire. Seguendo questa logica, non confermata nella traccia, ma ipotizzando che il DB debba essere funzionale, opterei per un Bridge tra gli insegnanti ed i corsi, e quindi per un approccio many to many. Ciò nonostante sottolineo che attenendoci strettamente alla traccia la logica mi avrebbe spinto ad un altro approccio one to many. (table bridge: corso_insegnante, dove le fk saranno Corso_id e insegnante_id )

5) ogni `corso` può avere più `appelli` (probabile table: appelli, one to many)
# Per gli appelli ritornerei all'approccio one to many, dove come fk userei Corso_id

6) ogni `studente` è iscritto solo ad un `Corso di Laurea` (probabile table: studenti, one to many)
# Approccio one to many come sopra con foreing key Corso_di_laurea_id

7) ogni `studente` può dare più `appelli` (approccio many to many, con bridge: appelli_studenti)
# Utilizzerò un approccio alla table many to many, creando un bridge che avrà come fk appello_id e studente_id

8) ogni `appello` avrà un `voto` da memorizzare (il voto è un evento conseguente all'appello, sarebbe possibile gestirlo internamente nel bridge)
# Anche qui si potrebbe lavorare su due strade, ovvero creando una table chiamata voti dove mettere i voti degli studenti, infine un ultimo bridge che mi permette di collegare i dati dei voti con quelli degli appelli, chiamato appello_voto, dove le fk saranno appello_id e voto_id.
# OPPURE utilizzare il bridge già esistente per evitare di scrivere ulteriormente, inserendo un campo voto in appelli_studenti


### Effettuata l'analisi e strutturata la linea guida, inizio a definire la tipologia di dati e gli attributi

# Tabella: Dipartimenti (1:N)
id: es. 1 (dato: INT, attributo: PRIMARY_KEY, AUTO_INCREMENT)
nome: es. Informatica (dato: stringa, VARCHAR(50), attributo: NOTNULL)
indirizzo: es. Informatica 2 (dato: stringa, VARCHAR(50), attributo: NOTNULL)

# Tabella: Corsi_di_laurea (1:N)

id: es. 1 (dato: INT, attributo: PRIMARY_KEY, AUTO_INCREMENT)
nome: es. Sviluppo Web (dato: stringa, VARCHAR(50), attributo: NOTNULL)
dipartimento_id: (dato: INT attributo: FOREIGN_KEY, NOTNULL)

# Tabella: Corso (1:N)

id: es. 1 (dato: INT, attributo: PRIMARY_KEY, AUTO_INCREMENT)
nome: es. UX/UI (dato: stringa, VARCHAR(50), attributo: NOTNULL)
cfu: es. 18 (dato: SMALLINT, attributo: NULL)
corso_di_laurea_id: (dato: INT attributo: FOREIGN_KEY, NOTNULL)

# Tabella: Insegnanti (N:M)

id: es. 1 (dato: INT, attributo: PRIMARY_KEY, AUTO_INCREMENT)
nome: es. Fabio (dato: stringa, VARCHAR(50), attributo: NOTNULL)
cognome: es. Pacifici (dato: stringa, VARCHAR(50), attributo: NOTNULL)

# Tabella: Corso_insegnante (BRIDGE Corso_Insegnante)

corso_id: (dato: INT, attributo: FOREIGN_KEY, PRIMARY KEY)
insegnante_id: (dato: INT, attributo: FOREIGN_KEY, PRIMARY KEY)

# Tabella: Appelli (1:N)

id: es. 1 (dato: INT, attributo: PRIMARY_KEY, AUTO_INCREMENT)
giorno_ora: es. 12/03/2026 12:30 (dato: DATETIME, attributo: NOTNULL )
aula: es. 3D (dato: stringa VARCHAR(15), attributo: NULL)
Corso_id (dato: INT, attributo: FOREIGN_KEY, NOTNULL)

# Tabella: Studente (1:M)

id: es. 1 (dato: INT, attributo: PRIMARY_KEY, AUTO_INCREMENT)
nome: es. Christian (dato: stringa, VARCHAR(50), attributo: NOTNULL)
cognome: es. Matarazzo (dato: stringa, VARCHAR(50), attributo: NOTNULL)
email: es. boolean@gmail.com (dato: stringa, VARCHAR(50), attributo: NOTNULL, UNIQUE)
telefono: es. +39 123456789 (dato: stringa, VARCHAR(50), attributo: NULL, UNIQUE)
corso_di_laurea_id: (dato:INT, attributo: FOREIGN_KEY, NOTNULL)

# Tabella : Appello_Studenti (N:M)
appello_id: (dato: INT, attributo: FOREIGN_KEY, PRIMARY KEY)
studente_id: (dato: INT, attributo: FOREIGN_KEY, PRIMARY KEY)
voto: es. 20 (dato:TINYINT, attributo: NOTNULL)






