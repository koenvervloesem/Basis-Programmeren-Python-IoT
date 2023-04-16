## Databases met Python

We hebben nu:

* een eerste introductie gehad in de principes van een **database** en SQL
* dit **toegepast** op een SQL-database, in dit geval de embedded database **SQLite**
* een **database en tabellen aangemaakt**
* data via **CRUD-statements** opgevraagd en gewijzigd

Dan gaan we nu met databases in Python werken.

### Voorbereiding 1: Hernemen van studenten-applicatie

Als voorbeeld hernemen we een variant van de applicatie die we eerder in de cursus hebben gemaakt rond studenten:

~~~python
class Student:
    def __init__(self, name, lab_points=0, theory_points=0):
        self.name = name
        self.lab_points = lab_points
        self.theory_points = theory_points

    def points(self):
        return (self.lab_points + self.theory_points) // 2

    def succeeded(self):
        return self.points() >= 10

    def __str__(self):
        return f"Student {self.name} heeft {self.lab_points} voor het labo en {self.theory_points} voor theorie, dus een gemiddelde van {self.points()}."


students = []

students.append(Student("Jan Janssens", 15, 17))
students.append(Student("Piet Pieters", 16, 12))

for student in students:
    print(student)
~~~

Deze applicatie laat ons toe om studentengegevens in een lijst op te slaan en deze te hergebruiken.

~~~
Student Jan Janssens heeft 15 voor het labo en 17 voor theorie, dus een gemiddelde van 16.
Student Piet Pieters heeft 16 voor het labo en 12 voor theorie, dus een gemiddelde van 14.
~~~

### Voorbereiding 2: Interactiviteit toevoegen

We breiden dit programma nu uit door er wat **interactiviteit** aan toe te voegen met een **eenvoudig menu**:

~~~python
class Student:
    def __init__(self, name, lab_points=0, theory_points=0):
        self.name = name
        self.lab_points = lab_points
        self.theory_points = theory_points

    def points(self):
        return (self.lab_points + self.theory_points) // 2

    def succeeded(self):
        return self.points() >= 10    
        
    def __str__(self):
        return f"Student {self.name} heeft {self.lab_points} voor het labo en {self.theory_points} voor theorie, dus een gemiddelde van {self.points()}."


students = []


def input_number(request):
    return int(input(request))


menu = """
1. Voeg student toe
2. Toon studenten
3. Stop applicatie
"""
while True:

    menu_input = input(menu)

    if menu_input == "1":
        student_name = input("Naam student: ")
        lab_points = input_number("Labo-punten: ")
        theory_points = input_number("Theorie-punten: ")
        students.append(Student(student_name, lab_points, theory_points))
    elif menu_input == "2":
        for student in students:
            print(student)
    elif menu_input == "3":
        print("")
        quit()
    else:
        print("Ongeldige invoer")
~~~

Hiermee kun je de studenten dan vanuit een eenvoudige command-line-interface **bewerken**:

~~~
python3 students.py 

1. Voeg student toe
2. Toon studenten
3. Stop applicatie
1
Naam student: Jan Janssens
Labo-punten: 15
Theorie-punten: 12

1. Voeg student toe
2. Toon studenten
3. Stop applicatie
1
Naam student: Bart Voet
Labo-punten: 15
Theorie-punten: 16

1. Voeg student toe
2. Toon studenten
3. Stop applicatie
2
Student Jan Janssens heeft 15 voor het labo en 12 voor theorie, dus een gemiddelde van 13.
Student Bart Voet heeft 15 voor het labo en 16 voor theorie, dus een gemiddelde van 15.

1. Voeg student toe
2. Toon studenten
3. Stop applicatie
3

~~~

Dit programma heeft één groot nadeel: zodra je het afgesloten hebt, zijn de data die je ingevoerd hebt verloren.
Als je dat niet wil, moeten we deze code omvormen om de **data in een database** bij te houden.

### SQLite in Python - basis

De SQL-opdrachten die we eerder gezien hebben, kun je vanuit een API (application programming interface) doorsturen naar een database.

~~~
+---------------+---+---------+----------------+
|               |   |         |                |
|               |   +-------->|                |
|               | A |   SQL   |                |
|  Applicatie   | P |         |  Database      |
|               | I |   DATA  |                |
|               |   |<--------+                |
|               |   |         |                |
+---------------+---+---------+----------------+
~~~

