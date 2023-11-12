### Set 

Een gelijkaardig datatype als een `list` is een `set` (verzameling). Dit is ook een collectie, maar met de volgende eigenschappen:

* Het is een **ongeordende verzameling** van elementen.
* Een element kan maar één keer in een set voorkomen.
* Er is géén index (want geen volgorde).
* Een set heeft een **grootte of dimensie**.
* Deze grootte of dimensie kan wijzigen over de duurtijd van een programma door elementen te verwijderen of toe te voegen.
* De elementen zelf kun je niet wijzigen.

### Een set aanmaken

Een set maak je zoals een lijst aan, maar dan met accolades `{}` in plaats van vierkante haakjes `[]`:

~~~python
>>> fruit = {"banaan", "peer", "kiwi", "appel"}
>>> fruit
{'appel', 'banaan', 'kiwi', 'peer'}
~~~

Je ziet hier al dat de volgorde van de elementen niet uitmaakt: Python toont ze in een andere volgorde dan de volgorde die je zelf opgaf.

Je kunt ook geen element uit de lijst opvragen met een index, want er is gewoon geen volgorde gedefinieerd:

~~~python
>>> fruit[0]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'set' object is not subscriptable
~~~

Als je eenzelfde element meer dan één keer in een set steekt, wordt dit maar één keer toegevoegd:

~~~python
>>> fruit = {"banaan", "peer", "peer", "kiwi", "banaan", "peer", "appel"}
>>> fruit
{'appel', 'banaan', 'kiwi', 'peer'}
~~~

Een lege set maak je overigens zo aan:

~~~python
>>> lege_verzameling = set()
>>> lege_verzameling
set()
~~~

### Geldige elementen in een set 

De elementen van een set hoeven niet hetzelfde type te hebben (hoewel dat vaak het geval zal zijn):

~~~python
>>> dingen = {"appel", 17, 2.5}
~~~

Maar je kunt geen lijst in een set steken:

~~~python
>>> dingen = {"appel", 17, 2.5, ["Koen", "Bart"]}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
~~~

De elementen in een set moeten immers onveranderlijk zijn. Een lijst kun je veranderen (je kunt bijvoorbeeld elementen toevoegen en verwijderen), dus je kunt geen lijst in een set opslaan.

### Dimensie van een set

De functie `len()` waarmee we de grootte van een lijst kunnen opvragen, werkt ook voor een set:

~~~python
>>> fruit = {"banaan", "peer", "peer", "kiwi", "banaan", "peer", "appel"}
>>> fruit
{'appel', 'banaan', 'kiwi', 'peer'}
>>> len(fruit)
4
>>> lege_verzameling = set()
>>> len(lege_verzameling)
0
~~~

### Sets vergelijken

Sets zijn identiek als ze exact dezelfde elementen bevatten, zelfs als je ze in een andere volgorde opgeeft of sommige elementen meermaals opgeeft:

~~~python
>>> {"banaan", "peer", "kiwi", "appel"} == {"appel", "banaan", "kiwi", "peer"}
True
>>> {"banaan", "peer", "peer", "kiwi", "banaan", "peer", "appel"} == {"appel", "banaan", "kiwi", "peer"}
True
>>> {"banaan", "peer", "kiwi", "appel"} == {"banaan", "peer", "kiwi", "mango"}
False
>>> {"banaan", "peer", "kiwi", "appel"} == {"banaan", "peer", "kiwi"}
False
~~~

Je kunt ook controleren of een set een (strikte) deelverzameling is van een andere:

~~~python
>>> {"banaan", "peer", "kiwi"} < {"banaan", "peer", "kiwi", "appel"}
True
>>> {"banaan", "peer", "mango"} < {"banaan", "peer", "kiwi", "appel"}
False
~~~

### Bewerkingen op sets

Op sets zijn de gekende bewerkingen uit de verzamelingenleer toe te passen:

~~~python
>>> a = {"banaan", "peer", "kiwi", "appel"}
>>> b = {"appel", "peer", "mango", "druif"}
>>> a - b
{'banaan', 'kiwi'}
>>> b - a
{'mango', 'druif'}
>>> a | b
{'appel', 'kiwi', 'mango', 'druif', 'banaan', 'peer'}
>>> a & b
{'appel', 'peer'}
>>> a ^ b
{'kiwi', 'mango', 'druif', 'banaan'}
~~~

Samengevat:

* De operator `-` geeft de set terug van de elementen in a maar niet in b. 
* De operator `|` geeft de set terug van de elementen die in a of b of beide zitten (de unie). 
* De operator `&` geeft de set terug van de elementen die in zowel a als b zitten (de doorsnede). 
* De operator `^` geeft de set terug van de elementen die in a of b maar niet in beide zitten. 

### Controleren of element in een set zit
Met het keyword `in` controleer je of een element in een set zit:

~~~python
>>> fruit = {"banaan", "peer", "kiwi", "appel"}
>>> "kiwi" in fruit
True
>>> "mango" in fruit
False
~~~

