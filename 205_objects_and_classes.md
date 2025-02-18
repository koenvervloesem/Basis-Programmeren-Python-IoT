## Objecten en klassen

Tot nu toe hebben we voornamelijk gewerkt met twee soorten datatypes:

* Enkelvoudige datatypes: kunnen één enkele waarde bevatten, voorbeelden zijn `int`, `float`, `bool`
* Collecties: vormen een container voor meerdere waardes: `list`, `set`

> Nota: Wat met een string (`str`)? Die wordt doorgaans als enkelvoudig datatype beschreven. Maar eigenlijk kun je een string beschouwen als een lijst van tekens, en dus is het ook een soort collectie...

In dit deel bekijken we klassen in Python, waarmee we data op willekeurige manieren kunnen structureren in een datatype dat we 'op maat van onze toepassing' maken.

### Voorbeeld: studentenapplicatie

Stel, je wilt een een applicatie bouwen die de punten voor een vak bijhoudt, zoals in de onderstaande tabel voorgesteld:

| Voornaam | Naam     | Labo  | Theorie |
| -------- | -------- | ----- | ------- |
| Jan      | Janssens |  15   | 16      |
| Piet     | Pieters  |  15   | 16      |
| Joris    | Jorissen |  15   | 16      |
| Korneel  | Korneels |  15   | 16      |

### Verschillende studenten => Lijst

We weten ondertussen hoe we meerdere elementen van hetzelfde type moeten bijhouden in een Python-programma, namelijk aan de hand van een lijst.

Zo zou je elke student als een apart element in de lijst kunnen bijhouden zoals in onderstaand voorbeeld...  

~~~python
students = ["Jan", "Piet", "Joris", "Korneel"]
~~~

### Groeperen van data

Probleem daarbij is dat we slechts één van die elementen uit de tabel (in dit geval de voornaam van de studenten) in de lijst kunnen bijhouden.  

Is er een manier om alle studentdata gestructureerd bij elkaar te houden? 

### Gestructureerd programmeren met klassen

In Python kun je dit met klassen.    
Een klasse is een **gestructureerd datatype** dat je toelaat om verschillende waardes (of attributen) te **groeperen** onder één enkel **object**.  

### Dataklasse voor studenten

Laten we direct van wal steken met ons voorgaande voorbeeld uit te breiden. We maken een dataklasse aan om alle data die bij een student horen voor te stellen.

Toegepast op een student ziet zo'n datatype er als volgt uit:

~~~python
from dataclasses import dataclass

@dataclass
class Student:
    """Class for keeping track of student data."""
    first_name: str = ""
    surname: str = ""
    lab_points: int = 0
    theory_points: int = 0
~~~

We zien hier enkele elementen:

* We creëren een klasse met het keyword **class**, gevolgd door een **naam** voor het type (Student in dit geval), gevolgd door een dubbele punt om aan te geven dat er een blok volgt.
* We documenteren de bedoeling van deze klasse met een **docstring** in het begin van dit blok: multiline commentaar met vooraan en achteraan drie aanhalingstekens.
* Daarna definiëren we meerdere **attributen** als volgt: de naam, een dubbele punt, het type, een gelijkheidsteken en de standaardwaarde.
* Om aan te geven dat het om een dataklasse gaat die we eenvoudig kunnen definiëren door de attributen op te sommen, moeten we de klasse laten voorafgaan door de regel **@dataclass**. Dit is een decorator (een functie die op de klasse werkt), die we eerst moeten importeren uit de module dataclasses.

Kortom, we definiëren hier een klasse Student met als attributen `first_name`, `surname`, `lab_points` en `theory_points`. De eerste worden standaard geïnitialiseerd als lege strings, de twee andere als gehele getallen met waarde 0.

> Nota: later gaan we zien dat we - naast attributen - ook functies kunnen aanhechten aan een klasse. 

### Aanmaken van een object

Als je een variabele aanmaakt van een klasse noemen we dit een **object**.  
Onderstaande code:

* definieert zo'n type
* instantieert een object van dit type

~~~python
from dataclasses import dataclass

@dataclass
class Student:
    """Class for keeping track of student data."""
    first_name: str = ""
    surname: str = ""
    lab_points: int = 0
    theory_points: int = 0

jan = Student()
~~~

Een **object** wordt aangemaakt door een speciale functie `Student()`, genaamd de **constructor**. 

Deze **constructor**:

* is een **functie**
* die **automatisch** wordt **aangemaakt**  
  (als je deze zelf niet maakt, wat we later zien)
* die **dezelfde naam** heeft als de **klasse**
* die je **aanroept** om een **object** (of instantie) van de klasse aan te maken

### Werken met attributen

Een object bestaat uit attributen (`first_name`, `surname`, `lab_points`, `theory_points`), zoals deze in de klasse werden beschreven. Elk van deze attributen kun je beschouwen als een variabele die bij dit object hoort.

Onderstaand voorbeeld illustreert hoe je deze attributen gebruikt.

~~~python
from dataclasses import dataclass

@dataclass
class Student:
    """Class for keeping track of student data."""
    first_name: str = ""
    surname: str = ""
    lab_points: int = 0
    theory_points: int = 0

