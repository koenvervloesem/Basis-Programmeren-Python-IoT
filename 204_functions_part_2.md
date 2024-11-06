## Functies (deel 2)

Hier bekijken we nog een **aantal zaken** die we **nog niet hebben gezien** in het eerste gedeelte van **functies**:

* **Optionele argumenten**
* **pass**-statement
* Arguments **by name**
* Een **lijst als argument** doorgeven
* **Variabel aantal argumenten**
* **Recursie**
* **Globale variabelen** aanpassen in een functie

### Optionele argumenten

Je kunt voor een functie **standaardwaardes** meegeven aan een argument.  
Zie bijvoorbeeld hieronder een functie om de macht van een getal te berekenen:

~~~python
def power_of(base, exponent=2):
    return base ** exponent
~~~

Bij het aanroepen van deze functie heb je de mogelijkheid om het tweede argument weg te laten:

~~~python
def power_of(base, exponent=2):
    return base ** exponent

print(power_of(2))     # prints 4
print(power_of(2, 2))  # prints 4
print(power_of(2, 3))  # prints 8
~~~

Met andere woorden: de exponent wordt dan **optioneel**. Als je dit argument niet invult, geeft Python dit automatisch de waarde 2 omdat dit als standaardwaarde in de definitie van de functie staat.

Dit kan interessant zijn in situaties waar er in de meeste gevallen een standaardwaarde is, maar je hier wel van wil kunnen afwijken. Dan hoef je in de meeste gevallen dat extra argument niet te vermelden.

### Het `pass`-statement

**Python** laat **geen lege blokken** toe.  
Onderstaande code is dus geen geldige Python-code:

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

Ook bij andere blokken (`if`, `while`, `for`, ...) is dit het geval.

Een commentaar met hashtag (`#`) toevoegen helpt overigens niet, zelfs niet als je die correct indenteert volgens het blok:

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

Als je nog niet weet wat te implementeren en ook nog geen commentaar weet, kun je ook in de plaats het statement `pass` toevoegen:

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

Het statement `pass` doet letterlijk niets. Het is dus handig als plaatshouder op een plaats waar een statement nodig is om van geldige code te kunnen spreken, maar waar (nog) niets hoeft te gebeuren.

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

Je kunt in dat geval zelfs **kiezen** in welke **volgorde** je deze meegeeft (als je per naam meegeeft).  

Je kunt dit ook **combineren** met de **standaard** manier van **argumenten** doorgeven, maar dat kan dan alleen voor de **laatste argumenten**. Dit kan dus niet:

```python
>>> print(power_of(base=2, 4))
  File "<stdin>", line 1
    print(power_of(base=2, 4))
                           ^
SyntaxError: positional argument follows keyword argument
```

### Een lijst als argument doorgeven

Tot nu toe hebben we aan functies vooral enkelvoudige types als argumenten doorgegeven, zoals een string of getal. We hebben gezien dat een string als argument zich gedraagt als een **lokale variabele** binnen de functie:

~~~python
def dupliceer(tekst):
    tekst = 2 * tekst
    print(tekst)

tekst = "banana"

dupliceer(tekst)
print(tekst)
~~~

Als je dit uitvoert, krijg je:

~~~
bananabanana
banana
~~~

We dupliceren binnen de functie dus het argument "banana" tot "bananabanana", maar buiten de functie blijft de variabele tekst de oorspronkelijke waarde "banana" hebben. Veranderingen binnen de functie aan het argument blijven lokaal binnen de functie, omdat Python het argument als waarde (**by value**) doorgeeft en dus een kopie maakt.

Als we een lijst doorgeven als argument, gedraagt Python zich anders:

~~~python
def dupliceer(lijst):
    lijst.extend(lijst)
    print(lijst)

fruit = ["apple", "banana", "kiwi"]

dupliceer(fruit)
print(fruit)
~~~

Dit geeft als uitvoer:

~~~
['apple', 'banana', 'kiwi', 'apple', 'banana', 'kiwi']
['apple', 'banana', 'kiwi', 'apple', 'banana', 'kiwi']
~~~

Wat is hier gebeurd? Python geeft een lijst als argument aan een functie niet als waarde door, maar geeft een verwijzing naar de lijst door (**by reference**). Dus als je binnen de functie iets aan de lijst verandert, is die verandering ook buiten de functie zichtbaar, want het gaat om dezelfde lijst, niet om een kopie.

Voor een set geldt hetzelfde: ook die wordt by reference doorgegeven aan functies.

### \*args

In een aantal functies (zoals bijvoorbeeld **print**) heb je de mogelijkheid om een **variabel aantal argumenten** mee te geven.  

Je kunt dit ook zelf doen door een **sterretje** te plaatsen **voor** je **argument** waarna je dit argument kan **behandelen** zoals een **lijst**:

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


### Recursie

De volgende code telt af vanaf 10:

~~~python
def countdown_function(count):
    for i in range(count, 0, -1):
        print(i)

countdown_function(10)
~~~

Je kunt deze ook **recursief** schrijven.
Dat is een techniek waarbij je een functie zichzelf laat aanroepen waar je telkens de loop state meegeeft als argument.

~~~python
def countdown_function(count):
    if count > 0:
        print(count)
        countdown_function(count - 1)

countdown_function(10)
~~~

Let op: als je een recursieve functie schrijft, moet je altijd zorgen dat het aanroepen van de functie ooit eindigt. In dit geval doen we dat door:

* de functie alleen aan te roepen als count groter is dan 0
* van count bij elke aanroep 1 af te trekken, zodat count uiteindelijk wel 0 wordt

### Globale variabelen

Als we een variabele bovenaan de code definiëren, kun je die ook gebruiken in functies:

~~~python
x = 10

def print_x():
    print(x)

print_x()
print(x)
~~~

Dit zal twee keer de waarde 10 tonen: binnen en buiten de functie heeft x gewoon dezelfde waarde. We noemen dit een **globale variabele**.

Stel nu dat we x in de functie een andere waarde geven:

~~~python
x = 10

def increment_x():
    x = 11
    print(x)

increment_x()
print(x)
~~~

Dan zien we:

~~~shell
$ python test.py
11
10
~~~

Met de regel `x = 11` in de functie maak je immers een nieuwe, **lokale variabele** aan die alleen binnen de functie gedefinieerd is. Toevallig heeft die dezelfde naam als de globale variabele x. Binnen de functie heeft x dan de waarde 11 (de lokale variabele), maar buiten de functie 10 (de globale variabele).

Maar wat als we nu in de functie de globale variabele een nieuwe waarde willen geven? Dat kan, maar dan moeten we expliciet aangeven dat we in de functie met de naam x naar de globale variabele verwijzen:

~~~python
x = 10

def increment_x():
    global x
    x = 11
    print(x)

increment_x()
print(x)
~~~

Dit geeft dan twee keer de waarde 11 terug.

Maar stel nu dat we de waarde van de variabele x op de volgende manier willen veranderen in de functie:

~~~python
x = 10

def increment_x():
    x = x + 1
    print(x)

increment_x()
print(x)
~~~

Dit levert een exception op:

~~~shell
$ python test.py 
Traceback (most recent call last):
  File "/home/koan/test.py", line 7, in <module>
    increment_x()
  File "/home/koan/test.py", line 4, in increment_x
    x = x + 1
UnboundLocalError: local variable 'x' referenced before assignment
~~~

Waarom? Omdat Python denkt dat je naar een lokale variabele x verwijst, maar die heeft nog geen waarde, er is namelijk nog geen assignment binnen de functie gebeurd.

Dat los je hier dus ook op door met `global x` aan te duiden dat je binnen de functie de globale variabele x wilt gebruiken:

~~~python
x = 10

def increment_x():
    global x
    x = x + 1
    print(x)

increment_x()
print(x)
~~~

En dan geven beide print-statements weer 11 terug.
