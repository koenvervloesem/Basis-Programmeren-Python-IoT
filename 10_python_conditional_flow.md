## Conditionele uitvoering

### Van sequentiële naar conditionele uitvoering

~~~
                            +------------------------------+
                            |                              |
                            |     Hergebruik uitvoering    |
                            |                              |
                        +---+------------------------------+---+
                        |                                      |
                        |        Repetitieve uitvoering        |
                        |                                      |
           _        +---+--------------------------------------+---+
            \       |                                              |     * if-elif-else
       _____ \      |           Conditionele uitvoering            |     * clausules
             /      |                                              |     * blocks
           _/   +---+----------------------------------------------+---+
                |                                                      | * Statements
                |                Sequentiële uitvoering                |    * Function calls
                |                                                      |    * Assignments
                |   +------------+   +------------+   +------------+   | * Expressions
                |   | Statements |   | Variabelen |   | Expressies |   | * Variables
                +---+------------+---+------------+---+------------+---+
~~~

We hebben nu kennisgemaakt met een aantal basiselementen uit de sequentiële uitvoering:

* **Statements**: assignment, function call, ...
* **Variabelen**: int, string
* Rekenkundige **expressies**

We starten nu aan **complexere code**. We gaan nu namelijk naar **conditionele uitvoering** kijken.  
Dit principe bouwt voort op sequentiële uitvoering, maar voegt het element van **keuze** toe.

Door een combinatie van **relationele expressies** en de **if-else-statements** kunnen we kiezen - **at runtime** (terwijl het script uitgevoerd wordt) - welk blok code er wordt uitgevoerd. Deze keuze hadden we niet bij zuiver sequentiële uitvoering: daar werd gewoon het ene na het andere statement uitgevoerd.  
Dit keuze-aspect is een **voorwaarde** (*condition*).

~~~
                                +----------------+ 
                                |      ...       |
                                +-------+--------+
                                        |
                                        V
                                       ***
                                     **   **
                          True     **       **     False
                        +-------+** CONDITION **+----------+
                        |          **       **             |
                        |            **   **               |
                        |              ***                 |
        +---    +-------V--------+                 +-------V--------+   ---+
        |       |   Statement    |                 |   Statement    |      |
        |       +-------+--------+                 +-------+--------+      |
        |               |                                  |               |
 BLOCK  |       +-------V--------+                 +-------V--------+      |   BLOCK
 TRUE --+       |   Statement    |                 |   Statement    |      +-- FALSE
        |       +-------+--------+                 +-------+--------+      |
        |               |                                  |               |
        |       +-------V--------+                 +-------V--------+      |
        |       |      ...       |                 |      ...       |      |
        +---    +-------+--------+                 +-------+--------+   ---+
                        |                                  |
                        +---------------+------------------+
                                        |
                                +-------V--------+ 
                                |      ...       |
                                +-------+--------+
                                        |
                                        V
                                       ...  
~~~

Dankzij deze **voorwaarde** kunnen we vanuit de code een keuze maken welk **blok** (van statements) we uitvoeren (afhankelijk van de voorwaarde).

### Relationele expressies

Het eerste element zijn de **relationele expressies**.  

| Operator   |   Betekenis                |
|------------|----------------------------|
| ==         | gelijk aan                 |
| !=         | niet gelijk aan            |
| <          | kleiner dan                |
| >          | groter dan                 |
| <=         | kleiner dan of gelijk aan  |
| >=         | groter dan of gelijk aan   |

Dit zijn expressies die een **vergelijking** maken **tussen twee variabelen** (in veel gevallen **numerieke variabelen**).  
Deze expressies geven aan of een vergelijking **waar** (`True`) of **onwaar** (`False`) is. Enkele voorbeelden:

~~~
>>> 1==1
True
>>> 1!=1
False
>>> 5>6
False
>>> 6>=6
True
~~~

Met variabelen wordt dat dan:

~~~
>>> a=5
>>> b=6
>>> c=a<b
>>> c
True
>>> d=a>b
>>> d
False
~~~

Als men **a (5) vergelijkt met b (6)** voor de **relatie kleiner dan** (`a < b`) verkrijgt men de waarde `True`.  
Daarna vergelijken we de zelfde variabelen voor de **relatie groter dan** en verkrijgen we de waarde `False`.

