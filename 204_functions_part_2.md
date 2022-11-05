## Functies (deel 2)

Hier bekijken we nog een **aantal zaken** die we **nog niet hebben gezien** in het eerste gedeelte van **functies**:

* **Optionele argumenten**
* **Types van argumenten**
* **pass**-statement
* Arguments **by name**
* **Variabel aantal argumenten**
* **Recursie**

### Optionele argumenten

Je kan voor een functie **default waardes** meegeven aan een argument.  
Zie bijvoorbeeld hieronder een functie om de macht van een getal te berekenen:

~~~python
def power_of(base, exponent=2):
    return base ** exponent
~~~

Bij het aanroepen van deze functie heb je de mogelijkheid het tweede argument weg te laten:

~~~python
def power_of(base, exponent=2):
    return base ** exponent

print(power_of(2))     # prints 4
print(power_of(2, 2))  # prints 4
print(power_of(2, 3))  # prints 8
~~~

Met andere woorden: de exponent wordt dan **optioneel**. Als je dit argument niet invult, geeft Python dit automatisch de waarde 2 omdat dit als standaardwaarde in de definitie van de functie staat.

Dit kan interessant zijn in situaties waar er in de meeste gevallen een standaardwaarde is, maar je hier wel van wil kunnen afwijken.

### Types van argumenten

Je kan in de definitie van een functie de types van de argumenten definiëren die je verwacht:

~~~python
def times(a: int, b: int):
    return a * b

print(times(2, 5))
print(times("2", 5))
~~~

We verwachten hier dus dat de argumenten die je aan de functie `times` doorgeeft twee ints zijn.

Als je dit programma uitvoert, krijg je het volgende te zien:

~~~
10
22222
~~~

Python 'vermenigvuldigt' dus gewoon de string "2" met 5, ook al hebben we aangegeven dat dit argument het type int moet hebben. Python houdt hier dus geen rekening mee: dit type is gewoon een aanduiding om het onszelf duidelijk te maken wat voor type we verwachten.

Later gaan we zien hoe je met externe tools zoals mypy toch kunt controleren of de juiste types in je programma gebruikt zijn.

### pass-statement

**Python** laat **geen lege blocks** toe.  
Onderstaande code is in Python niet toegelaten

~~~python
def a_function_not_yet_implemented():

a_function_not_yet_implemented()
~~~

Dan krijg je een foutmelding:

```
$ python test.py
  File "/home/koan/test.py", line 3
    a_function_not_yet_implemented()
    ^
IndentationError: expected an indented block after function definition on line 1
```

Ook bij andere blocks (if, while, for, ...) is dit het geval.

Een commentaar met hashtag toevoegen helpt overigens niet, zelfs niet als je die correct indenteert volgens het blok:

~~~python
a = int(input("Geef een getal in: "))
if a > 10:
    print("Getal is groter dan 10")
else:
    # TODO: reageren als getal <= 10
~~~

Wat wél geldige code geeft, is een multi-line commentaar toevoegen (tussen drie aanhalingstekens), zelfs als je die maar op één regel zet:

~~~python
def a_function_not_yet_implemented():
    """TODO"""

a_function_not_yet_implemented()
~~~

En:

~~~python
a = int(input("Geef een getal in: "))
if a > 10:
    print("Getal is groter dan 10")
else:
    """TODO: reageren als getal <= 10"""
~~~

Als je nog niet weet wat te implementeren en ook nog geen commentaar weet, kan je ook in de plaats een **pass-statement** toevoegen:

~~~python
def a_function_not_yet_implemented():
    pass

a_function_not_yet_implemented()
~~~

En:

~~~python
a = int(input("Geef een getal in: "))
if a > 10:
    print("Getal is groter dan 10")
else:
    pass
~~~

`pass` is een statement dat niets doet. Het is dus handig als plaatshouder op een plaats waar een statement nodig is om van geldige code te kunnen spreken, maar waar (nog) niets hoeft te gebeuren.

### Access by name

Tot nu toe hebben we functies aangeroepen door een lijst van argumenten mee te geven, gescheiden door komma's.

Er is echter een **tweede manier**, namelijk de argumenten **via hun naam** doorgeven.

~~~python
def power_of(base, exponent=2):
    return base ** exponent

print(power_of(base=2, exponent=4))  # prints 16
print(power_of(exponent=4, base=2))  # prints 16
print(power_of(2, exponent=4))       # prints 16
~~~

Je kan in dat geval zelfs **kiezen** in welke **volgorde** je deze meegeeft (als je per naam meegeeft).  

Je kan ook **combineren** met de **standaard** manier van **argumenten** passeren, maar dat kan dan enkel voor de **laatste argumenten**. Dit kan dus niet:

```python
>>> print(power_of(base=2, 4))
  File "<stdin>", line 1
    print(power_of(base=2, 4))
                           ^
SyntaxError: positional argument follows keyword argument
```

### \*args

In een aantal functies (zoals bijvoorbeeld **printf**) heb je de mogelijkheid om een **variabel aantal argumenten** mee te geven.  

Je kan dit ook zelf doen door een **sterretje** te plaatsen **voor** je **argument** waarna je dit argument kan **behandelen** zoals een **lijst** (zie de lessen met list en de for-lus)

~~~python
def print_students(*students):
    for student in students:
        print(student)

print_students("Jan", "Piet", "Joris", "Korneel")
~~~

Met als resultaat:

~~~
Jan
Piet
Joris
Korneel
~~~

Je zou dit ook kunnen implementeren door een lijst als argument door te geven. Maar dit heeft als nadeel dat je aanroepende code extra symbolen ([...]) moet plaatsen voor deze lijst.

~~~python
def print_students(students):
    for student in students:
        print(student)

print_students(["Jan", "Piet", "Joris", "Korneel"])
~~~

Dit heeft hetzelfde resultaat:

~~~
Jan
Piet
Joris
Korneel
~~~

Let wel dat je de lijst-notatie niet verward met de \*args-notatie.  
Stel dat je \*args-functionaliteit voorziet maar een lijst meegeeft als argument...

~~~python
def print_students(*students):
    for student in students:
        print(student)

print_students(["Jan", "Piet", "Joris", "Korneel"])
~~~

... dan wordt deze lijst beschouwd als één argument!

~~~
['Jan', 'Piet', 'Joris', 'Korneel']
~~~


### recursie

De volgende functie/code telt af vanaf 10

~~~python
def countdown_function(count):
    for i in range(count, 0, -1):
        print(i)

countdown_function(10)
~~~

Je kan deze ook recursief schrijven.
Dit is een techniek waarbij je een functie zichzelf laat aanroepen waar je telkens de loop state meegeeft als argument.

~~~python
def countdown_function(count):
    if count > 0:
        print(count)
        countdown_function(count - 1)

countdown_function(10)
~~~

> NOTA: dit stuk over recursie is enkel ter introductie en zal later in de cursus uitgebreider behandeld worden.
