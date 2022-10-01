## Errors en exceptions

### Syntax error => programma wordt niet uitgevoerd

Het **uitvoeren** van Python-code gebeurt in **twee fases**:

* Het **parsen en interpreteren** van de Python-code
* Het eigenlijke **uitvoeren**

Als er **fouten** in de **codesyntaxis** zijn, herkent de Python-interpreter dat al in de eerste fase en dus al vóór de **start** van het **programma**.

In de volgende code **vergeten** we bijvoorbeeld (bewust) een **":"** te plaatsen **na de for**:

~~~python
print("Van 1 tot en met 7")
for i in range(7)
    print(i + 1)
~~~

Dit is **ongeldige code**. Wat gebeurt er nu **als je deze code probeert uit te voeren?**

~~~
$ python test.py
  File "/home/koan/test.py", line 2
    for i in range(7)
                     ^
SyntaxError: expected ':'
~~~

Er gebeurt niets... De code op regel 1 wordt zelfs niet uitgevoerd, ook al staat de fout op regel 2. De interpreter heeft namelijk al vóór het uitvoeren van het programma de code bekeken en ziet een fout tegen de syntaxis, die het meldt.

### Runtime error (exception) => programma stopt

De syntax errors worden dus snel gedetecteerd.  
Er kunnen echter ook fouten gebeuren **"at runtime"** zoals **bijvoorbeeld**:

* Een functie oproepen die niet bestaat.
* Een string die niet kan omgezet worden naar een integer (bijvoorbeeld `int("6abc")`).
* Delen door 0 (bijvoorbeeld `a = 5/0`).

Deze "runtime errors" benoemen we ook als **exceptions**.

Neem bijvoorbeeld het volgende programma:

~~~python
print("Hello")
a = 5/0
print(a)
~~~

Voer je dit programma uit, dan zie je:

~~~bash
$ python test.py
Hello
Traceback (most recent call last):
  File "/home/koan/test.py", line 2, in <module>
    a = 5/0
ZeroDivisionError: division by zero
~~~

In dit geval zien we dat er ook duidelijk een **error** wordt aangegeven, in dit geval een **ZeroDivisionError**.  

Bemerk wel dat de **code** die **vóór de fout** (a = 5/0) komt wel **wordt uitgevoerd** (`print("Hello")`).

Het programma **start** wel degelijk **maar stopt** bij het **aangegeven punt waarop de fout gebeurt**.

In tegenstelling tot een syntax error kan de Python-interpreter niet al in de eerste fase bij het parsen van de Python-code bepalen dat er een fout is.

### Exceptions en functies

Het maakt ook niet uit als je deze **exception genereert** binnen een functie. Deze wordt dan **gepropageerd** naar de plaats waar de functie wordt aangeroepen zolang deze niet wordt opgevangen.

Bijvoorbeeld:

~~~python
def divide(a, b):
    return a / b

print("Hello")
a = divide(5, 0)
print(a)
~~~

Voer je dit uit, dan geeft dit als resultaat:

~~~
Hello
Traceback (most recent call last):
  File "tmp.py", line 5, in <module>
    a = divide(5, 0)
  File "tmp.py", line 2, in divide
    return a / b
ZeroDivisionError: division by zero
~~~

### Exceptions opvangen

Je kan in je code ervoor zorgen dat deze **exceptions** worden **opgevangen**, zodat ze het programma niet beëindigen.

Dit kan via een **try-block** in combinatie met een **except-block**  
In onderstaande code proberen we een variabele te tonen die niet bestaat.  

~~~python
print("Before try-catch")
try:
    print(x)
    print("After error")
except:
    print("An exception occurred")
print("After try-catch")
~~~

We kunnen dit doen aan de hand van een default **except-block**. Deze vangt gelijk welke exception (behalve een SyntaxError) op.

Dit heeft volgende uitvoering als resultaat...

~~~bash
$ python test.py
Before try-catch
An exception occured
After try-catch
~~~

We zien hier dat:

* Het **programma** wordt **uitgevoerd**.
* Het **try-block** wordt **onderbroken** (bij `print(x)`) omdat `x` niet gedefinieerd is en de print-opdracht de regel erna niet uitgevoerd wordt.
* Het **except-block** wordt **uitgevoerd**.
* De **code loopt verder** na de except.

### Exceptions opvangen per type

Het gebruik van except zoals hierboven is alles-of-niets: alle exceptions worden opgevangen. Doorgaans is het een betere stijl van je code om alleen specifieke exceptions op te vangen die je verwacht. Zo kun je gerichter reageren in het except-blok, en zal het programma nog altijd stoppen wanneer er onverwachte exceptions optreden waarop je je code niet voorbereid hebt.

Je kan in Python dus ook het **type exception aangeven** dat je wil opvangen.  
In dit geval beperk je het opvangen tot een specifieke exception, de `NameError`:

~~~python
print("Before try-catch")
try:
    print(x)
    print("After error")
except NameError:
    print("An exception occurred")
print("After try-catch")
~~~

Het volstaat hier om het type exception te definiëren na het except-keyword.

Wat dan als er een andere exception optreedt (bijvoorbeeld een ZeroDivisionError)?

~~~python
print("Before try-catch")
try:
    print(5 / 0)
    print(x)
    print("After error")
except NameError:
    print("An exception occurred")
print("After try-catch")
~~~

Dan wordt deze niet opgevangen:

~~~bash
$ python test.py
Before try-catch
Traceback (most recent call last):
  File "/home/koan/test.py", line 3, in <module>
    print(5 / 0)
ZeroDivisionError: division by zero
~~~