### Variabelen van het type bool

`True` en `False` zijn de **enige mogelijke** resultaten van relationele expressies.
Hiervoor is in Python een specifiek type gecreëerd, namelijk het type `bool` (boolean):

~~~
>>> a=5
>>> b=6
>>> c=a<b
>>> type(c)
<class 'bool'>
~~~

Dit datatype `bool` heeft slechts twee mogelijk waardes (`True` en `False`). We gebruiken het in **conditionele en repetitieve uitvoering**.

### Gebruik van booleans

Met een relationele expressie kunnen we dus twee getallen (variabelen, literals, resultaten van expressies, ...) vergelijken.

Stel dat je een programma wil schrijven dat twee getallen vergelijkt (groter/kleiner). Dan kunnen we al gebruikmaken van wat we daarnet hebben geleerd:

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b: "))

print(a > b)
~~~

Afhankelijk van wat je intypt zal dit `True` of `False` tonen:

~~~
$ python compare_number.py
Enter number a: 10
Enter number b: 11
False
~~~

Het programma toont in dit geval `False` aangezien 10 (a) niet groter is dan 11 (b).  

Maar wat als we iets anders willen tonen dan `True` of `False`?

### Gebruik van een relationele expressie binnen een if-statement

Om meer te kunnen doen met dit resultaat (of vergelijking) introduceren we het statement `if`.  

Dit soort statement laat je toe om een **vergelijking** te **evalueren** en te **beslissen** of je al dan niet een **stuk code** zal **uitvoeren**.

In onderstaande code laten we `if` beslissen om de tekst "a is bigger than b" al dan niet te tonen.

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b: "))

if a > b:
    print("a is bigger than b")
~~~

Merk op: de regel die met `print` begint, moet je inspringen met vier spaties. Dit is namelijk een **blok** (zie verder) dat hoort bij het `if`-statement.

Als je nu dit programma test, dan toont het:

~~~
$ python compare_number.py
Enter number a: 11
Enter number b: 10
a is bigger than b
~~~

Het blok dat bij het `if`-statement hoort, is uitgevoerd omdat aan de voorwaarde bij het statement (`a > b`) voldaan is.

### Tegengestelde voorwaarde

Het probleem is hier dat het programma niets toont als je voor a een kleiner getal invult dan voor b:

~~~
$ python compare_number.py
Enter number a: 10
Enter number b: 11
~~~

Er ontbreekt dus de **tegengestelde voorwaarde**.

Een optie zou kunnen zijn om deze voorwaarde om te keren in een tweede `if`-statement dat daarop volgt, zoals hieronder:

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b: "))

if a > b:
    print("a is bigger than b")
if a <= b:
    print("a is smaller than or equal to b")
~~~

Dan krijg je inderdaad de correcte melding:

~~~
$ python compare_number.py
Enter number a: 10
Enter number b: 11
a is smaller than or equal to b
~~~

Er is echter een elegantere oplossing...

### Een if-statement is eigenlijk een if-else-statement

Zo'n `if`-statement kan je namelijk uitbreiden met een `else`-clausule.  
Hieronder passen we het vorige voorbeeld aan door het tweede `if`-statement te vervangen door een `else`-clausule:

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b: "))

if a > b:
    print("a is bigger than b")
else:
    print("a is smaller than or equal to b")
~~~

Het blok dat bij `else` hoort, wordt uitgevoerd als aan de voorwaarde bij `if` niet voldaan is.

### elif

Een derde mogelijkheid is het toevoegen van één (of meerdere) `elif`-clausule(s).
Stel dat je ook expliciet wil tonen wanneer de parameters aan elkaar gelijk zijn. Dan kan je een `elif`-clausule (wat staat voor *else if*) toevoegen:

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b: "))

if a > b:
    print("a is bigger than b")
elif a == b:
    print("a is equal to b")
else:
    print("a is smaller than b")
~~~

### Meerdere elif-clausules

Je kan ook meerdere `elif`-clausules toevoegen.  
Wil je bijvoorbeeld ook een boodschap tonen als a 1 kleiner is dan b? Voeg dan een tweede `elif`-clausule toe:

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b: "))

if a > b:
    print("a is bigger than b")
elif a == b:
    print("a is equal to b")
elif (b - a) == 1:
    print("a is 1 less than b")
