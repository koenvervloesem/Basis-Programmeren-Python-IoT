## Repetitieve uitvoering (loops)

~~~
                             +------------------------------+
                             |                              |
                             |     Hergebruik uitvoering    |
            _                |                              |
             \           +---+------------------------------+---+
        _____ \          |                                      |         * while-loops
              /          |        Repetitieve uitvoering        |         * loop state
            _/           |                                      |         * for-loops
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


### Terugblik: conditionele uitvoering

Via **voorwaarden** hebben we met de **if-else-structuur** **beslissingen** kunnen maken in onze code.

~~~
                                          +----------------+
                                          |      ...       |
                                          +-------+--------+
                                                  |
                                                  V
                                                 ****
                                               **    **
                                    True     **        **     False
                                  +-------+** VOORWAARDE **+----------+
                                  |          **        **             |
                                  |            **    **               |
                                  |              ****                 |
                  +---    +-------V--------+                  +-------V--------+   ---+
                  |       |   Statement    |                  |   Statement    |      |
                  |       +-------+--------+                  +-------+--------+      |
                  |               |                                   |               |
           BLOCK  |       +-------V--------+                  +-------V--------+      |   BLOCK
           TRUE --+       |   Statement    |                  |   Statement    |      +-- FALSE
                  |       +-------+--------+                  +-------+--------+      |
                  |               |                                   |               |
                  |       +-------V--------+                  +-------V--------+      |
                  |       |      ...       |                  |      ...       |      |
                  +---    +-------+--------+                  +-------+--------+   ---+
                                  |                                   |
                                  +---------------+-------------------+
                                                  |
                                          +-------V--------+
                                          |      ...       |
                                          +-------+--------+
                                                  |
                                                  V
                                                 ...
~~~

Als de expressie van de voorwaarde True als waarde heeft, voer je een **stuk code** of **block** uit. 
Optioneel voer je een **ander stuk code** uit als de expressie van de voorwaarde False is (else- en elif-clausules).

### Repetitieve uitvoering

Waar je bij conditionele uitvoering een blok code eenmaal uitvoert, blijft je bij repetitieve uitvoering een blok code uitvoeren zolang een voorwaarde waar is.

~~~
                                          +----------------+
                                          |      ...       |
                                          +-------+--------+
                                                  |
                                                  V           "Zolang de voorwaarde
                                                 *+**          waar (True) is blijven de
                                               **    **        block-statement uitvoeren"
                                     False   **        **
                        +------------------+* VOORWAARDE +<-------------+
                        |                    **        **               |
                        |                      **    **                 |
                        |                        *+**                   |
                        |                         | True                |
                        |                         |                     |
                        |         +--+    +-------v--------+            |
                        |         |       |   Statement    |            |
                        |         |       +-------+--------+            |
                        |         |               |                     |
                        |  BLOCK  |       +-------V--------+            |
                        |  TRUE +-+       |   Statement    |            |
                        |         |       +-------+--------+            |
                        |         |               |                     |
                        |         |       +-------V--------+            |
                        |         |       |      ...       +------------+
                        |         +--+    +----------------+
                        |                                    "Bij de laatste statement
                        |                                     van het block keren we terug
                        +-------------------------+           naar de voorwaarde"
                                                  |
                                                  |
                                          +-------v--------+
                                          |      ...       |
                                          +-------+--------+
                                                  |
                                                  V
                                                 ...
~~~

Een repetitieve uitvoering wordt een lus of **loop** genoemd, omdat je elke keer de voorwaarde True is het blok uitvoert en weer terugkeert naar de voorwaarde, als het ware in een lus.

Waar bij een **if-statement** na het uitvoeren van het erbij horende blok het **volgende statement** wordt uitgevoerd, keer je bij een **loop** op het einde van het blok **terug naar de voorwaarde**.

Python kent twee soorten lussen: **while** en **for**.

### while-lus met teller

Om de while-lus te illustreren, starten we met een eenvoudig voorbeeld:

* We tonen een aantal sterren op de console.
* Je geeft het aantal in dat je wil tonen.

~~~python
number_of_stars = int(input("Number of stars to print? "))
counter = 0

while counter < number_of_stars:
    print("*")
    counter = counter + 1
~~~

Een while-lus is net zoals de if-statements een **block statement**. Dat betekent dus dat:

* de lus de **inhoud van het blok uitvoert**
* de while-clausule **eindigt met een :**
* het blok **geïndenteerd** is

Het verschil met een if-statement ligt in het feit dat deze constructie het blok zal blijven uitvoeren zolang de voorwaarde True geeft.

### Tellers

Als je dit Python-script uitvoert en op de vraag om het aantal sterren 5 intypt, zal de code 5 keer het blok dat ten opzichte van de while-regel is geïndenteerd uitvoeren.

~~~
$ python print_stars.py
Number of stars to print? 5
*
*
*
*
*
~~~

Om dit gedrag te bereiken, hebben we twee elementen nodig:

* Het **aantal keer** dat je **telt** => number_of_stars
* De **teller** zelf => counter