... en wordt het programma beëindigd: de print-opdrachten na de regel waarin de ZeroDivisionError optreedt worden niet uitgevoerd.

### Meerdere except-blokken

Om dit probleem te verhelpen - en meerdere types exceptions op te vangen - kan je meerdere except-blokken plaatsen.  
Door onderstaande except-blok voor ZeroDivisionError toe te voegen vermijd je dat het programma bij het optreden van die exception wordt beëindigd.

~~~python
print("Before try-catch")
try:
    print(5 / 0)
    print(x)
    print("After error")
except NameError:
    print("A NameError exception occurred")
except ZeroDivisionError:
   print("A ZeroDivisionError exception occurred")
print("After try-catch")
~~~

Dit geeft deze uitvoer:

~~~bash
$ python test.py
Before try-catch
A ZeroDivisionError exception occurred
After try-catch
~~~

Je kan ook nog altijd het except-blok zonder type toevoegen:

~~~python
print("Before try-catch")
try:
    x = [1, 2]
    print(x[2])
    print(5 / 0)
    print(x)
    print("After error")
except NameError:
    print("A NameError exception occurred")
except ZeroDivisionError:
    print("A ZeroDivisionError exception occurred")
except:
    print("Another error")
print("After try-catch")
~~~

De uitvoer is in dit geval:

~~~bash
$ python test.py
Before try-catch
Another error
After try-catch
~~~

De regel `print(x[2])` genereert immers geen NameError of ZeroDivisionError, maar een IndexError. Omdat die niet na een except-blok staat maar er wel een algemeen except-blok zonder type bestaat, wordt dit laatste except-blok uitgevoerd.

Let op: het standaard except-blok zonder type exception moet altijd als laatste staan als je meerdere except-blokken gebruikt.

### else-clausule

Je kan ook een else-clausule toevoegen. Dit blok zal **alleen** uitgevoerd worden als er geen exception optreedt:

~~~python
try:
    print("Welkom")
    a = int(input("Teller: "))
    b = int(input("Noemer: "))
    print(f"{a} / {b} = {a / b}")
except ZeroDivisionError:
    print("Delen door nul is flauwekul")
else:
    print("Tot de volgende keer")
~~~

Dat geeft:

~~~bash
$ python test.py
Welkom
Teller: 5
Noemer: 2
5 / 2 = 2.5
Tot de volgende keer
$ python test.py
Welkom
Teller: 5
Noemer: 0
Delen door nul is flauwekul
~~~

### finally

Er is ook een finally-clausule. Die wordt **altijd** uitgevoerd, ongeacht er een exception optreedt in het try-blok. Dit wordt doorgaans gebruikt om zaken 'op te kuisen'.

Een voorbeeld:

~~~python
try:
    print("Welkom")
    a = int(input("Teller: "))
    b = int(input("Noemer: "))
    print(f"{a} / {b} = {a / b}")
except ZeroDivisionError:
    print("Delen door nul is flauwekul")
finally:
    print("Tot de volgende keer")
~~~

Kijk hoe in beide gevallen het finally-blok uitgevoerd wordt:

~~~bash
$ python test.py
Welkom
Teller: 5
Noemer: 2
5 / 2 = 2.5
Tot de volgende keer
$ python test.py
Welkom
Teller: 5
Noemer: 0
Delen door nul is flauwekul
Tot de volgende keer
~~~

### Zelf exceptions opwerpen

Naast het afvangen van exceptions kan je deze ook zelf opwerpen met het keyword **raise**.

Stel dat je de volgende functie maakt:

* Berekent de oppervlakte van een cirkel op basis van de straal.
* Je wil een exception werpen als je de functie oproept met een negatieve straal.

Dat kan als volgt:

~~~python
from math import pi

def circumference(radius):
    if radius < 0:
        raise Exception("A negative radius is invalid")
    return 2 * radius * pi

print(circumference(1))
print(circumference(-1))
~~~

De functie-aanroep op de laatste regel zal het het programma doen beëindigen:

~~~bash
$ python test.py
6.283185307179586
Traceback (most recent call last):
  File "/home/koan/test.py", line 9, in <module>
    print(circumference(-1))
  File "/home/koan/test.py", line 5, in circumference
    raise Exception("A negative radius is invalid")
Exception: A negative radius is invalid
~~~

### Zelf (aangepaste) exceptions definiëren

Je kunt zelf een specifiek type exception definiëren:

~~~python
from math import pi

class NegativeRadiusException(Exception):
    pass

def circumference(radius):
    if radius < 0:
        raise NegativeRadiusException()
    return 2 * radius * pi

print(circumference(1))
print(circumference(-1))
~~~

Noot: je definieert hier een klasse, iets wat we verder in de cursus uitleggen.

Als je dit uitvoert, krijg je:

~~~bash
$ python test.py
6.283185307179586
Traceback (most recent call last):
  File "/home/koan/test.py", line 12, in <module>
    print(circumference(-1))
  File "/home/koan/test.py", line 8, in circumference
    raise NegativeRadiusException()
__main__.NegativeRadiusException
~~~

### En opvangen...

Je kan deze exception dan ook volgens het type opvangen zoals we eerder hebben gezien:

~~~python
from math import pi

class NegativeRadiusException(Exception):
    pass

def circumference(radius):
    if radius < 0:
        raise NegativeRadiusException()
    return 2 * radius * pi

try:
    print(circumference(1))
    print(circumference(-1))
except NegativeRadiusException:
    print("Straal mag niet negatief zijn")
~~~

Dit geeft:

~~~bash
$ python test.py
6.283185307179586
Straal mag niet negatief zijn
~~~