else:
    print("a is smaller than b")
~~~

> Nota: je kan zoveel `elif`-clausules toevoegen als je wil.

### Structuur

Een `if`-statement is samengesteld uit twee (soorten) onderdelen of componenten.

* Eén of meerdere **clausules**
  * **één** `if`
    * **optioneel één** `else`
    * **optioneel één of meerdere** `elif`
  * alleen `if` is **verplicht**
  * elke **clausule** **eindigt** op een `:` (dubbele punt)  
   (zoniet zal de interpreter een fout **SyntaxError: expected ':'** aangeven)
* telkens gevolgd door een **blok**
  * dat **één of meerdere statements** bevat
  * **geïndenteerd** ten opzichte van de clausule die voorafgaat
  * indentatie betekent **1 tab of 4 spaties**  

> Nota: Je kan kiezen tussen het gebruik van een tab of 4 spaties,
> maar binnen één Python-bestand moet je consequent zijn.
> Als je beide mixt, zal de Python-interpreter een foutmelding geven.


~~~python
if a > b:  # <----------------------------------- if-clausule
    print("a is bigger than b")  #              |
    print("2nd time a is bigger than b")  #     |---- block (3 regels geïndenteerd)
    print("3rd time a is bigger than b")  #     |
elif a == b:  # <-------------------------------- elif-clausule
    print("a is equal to b")  #                 |
    print("2nd time a is equal to b")  #        |---- block (2 regels) 
elif (b - a) == 1:  # <-------------------------- elif-clausule (2nd)
    print("a is 1 less than b")  #              |---- block (1 regel)
else:  # <--------------------------------------- else-clausule
    print("a is smaller than or equal to b")  # |---- block (1 regel)
~~~

Bovenstaande code toont aan dat je **één of meerdere statements** in zo'n blok kan steken.

### Geneste if-statements

Binnen een `if`-blok kunnen niet alleen enkelvoudige statements (van één regel) voorkomen, maar ook andere **block statements**, waaronder `if` en `while` (zie later).

Stel dat we een boodschap willen tonen wanneer een **getal c** zich **tussen** een getal **a en b** ligt.  
Voorlopig gaan we ervan uit dat **a** altijd **kleiner** is dan **b**.  
Dit houdt in dat je aan twee vergelijkingen moet voldoen:

* c moet groter dan of gelijk zijn aan a
* c moet kleiner dan of gelijk zijn aan b

Dat kunnen we als volgt testen:

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b (should be bigger than a): "))
c = int(input("Enter number c: "))

if c  >= a:
    if c <= b:
        print("c is in between a and b")
~~~

Door in bovenstaande oplossing de tweede voorwaarde (`c <= b`) te **nesten** binnen de eerste voorwaarde (`c >= a`), wordt de boodschap alleen getoond als aan beide voorwaarden voldaan is.

Je kunt ook meerdere niveaus nesten, maar beperk het aantal niveaus liever. Dat maakt een programma immers snel onoverzichtelijk.

### Logische expressies (en operatoren)

De **relationele expressies** die we net hebben bekeken zijn **boolean expressies** (expressies die een boolean als resultaat hebben en geen getal).  
Daarnaast kan je ook nog letterlijk de waarden `True` en `False` als **literal** gebruiken in je code.

~~~


                               +--------------------------+  "Logische expressie combineert
                               | Boolean expressies       |   Boolean expressies met elkaar"
                               | (boolean als resultaat)  +<---------------------+
                               +----------+---------------+                      |
                                          |                                      |
            +-----------------------------+--------------------------------+     |
            |                             |                                |     |
    +-------+----------+       +----------+--------------+       +---------+-----+-----+
    | Boolean literals |       | Relationele expressies  |       | Logische expressies |
    | (True of False)  |       | (==,>,<,>=,<=,!=)       |       | (and,or,not)        |
    +------------------+       +-------------------------+       +---------------------+


~~~

Een derde type boolean expressie is de **logische expressie**. Deze combineert één of twee boolean expressies met elkaar in een **logische relatie**.

### and, or en not

We bekijken drie operatoren/expressies:

| Operator   |   Betekenis                                           |
|------------|-------------------------------------------------------|
| and        | alle boolean expressies moeten waar zijn              |
| or         | minstens één van de boolean expressies moet waar zijn |
| not        | de boolean expressie is niet waar                     |

