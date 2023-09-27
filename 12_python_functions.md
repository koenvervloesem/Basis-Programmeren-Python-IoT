## Functies

Het laatste grote blok dat we bespreken in deel 1 van de cursus is het hergebruik van uitvoering.

~~~

                    _
                     \          +------------------------------+
                ______\         |                              |             * functies
                      /         |     Hergebruik uitvoering    |             * modules (implementatie in deel 2)
                    _/          |                              |             * klassen en objecten (deel 2)
                            +---+------------------------------+---+
                            |                                      |         * while loops
                            |        Repetitieve uitvoering        |         * loop state
                            |                                      |         * for loops
                        +---+--------------------------------------+---+
                        |                                              |     * if+elif+else
                        |           Conditionele uitvoering            |     * clausules
                        |                                              |     * blocks
                    +---+----------------------------------------------+---+
                    |                                                      | * Statements
                    |                Sequentiële uitvoering                |    * Function calls
                    |                                                      |    * Assignments
                    |   +------------+   +------------+   +------------+   | * Expressions
                    |   | Statements |   | Variabelen |   | Expressies |   | * Variables
                    +---+------------+---+------------+---+------------+---+
~~~

Hergebruik van uitvoering kan op drie manieren:

* Functies
    * **Stuk code** dat je **meermaals** kan **aanroepen**
    * Heeft een **naam** zoals een variabele
* Modules
    * (Logische) **groepering** van **functies**, klassen en variabelen
    * Bijvoorbeeld `math`, `serial`, `random`, ...
    * In dit deel van de cursus **gebruiken** we modules
    * In het volgende deel van de cursus ontwikkelen we zelf modules
* Objecten en klassen
    * Groepering van **variabelen** en **functies**
    * Verschillende **instanties** (objecten) mogelijk
    * In **deel 2 van de cursus**

### Functies en hergebruik

Het **sleutelwoord** bij **functies** is dus **hergebruik**.
We hebben al **eerder** **hergebruik** gezien ... namelijk met **variabelen**:

* voorzien **hergebruik** van **data/geheugen**
* **tussen** verschillende **statements**
* en je kan via de naam deze variabelen gebruiken

Nu bekijken we een **ander soort hergebruik**, namelijk **hergebruik van statements** of functionaliteit.


### Ingebouwde functies

Python heeft een aantal **ingebouwde functies** die we reeds hebben gebruikt.
We hebben al eerdere functies gebruikt zoals `input`, `int`, `str` en `print`:

~~~python
# input() gets a string from the user
input_a = input("Number a: ")
# int() converts the input string into an int we can use to calculate
a = int(input_a)
# str() converts an int back to a string
output_a = str(a + 1)
# the concatenated result is printed back through the print function
print(input_a + " + 1 = " + output_a)
~~~

### Gebruiken van functies

We hebben al eerder **function calls** (functie-aanroepen) gebruikt.  
Hoe gebruik je deze?

Een functie roep je aan met de **naam**, **gevolgd door haakjes**.

> Nota: de functie hieronder is een fictief voorbeeld om het gebruik te illustreren.
> We zien zo dadelijk echte voorbeelden.

Stel dat er een functie `an_example_function` zou bestaan, dan kan je deze functie uitvoeren als:

~~~python
an_example_function()
~~~

### Return value

Sommige functies geven een **waarde terug**. Deze kan je dan **opslaan** in een variabele. We noemen dit ook de **return value** van de functie.
Bijvoorbeeld een (fictieve) functie `hour` die het uur teruggeeft als string:

~~~python
result = hour()
print(result)  # e.g. 12:12
~~~

Een return value is de eerste manier van communiceren met de code in de functie.

### Argumenten

Daarnaast kan je een **argument** meegeven aan een functie, voor zover deze functie **parameters** definieert.