In plaats van een SQL-editor te gebruiken, kun je dus **vanuit je Python-code met de SQL-database praten**...

> Nota: Gedetailleerde documentatie rond het gebruik hiervan vind je in https://docs.python.org/3/library/sqlite3.html. Wij beperken ons hier tot de basisprincipes.


#### Verbinding maken met een database

In een eerste stap maak je een **verbindingsobject** aan.
Dat doe je met de volgende code:

~~~python
import sqlite3
con = sqlite3.connect('students.db')
cur = con.cursor()
# Hier komt de code om de database te ondervragen
con.close()
~~~

Deze code verbindt met een SQLite-database waarvan het bestand zich in dezelfde map bevindt als waar je de Python-code uitvoert.  
Als dit bestand nog niet bestaat, maakt de functie `connect` in dit bestand een lege database aan.

Het is belangrijk dat je achteraf de database vrijgeeft voor gebruik via de functie `close`.

#### Automatisch sluiten met with

Voorgaande code heeft een probleem: als het programma een exception genereert, wordt de functie `close` niet aangeroepen.

Dit kan je, zoals we eerder bij het openen van een bestand gezien hebben, in Python vermijden met het keyword `with`. Het gebruik hiervan is een **best practice** wanneer je in Python werkt met objecten zoals bestanden of database- of netwerkverbindingen.

Voor een database werkt dit als volgt:

~~~python
import sqlite3
with sqlite3.connect('students.db') as con:
    cur = con.cursor()
    # Hier komt de code om de database te ondervragen
~~~

Na het uitvoeren van dit `with`-blok wordt de functie `close` automatisch op het verbindingsobject aangeroepen. De verbinding wordt dan na het blok automatisch gesloten en de gebruikte resources worden teruggegeven.

Meer informatie over `with` vind je in https://www.python.org/dev/peps/pep-0343/ (Python Enhancement Proposal 343).

#### Een database query uitvoeren
In de bovenstaande code maakten we door de methode `cursor` op het verbindingsobject uit te voeren een cursorobject aan.

Op dit cursusobject kunnen we nu database queries uitvoeren via de methode `execute`:

~~~python
import sqlite3
with sqlite3.connect('students.db') as con:
    cur = con.cursor()
    query_result = cur.execute('SELECT student_id, name, lab, theory FROM student')
~~~

Het resultaat hiervan - een lijst van records - is in je Python-code beschikbaar als een iterator. Met een for-lus kun je door alle elementen gaan:

~~~python
    for row in query_result:
        print("Student", row[1], "met id", row[0])
~~~

Als we dit programma nu uitvoeren op een studentendatabase, krijgen we iets te zien als het volgende:

~~~
$ python3 students.py
Student Bart met id 1
Student Jan met id 2
Student Piet met id 3
Student Joris met id 4
~~~

Vergelijk dit met het resultaat van de SQL-query `SELECT student_id, name, lab, theory FROM student` als je die uitvoert in DB Browser for SQLite op dezelfde database. De variabele `row` in de `for`-lus van ons Python-programma verwijst in elke iteratie van de lus naar een nieuwe rij in de resultaten. Met `row[0]` verwijzen we naar het eerste veld in de rij, namelijk `student_id`, en met `row[1]` naar het tweede veld in de rij, namelijk `name`.

#### Gebruik van parameter substitution

Eerder hadden we gezien dat we binnen een `where`-clausule in een SQL-statement voorwaarden kunnen toevoegen om een filtering/selectie toe te passen.  
Hiervoor voorziet de SQLite API van Python de mogelijkheid van **substitutie**. Stel dat je alleen studenten wilt opvragen die meer dan 10 scoren op hun labo. Dan kun je dit als volgt doen:

~~~python
import sqlite3

with sqlite3.connect('students.db') as con:
    cur = con.cursor()
    query_result = cur.execute('SELECT student_id, name, lab, theory FROM student where lab > ?', [10])
    for row in query_result:
        print("Student", row[1], "met id", row[0])
~~~

De conventie is dat je op de plaats waar je een parameter wilt toevoegen een vraagteken plaatst. Het tweede argument van de methode `execute` is dan een lijst met de opeenvolgende parameters, hier één element, namelijk 10.

Het resultaat?

~~~
$ python3 students.py
Student Bart met id 1
Student Jan met id 2
Student Joris met id 4
~~~