jan = Student()
jan.first_name = "Jan"
jan.surname = "Janssens"
jan.lab_points = 15

print(f"{jan.first_name} {jan.surname} heeft {jan.lab_points} op het labo en {jan.theory_points} op theorie.")
~~~

Je kunt deze attributen via de dot-notatie - objectnaam gevolgd door punt (in het Engels 'dot') gevolgd door naam van het attribuut - uitlezen en bewerken, net zoals je dit zou doen bij gewone variabelen.

### Meerdere objecten

Vanzelfsprekend kun je ook meerdere objecten van ditzelfde type aanmaken. Bovendien kun je de attributen al in de constructor een waarde geven:

~~~python
from dataclasses import dataclass

@dataclass
class Student:
    """Class for keeping track of student data."""
    first_name: str = ""
    surname: str = ""
    lab_points: int = 0
    theory_points: int = 0

jan = Student()
jan.first_name = "Jan"
jan.surname = "Janssens"
jan.lab_points = 15

piet = Student("Piet", "Pieters")
piet.lab_points = 16
piet.theory_points = 17

print(f"{jan.first_name} {jan.surname} heeft {jan.lab_points} op het labo en {jan.theory_points} op theorie.")
print(f"{piet.first_name} {piet.surname} heeft {piet.lab_points} op het labo en {piet.theory_points} op theorie.")
~~~

In dit voorbeeld kun je duidelijk zien dat de attributen verbonden zijn aan het object: `jan.lab_points` is niet hetzelfde als `piet.lab_points`.

### Meerdere objecten in een lijst

Laten we het principe van objecten combineren met een lijst. Net zoals we een lijst van ints of strings kunnen maken, kunnen we ook een lijst van objecten van onze zelf gedefinieerde klasse maken.

Dit stelt je in staat om een dynamische collectie van studenten bij te houden:

~~~python
from dataclasses import dataclass

@dataclass
class Student:
    """Class for keeping track of student data."""
    first_name: str = ""
    surname: str = ""
    lab_points: int = 0
    theory_points: int = 0


students = []

jan = Student()
jan.first_name = "Jan"
jan.surname = "Janssens"
jan.lab_points = 15
students.append(jan)

piet = Student("Piet", "Pieters")
piet.lab_points = 16
piet.theory_points = 17
students.append(piet)

for student in students:
    print(f"{student.first_name} {student.surname} heeft {student.lab_points} op het labo en {student.theory_points} op theorie.")
~~~

We maken dus eerst een lege lijst aan. Daarna maken we twee keer een object van de klasse `Student` aan, geven de attributen van dit object een waarde en voegen het object aan de lijst toe. Daarna overlopen we in een `for`-lus alle objecten in de lijst en tonen we hun attributen.

Aangezien je een lijst bijhoudt, is het niet meer nodig om afzonderlijke variabelen `jan` en `piet` te definiëren. Je kunt in de plaats via de constructor direct de objecten/instanties van de klasse `Student` toevoegen aan de lijst:

~~~python
from dataclasses import dataclass

@dataclass
class Student:
    """Class for keeping track of student data."""
    first_name: str = ""
    surname: str = ""
    lab_points: int = 0
    theory_points: int = 0


students = []

students.append(Student("Jan", "Janssens", 15))
students.append(Student("Piet", "Pieters", 16, 17))

for student in students:
    print(f"{student.first_name} {student.surname} heeft {student.lab_points} op het labo en {student.theory_points} op theorie.")
~~~

### Expliciete constructor

De dataklasse die we tot nu toe hebben gemaakt is een vereenvoudigde versie van een klasse. Deze maakt immers automatisch een constructorfunctie aan om het object aan te maken. We kunnen deze constructor echter ook zelf expliciet definiëren. Dat ziet er dan als volgt uit:

~~~python
class Student:
    """Class for keeping track of student data."""

    def __init__(self, first_name: str = "", surname: str = "", lab_points: int = 0, theory_points: int = 0):
        self.first_name = first_name
        self.surname = surname
        self.lab_points = lab_points
        self.theory_points = theory_points

students = []

students.append(Student("Jan", "Janssens", 15))
students.append(Student("Piet", "Pieters", 16, 17))

for student in students:
    print(f"{student.first_name} {student.surname} heeft {student.lab_points} op het labo en {student.theory_points} op theorie.")
~~~

Merk op dat we nu niet meer de decorator `@dataclass` voor de klasse zetten, en deze dus ook niet meer hoeven te importeren.

De constructor definiëren we hier als een functie met de naam `__init__`, die in het blok van de klasse staat (geïndenteerd), en daardoor aan de klasse gekoppeld is. Zo'n aan een klasse gekoppelde functie noemen we een **methode**.

Deze methode heeft vijf parameters, waarvan de eerste `self`. Dit is een verwijzing naar het object zelf, en is altijd verplicht als eerste parameter bij een methode.

Daarna komen de vier parameters die we als attributen van onze klasse willen definiëren, inclusief hun type en standaardwaarde.

