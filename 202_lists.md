## Collecties 

Tot nog toe hebben we met enkelvoudige types gewerkt (ook wel primitieve types genoemd). Variabelen van dit type bevatten één waarde.

In dit tweede deel van de cursus bekijken we datatypes die meerdere waardes kunnen bevatten:

* Collecties: lists, sets, dictionaries, tuples
* Abstracte datatypes: klassen

Collecties zijn generieke datatypes waarin je meer dan één waarde kunt plaatsen, een beetje zoals een container die verschillende goederen kan bevatten.


### List

Het eerste en meest eenvoudige type is de `list` (lijst).

Praktisch uitgedrukt heeft een lijst in Python de volgende eigenschappen:

* Het is een **geordende verzameling** of collectie van elementen.
* Een element kan meermaals in een lijst voorkomen.
* Een lijst heeft een **grootte of dimensie** (die we voor de gemakkelijkheid aanduiden als n).
* Deze grootte of dimensie kan wijzigen over de duurtijd van een programma: je kunt elementen aan een lijst toevoegen of er elementen uit verwijderen.
* Elk **element** van een lijst kan **gelezen of gewijzigd** worden via een **index**.
* Deze index **start** bij **0** (eerste element) en **eindigt** bij **n-1** (laatste element).

### Voorbeeld van een lijst

Een lijst kun je aanmaken als een gewone variabele met onderstaande syntaxis.
Dit doe je door een  *list literal* aan te maken:

~~~python
x = [1.0, 2.0, 3.0]
y = ["a", "list", "of", "strings"]
z = ["a", 1, "mixed", 3.0, "list", "with", 1, "float"]
~~~

Samengevat:

* Je creëert een variabele.
* Daaraan ken je met een assignment een lijst van waardes toe:
    * omsloten door vierkante haakjes `[]`
    * de waardes zijn van elkaar gescheiden door een komma

### Lijst van een range

Als je een lijst wilt maken van de getallen 0 tot en met 9, kan dat als volgt:

~~~
>>> x = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> x
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
~~~

Maar dat is nogal veel typewerk. Kan het niet eenvoudiger? We hadden toch de functie `range()` gezien bij de `for`-lus?

~~~
>>> x = range(10)
>>> x
range(0, 10)
~~~

Helaas maakt dit geen lijst aan. Maar we kunnen hier wel een lijst van maken:

~~~
>>> x = list(range(10))
>>> x
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
~~~

### Waardes van een lijst

Een lijst kan alle soorten data-elementen bevatten.  
In de meeste gevallen ga je hetzelfde type gebruiken, maar dit is niet verplicht (zie `z` in het voorbeeld hierboven).

Je kunt zelfs een lijst van lijsten maken...

~~~python
x = [[1.0, 2.0, 3.0], ["a", 1, "mixed", 3.0, "list"]]
~~~

Of wat duidelijker weergegeven:

~~~python
x = [[1.0, 2.0, 3.0],
     ["a", 1, "mixed", 3.0, "list"]]
~~~

### Dimensie van een lijst

Een eerste actie die we kunnen uitvoeren is de grootte of dimensie van een lijst opvragen.  
Daarvoor bestaat een generieke functie `len()`, die niet alleen voor lijsten maar voor alle types containers werkt.

~~~
>>> x = [1.0, 2.0, 3.0]
>>> len(x)
3
>>> y = ["a", "list", "of", "strings"]
>>> len(y)
4
>>> z = [[1.0, 2.0, 3.0], ["a", 1, "mixed", 3.0, "list"]]
>>> len(z)
2
~~~

Zie je waarom die laatste lijst lengte 2 heeft?

### Lege lijst

Ook een lege lijst heeft een grootte...

~~~
>>> u = []
>>> len(u)
0
~~~

Dat is dus gewoon een lijst met lengte 0.

### Data uit een lijst halen (index)