### Een set doorlopen
Je kunt met een `for`-lus alle elementen in een set doorlopen:

~~~python
>>> fruit = {"banaan", "peer", "kiwi", "appel"}
>>> for stuk in fruit:
...     print(stuk, "is lekker")
... 
appel is lekker
banaan is lekker
kiwi is lekker
peer is lekker
~~~

Merk op: je hebt hier geen invloed op de volgorde waarin de elementen uit de set doorlopen worden.

### Een set wijzigen
Aan een set kun je nadat je die aangemaakt hebt nog elementen toevoegen of je kunt er elementen uit verwijderen:

~~~python
>>> fruit = {"banaan", "peer", "kiwi", "appel"}
>>> fruit.add("mango")
>>> fruit
{'appel', 'kiwi', 'mango', 'banaan', 'peer'}
>>> fruit.add("mango")
>>> fruit
{'appel', 'kiwi', 'mango', 'banaan', 'peer'}
>>> fruit.remove("peer")
>>> fruit
{'appel', 'kiwi', 'mango', 'banaan'}
>>> fruit.remove("peer")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'peer'
~~~

Je ziet hier dat een element toevoegen dat al in de set zit geen probleem is: dit wordt gewoon genegeerd. Maar als je een element wilt verwijderen dat niet in de set zit, krijg je een foutmelding.

### Een variabele is een naam

Net zoals bij een lijst moeten we opletten als we een set in een assignment aan een variabele toekennen:

~~~python
>>> fruit1 = {"banaan", "peer", "kiwi", "appel"}
>>> fruit2 = fruit1
>>> fruit1.remove("appel")
>>> fruit1
{'banaan', 'kiwi', 'peer'}
>>> fruit2
{'banaan', 'kiwi', 'peer'}
~~~

We maken hier een set `fruit1` aan, kennen `fruit1` aan `fruit2` toe en verwijderen een element uit `fruit1`. Daarna blijkt dit element zowel uit `fruit1` als `fruit2` verwijderd te zijn.

Hoe kan dat?

De verklaring: als Python een assignment doet met een set, steekt hij niet de waarde van die set in het bijbehorende geheugen, maar een verwijzing naar een geheugenplaats waar de set staat. Als je dan `fruit2 = fruit1` uitvoert, kopieer je dat geheugenadres van set `fruit1` naar `fruit2`, en daarna verwijzen `fruit1` en `fruit2` dus naar dezelfde set, het zijn gewoon aliassen.

Hoe zou je dan wel de ene set kunnen veranderen en niet de andere? Dan maak je eerst een kopie:

~~~python
>>> fruit1 = {"banaan", "peer", "kiwi", "appel"}
>>> fruit2 = fruit1.copy()
>>> fruit1.remove("appel")
>>> fruit1
{'banaan', 'kiwi', 'peer'}
>>> fruit2
{'appel', 'banaan', 'kiwi', 'peer'}
~~~

### Wanneer een set gebruiken en wanneer een lijst?

* Als de volgorde van elementen niet uitmaakt, gebruik dan een set.
* Als het niet uitmaakt hoeveel keer een element in de collectie zit, gebruik dan een set.

In de andere gevallen gebruik je een lijst.

Meer informatie over de mogelijkheden met een set vraag je in de REPL op met `help(set)`.

### Set comprehension

Net zoals je lijsten kunt aanmaken met list comprehension, kun je sets aanmaken met **set comprehension**. En ook dit is een krachtige techniek waarmee je in compacte code heel wat kunt doen. Bijvoorbeeld, hoe zou je een programma schrijven dat alle gehele getallen tussen 1 en 100 toont die geschreven kunnen worden als de som van twee kwadraten? Met set comprehension kan dat in één regel code:

~~~python
print(sorted({x**2+y**2 for x in range(1, 10) for y in range(1, 10) if x**2+y**2 <= 100}))
~~~

Het resultaat:

~~~
[2, 5, 8, 10, 13, 17, 18, 20, 25, 26, 29, 32, 34, 37, 40, 41, 45, 50, 52, 53, 58, 61, 65, 68, 72, 73, 74, 80, 82, 85, 89, 90, 97, 98, 100]
~~~

Hoe werkt deze list comprehension? Ze gaat alle getallen x van 1 tot en met 9 af en alle getallen y van 1 tot en met 9. Ze kwadrateert die elk en telt die op, en controleert dan of het resultaat kleiner dan of gelijk aan 100 is. Indien ja, dan wordt het aan de verzameling toegevoegd. Uiteindelijk wordt het resultaat gesorteerd met de funtie `sorted`.

Je ziet hier dat een set comprehension (maar ook een list comprehension genest kan zijn, door meerdere keren `for` in te voeren. Bovendien kun je met `if` beperken welke elementen er door de comprehension geselecteerd worden. Het is hierdoor dat onze voorbeeldopdracht zo compact opgelost kan worden.
