## Python-modules

### Modulariteit

**Modulair** programmeren of modulariteit houdt in dat je leert je **code** te **ordenen**  en te groeperen.
Tot nu toe hebben we al twee elementen gezien die ons daarbij kunnen helpen, namelijk **functies** en **klassen**.

#### Modulariteit met functies

**Functies** stelden ons in staat om stukken code te isoleren:

* Om code **op te delen** in logische onderdelen (en je code leesbaar te houden)
* Om code te **hergebruiken** en herhalingen te vermijden
  
#### Modulariteit met klassen

**Klassen** stelden ons in staat om data te groeperen in een structuur.  
Herinner bijvoorbeeld de student met een naam, labo-punten en theorie-punten.  

Wat klassen nog bijzonder maakt, is de mogelijkheid om functies te koppelen aan deze data. Zo'n functie heet dan een **methode**. 
Zo konden we een methode `points` definiëren die - via de referentie `self` - op de data van het object kon werken.

### Python-modules

Een module zit nog een niveau **hoger** dan **functies** en **klassen**.  
Een module is een **geheel/verzameling** van klassen, functies en variabelen die **logisch bij elkaar horen**.  

De redenen om deze te groeperen zijn divers:

* Je programma logisch onder te verdelen om de code overzichtelijk en onderhoudsvriendelijk te houden
* Je code te laten hergebruiken binnen andere programma's
* Een stuk van je code te isoleren om het gemakkelijker te testen
* ... en vele andere redenen die je nog zult ontdekken

### Python-modules in de praktijk

Een Python-module aanmaken is **heel eenvoudig** en je hebt het al de hele tijd gedaan. Elk **Python-bestand** is immers een **module**.

Maak bijvoorbeeld onderstaande code aan, die twee eenvoudige functies definieert:

~~~python
def hello():
    print("hello")

def world():
    print("world")
~~~

Bewaar deze in een bestand met de naam **hello.py**.

**Let op, deze bestandsnaam moet je onthouden voor de volgende stap.**

### Een module hergebruiken via import

Als we een Python-REPL openen vanuit dezelfde directory waarin we dit Python-bestand bewaard hebben, kunnen we deze **module importeren**:

~~~python
>>> import hello
>>> hello.hello()
hello
>>> hello.world()
world
~~~

Met het eerste statement, `import hello`, maken we de inhoud van het hele bestand hello.py beschikbaar. De naam van deze import dient overeen te komen met de naam van het bestand, zonder de extensie .py. Standaard zoekt Python naar modules in dezelfde directory en in een aantal voorgedefinieerde systeemdirectory's.

In dit geval bevat de module de twee functies `hello()` en `world()`. Deze kunnen we nu aanroepen door ze vooraf te laten gaan door de naam van de module. Als je dit niet doet, toont Python een exception van het type `NameError`:

~~~python
>>> hello()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'hello' is not defined
~~~

### Import vanuit een ander Python-bestand

Het voorgaande voorbeeld was vanuit de REPL, maar je kunt ook hetzelfde doen vanuit een ander Python-bestand.

Maar hiervoor - opnieuw binnen dezelfde directory - een ander bestand aan (bijvoorbeeld met de naam hello_application.py)

~~~python
import hello

hello.hello()
hello.world()
~~~

> *Nota:*  
> Voeg voor de leesbaarheid een lege lijn tussen de imports (meestal bovenaan de code)
> en de rest van je code toe.

Vanuit dit Python-bestand kun je dan beide functies hergebruiken:

~~~bash
$ ls 
hello_application.py hello.py
$ python hello_application.py
hello
world
~~~

### De naam van modules

Een module krijgt automatisch een naam mee, die overeenkomt met de naam die je gebruikt om deze te importeren.

~~~python
>>> import hello
>>> hello.__name__
'hello'
>>> __name__
'__main__'
>>> 
~~~

Deze variabele is ook beschikbaar als globale variabele, en deze heeft een specifieke waarde afhankelijk van de modus waar de code zich in bevindt.