~~~
                         """Functie kan meerdere
                            inputs hebben (argumenten)
     +-------------------+  maar slechts 1 output"""  +--------------------+
     |      INPUT        |                            |     OUTPUT         |
     +-------------------+                            +--------------------+
     |  +------------+   |                            |                    |
     |  | Argument 1 +---+--+                         |                    |
     |  +------------+   |  |      +-------------+    |   +------------+   |
     |                   |  |----->+   FUNCTIE   +----+-->+   return   |   |
     |  +------------+   |  |      +-------------+    |   +------------+   |
     |  | Argument 2 +---+--+                         |                    |
     |  +------------+   |                            |                    |
     |   ...             |                            |                    |
     +-------------------+                            +--------------------+
~~~

Dit argument wordt dan door de code van de functie gebruikt om iets mee te doen (zoals een bewerking).  
Stel dat je een functie maakt die een getal vermenigvuldigt met 2:

~~~python
result = times_two(1)
print(result)  # 2
~~~

Je kan een waarde, variabele (of andere expressies) meegeven als argument. In dit geval geef je de waarde 1 als argument mee aan de functie `times_two`, en geeft die als waarde 2 terug.

### Meerdere argumenten

Je bent **niet beperkt tot één argument**. Een functie kan van **0 tot oneindig aantal parameters** definiëren.

~~~python
result = multiply(3, 2)
print(result)  # 6
~~~

Als een functie een parameter definieert, moet je bij het aanroepen van die functie standaard verplicht het bijbehorende argument opgeven. Er bestaan ook optionele argumenten. Daar komen we dadelijk nog op terug.

### Gebruik Python-modules

Functies als `int`, `str`, `print` en `input` zijn rechtstreeks beschikbaar in Python.
Andere - meer gespecialiseerde - functies zijn gegroepeerd in modules.

~~~
                +---------------------+
                |                     |
                |  Module             |
                |                     |
                |   +------------+    |
                |   | Functie    |    |
                |   +------------+    |
                |                     |
                |   +------------+    |
                |   | Functie    |    |
                |   +------------+    |
                |                     |
                |   +------------+    |
                |   | Functie    |    |
                |   +------------+    |
                |                     |
                +---------------------+
~~~

Als je bijvoorbeeld wiskundige functies wil gebruiken in Python, maak je gebruik van de module `math` (die standaard voorzien is in Python):

~~~
>>> import math
>>> degrees = 45
>>> radians = math.pi * degrees / 180
>>> radians
0.7853981633974483
>>> math.sin(radians)
0.7071067811865475
>>> math.sqrt(2)/2
0.7071067811865476
~~~

Merk op: de module bevat niet alleen functies, maar ook constanten, zoals `math.pi` dat de waarde van het getal π (3,14159...) bevat.

Om een module te kunnen gebruiken dien je een `import`-statement toe te voegen. Deze maakt de functies en andere onderdelen van de module in je Python-applicatie beschikbaar.

Om dan de functies te gebruiken, volstaat het niet om de naam van deze functies te gebruiken.

* je moet de naam laten voorafgaan door de naam van de module
* gevolgd door een **punt**

Bijvoorbeeld: `math.sqrt`, waarbij `math` de module is en `sqrt` een functie in die module.

Een andere manier is dat je één of meer functies (of variabelen) expliciet uit een module importeert:

~~~
>>> from math import pi, sin, sqrt
>>> degrees = 45
>>> radians = pi * degrees / 180
>>> radians
0.7853981633974483
>>> sin(radians)
0.7071067811865475
>>> sqrt(2) / 2
0.7071067811865476
~~~

Hierdoor hoef je de naam van de functie/variabele niet meer te laten voorafgaan door de naam van de module en een punt. Je kunt nu bijvoorbeeld gewoon de functie `sqrt` aanroepen.

Samengevat heb je twee opties:

* Ofwel importeer je de module met `import math` en roep je de functie aan als `math.sqrt`.
* Ofwel importeer je expliciet de functie uit de module met `from math import sqrt` en roep je de functie aan als `sqrt`.

Documentatie over de module math vind je op <https://docs.python.org/3/library/math.html#module-math>.

Merk op: de functie `math.sin` verwacht zijn argument in radialen (*radians*), niet in graden. De module `math` bevat overigens de functie `math.radians` om een hoek in graden naar radialen om te zetten, zodat je dat niet zelf hoeft te doen zoals in het voorbeeld hierboven.