De operatoren `and` en `or` zijn (zoals de meeste operatoren tot nog toe) **binaire operatoren**: zij combineren twee (boolean) expressies.  
Daarentegen is `not` een **unitaire operator**, met slechts één operand.

### Logische and-operator

Een eerste voorbeeld is dus de `and`-operator. Deze heeft `True` als resultaat **alleen als beide operanden `True`** zijn.

~~~
               +-------+                               +-------+
               | True  +<------"Enkel als beide        | False |
               +---+---+        relationele            +---+---+
                   ^            epressies True             ^
                   |            zijn is resultaat          |
               +---+---+        True"                  +---+---+
        +------+  AND  +-------+                +------+  AND  +-------+
        |      +-------+       |                |      +-------+       |
        |                      |                |                      |
    +---+---+              +---+---+        +---+---+              +---+---+
    | True  |              | True  |        |  True |              | False |
    +-------+              +-------+        +-------+              +-------+


               +-------+       "In alle andere         +-------+
               | False |        gevallen False"        | False |
               +---+---+                               +---+---+
                   ^                                       ^
                   |                                       |
               +---+---+                               +---+---+
        +------+  AND  +-------+                +------+  AND  +-------+
        |      +-------+       |                |      +-------+       |
        |                      |                |                      |
    +---+---+              +---+---+        +---+---+              +---+---+
    | False |              | True  |        | False |              | False |
    +-------+              +-------+        +-------+              +-------+
~~~

Dit wordt ook wel eens in een **waarheidstabel** gezet:

| a     | b     | and   |
|-------|-------|-------|
| False | False | False |
| False | True  | False |
| True  | False | False |
| True  | True  | True  |

Ter **verduidelijkig** passen we dit toe op voorgaand **voorbeeld** waar een getal c tussen a en b moest vallen.

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b (should be bigger than a): "))
c = int(input("Enter number c: "))

if c >= a and c <= b:
    print("c is in between a and b")
~~~