### name-variabele met verschillende waardes

Om dit te illustreren, voegen we een regel toe aan ons Python-bestand **hello.py** om de naam van de module te tonen:

~~~python
def hello():
    print("hello")

def world():
    print("world")

print("Name: ", __name__)
~~~

Als je deze dan uitvoert, toont deze `__main__`:

~~~bash
$ python hello.py
Name: __main__
~~~

Als we echter nu echter hello_application.py uitvoeren, zien we het volgende:

~~~bash
python hello_application.py
Name:  hello
hello
world
~~~

Eerst en vooral zien we dat - ondanks dat we hello enkel importeren - het `print`-statement effectief wordt uitgevoerd.  
De naam die nu getoond wordt, is echter `hello` en niet `__main__`.  

Bij het importeren van een Python-module wordt dus alle code van dit bestand uitgevoerd. Hierdoor worden de erin gedefinieerde functies beschikbaar gesteld, maar ook andere code wordt uitgevoerd, zoals in dit geval het `print`-statement.

Doordat de globale variabele `__name__` in dit geval echter de waarde `hello` heeft, worden deze functies gekoppeld aan deze naam en niet aan de globale namespace. Daardoor dien je `hello` als prefix te gebruiken om de functies aan te roepen.

### Uitvoerbare modules

Deze variabele `__name__` heeft echter nog een belangrijke functie.  
Stel dat je volgende code schrijft:

~~~python
def hello():
    print("hello")

def world():
    print("world")

hello()
world()
~~~

...dan zal deze inderdaad de functies `hello()` en `world()` uitvoeren.

~~~
$ python hello.py
hello
world
~~~

Maar als we nu `hello_application.py` uitvoeren, zien we:

~~~
$ python hello_application.py
hello
world
hello
world
~~~

De code in hello.py wordt dus ook uitgevoerd als je deze vanuit een ander bestand importeert.

Om ervoor te zorgen dat je hello.py niet alleen als module kunt importeren, maar ook als uitvoerbaar bestand kunt gebruiken, voeg je het volgende toe:

~~~python
def hello():
    print("hello")

def world():
    print("world")

if __name__ == "__main__":
    hello()
    world()
~~~

We kijken dus of de globale variabele `__name__` de waarde `__main__` heeft. Die heeft deze waarde alleen als je de module als Python-script uitvoert, en niet als je ze importeert. De code binnen het if-blok wordt dan ook alleen in het eerste geval uitgevoerd:

~~~
$ python hello.py
hello
world
$ python hello_application.py
hello
world
~~~

### from-keyword

Eerder hebben het keyword `import` gebruikt om functies vanuit de module `hello` te importeren.  

Om die functies (en klassen als die er zouden zijn) te gebruiken, diende je ze te **prefixen** met de naam van de module (hello).  

Dit heeft als voordeel dat je niet zomaar **conflicten** kunt hebben met gelijknamige functies uit andere modules (of je eigen code), maar het geeft wel wat extra typewerk.

Soms wil je dat typewerk liever besparen. Dat kan met de volgende `from` ... `import` ... constructie: 

~~~python
from hello import world

world()
~~~

Ook hier wordt alle code in de module uitgevoerd, maar alleen de functies die je achter `import` aanduidt worden beschikbaar gesteld in de namespace van het programma.

In onderstaande uitvoering zie je twee zaken:

* Je kan de functie `world()` aanroepen zonder prefix
* De functie `hello()` is niet beschikbaar

~~~python
Python 3.8.5 (default, Jan 27 2021, 15:41:15) 
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from hello import world
>>> world()
world
>>> hello()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'hello' is not defined
~~~

Als je wilt dat `hello()` ook beschikbaar is, kan dat op twee manieren.  