Een andere nuttige module is `random`. Deze bevat functies om met willekeurige getallen te werken.  
Onderstaande code demonstreert het gebruik van deze module:

~~~python
>>> import random
>>> random.random()
0.06618251511918116
>>> random.random()
0.3704299911950565
>>> random.randint(1, 100)
77
>>> random.randint(1, 100)
12
~~~

De functie `random` geeft bij elke aanroep een willekeurig kommagetal tussen 0 en 1 terug, en de functie `randint` een willekeurig getal van de ondergrens tot en met de bovengrens die je opgeeft.

Documentatie over de module random vind je op <https://docs.python.org/3/library/random.html#module-random>.

### Zelf functies schrijven

Later gaan we nog zien hoe we zelf modules kunnen ontwikkelen. Maar laten we al starten met zelf functies te schrijven...

We starten met een functie die je naam toont:

~~~python
def greeting():  # <----------- function header ==> def + naam + () + :
    print("Hello")       #|<--- function body ==> block
    print(" from Koen")  #|
~~~

### Header en body

Deze functie bevat twee onderdelen:

* Een **functiedefinitie** of **function header**
    * Start met keyword `def`
    * Een **naam** (net zoals bij een variabele)
    * **Haakjes** (waar je argumenten kan tussenplaatsen)
    * **Eindigend** op een **dubbele punt** (die een code block aankondigt)
* Een **block** (van statements)
    * Geïndenteerd ten opzichte van de functie

Deze dubbele punt hadden we al eerder gezien bij voorwaarden en lussen.   
Dit teken duidt altijd het einde aan van een clausule die voorafgaat aan een code block.

> Nota: een functie is een herbruikbaar block van statements.  
> Op zichzelf is het echter geen statement, want een functie wordt niet uitgevoerd wanneer je ze alleen definieert.

### Gebruik van (zelfgeschreven) functies

Een functie op zich zal nooit uitgevoerd worden. Als je deze niet aanroept, zal er niet veel gebeuren.  
Je kan deze functie aanroepen net zoals andere functies.

~~~python
def greeting():
    print("Hello")
    print(" from Koen")
greeting()
~~~

Met als resultaat:

~~~
$ python3 greeting.py
Hello
 from Koen
$
~~~

### Volgorde is belangrijk

Belangrijk is dat je functie gedeclareerd is voordat je ze aanroept.  
Stel dat je de functie `greeting` zou aanroepen voor de declaratie...

~~~python
greeting()
def greeting():
    print("Hello")
    print(" from Koen")
~~~

...dan zal de Python-interpreter een foutmelding genereren zoals hieronder...

~~~
$ python3 test.py
Traceback (most recent call last):
  File "test.py", line 1, in <module>
    greeting()
NameError: name 'greeting' is not defined
~~~

Opdat code een functie kan aanroepen/gebruiken, moet deze dus eerder in de "sequentiële uitvoering" gedefinieerd zijn.

### Argumenten

Je kan een functie **algemener** maken door er **argumenten** of parameters aan toe te voegen.  
In onderstaand voorbeeld voegen we een argument `name` toe. Met dit argument kan je de naam bepalen die door de functie wordt getoond.

~~~python
def greeting(name):
    print("Hello")
    print(" from", name)
greeting("Koen")
~~~

Dit **argument** gedraagt zich als (en is in feite) een soort van variabele.   
Je kan als gevolg dezelfde functie meerdere malen **hergebruiken** met een verschillende naam.

~~~python
def greeting(name):
    print("Hello")
    print(" from", name)

greeting("Koen")
greeting("Bart")
~~~

Als je deze code dan uitvoert, krijg je twee keer dezelfde uitvoering maar met een andere naam.

~~~
$ python3 greeting.py
Hello
 from Koen
Hello
 from Bart
$
~~~

### Meerdere argumenten

Een functie kan ook meerdere argumenten gebruiken.  
In het volgende voorbeeld geven we zowel een voor- als achternaam mee...