Om met een lijst te kunnen werken, moet je er data kunnen uithalen. Om naar specifieke elementen in een lijst te kunnen verwijzen, werken we met het concept van **indexering**.

Met de **index** bedoelen we de **positie** van het **element binnen** deze **lijst**.  

~~~
>>> x = [1.0, 2.0, 3.0]
>>> x[0]
1.0
>>> y = ["a", "list", "of", "strings"]
>>> y[1]
'list'
~~~

Door de **naam** van de **list-variabele** te combineren met een **index** die je tussen vierkante haken plaatst, kun je de waarde uit deze positie ophalen en gebruiken in je code.

### Alles begint bij 0

... en **eindigt bij n-1**

~~~
>>> x = [2, 3, 5]
>>> x[0]
2
>>> x[1]
3
>>> x[2]
5
>>> len(x)
3
~~~

De **index** van het **eerste element** uit een lijst is **niet 1** (zou logisch kunnen zijn) maar 0.

~~~
 """            -----------------
 We starten     | 0 | 1 | 2 | 3 | """
 bij 0          -----------------   En eindigen bij 2
       """      | 2 | 3 | 5 | / |   Index 3 bestaat niet
                -----------------                      """
~~~

De index van het **laatste element** is n-1, of in dit geval 2 (lengte 3 - 1).


### Indexering vs range

Stel dat je toch een index adresseert > n -1, dan zal Python een foutmelding tonen:

~~~
>>> x = [2, 3, 5]
>>> len(x)
3
>>> x[3]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
~~~

### Data in een lijst wijzigen (index) 

Een element van een lijst wijzigen verloopt op een gelijkaardige manier.  
Je gebruikt opnieuw hetzelfde indexeringsmechanisme, maar past deze toe in een **assignment statement**:

~~~
>>> x = [2, 3, 5]
>>> x[1]
3
>>> x[1] = 7
>>> x[1]
7
>>> x
[2, 7, 5]
~~~

### Negatieve indexen

In Python kun je ook negatieve indexen gebruiken. Hierbij draai je de indexering gewoon om...  
Het idee is dat je de absolute waarde van de negatieve index van de dimensie of lengte aftrekt:

~~~
>>> x = list(range(10))
>>> x
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> x[-1]
9
>>> x[-3]
7
~~~

Het laatste element van `x` is dus `x[-1]`, het derde laatste element is `x[-3]`, enzovoort.

### Lijsten doorlopen (naïeve manier)

Aangezien we:

* de lengte van een lijst kunnen opvragen
* de individuele elementen kunnen opvragen

kunnen we ook een lijst doorlopen met het volgende programma:

~~~python
x = [2, 3, 5, 7, 11]
i = 0
while i < len(x):
    print(x[i])
    i = i + 1
~~~

Deze code werkt, maar is niet echt "Pythonic":

~~~bash
$ python doorlopen.py
2
3
5
7
11
~~~

### Lijsten doorlopen (Pythons manier)

Je kunt namelijk ook de for-lus gebruiken, die we eerder gebruikten in combinatie met range:

~~~python
x = [2, 3, 5, 7, 11]
for n in x:
    print(n)
~~~

We hoeven dus geen index meer te initialiseren, te gebruiken en op te tellen, maar kunnen `x` hier gewoon als een bereik gebruiken.

#### Selecteren met "slices"

Je kunt ook een stuk uit een lijst nemen. Dit noemt men *slicing*:

~~~python
x = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
for n in x[2:4]:
    print(n)
~~~

De syntaxis is dus: lijst[startindex:stopindex]. Let wel op: de stopindex is niet inclusief!

~~~bash
$ python test.python
2
3
~~~

Een andere manier om te onthouden wat de indexen van slicing juist betekenen, is dat je beide indexen interpreteert als de positie van de komma's waartussen het stuk zich bevindt dat je ermee selecteert:

~~~
x = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
#        2\   /4
for n in x[2:4]:
    print(n)
~~~