Niets weerhoudt je ervan om meerdere parameters toe te voegen, bijvoorbeeld:

~~~python
import sqlite3

with sqlite3.connect('students.db') as con:
    cur = con.cursor()
    query_result = cur.execute('SELECT student_id, name, lab, theory FROM student where lab > ? and theory > ?', [10, 12])
    for row in query_result:
        print("Student", row[1], "met id", row[0])
~~~

Het resultaat:

~~~
$ python3 students.py
Student Bart met id 1
Student Jan met id 2
~~~

### Persistentie in de studentenapplicatie

In onderstaande code passen we bovenstaande uitleg toe rond het gebruik van een verbinding en cursor binnen Python:

~~~python
import sqlite3
import sys


class Student:
    def __init__(self, name, lab_points=0, theory_points=0, id=0):
        self.name = name
        self.lab_points = lab_points
        self.theory_points = theory_points
        self.student_id = id

    def points(self):
        return (self.lab_points + self.theory_points) // 2

    def succeeded(self):
        return self.points() >= 10

    def __str__(self):
        return f"Student {self.name} heeft {self.lab_points} voor het labo en {self.theory_points} voor theorie, dus een gemiddelde van {self.points()}."


STUDENT_DB_FILE_NAME = "students.db"


def init_database():
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        con.execute(
            """
            CREATE TABLE IF NOT EXISTS student (
                student_id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
                name TEXT,
                lab INTEGER,
                theory INTEGER
            );
        """
        )


def get_students():
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        query_result = con.execute("SELECT student_id, name, lab, theory FROM student")
        students = []
        for row in query_result:
            students.append(Student(row[1], row[2], row[3], row[0]))
    return students


def save_new_student(student):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        cur = con.cursor()
        cur.execute(
            "INSERT INTO student(name, lab, theory) VALUES(?,?,?)",
            [student.name, student.lab_points, student.theory_points],
        )
        con.commit()


def input_number(request):
    number_input = input(request)
    return int(number_input)


MENU = """
1. Voeg student toe
2. Toon studenten
3. Stop applicatie
"""

init_database()

while True:
    menu_input = input(MENU)

    if menu_input == "1":
        student_name = input("Naam student: ")
        lab_points = input_number("Punten op labo: ")
        theory_points = input_number("Punten op theorie: ")
        save_new_student(Student(student_name, lab_points, theory_points))
    elif menu_input == "2":
        for student in get_students():
            print(student)
    elif menu_input == "3":
        print("")
        sys.exit()
    else:
        print("Ongeldige invoer")
~~~

Laten we hier eens door gaan... De klasse `Student` nemen we over van vroeger, maar we voegen in de constructor nog een attribuut `id` toe, met als standaardwaarde 0.

#### Initialiseren van de database

We maken de database aan bij opstarten van de applicatie. Verwijder een eventueel nog bestaand bestand students.db om dit te testen.

Het SQL-keyword `IF NOT EXISTS` zorgt ervoor dat de tabel de volgende keren dat je het programma uitvoert niet opnieuw aangemaakt wordt als deze dan al bestaat. We initialiseren de database dus met de volgende functie `init_database`:

~~~python
STUDENT_DB_FILE_NAME = "students.db"


def init_database():
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        con.execute(
            """
            CREATE TABLE IF NOT EXISTS student (
                student_id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
                name TEXT,
                lab INTEGER,
                theory INTEGER
            );
        """
        )
~~~

Die functie roepen we aan bij het opstarten van de applicatie:

~~~python
MENU = """
1. Voeg student toe
2. Toon studenten
3. Stop applicatie
"""

init_database()

while True:
~~~

#### Ophalen van studenten-data

We maken ook een functie die het resultaat van een SQL-query omzet naar een lijst met objecten van de klasse `Student`:

~~~python
def get_students():
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        query_result = con.execute("SELECT student_id, name, lab, theory FROM student")
        students = []
        for row in query_result:
            students.append(Student(row[1], row[2], row[3], row[0]))
    return students
~~~

We roepen deze functie aan wanneer de gebruiker in het menu voor optie 2 kiest en tonen alle studenten in de lijst:

~~~python
    elif menu_input == "2":
        for student in get_students():
            print(student)
~~~

#### Aanmaken van een student

Een nieuwe student aanmaken doen we in de functie `save_new_student`. Deze heeft als argument een object van de klasse `Student` en voegt deze in de databasetabel toe:

~~~python
def save_new_student(student):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        cur = con.cursor()
        cur.execute(
            "INSERT INTO student(name, lab, theory) VALUES(?,?,?)",
            [student.name, student.lab_points, student.theory_points],
        )
        con.commit()
~~~

We roepen die functie aan wanneer de gebruiker in het menu voor optie 1 kiest:

~~~python
    if menu_input == "1":
        student_name = input("Naam student: ")
        lab_points = input_number("Punten op labo: ")
        theory_points = input_number("Punten op theorie: ")
        save_new_student(Student(student_name, lab_points, theory_points))
~~~

Probeer het programma maar eens uit. Start het op, voeg wat studenten toe (optie 1 in het menu) en toon de lijst met studenten (optie 2). Sluit het programma af en start het opnieuw op. Wanneer je nu onmiddellijk de lijst met studenten opvraagt, krijg je de studenten te zien die je voorheen toegevoegd hebt. De persistentie werkt dus! Het programma leest immers de studenten uit de database in en toont ze. Als je nog extra studenten toevoegt, worden die ook aan de database toegevoegd.

### Studenten updaten, verwijderen en opzoeken

Nu we studenten kunnen toevoegen en bekijken, voegen we nog drie functies toe:

* Pas de punten van een student aan
* Verwijder een student
* Zoek een student individueel op

De volledige code ziet er als volgt uit:

~~~python
import sqlite3
import sys


class Student:
    def __init__(self, name, lab_points=0, theory_points=0, id=0):
        self.name = name
        self.lab_points = lab_points
        self.theory_points = theory_points
        self.student_id = id

    def points(self):
        return (self.lab_points + self.theory_points) // 2

    def succeeded(self):
        return self.points() >= 10

    def __str__(self):
        return f"Student {self.name} (ID {self.student_id}) heeft {self.lab_points} voor het labo en {self.theory_points} voor theorie, dus een gemiddelde van {self.points()}."


STUDENT_DB_FILE_NAME = "students.db"


def init_database():
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        con.execute(
            """
            CREATE TABLE IF NOT EXISTS student (
                student_id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
                name TEXT,
                lab INTEGER,
                theory INTEGER
            );
        """
        )


def get_students():
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        query_result = con.execute("SELECT student_id, name, lab, theory FROM student")
        students = []
        for row in query_result:
            students.append(Student(row[1], row[2], row[3], row[0]))
    return students


def save_new_student(student):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        cur = con.cursor()
        cur.execute(
            "INSERT INTO student(name, lab, theory) VALUES(?,?,?)",
            [student.name, student.lab_points, student.theory_points],
        )
        con.commit()


def get_student(id):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        query_result = con.execute(
            "SELECT student_id, name, lab, theory FROM student WHERE student_id = ?",
            [id],
        )
        row = query_result.fetchone()
    return Student(row[1], row[2], row[3], row[0])


def update_points(id, lab, theory):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        update_statement = (
            "UPDATE student SET lab = ? , theory = ? WHERE student_id = ?"
        )
        con.execute(update_statement, [lab, theory, id])
        con.commit()


def delete_student(id):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        con.execute("DELETE FROM student WHERE student_id = ?", [id])
        con.commit()


def input_number(request):
    number_input = input(request)
    return int(number_input)


MENU = """
1. Voeg student toe
2. Toon studenten
3. Toon student
4. Update punten student
5. Verwijder student
6. Stop applicatie
"""

init_database()

while True:
    menu_input = input(MENU)

    if menu_input == "1":
        student_name = input("Naam student: ")
        lab_points = input_number("Punten op labo: ")
        theory_points = input_number("Punten op theorie: ")
        save_new_student(Student(student_name, lab_points, theory_points))
    elif menu_input == "2":
        for student in get_students():
            print(student)
    elif menu_input == "3":
        id = input_number("ID: ")
        student = get_student(id)
        print(student)
    elif menu_input == "4":
        print("Kies student met id")
        for student in get_students():
            print("Student met id", student.student_id, "en naam", student.name)
        id = input_number("ID: ")
        lab = input_number("Punten op labo: ")
        theory = input_number("Punten op theorie: ")
        update_points(id, lab, theory)
    elif menu_input == "5":
        print("Verwijder student met id")
        for student in get_students():
            print("Student met id", student.student_id, "en naam", student.name)
        id = input_number("ID: ")
        delete_student(id)
    elif menu_input == "6":
        print("")
        sys.exit()
    else:
        print("Ongeldige invoer")