~~~python
def greeting(name, surname):
    print("Hello")
    print(" from", name, surname)

greeting("Koen", "Vervloesem")
greeting("Bart", "Voet")
~~~

Met als resultaat:

~~~
$ python3 greeting.py
Hello
 from Koen Vervloesem
Hello
 from Bart Voet
$
~~~

### Uitvoer via een return statement

Naast invoer kan een functie ook uitvoer hebben.

~~~
                         """Functie kan meerdere
                            inputs hebben (argumenten)
     +-------------------+  maar slechts 1 output"""  +--------------------+
     |      INPUT        |                            |     OUTPUT         |
     +-------------------+                            +--------------------+
     |  +------------+   |                            |                    |
     |  | Argument 1 +---+--+                         |                    |
     |  +------------+   |  |      +-------------+    |   +------------+   |
     |                   |  |----->+   FUNCTIE   +----+-->+   return   |   |
     |  +------------+   |  |      +-------------+    |   +------------+   |
     |  | Argument 2 +---+--+                         |                    |
     |  +------------+   |                            |                    |
     |   ...             |                            |                    |
     +-------------------+                            +--------------------+
~~~

In de code kan dit via een `return`-statement.

Stel dat je een functie schrijft om de som van twee variabelen te berekenen, dan kan dit als volgt:

~~~python
def som(a, b):
    result = a + b
    return result
~~~

Via het `return`-statement kan je dan het resultaat van deze som teruggeven aan de aanroepende code. Die kan dit resultaat dan opslaan in bijvoorbeeld een variabele.

~~~python
def som(a, b):
    result = a + b
    return result
a = som(2, 5)
print(a)  # 7
~~~

### Opsplitsen van functies

Een functie kan natuurlijk ook nog een andere functie aanroepen.

~~~python
def greeting(name, surname):
    print("Hello", name, surname)

def polite_conversation():
    name = input("What's your first name? ")
    surname = input("What's your surname? ")
    greeting(name, surname)

polite_conversation()
~~~

De functie `polite_conversation` roept hier niet alleen de standaard in Python gedefinieerde functie `input` twee keer aan, maar ook de functie `greeting` die je daarvoor gedefinieerd hebt.

### Lokale variabelen

Je kan binnen een functie variabelen declareren.  

~~~python
def som(a, b):
    c = a + b
    return c

result = som(2, 5)
print(result)
~~~

Je hebt deze vaak nodig - zoals in bovenstaand voorbeeld - om tussenresultaten in op te slagen.  
Wel kan je deze **lokale variabelen** niet buiten deze functie gebruiken, zoals in de functie hieronder...

~~~python
def som(a, b):
    c = a + b
    return c

som(2, 5)
print(c)  # NameError
~~~

Deze code probeert `c`, die alleen binnen de functie `som` bekend is, buiten de functie som te gebruiken.  
De Python-interpreter geeft dan de volgende foutmelding:

~~~
$ python3 test.py
Traceback (most recent call last):
  File "/home/bart/test.py", line 6, in <module>
    print(c)  # NameError
NameError: name 'c' is not defined
~~~

### Scope

Variabelen **binnen een functie** zijn dus **niet bereikbaar** vanuit aanroepende code.   

Variabelen hebben - afhankelijk waar ze worden gedefinieerd - een **scope**.  
Deze scope bepaalt vanuit welke code deze variabele **bereikbaar** is maar ook de **levensduur** van de variabele.  
Een lokale variabele wordt namelijk telkens opnieuw geïnstantieerd bij het aanroepen van de functie.

### En verder...

Onthoud voorlopig dat **lokale variabelen alleen bestaan binnen het bereik/scope van een functie**. In het volgende deel van de cursus gaan we hier nog verder op in samen met wat meer geavanceerde toepassingen met functies.

Onthoud vooral ook waarom (en wanneer) je functies moet gebruiken.

* **Leesbaarheid** en **modulariteit** -> Opdelen van je code in stukken
* **Hergebruik** -> Code die je meerdere malen wil gebruiken zonder copy/paste
