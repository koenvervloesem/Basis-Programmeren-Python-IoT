## Werken met een embedded database: introductie in SQLite

### Starten met SQL

Als vervolg gaan we nu leren werken met relationele databases.

* In een **eerste stap** leren we **werken met SQL**. Hiervoor gebruiken we een grafisch programma.
* Vervolgens gaan we een **embedded database** gebruiken vanuit **Python** door middel van een **API**.

### SQLite, een relationele database in de praktijk

Eerder maakten we melding van het verschil tussen client-server en embedded databases.  
In dit deel van de cursus (kennismaking met SQL) maken we gebruik van een **embedded database** vanwege de **gebruiksvriendelijkheid** en de **snelheid van ontwikkeling**. We hoeven zo geen server op te zetten.

We kiezen voor de embedded database **SQLite** (https://www.sqlite.org).  
Deze wordt binnen de industrie gebruikt door talloze toepassingen:

* Opslag voor het besturingssysteem en apps in Android
* Kantoortoepassingen
* Geschiedenis voor webbrowsers
* Embedded systemen
* Configuratie van desktoptoepassingen

### DB Browser for SQLite

Om met SQLite te leren werken, starten we met het gebruik van een handige tool om SQL-opdrachten uit te voeren op deze database.

Download DB Browser for SQLite op https://sqlitebrowser.org of via de pakketbeheerder van je Linux-distributie (bijvoorbeeld `sudo apt install sqlitebrowser`).

### Database aanmaken

Een SQLite-database bestaat uit **één bestand**, meestal met de extensie **db**.  

Onze eerste stap is zo'n nieuwe database aanmaken. Hiervoor open je DB Browser for SQLite en klik je bovenaan links op de knop **New Database**. Daarna dien je de locatie op je computer te kiezen waar je het bestand wilt opslaan:

![](sqlite_new_db.png)

Bij het bewaren geef je deze de naam **students.db**.

Daarna verschijnt er een venster **Edit table definition**. Druk hier onderaan op **Cancel**, want dit gaan we later doen.

### Werken met SQLite

Als we nu vervolgens de database willen gebruiken om data in op te slaan of op te vragen, kunnen we hiervoor de taal SQL gebruiken.  
SQL is - zoals we in het volgende deel gaan zien - een manier om met een relationele database te 'praten' via SQL-statements (of meerdere statements na elkaar via een SQL-script).  

We komen zo dadelijk terug op SQL zelf. Hier willen we alvast demonstreren hoe we zo'n script via DB Browser for SQLite kunnen uitvoeren, aangezien we dit nog veelvuldig gaan nodig hebben.

Gegeven het volgende SQL-script:

> *Nota:* We leggen de betekenis van dit script in het volgende deel uit.

~~~sql
CREATE TABLE student (
	student_id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
	name TEXT,
	lab INTEGER,
	theory INTEGER
);

INSERT INTO student (student_id, name, lab, theory) VALUES (1, 'Joe Biden', 12, 10);
INSERT INTO student (student_id, name, lab, theory) VALUES (2, 'Donald Trump', 10, 12);
INSERT INTO student (student_id, name, lab, theory) VALUES (3, 'George Bush', 10, 12);

select * from student;
~~~

Selecteer nu nadat je je SQLite-bestand aangemaakt hebt de tab **Execute SQL**."

In het tekstveld bovenaan dit tabblad kun je SQL-opdrachten (of queries) typen om je database te ondervragen (select) of te wijzigen (insert, update, delete).  

* Kopieer het script hierboven in de SQL-editor zoals hieronder geïllustreerd.
* Druk op het playknopje bovenaan of op de toets F5.

![](sqlite_execute_sql.png)

Na het uitvoeren van het SQL-script zie je onderaan twee belangrijke vensters.  
Het venster helemaal onderaan geeft aan of je script goed of slecht is uitgevoerd:

~~~
Execution finished without errors.
Result: 3 rows returned in 21ms
At line 12:
select * from student;
~~~

Daarboven zie je ook - in tabelvorm - het resultaat van de laatste opdracht in je script:

~~~sql
select * from student;
~~~

De inhoud van de tabel student wordt dan als volgt weergegeven:

~~~
student_id  name                lab          theory
----------  ----                ---          ------
1           Joe Biden           12           10
2           Donald Trump        10           12
3           George Bush         10           12
~~~

In het volgende deel gaan we deze SQL-syntax onder de loep nemen.