~~~

Laten we hier weer eens door gaan. Merk allereerst op dat we de methode `__str__` van de klasse `Student` aangepast hebben zodat die nu ook het ID van de student toont, omdat we dit nodig hebben.

#### Een student individueel opzoeken

We voegen een optie toe om een individuele student op te vragen, en creëren daarvoor een functie `get_student`. Het belangrijkste verschil met de functie `get_students` is dat we hier één rij verwachten van de database. We maken daarom gebruik van de methode `fetchone` op de query.

Aan de functie `get_student` geven we het ID van de gevraagde student door:

~~~python
def get_student(id):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        query_result = con.execute(
            "SELECT student_id, name, lab, theory FROM student WHERE student_id = ?",
            [id],
        )
        row = query_result.fetchone()
    return Student(row[1], row[2], row[3], row[0])
~~~

In het menu roepen we deze functie als volgt aan:

~~~python
    elif menu_input == "3":
        id = input_number("ID: ")
        student = get_student(id)
        print(student)
~~~

#### De punten van een student aanpassen

Met de functie `update_points` passen we de punten van een student aan:

~~~python
def update_points(id, lab, theory):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        update_statement = (
            "UPDATE student SET lab = ? , theory = ? WHERE student_id = ?"
        )
        con.execute(update_statement, [lab, theory, id])
        con.commit()
~~~

Aan deze functie geven we het ID van de student door en de punten die we de student willen geven voor het labo en voor theorie. Let op het SQL-statement waarbij we student met het gegeven ID selecteren.

Deze functie roepen we nu aan als de gebruiker in het menu optie 4 kiest:

~~~python
    elif menu_input == "4":
        print("Kies student met id")
        for student in get_students():
            print("Student met id", student.student_id, "en naam", student.name)
        id = input_number("ID: ")
        lab = input_number("Punten op labo: ")
        theory = input_number("Punten op theorie: ")
        update_points(id, lab, theory)
~~~

#### Een student verwijderen

De laatste toevoeging is het verwijderen van een student. De functie daarvoor is vrij eenvoudig:

~~~python
def delete_student(id):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        con.execute("DELETE FROM student WHERE student_id = ?", [id])
        con.commit()
~~~

En die roepen we als volgt aan als de gebruiker in het menu optie 5 kiest:

~~~python
    elif menu_input == "5":
        print("Verwijder student met id")
        for student in get_students():
            print("Student met id", student.student_id, "en naam", student.name)
        id = input_number("ID: ")
        delete_student(id)
~~~

### Werken met meerdere groepen

Toen we SQL introduceerden, hebben we gezien dat je ook met meerdere tabellen kunt werken. We maakten toen naast de tabel met studenten een tabel met klasgroepen aan voor die studenten. Dat kunnen we ook in ons programma doen:

~~~python
import sqlite3
import sys


class Student:
    def __init__(self, name, lab_points=0, theory_points=0, id=0):
        self.name = name
        self.lab_points = lab_points
        self.theory_points = theory_points
        self.student_id = id

    def points(self):
        return (self.lab_points + self.theory_points) // 2

    def succeeded(self):
        return self.points() >= 10

    def __str__(self):
        return f"Student {self.name} (ID {self.student_id}) heeft {self.lab_points} voor het labo en {self.theory_points} voor theorie, dus een gemiddelde van {self.points()}."


class StudentGroup:
    def __init__(self, name, teacher, room):
        self.name = name
        self.teacher = teacher
        self.room = room

    def __str__(self):
        return f"Group with name {self.name} and teacher {self.teacher} in room {self.room}"


STUDENT_DB_FILE_NAME = "students.db"


def init_database():
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        con.execute(
            """
            CREATE TABLE IF NOT EXISTS student_group
            (
                group_name TEXT PRIMARY KEY,
                teacher TEXT,
                room TEXT
            );
        """
        )

        con.execute(
            """
            CREATE TABLE IF NOT EXISTS student
            (
                student_id INTEGER PRIMARY KEY,
                name TEXT,
                lab INTEGER,
                theory INTEGER,
                fk_student_group TEXT REFERENCES student_group
            );
        """
        )


def get_students():
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        query_result = con.execute("SELECT student_id, name, lab, theory FROM student")
        students = []
        for row in query_result:
            students.append(Student(row[1], row[2], row[3], row[0]))
    return students