Net zoals in de vorige versie van dit programma zal de `print` **alleen uitgevoerd** worden onder de voorwaarde dat **beide vergelijkingen** `True** zijn.  
Nu we beide elementen in één expressie in één `if`-clausule gecombineerd hebben, kunnen we ook de tegengestelde boodschap eenvoudig in een `else`-blok tonen:

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b (should be bigger than a): "))
c = int(input("Enter number c: "))

if c >= a and c <= b:
    print("c is inbetween a and b")
else:
    print("c is not inbetween a and b")
~~~

Als je dit wilde tonen in de versie met de geneste `if`, dan was je verplicht om de tegengestelde boodschap ("c is not in between...") te tonen op twee plaatsen in je code (zoals hieronder):

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b (should be bigger than a): "))
c = int(input("Enter number c: "))

if c >= a:
    if c <= b:
        print("c is in between a and b")
    else:
        print("c is not inbetween a and b")
else:
    print("c is not in between a and b")
~~~

Doorgaans is het dus beter om met logische expressies in een `if`-voorwaarde te werken dan om geneste `if`-constructies te gebruiken.

### Logische or-operator

Met de `or`-operator controleer je of er aan **minstens één van de twee voorwaarden voldaan** is.  
Deze zal **alleen** `False` teruggeven als **beide operanden `False` (onwaar)** zijn.

~~~
               +-------+                               +-------+
               | False +<------"Enkel als beide        | True  |
               +---+---+        relationele            +---+---+
                   ^            epressies False            ^
                   |            zijn is resultaat          |
               +---+---+        False"                 +---+---+
        +------+  OR   +-------+                +------+  OR   +-------+
        |      +-------+       |                |      +-------+       |
        |                      |                |                      |
    +---+---+              +---+---+        +---+---+              +---+---+
    | False |              | False |        |  True |              | False |
    +-------+              +-------+        +-------+              +-------+


               +-------+       "In alle andere         +-------+
               | True  |        gevallen True"         | True  |
               +---+---+                               +---+---+
                   ^                                       ^
                   |                                       |
               +---+---+                               +---+---+
        +------+  OR   +-------+                +------+  OR   +-------+
        |      +-------+       |                |      +-------+       |
        |                      |                |                      |
    +---+---+              +---+---+        +---+---+              +---+---+
    | False |              | True  |        | True  |              | True  |
    +-------+              +-------+        +-------+              +-------+
~~~

Dit wordt ook wel eens in een **waarheidstabel** gezet:

| a     | b     | or    |
|-------|-------|-------|
| False | False | False |
| False | True  | True  |
| True  | False | True  |
| True  | True  | True  |

Ter illustratie, in onderstaand voorbeeld kijken we na of een getal c groter is dan één van beide getallen a of b:

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b: "))
c = int(input("Enter number c: "))

if c > a or c > b:
    print("c is bigger than a or b")
else:
    print("c is smaller than or equal to both a and b")
~~~

Je kan deze `or`-expressie trouwens uitbreiden met meerdere logische operatoren zoals hieronder:

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b: "))
c = int(input("Enter number c: "))
d = int(input("Enter number d: "))

if d > a or d > b or d > c:
    print("d is bigger than a or b or c")
else:
    print("d is smaller than or equal to a and b and c")
~~~

### Logische not-operator

De `not`-operator of logische inverter draait het resultaat van een boolean expressie om.  

~~~
>>> 4>5
False
>>> not(4>5)
True
>>> 5>4
True
>>> not(5>4)
False
~~~

> Nota: De haakjes rond de expressie die volgt op not zijn niet verplicht maar vermijden verwarring bij grotere expressies.

Anders gezegd: als het resultaat `True` is, wijzigt dit naar `False` en omgekeerd.

~~~

            +-------+        +-------+
            |  True |        | False |
            +---+---+        +---+---+
                ^                ^
                |                |
            +---+---+        +---+---+
            |  NOT  |        |  NOT  |
            +---+---+        +---+---+
                ^                ^
                |                |
            +---+---+        +---+---+
            | False |        | True  |
            +-------+        +-------+
~~~

Of in een **waarheidstabel**:

| a     | not   |
|-------|-------|
| False | True  |
| True  | False |

Je kan bijvoorbeeld ook het voorgaande voorbeeld (uit de `or`-operator) omdraaien door een `not` te gebruiken:

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b: "))
c = int(input("Enter number c: "))

if not(c > a or c > b):  # same as c <= a and c <= b
    print("c is smaller than or equal to both a and b")
else:
    print("c is bigger than a or b")
~~~

### Complexer voorbeeld

We hernemen het voorbeeld "getal c tussen a en b".  
Het probleem met de vorige oplossing is nog altijd dat a kleiner moet zijn dan b om de vergelijking te doen kloppen.

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b (should be bigger than a): "))
c = int(input("Enter number c: "))

if c >= a and c <= b:
    print("c is in between a and b")
else:
    print("c is not in between a and b")
~~~

Bijvoorbeeld het volgende zal worden gedetecteerd (3 tussen 1 en 5)

~~~
a=1 <--- c=3 ---> b=5
~~~

Maar als we onderstaande zouden testen, dan zouden we dit met bovenstaande code niet detecteren. De code zou verkeerdelijk "c is not in between a and b" afdrukken.

~~~
a=5 <--- c=3 ---> b=1
~~~

Om die situatie wel correct te detecteren, zou je volgende code moeten schrijven:

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b (should be smaller than a): "))
c = int(input("Enter number c: "))

if c >= b and c <= a:
    print("c is in between a and b")
else:
    print("c is not in between a and b")
~~~

Maar daarmee herkennen we dan weer niet het eerste geval (als a kleiner is dan b).  
De oplossing is **beide logische expressies te combineren** met een **logische `or`-expressie**:

~~~python
a = int(input("Enter number a: "))
b = int(input("Enter number b: "))
c = int(input("Enter number c: "))

if (c >= a and c <= b) or (c >= b and c <= a):
    print("c is in between a and b")
else:
    print("c is not in between a and b")
~~~

Als we dit testen, zien we dat beide gevallen worden gedekt:

~~~
$ python between.py
Enter number a: 1
Enter number b: 5
Enter number c: 3
print("c is in between a and b)
$ python between.py
Enter number a: 5
Enter number b: 1
Enter number c: 3
print("c is in between a and b)
$ python between.py
Enter number a: 5
Enter number b: 1
Enter number c: 7
print("c is not in between a and b)
~~~