Merk op: we initialiseren de teller op 0 en vergelijken in de while-lus of die kleiner is dan het aantal keren dat we de lus willen uitvoeren. Als je de teller op 1 zou initialiseren, zou de lus één keer te weinig uitgevoerd worden.

### Toestand van een lus (loop state)

Het gebruik van een variabele om een lus te besturen is een **patroon** dat bijna altijd terugkomt:

* Je houdt een **variabele** bij, een zogenoemde toestand of **state**  
  (in vele gevallen een teller)
* Deze toestand wordt geïnitialiseerd op een **beginwaarde** (stap **0**)  
  (bijvoorbeeld de teller wordt op 0 gezet bij de start)
* Deze toestand wordt **geëvalueerd** (stap **1**)  
  (bijvoorbeeld test of de teller kleiner is dan een maximum)
* Op basis van deze evaluatie wordt het **loop-blok uitgevoerd** (stap **3**)  
  (bijvoorbeeld teller is nog altijd kleiner dan maximum)
* Deze **state** wordt **bijgewerkt** in het blok
  (bijvoorbeeld 1 optellen bij de teller)
* We gaan terug naar stap 1
  (en stoppen ermee als de evaluatie False is)

~~~

            +------------+
            | CODE       |
            | BEFORE     +----(0) INITIALIZE STATE----------------------+
            +-+----------+                                              |
              |               "while-clause voert een                   |
              |                vergelijking uit op de toestand          |
              |                (bijvoorbeeld een teller) en beslist     |
            +-+----------+     of het blok wordt uitgevoerd"      +-----------+
            | WHILE-     |                                        |-----------|
            | CLAUSULE   +---+(1)EVALUATES STATE and DECIDES +----||  STATE  ||
            +-+----------+                                        |-----------|
              |                                                   +-----------+
              +                                                         |
             / \  (2)TRUE(1)    +------------+                          |
            +   +-+RUNS BLOCK +-> BLOCK MET  +---+(3)UPDATES STATE +----+
             \ /                | STATEMENTS |
              +( 2)FALSE(1)     +------------+
              |    STOPS
              |    THE LOOP
              |
            +-v----------+
            | REST OF    |
            | THE CODE   |
            +------------+

~~~

### while-lus met teller en toestand

Toestandsvariabelen wordt niet alleen gebruikt om te evalueren in de voorwaarde van een while-lus.
In het volgende voorbeeld berekenen we de macht van een getal (met basis en exponent).

> Nota:  
> Vanzelfsprekend is deze code niet zo nuttig, aangezien er al een operator bestaat in Python
> die een macht berekent. De bedoeling is om het gebruik van een lus te demonstreren.

De toestand die wordt geëvalueerd, is exponent_counter. De lus wordt uitgevoerd zolang deze kleiner is dan de ingevoerde exponent.

~~~python
base = int(input("Give base: "))
exponent = int(input("Give exponent: "))
result = 1
exponent_counter = 0

while exponent_counter < exponent:
    result = result * base
    exponent_counter = exponent_counter + 1

print(result)
~~~

We hebben hier twee toestandsvariabelen:

* exponent_counter die als teller dient voor de voorwaarde van de while-lus
* result die dient om bij elke uitvoering van het blok van de while-lus het resultaat van de berekening aan te passen

Voer dit programma uit:

~~~
$ python exponent.py
Give base: 2
Give exponent: 5
32
~~~

### while-lus met invoer

Een while-lus hoeft niet altijd een teller te gebruiken in zijn voorwaarde. Je kunt ook een stuk code herhalen zolang de invoer van de gebruiker aan een specifieke voorwaarde voldoet.

In onderstaand voorbeeld hernemen we de code die nakijkt of een getal c tussen a en b ligt:

~~~python
is_still_inbetween = True

while is_still_inbetween:
    a = int(input("Enter number a: "))
    b = int(input("Enter number b (should be bigger than a): "))
    c = int(input("Enter number c: "))

    is_still_inbetween = c >= a and c <= b
    if is_still_inbetween:
        print("c is in between a and b")
else:
    print("c is not in between a and b")
~~~

Als toestand wordt hier het resultaat van de vergelijking c >= a and c <= b genomen.  
Zolang c tussen a en b ligt, zal deze lus getallen blijven vragen.

### Niet eindigende lus

Wat iedereen wel eens als fout maakt in code, is een lus die niet stopt.  
Bijvoorbeeld als we in onderstaande correcte code:

~~~python
number_of_stars = int(input("Number of stars to print? "))
counter = 0

while counter < number_of_stars:
    print("*")
    counter = counter + 1
~~~

een **foutje** introduceren:

~~~python
number_of_stars = int(input("Number of stars to print? "))
counter = 0

while counter < number_of_stars:
    print("*")
counter = counter + 1
~~~

Zie je het **verschil**?
In de laatste regel is de **indentatie** namelijk **weggenomen**.
Daardoor gebeurt de aanpassing van de teller niet in het blok van de while-lus, maar erachter.
Maar omdat de teller nu niet in het blok aangepast wordt, blijft die altijd dezelfde waarde hebben als vóór de while-lus.