def get_students_for_group(group_name):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        query_result = con.execute(
            "SELECT student_id, name, lab, theory FROM student where fk_student_group = ?",
            [group_name],
        )
        students = []
        for row in query_result:
            students.append(Student(row[1], row[2], row[3], row[0]))
    return students


def get_groups():
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        query_result = con.execute(
            "SELECT group_name, teacher, room FROM student_group"
        )
        groups = []
        for row in query_result:
            groups.append(StudentGroup(row[0], row[1], row[2]))
    return groups


def save_new_student(student, group_name):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        cur = con.cursor()
        cur.execute(
            "INSERT INTO student(name, lab, theory,fk_student_group) VALUES(?,?,?,?)",
            [student.name, student.lab_points, student.theory_points, group_name],
        )
        con.commit()


def save_new_group(group_name, teacher, room):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        cur = con.cursor()
        cur.execute(
            "INSERT INTO student_group(group_name, teacher, room) values(?,?,?)",
            [group_name, teacher, room],
        )
        con.commit()


def get_student(id):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        query_result = con.execute(
            "SELECT student_id, name, lab, theory FROM student WHERE student_id = ?",
            [id],
        )
        row = query_result.fetchone()
    return Student(row[1], row[2], row[3], row[0])


def update_points(id, lab, theory):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        update_statement = (
            "UPDATE student SET lab = ? , theory = ? WHERE student_id = ?"
        )
        con.execute(update_statement, [lab, theory, id])
        con.commit()


def delete_student(id):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        con.execute("DELETE FROM student WHERE student_id = ?", [id])
        con.commit()


def input_number(request):
    number_input = input(request)
    return int(number_input)


MENU = """
a. Maak studentengroep aan
b. Toon groepen
1. Voeg student toe
2. Toon studenten
3. Toon student
4. Update punten student
5. Verwijder student
6. Stop applicatie
"""

SUB_MENU_STUDENTS = """
a. Alle studenten
b. Studenten voor groep
"""

init_database()

while True:
    menu_input = input(MENU)

    if menu_input == "a":
        group_name = input("Naam groep: ")
        lector = input("Lector: ")
        room = input("Lokaal ")
        save_new_group(group_name, lector, room)
    elif menu_input == "b":
        for student_group in get_groups():
            print(student_group)
    elif menu_input == "1":
        student_name = input("Naam student: ")
        lab_points = input_number("Punten op labo: ")
        theory_points = input_number("Punten op theorie: ")
        print("Kies uit volgende groepen: ")
        for student_group in get_groups():
            print(student_group.name)
        group_name = input("Groep: ")
        save_new_student(Student(student_name, lab_points, theory_points), group_name)
    elif menu_input == "2":
        choice = input(SUB_MENU_STUDENTS)
        if choice == "a":
            students = get_students()
        else:
            print("Kies uit volgende groepen: ")
            for student_group in get_groups():
                print(student_group.name)
            group_name = input("Groep: ")
            students = get_students_for_group(group_name)

        for student in students:
            print(student)
    elif menu_input == "3":
        id = input_number("ID: ")
        student = get_student(id)
        print(student)
    elif menu_input == "4":
        print("Kies student met id")
        for student in get_students():
            print("Student met id", student.student_id, "en naam", student.name)
        id = input_number("ID: ")
        lab = input_number("Punten op labo: ")
        theory = input_number("Punten op theorie: ")
        update_points(id, lab, theory)
    elif menu_input == "5":
        print("Verwijder student met id")
        for student in get_students():
            print("Student met id", student.student_id, "en naam", student.name)
        id = input_number("ID: ")
        delete_student(id)
    elif menu_input == "6":
        print("")
        sys.exit()
    else:
        print("Ongeldige invoer")
~~~

We gaan hier weer door, maar verwijder eerst het databasebestand `students.db` voor je het programma uitvoert, zodat we met een schone lei kunnen beginnen.

#### Extra tabel

De eerste wijziging in onze oorspronkelijke code is zorgen dat er een nieuwe tabel `student_group` wordt aangemaakt, en dat de tabel `student` met een foreign key naar die tabel verwijst. De functie `init_database` breiden we daarom uit tot:

~~~python
def init_database():
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        con.execute(
            """
            CREATE TABLE IF NOT EXISTS student_group
            (
                group_name TEXT PRIMARY KEY,
                teacher TEXT,
                room TEXT
            );
        """
        )

        con.execute(
            """
            CREATE TABLE IF NOT EXISTS student
            (
                student_id INTEGER PRIMARY KEY,
                name TEXT,
                lab INTEGER,
                theory INTEGER,
                fk_student_group TEXT REFERENCES student_group
            );
        """
        )
~~~

#### Extra opties voor groepen

Het menu breiden we uit met twee opties, één om een nieuwe groep aan te maken en een andere om alle groepen op te vragen:

~~~python
    if menu_input == "a":
        group_name = input("Naam groep: ")
        lector = input("Lector: ")
        room = input("Lokaal ")
        save_new_group(group_name, lector, room)
    elif menu_input == "b":
        for student_group in get_groups():
            print(student_group)
~~~

Die functies `save_new_group` en `get_groups` moeten we uiteraard ook nog creëren.

#### Opvragen van de groepen

De groepen opvragen gaat met een eenvoudige SQL-query, gelijkaardig aan die om de studenten op te vragen. De functie `get_groups` ziet er als volgt uit:

~~~python
def get_groups():
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        query_result = con.execute(
            "SELECT group_name, teacher, room FROM student_group"
        )
        groups = []
        for row in query_result:
            groups.append(StudentGroup(row[0], row[1], row[2]))
    return groups
~~~

#### Aanmaken van een nieuwe groep

Een nieuwe groep aanmaken doen we met een SQL-statement `INSERT INTO`:

~~~python
def save_new_group(group_name, teacher, room):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        cur = con.cursor()
        cur.execute(
            "INSERT INTO student_group(group_name, teacher, room) values(?,?,?)",
            [group_name, teacher, room],
        )
        con.commit()
~~~

#### Groep opgeven bij het aanmaken van een student

Bij het aanmaken van een student moeten we nu ook de student aan een groep toekennen. Daarom tonen we eerst alle groepen waaruit je kunt kiezen (met de functie `get_groups`) en vragen we dan om een groep. De optie 1 uit het menu passen we daarom aan tot:

~~~python
    elif menu_input == "1":
        student_name = input("Naam student: ")
        lab_points = input_number("Punten op labo: ")
        theory_points = input_number("Punten op theorie: ")
        print("Kies uit volgende groepen: ")
        for student_group in get_groups():
            print(student_group.name)
        group_name = input("Groep: ")
        save_new_student(Student(student_name, lab_points, theory_points), group_name)
~~~

En de bijbehorende functie `save_new_student` wordt:

~~~python
def save_new_student(student, group_name):
    with sqlite3.connect(STUDENT_DB_FILE_NAME) as con:
        cur = con.cursor()
        cur.execute(
            "INSERT INTO student(name, lab, theory,fk_student_group) VALUES(?,?,?,?)",
            [student.name, student.lab_points, student.theory_points, group_name],
        )
        con.commit()
~~~

Merk op hoe we hierin nu de verwijzing naar de studentgroep in de tabel opnemen (`fk_student_group`).

#### Verdere uitbreidingen

Er zijn nog allerlei uitbreidingen mogelijk aan dit programma. Enkele suggesties:

* Maak het programma gebruiksvriendelijker. Geef bijvoorbeeld een speciale melding wanneer de gebruiker de studenten (van een groep of alle) opvraagt en er geen studenten zijn. Nu wordt er gewoon niets getoond, wat verwarrend kan zijn.
* Maak het programma robuuster. Momenteel kun je bijvoorbeeld een student aanmaken met een onbestaande klasgroep. Ga eens door de hele code, kijk wat er nog allemaal kan mislopen met ongeldige gebruikersinvoer en laat het programma hierop reageren.

### Opdracht: Sensordatabase

In deze opdracht maak je een eenvoudige commandlinetoepassing die informatie over sensoren en metingen opslaat in een SQLite-database.

Hiervoor kun je starten vanuit onderstaande code. Het is de bedoeling dat je de TODO's aanvult...

#### Startcode

~~~python
import sqlite3
from datetime import datetime

SENSORS_DB_FILE_NAME = "sensors.db"

def init_database():
    con = sqlite3.connect(SENSORS_DB_FILE_NAME)
    with con:
        con.executescript("""
            CREATE TABLE IF NOT EXISTS sensor (
                sensor_name text,
                sensor_type text
            );

            CREATE TABLE IF NOT EXISTS sensor_measurement (
                sensor_id_fk references sensor,
                measurement integer,
                measurement_time text
            );
        """)