De inhoud van de constructor is eenvoudig: ze kent elk van de argumenten toe aan een gelijknamig attribuut van de klasse, waarnaar we verwijzen met `self`, een punt en de naam van het attribuut.

In de volgende regel:

~~~python
self.first_name = first_name
~~~

verwijst `self.first_name` dus naar het attribuut `first_name` van het object, terwijl `first_name` aan de rechterkant van de assignment verwijst naar het gelijknamige argument van de methode `__init__`.

Bovenstaande code doet exact hetzelfde als de gelijknamige code met de dataklasse die we eerder zagen, maar op basis hiervan kunnen we nu variëren.

### Constructor zonder types en standaardwaardes

We hoeven overigens de types en de standaardwaardes niet bij de parameters van de constructor te definiëren:

~~~python
class Student:
    """Class for keeping track of student data."""

    def __init__(self, first_name, surname, lab_points, theory_points):
        self.first_name = first_name
        self.surname = surname
        self.lab_points = lab_points
        self.theory_points = theory_points

students = []

students.append(Student("Jan", "Janssens", 15))
students.append(Student("Piet", "Pieters", 16, 17))

for student in students:
    print(f"{student.first_name} {student.surname} heeft {student.lab_points} op het labo en {student.theory_points} op theorie.")
~~~

Maar als we dit uitvoeren, krijgen we een foutmelding:

~~~
$ python student.py
Traceback (most recent call last):
  File "/home/koan/student.py", line 15, in <module>
    students.append(Student("Jan", "Janssens", 15))
TypeError: Student.__init__() missing 1 required positional argument: 'theory_points'
~~~

Wat is hier mis? Omdat we nu geen standaardwaardes meer gedefinieerd hebben in de constructor, kunnen we het argument `theory_points` niet meer weglaten bij het aanmaken van student Jan.

### Methodes van een klasse

Naast attributen kun je ook willekeurige methodes toevoegen.  
Dit zijn functies die gekoppeld worden aan een object (een instantie van een klasse). De constructor was zoals we al zagen een eerste speciaal geval hiervan.  

In onderstaand voorbeeld voegen we een methode toe die de uiteindelijke punten berekent van de studenten:

~~~python
class Student:
    """Class for keeping track of student data."""

    def __init__(self, first_name = "", surname = "", lab_points = 0, theory_points = 0):
        self.first_name = first_name
        self.surname = surname
        self.lab_points = lab_points
        self.theory_points = theory_points

    def points(self):
        return (self.lab_points + self.theory_points) / 2

students = []

students.append(Student("Jan", "Janssens", 15))
students.append(Student("Piet", "Pieters", 16, 17))

for student in students:
    print(f"{student.first_name} {student.surname} heeft {student.lab_points} op het labo en {student.theory_points} op theorie, of {student.points()} in totaal.")
~~~

We definiëren dus een functie `points` met als enige parameter `self` dat naar het object zelf verwijst. Omdat we deze functie in het blok van de klasse `Student` definiëren, is dit een methode. Hierin tellen we de attributen `lab_points` en `theory_points` van het object op (let op: daarvoor moeten we `self` en de dot-notatie gebruiken) en delen we dit door 2. Het resultaat geven we met `return` terug.

Als we dit uitvoeren, krijgen we:

~~~
$ python student.py
Jan Janssens heeft 15 op het labo en 0 op theorie, of 7.5 in totaal.
Piet Pieters heeft 16 op het labo en 17 op theorie, of 16.5 in totaal.
~~~

Let op het verschil tussen attribuut en methode:

* Omdat `first_name` een attribuut is van het object, kunnen we dit opvragen met `student.first_name`, dus objectnaam, punt en attribuutnaam.
* Omdat `points` een methode is, kunnen we dit opvragen met `student.points()`, dus objectnaam, punt, methodenaam en haakjes. Als de methode nog extra parameters zou hebben, zouden deze tussen de haakjes komen te staan.

### De methode `__str__`

Een andere speciale methode (eerder hadden we al reeds de constructor gezien) is de methode `__str__`. Als je die aan een klasse toevoegt, wordt die automatisch aangeroepen wanneer je een object van de klasse naar een string wilt converteren:

* door de functie `str()` te gebruiken
* door het object mee te geven aan de functie `print()` (die op zijn beurt `str()` aanroept)

~~~python
class Student:
    """Class for keeping track of student data."""

    def __init__(self, first_name = "", surname = "", lab_points = 0, theory_points = 0):
        self.first_name = first_name
        self.surname = surname
        self.lab_points = lab_points
        self.theory_points = theory_points

    def points(self):
        return (self.lab_points + self.theory_points) / 2

    def __str__(self):
        return f"{self.first_name} {self.surname} heeft {self.lab_points} op het labo en {self.theory_points} op theorie, of {self.points()} in totaal."


students = []

students.append(Student("Jan", "Janssens", 15))
students.append(Student("Piet", "Pieters", 16, 17))

for student in students:
    print(student)
~~~

Geeft volgend resultaat:

~~~
Jan Janssens heeft 15 op het labo en 0 op theorie, of 7.5 in totaal.
Piet Pieters heeft 16 op het labo en 17 op theorie, of 16.5 in totaal.
~~~
