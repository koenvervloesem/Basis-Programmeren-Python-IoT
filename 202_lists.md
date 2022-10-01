## Lists

### Collecties

Tot nog toe hebben we met enkelvoudige types gewerkt (ook wel primitieve types genoemd). Variabelen van dit type bevatten één waarde.

In het tweede deel van de cursus bekijken we datatypes die meerdere waardes kunnen bevatten:

* Collecties: lists, dictionaries, tuples
* Abstracte datatypes en concepten: klassen, modules

Collecties zijn generieke datatypes waarin je meer dan één waarde kan plaatsen, een beetje zoals een container die verschillende goederen kan bevatten.


### List

I
Het eerste en meest eenvoudige type is de List (lijst).

> *Nota:*  
>In de meeste programmeertalen starten we met het concept van een *array* als we over collecties spreken.  
>Dit concept bestaat echter niet in Python. We komen hier later nog op terug in de cursus Embedded Programmeren.

Praktisch uitgedrukt heeft een list in Python de volgende eigenschappen:

* Het is een **geordende verzameling** of collectie van elementen.
* Elk **element** van zo'n lijst kan **gelezen of gewijzigd** worden via een **index**.
* Deze index **start** bij **0** (eerste element) en **eindigt** bij de index **n-1**.
* Een lijst heeft een **grootte of dimensie** (die we voor de gemakkelijkheid aanduiden als n).
* Deze grootte of dimensie kan wijzigen over de duurtijd van een programma.

### Voorbeeld van een lijst

Een lijst kan je aanmaken als een gewone variabele met onderstaande syntax.
Dit doe je door een List-literal aan te maken:

* Een variabele
* Een lijst van waardes
    * Omsloten door vierkante haakjes []
    * De waardes gescheiden door komma's

~~~python
x = [1.0, 2.0, 3.0]
y = ["a", "list", "of", "strings"]
z = ["a", 1, "mixed", 3.0, "list"]
~~~


### Lijst van een range

Als je een lijst wilt maken van de getallen 0 tot en met 9, kan dat als volgt:

~~~
>>> x = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> x
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
~~~

Maar dat is nogal veel typewerk. Kan het niet eenvoudiger? We hadden toch de functie range() gezien?

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
In de meeste gevallen ga je hetzelfde type gebruiken, maar dit is niet verplicht (zie z in het voorbeeld).

Je kan zelfs een lijst van lijsten maken...

~~~python
x = [[1.0, 2.0, 3.0], ["a", 1, "mixed", 3.0, "list"]]
~~~

### Dimensie van een lijst

Een eerste actie die we kunnen uitvoeren is de grootte of dimensie van een lijst opvragen.  
Daarvoor bestaat een generieke functie len(), die niet alleen voor lijsten maar alle types containers werkt.

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

Door de **naam** van de **list-variabele** te combineren met een **index** die je tussen vierkante haken plaatst, kan je de waarde uit deze positie ophalen en gebruiken in je code.

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

De index van het **laatste element** is n-1, of in dit geval 2 (lengte 3 - 1)


### Indexering vs range

Stel dat je toch een index adresseert > n -1, zal Python een foutmelding genereren:

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

Een element van een lijst kan je wijzigen op een vergelijkbare manier.  
Je gebruikt opnieuw het zelfde indexeringsmechanisme, maar past deze toe in een **assignment statement**:

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

In Python kan je ook negatieve indexen gebruiken. Hierbij draai je de indexering gewoon om...  
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

### Lijsten doorlopen (naïeve manier)

Gezien we:

* De lengte van een lijst kunnen opvragen
* De individuele elementen kunnen opvragen

kunnen we ook een lijst doorlopen met het volgende programma:

~~~python
x = [2, 3, 5, 7, 11]
i = 0
while i < len(x):
    print(x[i])
    i = i + 1
~~~

Deze code werkt maar is echter niet echt "Pythonic":

~~~bash
$ python doorlopen.py
2
3
5
7
11
~~~

### Lijsten doorlopen (Pythons manier)

Je kan namelijk ook de for-lus gebruiken, die we eerder gebruikten in combinatie met range:

~~~python
x = [2, 3, 5, 7, 11]
for n in x:
    print(n)
~~~

#### Selecteren met "slices"

Je kan ook een stuk uit een lijst nemen. Dit noemt men slicing:

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

#### Slicing met negatieve indexen

Je kan hier ook negatieve indexen gebruiken:

~~~python
x = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
for n in x[2:-2]:
    print(n)
~~~

Dat levert dus een slice op met de waardes van positie 2 tot en met 7 (omdat 10-2 = 8 de stopindex is en die niet inclusief geldt):

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

### Bewerken van een lijst 

Een lijst kan je ook nog vergroten eenmaal je die gecreëerd hebt:

* append(): element toevoegen aan einde van de lijst
* extend(): zelfde maar een ineens een andere lijst of iterable toevoegen 
* count(): aantal elementen met een specifieke waarde opvragen
* index():	index van het eerste element voor een specifieke waarde opvragen
* insert(): element toevoegen op een specifieke positie
* pop() : element op een index verwijderen
* remove() : eerste item met een specifieke waarde verwijderen
* reverse() : volgorde van de elementen omdraaien
* sort() : de lijst sorteren

Zie volgende sequentie in de REPL voor voorbeelden hiervan:

~~~python
>>> car_park = ["Audi", "Volkswagen", "Skoda"]
>>> car_park.append("Porsche")
>>> car_park
['Audi', 'Volkswagen', 'Skoda', 'Porsche']
>>> car_park.extend(["SEAT", "Cupra"])
>>> car_park
['Audi', 'Volkswagen', 'Skoda', 'Porsche', 'SEAT', 'Cupra']
>>> car_park.remove("Porsche")
>>> car_park
['Audi', 'Volkswagen', 'Skoda', 'SEAT', 'Cupra']
>>> car_park.pop(1)
'Volkswagen'
>>> car_park
['Audi', 'Skoda', 'SEAT', 'Cupra']
>>> car_park.insert(1, "Lamborghini")
>>> car_park
['Audi', 'Lamborghini', 'Skoda', 'SEAT', 'Cupra']
>>> car_park.reverse()
>>> car_park
['Cupra', 'SEAT', 'Skoda', 'Lamborghini', 'Audi']
>>> car_park.sort()
>>> car_park
['Audi', 'Cupra', 'Lamborghini', 'SEAT', 'Skoda']
>>> del car_park[:]
>>> car_park
[]
~~~

Meer informatie over de mogelijkheden met een lijst vraag je in de REPL op met `help(list)`.