init_database()

MENU = """
1. Voeg sensor toe
2. Vraag sensoren op
3. Voeg meting toe
4. Vraag metingen op
5. Sluit af
"""

class Sensor:
    def __init__(self, name, type):
        self.name = name
        self.type = type

class SensorMeasurement:
    def __init__(self, time, value):
        self.time = time
        self.value = value

def add_sensor(id, sensor_type):
    # TODO: Voeg een sensor toe aan de database

def get_sensors():
    # TODO: Haal een lijst van sensoren op uit de database

def add_measurement(sensor, moment, value):
    # TODO: Voeg een sensormeting toe aan de database

def get_sensor_measurements(sensor):
    # TODO: Haal een lijst van metingen op uit de database

if __name__ == "__main__":
    while True:
        read = input(MENU).strip()
        if read == "1":
            sensor_name = input("Naam van de sensor: ")
            sensor_type = input("Type van de sensor: ")
            add_sensor(sensor_name, sensor_type)
        elif read == "2":
            for sensor in get_sensors():
                print("Sensor met naam", sensor.name, "van het type", sensor.type)
        elif read == "3":
            sensor = input("Naam van de sensor: ")
            moment = datetime.now()
            sensor_value = int(input("Waarde van de sensor: "))
            add_measurement(sensor, moment, sensor_value)
        elif read == "4":
            sensor = input("ID van de sensor: ")
            for measurement in get_sensor_measurements(sensor):
                print("Op", measurement.value, "werd gemeten", measurement.time)
        elif read == "5":
            break
        else:
            print(read, "is een ongeldige keuze")
~~~

#### Demo

Hieronder zie je een voorbeeld van wat je met de applicatie allemaal moet kunnen doen.

##### Optie 1: sensor toevoegen

Optie 1 geeft je de mogelijkheid om een sensor toe te voegen.  
In onderstaand voorbeeld voegen we een luchtvochtigheidssensor en een luchtdruksensor toe:

~~~
1. Voeg sensor toe
2. Vraag sensoren op
3. Voeg meting toe
4. Vraag metingen op
5. Sluit af
1
Naam van de sensor: A1
Type van de sensor: luchtvochtigheid

1. Voeg sensor toe
2. Vraag sensoren op
3. Voeg meting toe
4. Vraag metingen op
5. Sluit af
1
Naam van de sensor: A2
Type van de sensor: luchtdruk
~~~

##### Optie 2: sensoren opvragen

Met optie 2 kun je deze sensoren opvragen...

~~~
1. Voeg sensor toe
2. Vraag sensoren op
3. Voeg meting toe
4. Vraag metingen op
5. Sluit af
2
Sensor met naam A1 van het type luchtvochtigheid
Sensor met naam A2 van het type luchtdruk
~~~

##### Optie 3: meting toevoegen

Vervolgens kunnen we via optie 3 metingen toevoegen:

~~~
1. Voeg sensor toe
2. Vraag sensoren op
3. Voeg meting toe
4. Vraag metingen op
5. Sluit af
3
Naam van de sensor: A1
Waarde van de sensor: 100

1. Voeg sensor toe
2. Vraag sensoren op
3. Voeg meting toe
4. Vraag metingen op
5. Sluit af
3
Naam van de sensor: A1
Waarde van de sensor: 150

1. Voeg sensor toe
2. Vraag sensoren op
3. Voeg meting toe
4. Vraag metingen op
5. Sluit af
3
Naam van de sensor: A2
Waarde van de sensor: 64
~~~

##### Optie 4: metingen opvragen

De eerder ingegeven sensormetingen kunnen we uiteindelijk ook opvragen in de applicatie:

~~~
1. Voeg sensor toe
2. Vraag sensoren op
3. Voeg meting toe
4. Vraag metingen op
5. Sluit af
4
ID van de sensor: A1
Op 2022-04-13 22:45:38.335312 werd gemeten 100
Op 2022-04-13 22:46:01.097266 werd gemeten 150
~~~

##### Optie 5: de applicatie afsluiten

En uiteraard willen we de applicatie ook proper kunnen afsluiten met optie 5:

~~~
1. Voeg sensor toe
2. Vraag sensoren op
3. Voeg meting toe
4. Vraag metingen op
5. Sluit af
5
~~~