Het gevolg? De variabele counter blijft **voor eeuwig 0**, aan de voorwaarde van de while-lus blijft altijd voldaan en de lus wordt nooit beëindigd. Het programma blijft regel per regel sterretjes tonen op de opdrachtprompt. Dit noemen we een oneindige lus of **infinite loop**.

### Geen paniek, Ctrl+C helpt je

Als dit gebeurt, hoef je niet je console af te sluiten. Druk gewoon de toetsencombinatie **Ctrl+C** in om het Python-proces te stoppen.

### for-lus

Heel veel lussen zijn lussen met een teller: de bedoeling is dat een codeblok een exact aantal keren uitgevoerd wordt.
Python heeft daarom - naast de while-lus - een **tweede soort lus**, de **for-lus**, die dikwijls meer aangewezen is dan de while-lus.

De voorgaande code met **while-lus met teller**:

~~~python
number_of_stars = int(input("Number of stars to print? "))
counter = 0

while counter < number_of_stars:
    print("*")
    counter = counter + 1
~~~

vervangen we door een **equivalente for-lus**

~~~python
number_of_stars = int(input("Number of stars to print? "))

for counter in range(number_of_stars) :
    print("*")
~~~

De **for-lus** heeft twee componenten:

* Een **teller-variabele** volgend op het **keyword for** die je kan gebruiken in het blok van de for-lus
* Een **range-functie** volgend op het **keyword in** die bepaalt welke waardes de teller aanneemt

Net zoals bij een while-lus zal deze lus **het block-statement blijven uitvoeren**, maar dan **tot** alle waardes in de **gehele range afgelopen** zijn.

### Voordelen van een for-lus

Zowel het bijwerken van de toestand (**state update**) als het evalueren van de toestand (**state evaluatie**) wordt impliciet door de for-lus uitgevoerd, zodat we dit zelf niet meer moeten doen. Dit brengt ons twee voordelen:

* De code is **compacter**
* De code is ook **veiliger**  
  want de **state update** gebeurt **automatisch**  
  => minder risico op oneindige lus

### Tweede voorbeeld van een for-lus: macht van een getal

Als we dit toepassen op het voorbeeld van de while-lus om de macht van een getal te berekenen:

~~~python
base = int(input("Give base: "))
exponent = int(input("Give exponent: "))
result = 1
exponent_counter = 0

while exponent_counter < exponent:
    result = result * base
    exponent_counter = exponent_counter + 1

print(result)
~~~

kunnen we dit **herwerken tot een for-lus**:

~~~python
base = int(input("Give base: "))
exponent = int(input("Give exponent: "))
result = 1

for exponent_counter in range(exponent):
    result = result * base

print(result)
~~~

### range-functie

De range-functie genereert een *range* (bereik).  

Met range kun je tellen van **0** tot een **eindwaarde - 1**.  
range(5) zal tellen van 0 tot en met 4, en in totaal dus 5 getallen afgaan.

### Derde voorbeeld van een for-lus: range van ... tot ...

De range-functie kan ook worden aangeroepen met een start-waarde.  
Stel dat je wil tellen van een bepaald getal tot een ander getal, dan geef je twee argumenten door aan range:

~~~python
start = int(input("Count from: "))
from = int(input("Count to: "))
for counter in range(start, stop + 1):
    print(counter)
~~~

Het eerste argument is het getal waarvan je begint tellen, het tweede argument tot welke waarde.
Let wel, dit tweede argument is een eindwaarde die **telt tot**, **niet tot en met**.

~~~
$ python count_from_to.py
Count from: 5
Count to: 10
5
6
7
8
9
10
~~~

### Geneste lus

Binnen een for- of while-lus kan je ook **andere block statements** toevoegen, en dus ook weer een lus. Dat noemen we een **geneste lus**.
In onderstaand voorbeeld tonen we bijvoorbeeld de **maaltafels** met een **geneste for-lus**.

~~~python
for left_part in range(1, 10):
    for right_part in range(1, 10):
        print(str(left_part) + " * " + str(right_part) + " = " + str(left_part * right_part))
~~~

De **eerste lus** is wat we noemen de **outer loop** (buitenste lus). Deze zal het linker-gedeelte laten optellen van **1 tot en met 9**.  
De **tweede lus** noemen we de **inner loop** (binnenste lus). Deze zal telkens van 1 tot en met 9 tellen voor **iedere waarde** van de **outer loop** en zo een rechtergedeelte geven.

~~~
$ python multiplication_table.py
1 * 1 = 1
1 * 2 = 2
...
9 * 6 = 54
9 * 7 = 63
9 * 8 = 72
9 * 9 = 81
~~~

### String concatenation

De +-operator op twee strings voert **string concatenation** uit. Dit **plakt** letterlijk verschillende strings aan elkaar.

~~~python
...
        print(str(left_part) + " * " + str(right_part) + " = " + str(left_part * right_part))
~~~

Om deze string concatenation te kunnen uitvoeren, moet elke operand van de +-operator van het type string zijn.
Aangezien de waardes die range teruggeeft van het type int zijn, gebruiken we de str-functie om elk deel te converteren naar een string.

