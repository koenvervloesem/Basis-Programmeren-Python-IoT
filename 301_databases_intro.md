## Data opslaan met databases

Software-applicaties dienen in de meeste gevallen data op te slaan, we noemen dit ook wel **persisteren** of **persistentie**.

Een eerste manier is data opslaan via een bestand, zoals we in deel 2 al gezien hebben.   
Als we echter veel of complexe data willen opslaan, maken we beter gebruik van een database (databank).  

### Wat is een database?

Een **database** is een apart stuk **software of service** dat:

* **data** kan opslaan
* op een **gestructureerde manier** 
* op een **duurzame** manier (**persisteren**)
* en deze nadien terug kan **opvragen** (query)

#### CRUD en queries

Een database voorziet voor het opslaan van data in **CRUD**-operaties:

* **C**reate: je kan **nieuwe data** aanmaken
* **R**ead: je kan deze data opnieuw **opvragen** via **zoekopdrachten** of **queries**
* **U**pdate: je kan deze data opnieuw **wijzigen**
* **D**elete: je kan de data ook (selectief indien nodig) verwijderen

### Client-server vs embedded databases

Een **DBMS** (Database Management System) is meestal als een **service** in een **client-server** model beschikbaar op een netwerk en wordt dan gedeeld door meerdere gebruikers.

#### Client-server

Een applicatie maakt over het **netwerk** een **verbinding** met een **database** om data op te vragen of te manipuleren (via de querytaal SQL of op een andere manier) en krijgt data terug.

~~~
+-------------------+  NETWORK   +----------------+
|                   |            |                |
|                   +----------->|                |
|                   |    SQL     |                |
|  Applicatie       |            |    Database    |
|                   |    DATA    |                |
|                   |<-----------+                |
|                   |            |                |
+-------------------+            +----------------+
~~~

Bekende voorbeelden van zulke databases zijn bijvoorbeeld MySQL, MariaDB, PostgreSQL, Oracle Database, Microsoft SQL Server, ...  

#### Database (SQL) API's

Een applicatie gaat meestal **niet rechtstreeks** praten met de database. Om **complexiteit** te **verbergen** (netwerkverbinding, SQL en parameters aanbrengen, ...) bieden de meeste datatabases een **API** (en/of **driver**) aan om de **communicatie** te verzorgen met de database.

~~~
+---------------+---+  NETWORK   +----------------+
|               |   |            |                |
|               |   +----------->|                |
|               | A |    SQL     |                |
|  Applicatie   | P |            |    Database    |
|               | I |    DATA    |                |
|               |   |<-----------+                |
|               |   |            |                |
+---------------+---+            +----------------+
~~~

#### Embedded databases

In veel gevallen hoeven de data niet noodzakelijk gedeeld te worden tussen verschillende applicaties of apparaten op het netwerk.  
Een voorbeeld is een smartphone (of een app op de smartphone) die wat lokale gegevens of instellingen wil bijhouden, de geschiedenis van een webbrowser, ...

Deze tegenhanger van  **client-server-systemen** noemen we een **embedded database**.  
In dit geval draait de database **op dezelfde machine** en in de **meeste gevallen** is de database ook **ingebed** (embedded) in dezelfde **applicatie**. Ze maakt een onderdeel uit can de applicatie.

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

### Relationele databases (of databanken)

Voorgaande opdeling was op basis van **infrastructuur**.  
Daarnaast kun je ook een opdeling maken tussen database op basis van **technologie**, zoals er zijn **relationele**, **documentgebaseerde**, ...  

De databasetechnologie die wij gaan gebruiken zijn **relationele databases**, of ook wel **RDBMS** (relational database management system) genoemd.

Zo'n database is in de kern gebaseerd op een aantal **basisprincipes**:

* Data wordt gestructureerd opgeslagen in **tabellen**.
* Deze tabellen bestaan uit **kolommen** of **velden**.
* Men kan **relaties** of verbanden leggen **tussen** deze **tabellen** (vandaar de naam **relationeel**).
* Om deze **verbanden** te kunnen leggen, maakt men gebruik van **sleutels** of **keys**.
* Men kan een aantal **regels** of **constraints** opleggen aan deze tabellen of velden.
* Deze data kunnen worden opgevraagd via de taal **SQL** (Structured Query Language).

