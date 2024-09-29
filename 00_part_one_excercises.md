## Extra oefeningen

Dit zijn nog wat extra oefeningen van wisselende moeilijkheidsgraad voor deel 1 die je kunt uitproberen om de leerstof toe te passen.

### Oefening: Oppervlakte tuin

#### Deel A

Schrijf een programma dat de oppervlakte van een tuin berekent op basis van de totale grondoppervlakte en de oppervlakte van het huis dat erop staat.

Je geeft als invoer de breedte van de tuin en de breedte van het huis in. Ga ervan uit dat de grond en het huis vierkant zijn en dus dezelfde lengte als breedte hebben. Bijvoorbeeld:

~~~
             15 m
        +--------------+
        |     8 m      |
        |   +-----+    |
        |   |HUIS |8 m |15 m
        |   |     |    |
        |   +-----+    |
        |    TUIN      |
        +--------------+

Oppervlakte grond  = 15 m * 15 m = 225 m^2
Oppervlakte huis   = 10 m *  8 m =  64 m^2
                                  - -------
Oppervlakte tuin                 = 161 m^2
~~~

Kijk na of de breedte van de tuin groter is dan die van het huis.
Als dat niet zo is, beëindig dan het programma.

#### Deel B

Doe hetzelfde maar zonder de veronderstelling dat de grond en het huis een vierkant zijn. De lengte en breedte kunnen dus verschillen. Bijvoorbeeld:

~~~
                20 m
        +-----------------+
        |       10 m      |
        |    +------+     |
        |    | HUIS | 8 m |  15 m
        |    |      |     |
        |    +------+     |
        |      TUIN       |
        +-----------------+

Oppervlakte grond  = 20 m * 15 m = 300 m^2
Oppervlakte huis   = 10 m *  8 m =  80 m^2
                                  - -------
Oppervlakte tuin                 = 220 m^2
~~~

### Oefening: Minuten

Schrijf een programma dat een aantal minuten opvraagt en omzet in het aantal dagen, uren en minuten. Bijvoorbeeld:

~~~
Aantal minuten? 128000
Dit is gelijk aan 88 dagen, 21 uren en 20 minuten
~~~

### Oefening: Willekeurige getallen

#### Basisoefening

Maak een programma dat de gebruiker laat raden naar een getal.
Geef bij een foute gok aan of het getal te laag of te hoog is.
Het programma blijft vragen tot de gebruiker een correcte oplossing geeft.

Om een willekeurig getal te berekenen, gebruik je de Python-module `random`.
Dit kan je doen met onderstaande code:

~~~python
import random

print(random.randint(1, 100))  # Toont een willekeurig getal van 1 tot 100
~~~

#### Uitbreiding

Beperk het aantal pogingen dat je kan raden.

### Cirkel

Schrijf code die de oppervlakte van een cirkel berekent, gebruikmakend van de variabele straal en de constante `math.pi` die je uit de module `math` importeert. 
Voor het geval je het vergeten bent, de formule voor de oppervlakte van een cirkel is straal maal straal maal π. Toon de uitkomst als volgt:

~~~
De oppervlakte van een cirkel met straal ... is ...
~~~

Zet deze code in een functie.

### Oefening: Getallen opvragen

Schrijf een programma dat de gebruiker om 10 getallen vraagt, en dan de
grootste, de kleinste, en het aantal dat deelbaar is door 3 toont.

### Oefening: Som van twee kwadraten

Schrijf een programma dat alle gehele getallen tussen 1 en 100 toont
die geschreven kunnen worden als de som van twee kwadraten. De uitvoer is een lijst van
regels van de vorm `z = x**2 + y**2` , bijvoorbeeld, `58 = 3**2 + 7**2`.  

### Oefening: Lus

Schrijf een programma dat telt tot een getal x.
Vraag dit getal op aan het begin van het programma en controleer of het groter is dan 0.

### Oefening: Minimum, maximum, totaal en gemiddelde

Schrijf een programma dat 

* in een lus getallen opvraagt
* stopt wanneer je een negatief getal ingeeft
* een aantal statistieken bijhoudt over (de positieve) getallen

Zie onderstaand voorbeeld van uitvoering.