Met de slice `2:4` selecteer je dus alle elementen tussen de tweede en de vierde komma.

Zowel de startindex als stopindex zijn overigens optioneel:

~~~python
>>> x = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> x[2:]
[2, 3, 4, 5, 6, 7, 8, 9]
>>> x[:4]
[0, 1, 2, 3]
>>> x[:]
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
~~~

En je kunt met een slice ook een deel van je lijst veranderen:

~~~python
>>> x = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> x[2:4] = [99]
>>> x
[0, 1, 99, 4, 5, 6, 7, 8, 9]
~~~

Let op: als je een waarde toekent aan een slice, moet dit ook een lijst zijn. Dit kan dus niet:

~~~python
>>> x = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> x[2:4] = 99
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only assign an iterable
~~~

Want het getal `99` is iets anders dan de lijst `[99]`.

#### Slicing met negatieve indexen

Je kunt hier ook negatieve indexen gebruiken:

~~~python
x = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
for n in x[2:-2]:
    print(n)
~~~

Dat levert dus een slice op met de waardes van positie 2 tot en met 7 (omdat 10 - 2 = 8 de stopindex is en die niet inclusief geldt):

~~~bash
$ python test.python
2
3
4
5
6
7
~~~

Ook hier kun je weer de interpretatie met de komma's gebruiken: je selecteert de waardes tussen de komma's op positie 2 vanaf het begin geteld en positie 2 vanaf het einde teruggeteld:

~~~python
x = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
#        2\    /--------- -2
for n in x[2:-2]:
    print(n)
~~~

### Een variabele is een naam

We moeten even teruggaan naar het concept van een variabele. Zoals we eerder zagen is een variabele:

* een stuk **geheugen** dat je kunt hergebruiken
* dat een **waarde** kan bevatten
* waaraan een **naam** is gelinkt
  (of ook wel **symbool** genoemd)

Stel dat je nu een variabele x de waarde 5 geeft met een assignment, en daarna de variabele y de waarde van de variabele x geeft met een assignment, dan krijg je de volgende waardes:

~~~python
>>> x = 5
>>> y = x
>>> x
5
>>> y
5
~~~

Als je nu de waarde van x verandert, blijft y de originele waarde 5 behouden:

~~~python
>>> x = x + 1
>>> x
6
>>> y
5
~~~

Bij het assignment `y = x` werd immers de inhoud van het geheugen van variabele x gekopieerd naar het geheugen van variabele y. Veranderingen aan x daarna hebben geen impact meer op y, want dat is een ander stukje geheugen.

Hetzelfde geldt voor strings:

~~~python
>>> x = "foobar"
>>> y = x
>>> x
'foobar'
>>> y
'foobar'
>>> x = x + "Z"
>>> x
'foobarZ'
>>> y
'foobar'
~~~

Wat als we nu hetzelfde doen met een lijst? We creëren een lijst x, kennen x aan y toe, veranderen x, en kijken naar de waardes van x en y:

~~~python
>>> x = [0, 1, 2, 3, 4]
>>> y = x
>>> x
[0, 1, 2, 3, 4]
>>> y
[0, 1, 2, 3, 4]
>>> x[0] = 99
>>> x
[99, 1, 2, 3, 4]
>>> y
[99, 1, 2, 3, 4]
~~~

Deze keer is de waarde van y wel mee veranderd met die van x! Hoe kan dat?

De verklaring: als Python een assignment doet met een lijst, steekt hij niet de waarde van die lijst in het bijbehorende geheugen, maar een verwijzing naar een geheugenplaats waar de lijst staat. Als je dan `y = x` uitvoert, kopieer je dat geheugenadres van lijst x naar y, en daarna verwijzen x en y dus naar dezelfde lijst, het zijn gewoon *aliassen*.

Hoe zou je dan wel de ene lijst kunnen veranderen en niet de andere? Dan moet je de lijst x naar de lijst y kopiëren, zodat er een nieuwe lijst wordt aangemaakt, op een nieuw geheugenadres, met dezelfde elementen:

~~~python
>>> x = [0, 1, 2, 3, 4]
>>> y = x.copy()
>>> x
[0, 1, 2, 3, 4]
>>> y
[0, 1, 2, 3, 4]
>>> x[0] = 99
>>> x
[99, 1, 2, 3, 4]
>>> y
[0, 1, 2, 3, 4]
~~~

Met `x.copy()` maak je dus een kopie van de lijst `x` aan.

### Bewerken van een lijst 

Een lijst kun je nog op allerlei manieren verwerken nadat je die gecreëerd hebt:

* `append()`: element toevoegen aan einde van de lijst
* `extend()`: zelfde maar dan ineens een andere lijst of iterable toevoegen 
* `count()`: aantal elementen met een specifieke waarde opvragen
* `index()`:	index van het eerste element voor een specifieke waarde opvragen
* `insert()`: element toevoegen op een specifieke positie
* `pop()`: element op een index verwijderen
* `remove()`: eerste item met een specifieke waarde verwijderen
* `reverse()`: volgorde van de elementen omdraaien
* `sort()`: de lijst sorteren

Zie volgende sequentie in de REPL voor voorbeelden hiervan:

~~~python
>>> car_park = ["Audi", "Volkswagen", "Skoda"]
>>> car_park.append("Porsche")
>>> car_park
['Audi', 'Volkswagen', 'Skoda', 'Porsche']
>>> car_park.extend(["SEAT", "Cupra", "Porsche"])
>>> car_park
['Audi', 'Volkswagen', 'Skoda', 'Porsche', 'SEAT', 'Cupra', 'Porsche']
>>> car_park.count("Porsche")
2
>>> car_park.index("Porsche")
3
>>> car_park.remove("Porsche")
>>> car_park
['Audi', 'Volkswagen', 'Skoda', 'SEAT', 'Cupra', 'Porsche']
>>> car_park.pop(1)
'Volkswagen'
>>> car_park
['Audi', 'Skoda', 'SEAT', 'Cupra', 'Porsche']
>>> car_park.insert(1, "Lamborghini")
>>> car_park
['Audi', 'Lamborghini', 'Skoda', 'SEAT', 'Cupra', 'Porsche']
>>> car_park.reverse()
>>> car_park
['Porsche', 'Cupra', 'SEAT', 'Skoda', 'Lamborghini', 'Audi']
>>> car_park.sort()
>>> car_park
['Audi', 'Cupra', 'Lamborghini', 'Porsche', 'SEAT', 'Skoda']
>>> del car_park[:]
>>> car_park
[]
~~~

Meer informatie over de mogelijkheden met een lijst vraag je in de REPL op met `help(list)`.

### List comprehension

Het komt regelmatig voor dat je op basis van één lijst een andere lijst moet aanmaken. Je hebt bijvoorbeeld een lijst met de getallen van 1 tot en met 10, en wilt nu een lijst maken met kwadraten van elk getal in die lijst. Dat zou je als volgt kunnen doen:

~~~python
getallen = list(range(1, 11))
kwadraten = []

for getal in getallen:
    kwadraten.append(getal**2)

print(kwadraten)
~~~

Dit toont als resultaat:

~~~
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
~~~

Maar er is een eenvoudiger manier: *list comprehension*. Hiermee maak je in één stap een lijst aan, gebaseerd op elementen van een andere lijst. Dat ziet er als volgt uit:

~~~python
kwadraten = [getal**2 for getal in range(1, 11)]
print(kwadraten)
~~~

Dit is heel wat compactere code, maar doet exact hetzelfde. Het verschil is dat je geen lege lijst moet initialiseren en niet expliciet elk van de elementen aan die lijst moet toevoegen. List comprehension maakt die lijst zelf element per element aan.

List comprehension is een krachtige techniek om lijsten aan te maken. Je vindt meer uitleg in <https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions>.