#### Tabellen, kolommen en rijen

Een database bestaat uit **één of meerdere tabellen**:

* Een tabel kun je het best vergelijken met een tabel zoals je ze uit spreadsheets kent.
* Zo'n tabel definieert **kolommen of velden**. Die hebben een **naam** en een **type**.
* De eenheden van data noemen we **rijen**. Deze bevatten voor elke kolom een waarde.

Ter illustratie zie je hier een voorbeeld van een tabel **student** met 3 kolommen (velden) en 5 rijen met data over studenten:

~~~
        Tabel: student
            kolom 1         kolom 2          kolom 3
            type: text      type: int        type: int 
               |               |                |
         +---------------+---------------+---------------+
         | student_name  |      lab      |     theory    |
         +---------------+---------------+---------------+  
rij 1 -- |  Bart Voet    |      15       |      16       |
         +---------------+---------------+---------------+  
rij 2 -- | Jan Janssens  |      17       |      14       |
         +---------------+---------------+---------------+
rij 3 -- | Piet Pieters  |      15       |      14       |
         +---------------+---------------+---------------+
rij 4 -- | Korneel Kos   |      11       |      12       |
         +---------------+---------------+---------------+
rij 5 -- | Joris Jos     |      10       |      14       |
         +---------------+---------------+---------------+
~~~

#### Sleutels, relaties en verbanden

Een database is vanzelfsprekend niet beperkt tot één tabel.  
Er kunnen meerdere tabellen worden gedefinieerd en gebruikt.

Stel dat je informatie wilt bijvoegen over de klas waar deze studenten aan verbonden zijn.
Een eerste probeersel - maar **niet zo'n efficiënte** manier - is deze data **toe te voegen** aan **dezelfde tabel**.

~~~
         +---------------+                                                      
         | student       | 
         +---------------+---------------+---------------+---------------+---------------+---------------+     
         | student_name  |      lab      |     theory    |  class_name   |    teacher    |     room      |
         +---------------+---------------+---------------+---------------+---------------+---------------+  
         |  Bart Voet    |      15       |      16       |      1        |    Wim        |      A        |
         +---------------+---------------+---------------+---------------+---------------+---------------+
         | Jan Janssens  |      17       |      14       |      1        |    Wim        |      A        |
         +---------------+---------------+---------------+---------------+---------------+---------------+
         | Piet Pieters  |      15       |      14       |      2        |    Thierry    |      B        |
         +---------------+---------------+---------------+---------------+---------------+---------------+
         | Korneel Kos   |      11       |      12       |      2        |    Thierry    |      B        |
         +---------------+---------------+---------------+---------------+---------------+---------------+
         | Joris Jos     |      10       |      14       |      2        |    Thierry    |      B        |
         +---------------+---------------+---------------+---------------+---------------+---------------+
~~~

Hier krijg je echter het **probleem** dat je de klas-informatie (leraar en lokaal) moet **dupliceren** voor elke student.  

Een andere mogelijkheid is dat je een **aparte tabel** bijhoudt met alleen de **klas-informatie** (gemeenschappelijk voor alle studenten van dezelfde klas).  
Zo'n tabel zou er dan als volgt uit kunnen zien:

~~~
          **PRIMARY KEY**
         +---------------+                                                      
         |    class      |                                                      
         +---------------+---------------+---------------+                      
         | class_name (*)|    Teacher    |     Room      |                      
         +---------------+---------------+---------------+                      
         |      1        |      Wim      |      A        |
         +---------------+---------------+---------------+
         |      2        |    Thierry    |      B        |
         +---------------+---------------+---------------+
~~~

Bemerk het **sterretje** dat ik in de tekening heb geplaatst naast de **kolom class_name**.  
Hiermee duid ik aan dat de waarde van deze kolom **uniek** is (en moet zijn) **over alle rijen** heen... 

Dit noemen we in relationele databases een **primary key**.  
Wat is het **nut** van zo'n primaire of unieke sleutel?

Dat wordt duidelijk als we de twee tabellen naast (of onder) elkaar zetten.  
Je kan nu namelijk - relationele - **verbanden tussen** de verschillende **tabellen** (student en class) gaan leggen.  