~~~
$ python number.py
Enter a number (negative stops the loop): 3
Enter a number (negative stops the loop): 8
Enter a number (negative stops the loop): 1
Enter a number (negative stops the loop): 2
Enter a number (negative stops the loop): 1
Enter a number (negative stops the loop): -5
Loop ended due to number -5
Max number is 8
Min number is 1
Total is 15
Average is 3
~~~

Toon bij het beëindigen:

* het grootste getal
* het kleinste getal
* het totaal
* het gemiddelde

### Oefening: Faculteit van een getal

De **faculteit** (in het Engels *factorial*) van een getal **n**, genoteerd als **n!**, is het **product van de getallen 1 tot en met n**.   
Bijvoorbeeld:

* Voor het getal 0 is dit 1 = 1
* Voor het getal 1 is dit 1 = 1
* Voor het getal 2 is dit 2 = 1 * 2
* Voor het getal 3 is dit 6 = 1 * 2 * 3
* Voor het getal 4 is dit 24 = 1 * 2 * 3 * 4
* Voor het getal 5 is dit 120 = 1 * 2 * 3 * 4 * 5
* ...

Zie ook <https://nl.wikipedia.org/wiki/Faculteit_(wiskunde)> voor meer info.  

#### Deel 1: factorial-functie

Maak een functie die een faculteit berekent voor een getal en test dit uit.

~~~python
def factorial(number):
    # TODO => code...

print(factorial(5))  # 120
print(factorial(6))  # 720
~~~

#### Deel 2: Vraag invoer

Schrijf nu een programma dat de gebruiker om een getal vraagt en dan van dit getal de faculteit berekent met de functie uit deel 1:

~~~
$ python factorial.py
Give a number to calculate the factorial: 5
5! = 120
~~~

#### Deel 3: Blijf invoer vragen

Schrijf nu een programma dat de gebruiker om een getal blijft vragen tot die een negatief getal invoert. Van elk niet-negatief getal wordt de faculteit berekend:

~~~
$ python factorial_loop.py
Give a number to calculate the factorial: 5
5! = 120
Give a number to calculate the factorial: 3
3! = 6
Give a number to calculate the factorial: 1
1! = 1
Give a number to calculate the factorial: -1
End of program
$
~~~

### Oefening: BTW berekenen

#### Deel A

Schrijf een programma dat BTW op een bedrag uitrekent.  
Het programma vraagt het nettobedrag op.  
Ga uit van een percentage van 6% BTW.

Voorbeeld van gebruik:

~~~
Geef het bedrag van het goed in: 50
De BTW bedraagt 3 euro.
~~~

#### Deel B

Laat de gebruiker kiezen tot welke BTW-categorie het goed behoort => 6%, 12% of 21%

Voorbeeld van gebruik:

~~~
Geef het bedrag van het goed in: 50
Geef de BTW-categorie in (type a voor 6%, b voor 12% en c voor 21%): b
De BTW bedraagt 6 euro.
~~~

#### Deel C

Blijf deze vragen stellen tot de gebruiker 0 ingeeft.  
Toon daarna de totale BTW over alle goederen.

Voorbeeld van gebruik:

~~~
Geef het bedrag van het goed in (typ 0 om berekening te stoppen): 50
Geef de BTW-categorie in (type a voor 6%, b voor 12% en c voor 21%): b
Geef het bedrag van het goed in (typ 0 om berekening te stoppen): 100
Geef de BTW-categorie in (type a voor 6%, b voor 12% en c voor 21%): c
Geef het bedrag van het goed in (typ 0 om berekening te stoppen): 0
De BTW bedraagt 27 euro.
~~~

### Oefening: Priemgetal berekenen

#### Deel 1: functie

Schrijf een functie die nakijkt of een getal een priemgetal is. Een priemgetal is een natuurlijk getal groter dan 1 dat slechts twee natuurlijke getallen als deler heeft, namelijk 1 en zichzelf.

~~~python
print(is_prime(13)) # True
print(is_prime(4))  # False
~~~

De eenvoudigste (maar niet de efficiëntste) manier is een lus te schrijven die loopt van 2 tot en met het getal en kijkt of het getal deelbaar is (maak hiervoor gebruik van de rest-operator `%`).