Of je importeert beide (gescheiden door komma's):

~~~python
from hello import hello, world

world()
hello()
~~~

...met als resultaat...

~~~python
Python 3.8.5 (default, Jan 27 2021, 15:41:15) 
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from hello import hello, world
>>> world()
world
>>> hello()
hello
~~~

Of je gebruikt `*` om alle functies te importeren.

> Let op:  
> Het gebruik van `import *` is meestal geen goed idee.
> Je weet niet altijd welke namen je op deze manier importeert
> en dat kan tot naamconflicten in je code leiden.

~~~python
from hello import *

world()
hello()
~~~

### Herwerken van studentenapplicatie

Als voorbeeld van een modularisatie gaan we onze voorgaande studentenapplicatie in modules opsplitsen.  
Als je de code bekijkt, zou je deze in drie delen kunnen opsplitsen:

* **student_command.py**   
  Code die zich bezighoudt met de command-line-interactie
* **student_service.py**  
  Code die zich bezighoudt met de applicatie-logica en de verbinding met de database
* **student_entities.py**  
  Code die de studenten-datatypes bevat

Elk stuk code heeft zijn eigen verantwoordelijkheid.

#### Afhankelijkheden

In onderstaand diagram zie je ook de relaties en afhankelijkheden tussen de modules onderling.

* Bovenaan staat de command-line-module, deze heeft de service-module nodig om de studenten in de database op te slaan.
* Zowel de command- als de service-modules zijn op hun beurt afhankelijk van de entities
* entities staat op zichzelf en heeft geen afhankelijheden

~~~
                             +-------------------------------+
                             |                               |
                +------------+       STUDENT_COMMAND         +------------+
                |            |                               |            |
                |            +-------------------------------+            |
                v                                                         |
+---------------+--------------+                                          |
|                              |                                          |
|        STUDENT_SERVICE       |                                          |
|                              |                                          |
+---------------+--------------+                                          V
                |                                         +---------------+--------------+
                |                                         |                              |
                +---------------------------------------->+        STUDENT_ENTITIES      |
                                                          |                              |
                                                          +------------------------------+
~~~

#### Nut van modules

Modules zijn zoals we al zagen nuttig voor verschillende doeleinden: 

* om code (bij grotere projecten) op te splitsen in kleinere beheersbare delen
* om code te hergebruiken binnen andere applicaties
* om code individueel te testen (je wil bijvoorbeeld je service-applicatie testen zonder de command-line)
  
Stel nu dat we naast onze command-line-module ook een webversie willen maken, dan kan deze nieuwe module ook de service-module gebruiken:

~~~
                     +-------------------------------+
                     |                               |
                +----+       STUDENT_WEB             +--------------------+
                |    |                               |                    |
                |    +-------------------------------+                    |
                |                                                         |
                |                    +-------------------------------+    |
                |                    |                               |    |
                +--------------------+       STUDENT_COMMAND         +----+
                |                    |                               |    |
                |                    +-------------------------------+    |
                v                                                         |
+---------------+--------------+                                          |
|                              |                                          |
|        STUDENT_SERVICE       |                                          |
|                              |                                          |
+---------------+--------------+                                          V
                |                                         +---------------+--------------+
                |                                         |                              |
                +---------------------------------------->+        STUDENT_ENTITIES      |
                                                          |                              |
                                                          +------------------------------+
~~~

Dit passen we toe in het volgende deel van de cursus.

De code van onze drie modules ziet er nu als volgt uit:

#### student_entities.py

~~~python
class Student:
    def __init__(self,name,lab=0,theory=0,id=0):
        self.name = name
        self.lab_points = lab
        self.theory_points = theory
        self.student_id=id

    def points(self):
        return (self.lab_points + self.theory_points) // 2

    def succeeded(self):
        return self.points() >= 10    
        
    def __str__(self):
        return "Student {} - {} has {} for lab and {} for theory, so average of {}".format(
            self.student_id, self.name, self.lab_points,self.theory_points,self.points())

class StudentGroup:
    def __init__(self, name, teacher, room):
        self.name = name
        self.teacher = teacher
        self.room = room

    def __str__(self):
        return "Group with name '{}' and teacher {} at room {}".format(
            self.name, self.teacher, self.room)   
~~~

#### student_service.py

~~~python
import sqlite3 as sl
from student_entities import Student, StudentGroup

STUDENT_DB_FILE_NAME = "students.db"

def init_database():
    with sl.connect(STUDENT_DB_FILE_NAME) as con:
        con.execute("""
            create table if not exists student_group
            (
                group_name text primary key,
                teacher text,
                room text
            );
        """)

        con.execute("""
            create table if not exists student
            (
                student_id INTEGER PRIMARY KEY,
                name TEXT,
                lab INTEGER,
                theory INTEGER,
                fk_student_group text references student_group
            );
        """)

def get_students():
    with sl.connect(STUDENT_DB_FILE_NAME) as con:
        query_result = con.execute('SELECT student_id, name, lab, theory FROM student')
        students = []
        for row in query_result:
            students.append(Student(row[1],row[2],row[3],row[0]))
    return students

def get_students_for_group(group_name):
    with sl.connect(STUDENT_DB_FILE_NAME) as con:
        query_result = con.execute('SELECT student_id, name, lab, theory FROM student where fk_student_group = ?',[group_name])
        students = []
        for row in query_result:
            students.append(Student(row[1],row[2],row[3],row[0]))
    return students

def get_groups():
    with sl.connect(STUDENT_DB_FILE_NAME) as con:
        query_result = con.execute('SELECT group_name, teacher, room FROM student_group')
        groups = []
        for row in query_result:
            groups.append(StudentGroup(row[0],row[1],row[2]))
    return groups

def save_new_student(student, group_name):
    with sl.connect(STUDENT_DB_FILE_NAME) as con:
        con.execute('insert into student(name, lab, theory,fk_student_group) values(?,?,?,?)', [student.name,student.lab_points,student.theory_points,group_name])
        con.commit()

def save_new_group(group_name, lector, room):
    with sl.connect(STUDENT_DB_FILE_NAME) as con:
        con.execute("insert into student_group(group_name, teacher, room) values(?,?,?)", [group_name, lector, room])
        con.commit()

def get_student(id):
    with sl.connect(STUDENT_DB_FILE_NAME) as con:
        query_result = con.execute('select student_id, name, lab, theory FROM student where student_id = ?',str(id))
        row = query_result.fetchone()
    return Student(row[1],row[2],row[3],row[0])

def update_points(id,lab,theory):
    with sl.connect(STUDENT_DB_FILE_NAME) as con:
        update_statement = 'update student SET lab = ? , theory = ? WHERE student_id = ?'
        con.execute(update_statement, [lab,theory,id])
        con.commit()

def delete_student(id):
    with sl.connect(STUDENT_DB_FILE_NAME) as con:
        con.execute("delete from student where student_id = ?", [id])
        con.commit()

init_database()
~~~

#### student_command.py

~~~python
from student_entities import Student, StudentGroup
from student_service import *

def input_number(request):
    number_input = input(request)
    return int(number_input)   

menu = """
a. Maak studenten-groep aan
b. Toon groepen
1. Voeg student toe
2. Toon studenten
3. Toon student
4. Update van punten student
5. Verwijderen van een student
6. Stop applicatie
"""

sub_menu_students = """
a. Alle studenten
b. Studenten voor groep
"""

while True:
    menu_input = input(menu)

    if menu_input == "a":
        group_name = input("Naam groep: ")
        lector = input("Lector: ")
        room = input("Lokaal ")
        save_new_group(group_name, lector, room)
    elif menu_input == "b":
        for student_group in get_groups():
            print(student_group)    
    elif menu_input == "1":
        student_name = input("Naam student(e): ")
        lab_points = input_number("Labo-punten: ")
        theory_points = input_number("Theorie-punten: ")
        print("Kies uit volgende groepen: ")
        for student_group in get_groups():
            print(student_group.name)  
        group_name = input("> ")
        save_new_student(Student(student_name,lab=lab_points,theory=theory_points), group_name)
    elif menu_input == "2":
        choice = input(sub_menu_students)
        if choice == "a":
            students = get_students()
        else:
            print("Kies uit volgende groepen: ")
            for student_group in get_groups():
                print(student_group.name)
            group_name = input("Kies groep> ")
            students = get_students_for_group(group_name)
            
        for student in get_students():
            print(student)

    elif menu_input == "3":
        id = input_number("> ")
        student = get_student(id)
        print(student)
    elif menu_input == "4":
        print("Kies student op id")
        for student in get_students():
            print("Student met id",student.student_id,"en naam",student.name)
        id = input_number("> ")
        lab = input_number("Labo-punten:")
        theory = input_number("Theorie-punten:")
        update_points(id,lab,theory)
    elif menu_input == "5":
        print("Verwijder student op id")
        for student in get_students():
            print("Student met id",student.student_id,"en naam",student.name)
        id = input_number("> ")
        delete_student(id)
    elif menu_input == "6":
        print("De applicatie wordt afgesloten")
        quit()
    else:
        print("ongekende input")
~~~

### Andere weetjes

We bespreken nog enkele interessante weetjes in verband met Python-modules.

#### Waar vind je Python-modules?

Om een module te gebruiken, moesten we het bestand in dezelfde directory plaatsen.  

Python voorziet echter - naast je eigen modules - ook veel standaardmodules. Maar waar bevinden die zich dan?

De locaties waar Python deze gaat zoeken, bevinden zich in de variabele `path` van de standaardmodule `sys`:

~~~python
>>> import sys
>>> print(sys.path)
['', '/usr/lib/python38.zip', '/usr/lib/python3.8', '/usr/lib/python3.8/lib-dynload', '/home/bart/.local/lib/python3.8/site-packages', '/usr/local/lib/python3.8/dist-packages', '/usr/lib/python3/dist-packages', '/usr/lib/python3.8/dist-packages']
~~~


#### dir-functie

Met de functie `dir` kun je de verschillende onderdelen (functies, klassen, variabelen, ...) van een module opvragen:

~~~python
>>> import hello
>>> dir(hello)
['__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', 'hello', 'world']
~~~

Diezelfde functie kun je overigens ook gebruiken op klassen om te weten welke attributen en methodes die bevatten. Zie hieronder voor het type `bytearray`:

~~~python
>>> dir(bytearray)
['__add__', '__alloc__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'capitalize', 'center', 'clear', 'copy', 'count', 'decode', 'endswith', 'expandtabs', 'extend', 'find', 'fromhex', 'hex', 'index', 'insert', 'isalnum', 'isalpha', 'isascii', 'isdigit', 'islower', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'pop', 'remove', 'replace', 'reverse', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
~~~

Of voor het type `list`:

~~~python
>>> dir(list)
['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
~~~

Je kunt dit ook bij een object opvragen:

~~~python
>>> a = [1,2,3]
>>> dir(a)
['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
>>> type(a)
<class 'list'>
>>> 
~~~

### Pip en PyPI

Een groot deel van programmeren bestaat uit het leren hergebruiken van bestaande code.  
We hebben gezien hoe je code kan afzonderen in aparte bestanden om deze functionaliteit te isoleren en eventueel te hergebruiken in verschillende applicaties.  

Om code van andere ontwikkelaars (over heel de wereld) te gebruiken, hebben veel programmeeromgevingen het concept van **package managers** (pakketbeheerders).  

Dit zijn tools die in staat zijn om andere modules (en eventueel modules waarvan die andere modules afhangen) automatisch te downloaden. Zo heb je in andere programmeertalen:

* Maven in Java
* Nuget in C#
* npm in Javascript
* gem in Ruby

In Python is dit **pip** (acroniem voor **p**ip **i**nstalls **p**ackages). Deze tool wordt standaard **meegeleverd met Python** (vanaf versie 3.4).

Pip gaat typisch modules downloaden van internet. Hiervoor werkt het standaard samen met de Python Package Index, ook wel bekend als PyPI (spreek uit als Pie Pea Eye).

Deze website bevindt zich op <https://pypi.org> en daar kun je ook zoeken naar interessante Python-modules.

PyPI is een webservice die een uitgebreide verzameling Python-pakketten (frameworks, tools en bibliotheken) huisvest.  

Om een Python-pakket te installeren, gebruik je de opdracht `pip install` gevolgd door de naam van het pakket.

Voor het pakket `requests`:

~~~
[student@fedora ~]$ pip install requests
Defaulting to user installation because normal site-packages is not writeable
Requirement already satisfied: requests in /usr/lib/python3.10/site-packages (2.27.1)
Requirement already satisfied: idna<4,>=2.5 in /usr/lib/python3.10/site-packages (from requests) (3.2)
Requirement already satisfied: urllib3<1.27,>=1.21.1 in /usr/lib/python3.10/site-packages (from requests) (1.26.7)
Requirement already satisfied: charset-normalizer~=2.0.0 in /usr/lib/python3.10/site-packages (from requests) (2.0.4)
~~~

Voor het pakket Flask:

~~~
[student@fedora ~]$ pip install flask
Defaulting to user installation because normal site-packages is not writeable
Collecting flask
  Downloading Flask-2.1.2-py3-none-any.whl (95 kB)
     |████████████████████████████████| 95 kB 549 kB/s 
Collecting itsdangerous>=2.0
  Downloading itsdangerous-2.1.2-py3-none-any.whl (15 kB)
Requirement already satisfied: click>=8.0 in /usr/lib/python3.10/site-packages (from flask) (8.0.1)
Collecting Jinja2>=3.0
  Downloading Jinja2-3.1.2-py3-none-any.whl (133 kB)
     |████████████████████████████████| 133 kB 52.5 MB/s 
Collecting Werkzeug>=2.0
  Downloading Werkzeug-2.1.2-py3-none-any.whl (224 kB)
     |████████████████████████████████| 224 kB 3.8 MB/s 
Requirement already satisfied: MarkupSafe>=2.0 in /usr/lib64/python3.10/site-packages (from Jinja2>=3.0->flask) (2.0.0)
Installing collected packages: Werkzeug, Jinja2, itsdangerous, flask
Successfully installed Jinja2-3.1.2 Werkzeug-2.1.2 flask-2.1.2 itsdangerous-2.1.2
~~~

Je kunt ook meerdere pakketten tegelijk installeren met pip. Geef gewoon meerdere namen na elkaar op, gescheiden door een spatie:

~~~
[student@fedora ~]$ pip install serial pandas
Defaulting to user installation because normal site-packages is not writeable
Collecting serial
  Downloading serial-0.0.97-py2.py3-none-any.whl (40 kB)
     |████████████████████████████████| 40 kB 2.7 MB/s 
Collecting pandas
  Downloading pandas-1.4.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (11.7 MB)
     |████████████████████████████████| 11.7 MB 2.9 MB/s 
Collecting future>=0.17.1
  Downloading future-0.18.2.tar.gz (829 kB)
     |████████████████████████████████| 829 kB 13.4 MB/s 
Collecting iso8601>=0.1.12
  Downloading iso8601-1.0.2-py3-none-any.whl (9.7 kB)
Collecting pyyaml>=3.13
  Downloading PyYAML-6.0-cp310-cp310-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_12_x86_64.manylinux2010_x86_64.whl (682 kB)
     |████████████████████████████████| 682 kB 20.7 MB/s 
Collecting pytz>=2020.1
  Downloading pytz-2022.1-py2.py3-none-any.whl (503 kB)
     |████████████████████████████████| 503 kB 21.5 MB/s 
Requirement already satisfied: python-dateutil>=2.8.1 in /usr/lib/python3.10/site-packages (from pandas) (2.8.1)
Collecting numpy>=1.21.0
  Downloading numpy-1.22.3-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (16.8 MB)
     |████████████████████████████████| 16.8 MB 101.6 MB/s 
Requirement already satisfied: six>=1.5 in /usr/lib/python3.10/site-packages (from python-dateutil>=2.8.1->pandas) (1.16.0)
Using legacy 'setup.py install' for future, since package 'wheel' is not installed.
Installing collected packages: pyyaml, pytz, numpy, iso8601, future, serial, pandas
    Running setup.py install for future ... done
Successfully installed future-0.18.2 iso8601-1.0.2 numpy-1.22.3 pandas-1.4.2 pytz-2022.1 pyyaml-6.0 serial-0.0.97
~~~

Je kunt ook opvragen welke pakketten je al met pip geïnstalleerd hebt:

~~~
student@fedora ~]$ pip list 
Package            Version
------------------ ---------
argcomplete        1.12.3
Beaker             1.10.0
beautifulsoup4     4.11.0
...
fedora-third-party 0.8
Flask              2.1.2
fros               1.1
gpg                1.15.1
...
regex              2022.3.15
requests           2.27.1
requests-file      1.5.1
requests-ftp       0.3.1
...
urllib3            1.26.7
Werkzeug           2.1.2
~~~

Je zult zien dat hier heel wat pakketten bij staan die je zelf niet geïnstalleerd hebt. Dat is omdat bijvoorbeeld Flask zelf afhangt van andere pakketten zoals Jinja2. Als je dus Flask met pip installeert, gaat die automatisch Jinja2 en andere afhankelijkheden van Flask installeren.

Van een geïnstalleerd pakket kun je ook meer informatie opvragen:

~~~
[student@fedora ~]$ pip show flask
Name: Flask
Version: 2.1.2
Summary: A simple framework for building complex web applications.
Home-page: https://palletsprojects.com/p/flask
Author: Armin Ronacher
Author-email: armin.ronacher@active-4.com
License: BSD-3-Clause
Location: /home/student/.local/lib/python3.10/site-packages
Requires: click, itsdangerous, Jinja2, Werkzeug
Required-by: 
~~~

Als er ondertussen een nieuwe versie van een pakket uit is en je dit wilt gebruiken, installeer je het opnieuw, maar dan met de optie `-U` (voor update):

~~~
[student@fedora ~]$ pip install -U flask
Defaulting to user installation because normal site-packages is not writeable
Collecting flask
  Downloading Flask-2.2.3-py3-none-any.whl (101 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 101.8/101.8 KB 543.5 kB/s eta 0:00:00
Collecting itsdangerous>=2.0
  Downloading itsdangerous-2.1.2-py3-none-any.whl (15 kB)
Collecting Werkzeug>=2.2.2
  Downloading Werkzeug-2.2.3-py3-none-any.whl (233 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 233.6/233.6 KB 908.7 kB/s eta 0:00:00
Requirement already satisfied: click>=8.0 in /usr/lib/python3/dist-packages (from flask) (8.0.3)
Requirement already satisfied: Jinja2>=3.0 in /usr/lib/python3/dist-packages (from flask) (3.0.3)
Collecting MarkupSafe>=2.1.1
  Downloading MarkupSafe-2.1.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (25 kB)
Installing collected packages: MarkupSafe, itsdangerous, Werkzeug, flask
Successfully installed MarkupSafe-2.1.2 Werkzeug-2.2.3 flask-2.2.3 itsdangerous-2.1.2
~~~

En als je een pakket niet meer nodig hebt, verwijder je het met `pip uninstall`:

~~~
[student@fedora ~]$ pip uninstall flask
Found existing installation: Flask 2.1.2
Uninstalling Flask-2.1.2:
  Would remove:
    /home/student/.local/bin/flask
    /home/student/.local/lib/python3.10/site-packages/Flask-2.1.2.dist-info/*
    /home/student/.local/lib/python3.10/site-packages/flask/*
Proceed (Y/n)? Y
  Successfully uninstalled Flask-2.1.2
~~~