In onderstaand voorbeeld zie je dat we aan de class-tabel een kolom (class_name) toegevoegd hebben.  
Deze kolom bevat een waarde die **verwijst** naar de kolom class_name in de tabel class, we noemen dit ook wel een **foreign key**

~~~
         +---------------+                                                      
         | student       |                               **FOREIGN KEY**
         +---------------+---------------+---------------+---------------+     
         | student_name  |      lab      |     theory    |  class_name   |
         +---------------+---------------+---------------+---------------+  
         |  Bart Voet    |      15       |      16       |      1        +-------+
         +---------------+---------------+---------------+---------------+       |
         | Jan Janssens  |      17       |      14       |      1        +-------+
         +---------------+---------------+---------------+---------------+       |
         | Piet Pieters  |      15       |      14       |      2        +---+   |
         +---------------+---------------+---------------+---------------+   |   |
         | Korneel Kos   |      11       |      12       |      2        +---+   |
         +---------------+---------------+---------------+---------------+   |   |
         | Joris Jos     |      10       |      14       |      2        +---+   |
         +---------------+---------------+---------------+---------------+   |   |
                                                                             |   |
+--------------------------------------<-------------------------------------+   |
|                                                                                |
|   +----------------------------------<-----------------------------------------+
|   |
|   |     **PRIMARY KEY**
|   |    +---------------+                                                      
|   |    |    class      |                                                      
|   |    +---------------+---------------+---------------+                      
|   |    | class_name (*)|    Teacher    |     Room      |                      
|   |    +---------------+---------------+---------------+                      
|   +--->|      1        |      Wim      |      A        |
|        +---------------+---------------+---------------+
+------->|      2        |    Thierry    |      B        |
         +---------------+---------------+---------------+
~~~

#### Normalisatie

Wat je hier ziet is het proces van **normalisatie**. We vermijden duplicatie van data door herhaalde of gemeenschappelijke data in een afzonderlijke tabel te plaatsen.

#### Constraints

Elke relationele database zal deze begrippen van **primary key** en **foreign key** ondersteunen en dit zelfs **garanderen**.
Deze **garantie** zorgt voor **consistentie** tussen de **verschillende tabellen**. We noemen dit ook wel **referentiële integriteit**:

* Zo'n primary key is **gegarandeerd uniek** binnen een tabel, dus ze kan maar naar één rij verwijzen.
* Elke foreign key moet naar een geldige/bestaande waarde van een unieke **primary key** van de andere tabel verwijzen.

Het garanderen van deze regels noemen we in een database ook wel **constraints**.

#### Joins

Dankzij de constraints kunnen we informatie uit verschillende tabellen **samenvoegen** via een **join**-operatie als je de database ondervraagt.  
Hoe dit gebeurt, gaan we verder bekijken als we SQL uitleggen. Een voorbeeld van zo'n SQL-query is:

TODO: deze query checken

~~~sql
select student.student_name, student.lab, student.theory, class.class_name, class.teacher, class.room
from student, class
where student.class_name = student.class_name
~~~

Met als **resultaat** de gegevens van beide tabellen **gecombineerd** met elkaar:

~~~
         +---------------+                                                      
         | student-query | 
         +---------------+---------------+---------------+---------------+---------------+---------------+     
         | student_name  |      lab      |     theory    |  class_name   |    teacher    |     room      |
         +---------------+---------------+---------------+---------------+---------------+---------------+  
         |  Bart Voet    |      15       |      16       |      1        |    Wim        |      A        |
         +---------------+---------------+---------------+---------------+---------------+---------------+
         | Jan Janssens  |      17       |      14       |      1        |    Wim        |      A        |
         +---------------+---------------+---------------+---------------+---------------+---------------+
         | Piet Pieters  |      15       |      14       |      2        |    Thierry    |      B        |
         +---------------+---------------+---------------+---------------+---------------+---------------+
         | Korneel Kos   |      11       |      12       |      2        |    Thierry    |      B        |
         +---------------+---------------+---------------+---------------+---------------+---------------+
         | Joris Jos     |      10       |      14       |      2        |    Thierry    |      B        |
         +---------------+---------------+---------------+---------------+---------------+---------------+
~~~