#### Deel 2: priemgetallen

Schrijf een lus die alle priemgetallen toont lager dan een getal dat je invoert (gebruikmakende van de voorgaande functie).

~~~
$ python prime_numbers.py
Give a number: 20
2
3
7
11
13
17
19
~~~

### Oefening: Teken een vierkant

#### Deel 1

Maak een functie die een rechthoek op de console toont:

~~~python
def print_rectangle(rows, columns)
    # TODO write double loop....

print_rectangle(6, 5)
~~~


~~~
$ python print_rectangle.py
*****
*****
*****
*****
*****
***** 
~~~

#### Deel 2

Maak een functie die een rechthoek op de console toont maar deze niet vult:

~~~
$ python print_rectangle.py
*****
*   *
*   *
*   *
*   *
***** 
~~~

### Oefening: Teken een driehoek

#### Deel 1

Maak een functie die een driehoek op de console toont:

~~~python
def print_triangle(rows)
    # TODO write double loop....

print_triangle(6)
~~~


~~~
$ python print_triangle.py
     *
    ***
   *****
  *******
 *********
***********
$
~~~

#### Deel 2

Maak een variant die de driehoek niet vult:

~~~
$ python print_triangle.py
     *
    * *
   *   *
  *     *
 *       *
***********
$
~~~

#### Deel 3 (uitdagender...)

~~~python
def print_multiple_triangles(rows, triangles)
    # TODO This is where the magic happens...

print_multiple_triangles(6, 3)
~~~


~~~
$ python print_multiple_triangles.py
     *           *            *      
    * *         * *          * *
   *   *       *   *        *   *
  *     *     *     *      *     *
 *       *   *       *    *       *
*********** ***********  ***********
$
~~~


### Oefening: Grootste gemene deler

Maak een functie die de grootste gemene deler (*greatest common divider*) van twee getallen berekent. Dit is het grootste gehele getal waardoor beide getallen gedeeld kunnen worden.

~~~python
def gcd(a, b):
    # TODO

print(gcd(10, 15))  # 5
print(gcd(9, 15))   # 3
print(gcd(13, 18))  # 1
~~~


### Oefening: Rekenmachine

Simuleer een rekenmachine in Python:

~~~
(type number, command or x to close)
       M+    M-    MR    C
       7     8     9     /
       4     5     6     x
       1     2     3     -
       0          (-)    +
       x     =     %     ^

> 37 + 5 = 
42
~~~


### Oefening: Blackjack

#### Opdracht 1

Maak een programma om op de opdrachtregel het kaartspel Blackjack te spelen:

De regels:

* Je trekt kaarten, die elke een aantal punten voorstellen.
* De volgende puntenregels tellen:
  * Koning, Vrouw, Boer (de 'plaatjes') zijn 10 punten waard.
  * De rest van de kaarten zijn zoveel waard als het cijfer dat op de kaart staat.
* Als je 21 hebt, win je.
* Als je er boven zit, verlies je.

Om een willekeurige kaart te trekken, kan je de volgende code gebruiken:

~~~python
import random

kaart_waarde = random.randint(1, 13)
~~~


Als je start, dient het volgende te verschijnen:

~~~
Welkom: 
* Je trekt kaarten, die elke een aantal punten voorstellen.
* De volgende puntenregels tellen:
  * Koning, Vrouw, Boer (de 'plaatjes') zijn 10 punten waard.
  * De rest van de kaarten zijn zoveel waard als het cijfer dat op de kaart staat.
* Als je 21 hebt, win je.
* Als je er boven zit, verlies je.

Type N voor een nieuw spel, X om af te sluiten.

>

~~~

Vervolgens type je N in om te starten. Het spel start en een eerste kaart is getrokken:

~~~
> N
Je hebt 5 getrokken
Je huidige totaal is 5

N: Nieuw spel
X: Afsluiten
K: Trek kaart

> K
~~~

Als je K typt, krijg je een volgende kaart:

~~~
Je hebt 7 getrokken
Je huidige totaal is 12

N: Nieuw spel
X: Afsluiten
K: Trek kaart

> K
~~~

Indien de volgende kaart voldoende is om 21 te halen, heb je gewonnen:

~~~
Je hebt 9 getrokken
Je hebt 21 punten en hebt gewonnen!!!

N: Nieuw spel
X: Afsluiten
K: Trek kaart

> K
~~~

#### Opdracht 2

Alleen spelen is wat saai. Zorg nu dat je met dezelfde regels met twee kan spelen.

~~~
Welkom: 
* Je trekt kaarten, die elke een aantal punten voorstellen.
* De volgende puntenregels tellen:
  * Koning, Vrouw, Boer (de 'plaatjes') zijn 10 punten waard.
  * De rest van de kaarten zijn zoveel waard als het cijfer dat op de kaart staat.
* Als je 21 hebt, win je.
* Als je er boven zit, verlies je.

Je speelt met twee spelers A en B.

Type N voor een nieuwe spel, X om af te sluiten.

> N
~~~

Speler A start en krijgt automatisch een kaart.

~~~
Speler A:
Je hebt 6 getrokken
Je huidige totaal is 6
Speler B:
Je huidige totaal is 0

N: Nieuw spel
X: Afsluiten
K: Trek kaart

Speler B> K
~~~

Vervolgens speler B:

~~~
Speler A:
Je huidige totaal is 6
Speler B:
Je hebt een Heer getrokken
Je huidige totaal is 13

N: Nieuw spel
X: Afsluiten
K: Trek kaart

Speler A> K
~~~

En terug speler A:

~~~
Speler A:
Je hebt een Dame getrokken
Je huidige totaal is 18
Speler B:
Je huidige totaal is 13

N: Nieuw spel
X: Afsluiten
K: Trek kaart

Speler B> K
~~~


Uiteindelijk trekt speler B nog een kaart waarmee het totaal
op 21 komt en is gewonnen.

~~~
Speler A:
Je huidige totaal is 18
Speler B:
Je hebt een 8 getrokken
Je huidige totaal is 21

Speler B is gewonnen!!!

N: Nieuw spel
X: Afsluiten

> 
~~~

Of speler B trekt een te hoge kaart en verliest:

~~~
Speler A:
Je huidige totaal is 18
Speler B:
Je hebt een 9 getrokken
Je huidige totaal is 22

Speler B is gewonnen!!!

N: Nieuw spel
X: Afsluiten

> 
~~~

#### Opdracht 3

Zorg er nu voor dat een speler de optie krijgt om geen kaarten meer te trekken (zolang er geen 21 is):

~~~
Speler A:
Je hebt een Dame getrokken
Je huidige totaal is 18
Speler B:
Je huidige totaal is 19

N: Nieuw spel
X: Afsluiten
K: Trek kaart
S: Stop met kaarten trekken

Speler B> S
~~~

Vervolgens is alleen speler A nog aan de beurt:

~~~
Speler A:
Je hebt een Dame getrokken
Je huidige totaal is 18
Speler B:
Je huidige totaal is 19 en wenst geen kaarten meer

N: Nieuw spel
X: Afsluiten
K: Trek kaart
S: Stop met kaarten trekken

Speler A> 
~~~

Vanaf het moment dat de andere speler er boven zit, heeft deze gewonnen:

~~~
Speler A:
Je hebt een 2 getrokken
Je huidige totaal is 20
Speler B:
Je huidige totaal is 19 en wenst geen kaarten meer

Speler A is gewonnen!!!

N: Nieuw spel
X: Afsluiten
~~~

#### Opdracht 4

Hou er nu rekening mee dat een aas zowel 1 als 11 waard kan zijn.
De regels zijn nu als volgt:

* Je trekt kaarten, die elke een aantal punten voorstellen.
* De volgende puntenregels tellen:
  * Een Aas is 1 of 11 waard
  * Koning, Vrouw, Boer (de 'plaatjes') zijn 10 punten waard.
  * De rest van de kaarten zijn zoveel waard als het cijfer dat op de kaart staat.
* Als je 21 hebt, win je.
* Als je er boven zit, verlies je.

#### Opdracht 5

Zorg ervoor dat je niet twee keer dezelfde kaart uitdeelt.

##### Opdracht 6

Zorg ervoor dat je meer dan twee spelers kunt laten meedoen.
