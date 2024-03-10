## Werken met SQL

### Starten met SQL

Om te kunnen werken met een (relationele) database heb je dus SQL nodig. We gaan met SQL leren werken in twee stappen:

#### SQL via DB Browser for SQLite

Om een goed begrip te krijgen van SQL, gaan we eerst leren **werken met SQL**.  
Hiervoor gebruiken we DB Browser for SQLite om op een gebruiksvriendelijke manier tabellen aan te maken, data te manipuleren en op te vragen.

~~~
+-------------------+------------+----------------+
|                   |            |                |
|                   +----------->|                |
|                   |    SQL     |                |
|   SQLitebrowser   |            |    Database    |
|                   |    DATA    |                |
|                   |<-----------+                |
|                   |            |                |
+-------------------+------------+----------------+
~~~

#### SQL vanuit een Python-applicatie

Vervolgens gaan we een **database** gebruiken vanuit **Python**. Door middel van een **API** sturen we vanuit Python SQL-opdrachten naar de (embedded) database.

~~~
+---------------+---+------------+----------------+
|               |   |            |                |
|               |   +----------->|                |
|               | A |    SQL     |                |
|  Applicatie   | P |            |    Database    |
|               | I |    DATA    |                |
|               |   |<-----------+                |
|               |   |            |                |
+---------------+---+------------+----------------+
~~~

### Wat is SQL?

SQL of **Structured Query Language** is een taal die we gebruiken om met een **database** te spreken.  
De taal is onderverdeeld in drie delen:

* **DDL** - Data Definition Language  => Beschrijven van datastructuren, in ons geval tabellen
* **DML** - Data Modification Language => Ondervragen en wijzigen van data
* **DCL** - Data Control Language => Toezicht houden op de toegang tot de data

We gaan ons in deze cursus grotendeels bezig houden met **DML** om de inhoud van de database te wijzigen en op te vragen.  
We gaan echter van start met DDL om tabellen te definiëren waarin we onze data kunnen opslaan.

#### ANSI/ISO-standaard

SQL is ook een **ANSI/ISO-standaard** die ondersteund wordt door diverse **databasemanagementsystemen** (DBMS).  
Alle moderne (relationele) databaseproducten gebruiken vandaag dezelfde taal om data te definiëren en te manipuleren.  
Uiteraard voorzien vele systemen wel **eigen uitbreidingen of dialecten**... Als je die gebruikt, werkt je software alleen met dit specifieke DBMS samen. Dat bemoeilijkt het overstappen naar een ander DBMS.

#### Declaratieve programmeertaal

SQL wordt ook beschouwd als een programmeertaal. Maar het is zeker geen programmeertaal zoals we deze hebben gezien met Python.  

SQL is wat we noemen een **declaratieve programmeertaal**. Je **beschrijft** eerder **welke data** je wilt hebben.  
Dit in tegenstelling tot **imperatieve** programmeertalen waar je beschrijft **hoe** je deze data wilt verkrijgen (met behulp van variabelen, voorwaarden, lussen, ...)

#### SQL-statements en -scripts

SQL maakt gebruik van verschillende soorten statements:

* **create-** en **drop-statements** om tabellen aan te maken (DDL)
* **select-statements** of **queries** om data op te halen (DML)
* **insert-**, **update-** en **delete-**statements om de data te wijzigen (DML)

Een voorbeeld hieronder is een select-statement om gegevens op te halen uit een studenten-tabel.

~~~sql
select * from student;
~~~

Deze statements worden bijna altijd ook beëindigd met een `;`. Dit is zeker van belang wanneer je met SQL-scripts werkt, waarbij je meerdere statements combineert om uit te voeren.

In onderstaand voorbeeld voeren we drie insert-statements in één script uit:

~~~sql
INSERT INTO student (student_id, name, lab, theory) VALUES (1, 'Joe Biden', 12, 10);
INSERT INTO student (student_id, name, lab, theory) VALUES (2, 'Donald Trump', 10, 12);
insert into student (student_id, name, lab, theory) values (3, 'George Bush', 10, 12);
~~~

> Merk op: SQL is **case-insensitive**. Het laatste statement van vorig script is volledig in kleine letters geschreven om te laten zien dat dit exact hetzelfde werkt.

Om de verschillende statements van elkaar te scheiden, dien je er een `;` tussen te zetten.

### DDL => Tabellen aanmaken

Alvorens de database te kunnen gebruiken, dien je eerst tabellen aan te maken. Dat doe je met **create-statements**.

In de eerdere hoofdstukken hadden we een studenten-applicatie gemaakt. We bouwen hier op verder om SQL te demonstreren.

We gaan van start met een tabel aan te maken om **studentendata op te slaan**. 

Deze tabel zal dan de data bevatten van de klasse `Student`. Ter herhaling, die bevatte de volgende drie attributen:

* Naam
* Labo-punten
* Theorie-punten

Voor elk van de attributen van deze klasse maken we een veld aan binnen een **tabel**. Die velden vormen de **kolommen** van de tabel.  
Een tabel bestaat dus uit:

* Kolommen of velden die zo'n tabel bevat (projectie)
* Rijen van data

In het voorbeeld hieronder bevat de tabel twee rijen met studentendata:

~~~
+---------------+---------------+---------------+
| student_name  |      lab      |     theory    |
+---------------+---------------+---------------+  
|  Bart Voet    |      15       |      16       |
+---------------+---------------+---------------+  
| Jan Janssens  |      17       |      14       |
+---------------+---------------+---------------+
~~~

#### CREATE TABLE

##### Poging 1 (zonder velden)

Om deze structuur aan te maken, gebruiken we een `create`-statement:

* Dit start met `create table`
* Gevolgd door een **naam** voor deze structuur (`student` in dit geval)
* Gevolgd door een paar **haakjes**
* En gevolgd door een `;`

> Merk op: SQL is voor wat betreft syntaxis (niet voor data) case-insensitive... `CREATE TABLE` is dus gelijk aan `create table`, maar een tabel met de naam `student` is een andere dan een tabel met de naam `Student`.

Vul dit statement in het tekstveld links bovenaan van het tabblad **Execute SQL** van DB Browser for SQLite in:

~~~sql
CREATE TABLE student 
(

);
~~~

We klikken vervolgens op de play-knop om dit statement uit te voeren... en we krijgen een foutmelding:

![](sql_error.png)

~~~
Result: near ")": syntax error
At line 1:
CREATE TABLE student 
(

)
~~~

Deze foutmelding is normaal, want dit is nog **geen geldig statement**. Een tabel moet immers **minstens één kolom**/veld bevatten.

##### Poging 2 (met velden)

Deze velden voeg je tussen de haakjes toe:

* gescheiden door komma's
* met een type (`text` of `integer`)

Er zijn vanzelfsprekend meerdere types, maar we beperken ons voorlopig tot de bovenstaande.

~~~sql
CREATE TABLE student 
(
    student_name TEXT,
    lab INTEGER,
    theory INTEGER
);
~~~

![](sql_success.png)

Deze poging is succesvol. De uitvoer duidt aan dat de tabel gecreëerd is:

~~~
Result: query executed successfully. Took 1ms
At line 1:
CREATE TABLE student 
(
    student_name TEXT,
    lab INTEGER,
    theory INTEGER
);
~~~

Als je nu navigeert naar de tab **Database Structure**, zie je ook dat deze tabel is **aangemaakt**:

![](sql_structure.png)

#### DROP TABLE: Tabellen verwijderen

Een ander DDL-statement (naast `create`) is `drop`.  
Dit gebruik je om een tabel te verwijderen uit een database.

We gaan dit vervolgens doen om daarna de tabel weer aan te maken (om primary en foreign keys te demonstreren).

~~~sql
drop table student;
~~~

Als je dan probeert de tabel op te vragen, krijg je de melding dat de tabel niet bestaat:

~~~
Execution finished with errors.
Result: no such table: student
At line 1:
select * from student;
~~~

> **Waarschuwing:** Je hebt nu zowel de definitie van de tabel als eventueel aanwezige data verwijderd. Dit is onherroepelijk.

### Data manipuleren (DML)

Met de voorgaande operaties (**DDL**) kunnen we dus tabellen **definiëren**.  
Om de tabellen met data te vullen, hebben we DML nodig.

#### CRUD

Bij het werken met data spreken we vaak van CRUD.
Dit acroniem staat voor de vier **basisoperaties** die op data uitgevoerd kunnen worden:

* **Create** (of insert): Toevoegen van nieuwe data
* **Read** (of select): Opvragen van gegevens
* **Update**: Wijzigen van gegevens
* **Delete**: Verwijderen van gegevens

De vertaling van deze CRUD-operaties gebeurt binnen SQL via DML-statements.

#### Data toevoegen via het insert-statement

We keren hiervoor terug naar het tabblad **Execute SQL**  om te zien hoe we **data toevoegen** aan een **tabel**. Maak eerst de tabel weer aan. Voeg dan een regel met data toe met behulp van het `insert`-statement:

~~~sql
insert into student(student_name,lab,theory) values("Bart Voet",15,16);
~~~

Als dit succesvol is, krijg je een vergelijkbare uitvoer zoals hieronder:

~~~
Execution finished without errors.
Result: query executed successfully. Took 0ms, 1 rows affected
At line 1:
insert into student(student_name,lab,theory) values("Bart Voet",15,16);
~~~

Dit **insert-statement** bestaat uit:

* de **keyword**-combinatie `insert into`
* gevolgd door de **tabel**  `student`
* de **velden** die je wilt **updaten** (tussen de haakjes gescheiden door komma's)
* gevolgd door het **keyword** `values`
* en daarna de **waarden** tussen **haakjes**, ook **gescheiden door komma's** (zoals bij de kolomnamen)


Bemerk ook dat rond **strings** aanhalingstekens komen, net zoals we in Python gewoon zijn.  
Om de volgende stappen te kunnen uitvoeren, voegen we nog een **tweede rij** toe aan de database:

~~~sql
insert into student(student_name,lab,theory) values("Jan Janssens",17,14);
~~~

met als resultaat:

~~~
Execution finished without errors.
Result: query executed successfully. Took 0ms, 1 rows affected
At line 1:
insert into student(student_name,lab,theory) values("Jan Janssens",17,14);
~~~

#### Data lezen via het select-statement

Om de data die we zojuist hebben ingevoerd te kunnen **lezen**, gebruiken we een tweede soort SQL-statements, namelijk `select`-statements of korter gezegd **queries**.  
Typ het volgende statement in de SQL-editor:

~~~sql
select * from student;
~~~

Dit statement bestaat uit drie onderdelen:

* het keyword `select`
* een \* om aan te geven dat je **alle velden** wilt selecteren
* het keyword `from` gevolgd door de naam van de tabel waaruit je data wilt opvragen

Als dit succesvol is, krijg je de data te zien zoals hieronder:

![](sql_select.png)

De data die je in tabelvorm te zien krijgt, stemt dan overeen met de data die je hebt toegevoegd via de voorgaande `insert`-statements:

~~~
student_name  lab         theory    
------------  ----------  ----------
Bart Voet     15          16        
Jan Janssens  17          14  
~~~

#### Beperken van de velden (projectie)

Het symbool `*` zorgt ervoor dat **alle velden** worden **geprojecteerd** in de uitkomst.  

Als je echter niet alle velden nodig hebt, kun je in plaats van een `*` ook de **velden projecteren** die je wilt zien, gescheiden door een komma:

~~~sql
select student_name, lab from student;
~~~

In dit geval zie je dat alleen de naam en de de labopunten worden **geprojecteerd**:

~~~
student_name  lab       
------------  ----------
Bart Voet     15        
Jan Janssens  17
~~~

Dit bepalen of beperken van de kolommen noemen we een **projectie**.

#### Gebruik van `where`-clausules (selectie)

Naast de **projectie** die het aantal kolommen beperkt, kunnen we ook het aantal rijen in het resultaat beperken. Dat noemen we **selectie** of filteren.  

Dit doen we door een `where`-clausule toe te voegen.
We passen bijvoorbeeld de vorige query aan om de studenten te selecteren met meer dan 16 punten op hun labo:

~~~sql
select student_name, lab from student where lab > 16;
~~~

Het resultaat wordt **gefilterd** en we krijgen alleen de student te zien met meer dan 16 punten:

~~~
student_name  lab       
------------  ----------
Jan Janssens  17  
~~~

Dit filteren of beperken van het aantal rijen noemen we binnen **SQL** (en relationele algebra) een **selectie**.  
Je kunt voor dit filteren verschillende operatoren gebruiken, zoals:

~~~
=	Gelijk aan	
>	Groter dan	
<	Kleiner dan	
>=	Groter dan of gelijk aan
<=	Kleiner dan of gelijk aan	
<>	Niet gelijk aan
~~~

Deze zijn (met uitzondering van `<>` dat overeenkomt met `!=` in Python) **hetzelfde** zoals je deze in **Python-code** zou gebruiken in voorwaarden.  

Als je de selectie in het statement omdraait (kleiner dan of gelijk aan 16 in plaats van groter dan 16)...

~~~sql
select student_name, lab from student where lab <= 16;
~~~

... krijg je als resultaat:

~~~
student_name  lab       
------------  ----------
Bart Voet     15    
~~~

#### `where` combineren met `and` of `or`

Om de volgende oefeningen te kunnen voortzetten, voegen we eerst nog wat rijen toe:

~~~sql
insert into student(student_name,lab,theory) values("Piet Pieters",9,12);
insert into student(student_name,lab,theory) values("Joris Jorissen",11,12);
~~~

Je kan nu (net zoals in Python) ook **voorwaarden combineren** met **and** of **or**.  
We willen bijvoorbeeld de studenten opvragen die meer dan 14 op hun labo of hun theorie hebben:

~~~sql
select student_name, lab, theory from student where lab > 14 or theory > 14;
~~~

We zien hier dat Jan Janssens op labo een 17 scoort en op theorie een 14. Omdat aan één van beide voorwaarden voldaan is (`lab > 14` of `theory > 14`), wordt hij ook geselecteerd:

~~~
student_name  lab         theory
------------  ----------  ----------
Bart Voet     15          16
Jan Janssens  17          14
~~~

De twee nieuwe studenten krijg je echter niet te zien, omdat hun scores voor zowel labo als theorie lager zijn. We kunnen nu ook voorwaarden combineren met **and** om de studenten op te vragen die zowel op theorie als labo punten kleiner dan of gelijk aan 14 hebben:

~~~sql
select student_name, lab, theory from student where lab <= 14 and theory <= 14;
~~~

Dan krijg je:

~~~
student_name  lab         theory    
------------  ----------  ----------
Piet Pieters  9           12        
Joris Joriss  11          12    
~~~

#### Data bewerken met update

Tot nu toe kunnen we:

* Een tabel aanmaken met `create`
* Data injecteren met `insert`
* Data ophalen via `select`
* Data filteren via `where`

Wat als je data wilt **aanpassen** in een **bestaande rij**? Dit kun je met het statement `update`:

~~~sql
update student set lab = 10 where student_name = "Piet Pieters";
~~~

Als we nadien de tabel bekijken, zien we dat de de labopunten van de betreffende student (Piet Pieters) aangepast zijn (van 9 naar 10):

~~~
student_name  lab         theory    
------------  ----------  ----------
Bart Voet     15          16        
Jan Janssens  17          14        
Piet Pieters  10          12        
Joris Joriss  11          12  
~~~

De syntaxis is hier:

* keyword `update`
* gevolgd door de **tabelnaam**
* dan het keyword `set`
* gevolgd door de eigenlijke update met de vorm **kolomnaam = waarde**
* en als laatste het keyword `where` gevolgd door de voorwaarde die aangeeft welke rijen een update moeten krijgen

> **Waarschuwing:**  
> Als je deze where-clausule vergeet, loop je het risico dat je alle rijen aanpast.

#### Bewerk meerdere velden

Je kunt ook **meerdere velden tegelijk** updaten. Hiervoor voeg je een nieuw update-veld/onderdeel toe, gescheiden door een komma:

~~~sql
update student set lab = 16, theory = 15 where student_name = "Jan Janssens";
~~~

Hierdoor passen we zowel de labo- als de theoriepunten van Jan Janssens aan:

~~~
student_name  lab         theory    
------------  ----------  ----------
Bart Voet     15          16        
Jan Janssens  16          15        
Piet Pieters  10          12        
Joris Joriss  11          12   
~~~

#### Bewerk meerdere rijen

Een update werkt met een `where`-clausule waarmee je de rij selecteert die je wilt aanpassen.  
Meerdere rijen tegelijk is op deze manier ook mogelijk.  

Stel dat we alle studenten die minder dan 12 punten hebben op het labo een punt willen bijgeven wegens goede medewerking in de les. Dan doen we dat als volgt:

~~~sql
update student set lab = lab + 1 where lab < 12;
~~~

Het resultaat? Piet Pieters en Joris Jorissen die 10 respectievelijk 11 op hun labo hadden, hebben nu 11 respectievelijk 12 punten:

~~~
student_name  lab         theory    
------------  ----------  ----------
Bart Voet     15          16        
Jan Janssens  16          15        
Piet Pieters  11          12        
Joris Joriss  12          12  
~~~

Je zag trouwens dat je in de update kunt verwijzen naar een waarde van deze rij zelf, door bij lab 1 bij te tellen (`set lab = lab + 1`).  

Een tweede voorbeeld: we willen studenten die meer labopunten hebben dan theorie een punt bijgeven op theorie. Dat kan als volgt:

~~~sql
update student set theory = theory + 1 where lab > theory;
~~~

Jan Janssens, die op labo 16 had en op theorie 15, krijgt hierdoor op theorie een punt extra:

~~~
student_name  lab         theory    
------------  ----------  ----------
Bart Voet     15          16        
Jan Janssens  16          16        
Piet Pieters  11          12        
Joris Joriss  12          12   
~~~

#### Delete

De laatste operatie die we nog niet in SQL hebben gezien, is `delete`. Net zoals bij `update` dien je hierbij een `where`-clausule te gebruiken. Dit is eigenlijk niet verplicht, maar zonder die clausule verwijder je alle gegevens, en dat wil je meestal niet.

Stel dat de student "Bart Voet" onrechtmatig toegang heeft gekregen tot de cursus. Dan verwijder je deze als volgt:

~~~sql
delete from student where student_name = "Bart Voet";
~~~

Nadien zie je dat deze student verdwenen is uit de tabel:

~~~
student_name  lab         theory    
------------  ----------  ----------
Jan Janssens  16          16        
Piet Pieters  11          12        
Joris Joriss  12          12   
~~~

> **Waarschuwing:**  
> Opnieuw dezelfde waarschuwing als bij `update`. Let op dat je
> `where`-clausule correct gedefinieerd is. Want je kan **meerdere** 
> **studenten** tot en met de **hele tabel** verwijderen.

### Identiteit en sleutels

In een database is het belangrijk dat je rijen **uniek** kunt identificeren/herkennen.  
Dit gebeurt door één of een combinatie van meerdere velden te gebruiken als **sleutel**.

#### Primary key

Tot nu gebruikten we in onze database met studenten de kolom `student_name` om een student uniek te identificeren.  
Je kunt ervoor zorgen dat de database voor jou controleert of deze naam uniek is via een **primary key constraint**:

* **Primary key** slaat op het feit dat elke waarde in deze kolom uniek is en dat dit de primaire sleutel is om een rij op te zoeken.
* **Constraints** zijn controles of voorwaardes die een database voor jou kan controleren en forceren.

> Merk op: Dit kan ook via een **unique-key constraint**. Daar komen we later nog op terug...

Verwijder de bestaande tabel met `drop table student;` en maak een nieuwe tabel aan met het `create`-statement. Deze keer kennen we aan de kolom name de primary key constraint toe:

~~~sql
CREATE TABLE student 
(
    name TEXT PRIMARY KEY,
    lab INTEGER,
    theory INTEGER
);
~~~

We hebben hier een keyword `PRIMARY KEY` toegevoegd aan de eerste kolom.  
Hierdoor zal de database er voor zorgen dat de tabel altijd consistent is volgens deze constraint:

* Er kan maar één student met dezelfde naam zijn.
* Elke student heeft verplicht een naam.

 
Vervolgens voegen we twee nieuwe studenten toe:

~~~sql
insert into student(name,lab,theory) values("Bart Voet",15,16);
insert into student(name,lab,theory) values("Jan Janssens",17,14);
~~~

Tot nog toe zien we het zelfde resultaat als voorheen:

~~~
name        lab         theory    
----------  ----------  ----------
Bart Voet   15          16        
Jan Jansse  17          14      
~~~

Daarna voegen we nog een student toe, maar zijn naam bestaat al in de database...

~~~sql
insert into student(name,lab,theory) values("Bart Voet",17,18);
~~~

De **database** zal dit echter **niet toestaan** dankzij de **constraint** die je eerder hebt toegevoegd.

~~~
Execution finished with errors.
Result: UNIQUE constraint failed: student.name
At line 1:
insert into student(name,lab,theory) values("Bart Voet",17,18);
~~~

#### Automatische primary keys (increment)

Eén van de problemen met de voorgaande aanpak is dat in principe wel mogelijk is dat twee studenten dezelfde naam hebben.
Wat daarom veel wordt gedaan, is gebruikmaken van **numerieke ID's** om verschillende studenten een unieke nummer te geven. Jullie studentennummers bij UCLL zijn daar een voorbeeld van.

Om dit te doen, voegen we een veld/kolom `student_id` toe en definiëren we deze als uniek ID in plaats van de naam: 

~~~sql
drop table student;

CREATE TABLE student 
(
    student_id INTEGER PRIMARY KEY,
    name TEXT,
    lab INTEGER,
    theory INTEGER
);
~~~

Als je bij een integer een **primary key** maakt, kun je gebruikmaken van **autoincrement**.  
Dit houdt in dat deze waarde - indien je ze niet meegeeft aan de `insert` - automatisch de huidige hoogste waarde + 1 verkrijgt.

~~~sql
insert into student(name,lab,theory) values("Bart Voet",15,16);
insert into student(name,lab,theory) values("Jan Janssens",17,14);
insert into student(name,lab,theory) values("Piet Pieters",9,12);
insert into student(name,lab,theory) values("Joris Jorissen",11,12);
~~~

Als we hierna in de database kijken, zien we dat dit veld automatisch wordt aangevuld:

~~~
student_id  name        lab         theory    
----------  ----------  ----------  ----------
1           Bart Voet   15          16        
2           Jan Jansse  17          14        
3           Piet Piete  9           12        
4           Joris Jori  11          12   
~~~

Zo'n **autoincrement** mechanisme bestaat voor elke relationele database, maar het mechanisme om dit te definiëren **kan verschillen** per database.  

Het bestaan van autoincrement betekent trouwens niet dat je deze waarde niet meer expliciet mag meegeven. In onderstaande code voegen we een student toe met ID 8:

~~~sql  
insert into student(student_id,name,lab,theory) values(8,"Piet Pieters",9,12);
~~~

We zien vervolgens dat deze waarde ook effectief wordt aangenomen, aangezien er nog geen student met ID 8 in de tabel stond:

~~~
student_id  name        lab         theory    
----------  ----------  ----------  ----------
1           Bart Voet   15          16        
2           Jan Jansse  17          14        
3           Piet Piete  9           12        
4           Joris Jori  11          12        
8           Piet Piete  9           12        
~~~

Als je nadien weer een student zonder ID toevoegt, zal dit weer automatisch een ID krijgen dat 1 hoger is dan het **hoogste ID**:

~~~sql
insert into student(name,lab,theory) values("Korneel Kortens",9,12);
~~~

De student heeft hier het ID 9 verkregen, hetgeen 1 hoger is dan het (eerder) hoogste ID (dat we handmatig hadden ingesteld):

~~~
student_id  name        lab         theory    
----------  ----------  ----------  ----------
1           Bart Voet   15          16        
2           Jan Jansse  17          14        
3           Piet Piete  9           12        
4           Joris Jori  11          12        
8           Piet Piete  9           12        
9           Korneel Ko  9           12  
~~~

### Werken met meerdere tabellen

In een relationele database werk je in de meeste gevallen met meerdere tabellen. Het is de bedoeling om relaties te leggen tussen deze tabellen.

#### Startsituatie

We starten vanuit de voorgaande tabel:

~~~
+------------------+
| student          |
+------------------+
| student_id (*)   |
| name             |
| theory           |
| lab              |
+------------------+
~~~

met volgende data:

~~~
student_id  name        lab         theory    
----------  ----------  ----------  ----------
1           Bart Voet   15          16        
2           Jan Jansse  17          14        
3           Piet Piete  9           12        
4           Joris Jori  11          12        
8           Piet Piete  9           12        
9           Korneel Ko  9           12  
~~~

#### Gedupliceerde data

Stel dat we de volgende **drie nieuwe velden** willen toevoegen:

~~~
+------------------+
| student          |
+------------------+
| student_id (*)   |
| name             |
| theory           |
| lab              |
| group            |
| teacher          |
| room             |
+------------------+
~~~

Als we naar de data kijken die in onze tabel zouden moeten komen, zien we echter dat deze nieuwe velden zich per groep herhalen:

~~~
student_id  name        lab         theory       group   teacher          room
----------  ----------  ----------  ----------   ------  -------          ----
1           Bart Voet   15          16           A       Linus Torvalds   1b
2           Jan Jansse  17          14           A       Linus Torvalds   1b
3           Piet Piete  9           12           A       Linus Torvalds   1b
4           Joris Jori  11          12           B       Bill Gates       2c
8           Piet Piete  9           12           B       Bill Gates       2c
9           Korneel Ko  9           12           B       Bill Gates       2c
~~~

Er is dus eigenlijk sprake van **duplicatie** over de verschillende rijen.

#### Normalisatie

Om deze duplicatie te vermijden, kunnen we informatie in verband met een studentengroep isoleren in een specifieke tabel:

~~~
+------------------+             +------------------+
| student          |             |   student_group  |
+------------------+             +------------------+
| student_id  (*)  |             | group_name (*)   |
| theory           |             | teacher          |
| lab              |             | room             |
+------------------+             +------------------+

~~~

We maken dus in de database een **nieuwe tabel** aan met de naam `student_group` die **drie velden** bevat:

~~~sql
create table student_group
(
    group_name text primary key,
    teacher text,
    room text
);
~~~

De inhoud van de tabellen zou er als volgt kunnen uitzien:

~~~
student_id  name        lab         theory    
----------  ----------  ----------  ----------
1           Bart Voet   15          16        
2           Jan Jansse  17          14        
3           Piet Piete  9           12        
4           Joris Jori  11          12        
8           Piet Piete  9           12        
9           Korneel Ko  9           12        


group_name  teacher         room
----------  --------------  ----------
A           Linus Torvalds  1A
B           Bill Gates      2A
~~~

We noemen dit proces **normalisatie**: we ontdubbelen gegevens door ze in een afzonderlijke tabel te plaatsen.

#### Relaties: Verwijzen vanuit tabellen

Wat er nu nog **ontbreekt**, is een **link tussen** beide **tabellen**.  
Binnen relationele databases wordt dit gedaan door een **veld** toe te voegen waarin men de **waarde** van de **primary key** bewaart van een rij in de andere tabel.

Binnen ons voorbeeld kunnen we dit doen door aan de tabel `student` een veld `fk_student_group` toe te voegen. Dit dan kan gebruikt worden om een **link** te leggen tussen een **student** en haar/zijn **groep**.

> Merk op: je mag dit veld noemen zoals je wilt. Maar we volgen voorlopig de conventie om "fk_" te laten volgen door de naam van de tabel waarnaar je verwijst...

~~~
+------------------+             +--------------------+
| student          |             |   student_group    |
+------------------+             +--------------------+
| student_id (*)   |    +------->| group_name (*)     |
| name             |    |        | teacher            |
| theory           |    |        | room               |
| lab              |    |        +--------------------+
| fk_student_group +----- 
+------------------+
~~~

Als je dan naar de inhoud kijkt, zie je dat elke student - binnen het veld `fk_student_group` - een verwijzing bevat naar de betreffende groep.

~~~
student_id  name        lab         theory       fk_student_group
----------  ----------  ----------  ----------   ----------------
1           Bart Voet   15          16           A
2           Jan Jansse  17          14           A
3           Piet Piete  9           12           A
4           Joris Jori  11          12           B
8           Piet Piete  9           12           B
9           Korneel Ko  9           12           B


group_name  teacher         room
----------  --------------  ----------
A           Linus Torvalds  1A
B           Bill Gates      2A
~~~

De bijhorende tabeldefinities zouden er dan als volgt uitzien:

~~~sql
drop table if exists student;
drop table if exists student_group;

create table student_group
(
    group_name text primary key,
    teacher text,
    room text
);

create table student
(
    student_id integer primary key,
    name text,
    lab integer,
    theory integer,
    fk_student_group text
);
~~~

Merk op: met `if exists` zorgen we dat we de tabellen alleen verwijderen als ze bestaan. Zo vermijden we een foutmelding als we een niet bestaande tabel proberen te verwijderen.

#### Relaties leggen in SQL

##### Foreign keys

Eerder vermeldden we dat we de database konden laten verzekeren/controleren dat een veld uniek is.  
Naast deze eerder vermelde **primary key** bieden relationele databases de mogelijkheid om deze link te maken via een **foreign key**.  

Deze foreign key is een constraint die garandeert dat gelijk welke waarde die je in het veld `fk_student_group` zet een waarde is die effectief ook voorkomt in de database.

Om een **kolom** aan te duiden als een **foreign key** gebruik je het keyword `references`.  
Bekijk hieronder de **gewijzigde tabeldefinitie** voor `student`:

~~~sql
drop table if exists student;
drop table if exists student_group;

create table student_group
(
    group_name text primary key,
    teacher text,
    room text
);

create table student
(
    student_id INTEGER PRIMARY KEY,
    name TEXT,
    lab INTEGER,
    theory INTEGER,
    fk_student_group text references student_group
);
~~~

##### Referentiële integriteit

Er zijn twee belangrijke redenen voor het gebruik van een foreign key:

* Garanderen van **referentiële integriteit**
* Performantie en creatie van een index

**Referentiële integriteit** betekent dat de database controleert dat de links tussen tabellen correct zijn.  
Met andere woorden: de database zorgt ervoor dat je geen data kunt toevoegen met een foutieve link.

Laten we eerst twee studentengroepen toevoegen:

~~~sql
insert into student_group(group_name, teacher, room) values("A","Linus Torvalds","1A");
insert into student_group(group_name, teacher, room) values("B","Bill Gates","2A");
~~~

Stel dat je nu een student wilt toevoegen, maar verwijst naar een niet bestaande studenten-groep:

~~~sql
insert into student(name,lab,theory,fk_student_group) values("bart",10,10,"C");
~~~

Dan zal de SQL-engine je een foutmelding geven:

~~~
Execution finished with errors.
Result: FOREIGN KEY constraint failed
At line 1:
insert into student(name,lab,theory,fk_student_group) values("bart",10,10,"C");
~~~

Een foreign key garandeert dus dat de links tussen tabellen correct zijn.  

Als je daarentegen eerst een student_group-record aanmaakt met de primaire sleutel "C" en dan ook een student die naar die groep verwijst:

~~~sql
insert into student_group(group_name,teacher,room) values("C","Jan Janssens","5B");
insert into student(name,lab,theory,fk_student_group) values("bart",10,10,"C")
~~~

... dan wordt de student wel aangemaakt zonder foutmelding:

~~~
Execution finished without errors.
Result: query executed successfully. Took 0ms, 1 rows affected
At line 2:
insert into student(name,lab,theory,fk_student_group) values("bart",10,10,"C")
~~~

##### Foreign key en lege velden

Een foreign key constraint zal wel toelaten dat de link/referentie geen waarde bevat (`null`, wat overeenkomt met `None` in Python):

~~~sql
insert into student(name,lab,theory,fk_student_group) values("jan",10,10,null);
~~~

Dit is overigens hetzelfde als:

~~~sql
insert into student(name,lab,theory) values("bart",10,10);
~~~

Als je nu alle studenten opvraagt:

~~~sql
select * from student;
~~~

... krijg je als resultaat:

~~~
student_id  name        lab         theory       fk_student_group
----------  ----------  ----------  ----------   ----------------
1           bart        10          10           C
2           jan         10          10           NULL
~~~

Je ziet hier dat student jan geen link naar een student-groep heeft, maar de lege waarde `NULL` heeft.

Merk op: je kunt bij het aanmaken van een tabel altijd extra constraints opleggen aan kolommen. Zo kun je met `NOT NULL` bij een kolom aanduiden dat deze altijd een waarde moet hebben die niet gelijk aan `NULL` is.

### Joins

Als je werkt met je data verspreid over meerdere tabellen, is het nuttig om het principe van een SQL-join te kennen.

#### Startsituatie

Laten we beginnen met een startsituatie aan te maken:

~~~sql
drop table if exists student;
drop table if exists student_group;

create table student_group
(
    group_name text primary key,
    teacher text,
    room text
);

create table student
(
    student_id INTEGER PRIMARY KEY,
    name TEXT,
    lab INTEGER,
    theory INTEGER,
    fk_student_group text references student_group
);

insert into student_group(group_name, teacher, room) values("A","Linus Torvalds","1A");
insert into student_group(group_name, teacher, room) values("B","Bill Gates","2A");

insert into student(name,lab,theory,fk_student_group) values("Bart",15,16,"A");
insert into student(name,lab,theory,fk_student_group) values("Jan",17,14,"A");
insert into student(name,lab,theory,fk_student_group) values("Piet",9,12,"B");
insert into student(name,lab,theory,fk_student_group) values("Joris",11,12,"B");
~~~

Bekijk de tabel met studenten:

~~~sql
select * from student;
~~~

Het resultaat:

~~~
student_id  name        lab         theory      fk_student_group
----------  ----------  ----------  ----------  ----------------
1           Bart        15          16          A               
2           Jan         17          14          A               
3           Piet        9           12          B               
4           Joris       11          12          B
~~~

En de tabel met studentgroepen:

~~~sql
select * from student_group;
~~~

Het resultaat:

~~~
group_name  teacher         room      
----------  --------------  ----------
A           Linus Torvalds  1A        
B           Bill Gates      2A         
~~~

#### Zonder joins...

Als we - met wat we tot nu toe weten - alle data willen bekijken voor één student, moeten we hier altijd twee queries voor aanmaken:

* Een eerste waarmee we de data zoeken voor de student (bijvoorbeeld met id 2)

~~~sql
select * from student where student_id = 2;
~~~

...met als resultaat:

~~~
student_id  name        lab         theory      fk_student_group
----------  ----------  ----------  ----------  ----------------
2           Jan         17          14          A               
~~~

* Een tweede waarmee we de studentengroep-data oproepen aan de hand van de `fk_student_group`

~~~sql
select * from student_group where group_name = "A";
~~~

...met als resultaat:

~~~
group_name  teacher         room      
----------  --------------  ----------
A           Linus Torvalds  1A        
~~~

#### Cartesisch product

Er is een manier om de data in één keer binnen te trekken. SQL geeft ons namelijk de mogelijkheid om data van meerdere tabellen te combineren.  

In onderstaande `select`-statement vragen we de data op van zowel de tabellen student als student_group:

~~~sql
select name, lab, theory, group_name, teacher, room from student, student_group where student_id = 2;
~~~

...met als resultaat:

~~~
name        lab         theory      group_name  teacher         room      
----------  ----------  ----------  ----------  --------------  ----------
Jan         17          14          A           Linus Torvalds  1A        
Jan         17          14          B           Bill Gates      2A 
~~~

Dit resultaat is echter **niet correct** of toch niet wat we wensen...  
Dit is het resultaat van een **cartesisch product**, namelijk dat als je meerdere tabellen adresseert de SQL-engine alle rijen van de ene tabel (student) met die van andere tabel (student_group) zal **combineren**.

Dit wordt nog duidelijker als je de `where`-conditie weglaat:

~~~sql
select name, lab, theory, group_name, teacher, room from student, student_group;
~~~

Hier zie je ook dat alle rijen van student en student_group worden gecombineerd.  

Het **aantal rijen** in dit **resultaat** is dus ook het **aantal rijen** van **student** **vermenigvuldigd** met het **aantal rijen** van **student_group**.  

Gezien we 4 studenten hebben en 2 groepen zitten we aan een totaal van 8 rijen...

~~~
name        lab         theory      group_name  teacher         room      
----------  ----------  ----------  ----------  --------------  ----------
Bart        15          16          A           Linus Torvalds  1A        
Bart        15          16          B           Bill Gates      2A        
Jan         17          14          A           Linus Torvalds  1A        
Jan         17          14          B           Bill Gates      2A        
Piet        9           12          A           Linus Torvalds  1A        
Piet        9           12          B           Bill Gates      2A        
Joris       11          12          A           Linus Torvalds  1A        
Joris       11          12          B           Bill Gates      2A 
~~~

#### Joins (impliciete syntax)

Als we **alleen** de **info** willen zien van de **studenten-groepen** die **gelinkt** zijn aan de **betreffende studenten**, kunnen we dit oplossen door een **where-conditie** toe te voegen:

~~~sql
select name, lab, theory, group_name, teacher, room 
from student, student_group 
where student.fk_student_group = student_group.group_name;
~~~

> Merk op: als de SQL-queries complexer worden, is het duidelijker
> als je de query verspreidt over meerdere regels.

Hier zien we dat de studenten worden getoond met hun **groep** waarnaar verwezen wordt vanuit de **foreign-key**:

~~~
name        lab         theory      group_name  teacher         room      
----------  ----------  ----------  ----------  --------------  ----------
Bart        15          16          A           Linus Torvalds  1A        
Jan         17          14          A           Linus Torvalds  1A        
Piet        9           12          B           Bill Gates      2A        
Joris       11          12          B           Bill Gates      2A  
~~~

Het toevoegen en beperken van deze conditie/filter noemen we een **join**.  
Als we dit nu toepassen op ons oorspronkelijk voorbeeld (student met id 2), moet je enkel deze `where`-conditie toevoegen met een `and`:

~~~sql
select name, lab, theory, group_name, teacher, room 
from student, student_group 
where student_id = 2
and student.fk_student_group = student_group.group_name;
~~~

...met als resultaat:

~~~
name        lab         theory      group_name  teacher         room      
----------  ----------  ----------  ----------  --------------  ----------
Jan         17          14          A           Linus Torvalds  1A        
~~~

En zo verkrijgen we dus alle informatie over één student met één query.

#### Joins (expliciete syntax)

Gezien dit join-patroon constant wordt gebruikt, heeft SQL hier ook een speciale syntaxis voor voorzien.  
Het voorbeeld - equivalent aan voorgaande query - met deze syntaxis gaat als volgt:

~~~sql
select name, lab, theory, group_name, teacher, room 
from student
inner join student_group on fk_student_group = group_name
where student_id = 2;
~~~

Dit werkt als volgt:

* In plaats van een tweede tabel toe te voegen aan de `where`-clausule voeg je een `inner join` toe.
* In plaats van de voorwaarde toe te voegen aan de `where` (via `and`) wordt deze verwerkt in deze `inner join`.
