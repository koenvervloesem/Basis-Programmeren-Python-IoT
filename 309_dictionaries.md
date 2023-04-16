
## Werken met dictionary's

Heel wat van de API's voor netwerkcommunicatie in Python maken gebruik van een datatype dat we nog niet gezien hebben: een dictionary. Dit is een vorm van collectie, zoals een lijst.

### Korte herhaling lijsten

#### Lijst is een geordende collectie

We hebben reeds kennisgemaakt met lijsten binnen Python.

Een lijst liet ons toe om verschillende elementen in één variabele te verzamelen.
In onderstaand voorbeeld verzamelen we bijvoorbeeld de punten van vier studenten:

~~~python
punten = [20, 15, 16, 17]
print(f'student 0 heeft {punten[0]} punten')
print(f'student 3 heeft {punten[3]} punten')
~~~

#### Een lijst is geindexeerd

De individuele elementen kun je bereiken via een **index** (in bovenstaand voorbeeld 0 en 3) met als resultaat:

~~~
student 0 heeft 20 punten
student 3 heeft 17 punten
~~~

Deze index kan variëren tussen **0** en **n-1** (waar n de dimensie is van deze lijst).

#### Een element updaten

Via deze index-operator kun je ook een element updaten...

~~~python
punten = [20, 15, 16, 17]
punten[0] = 18
print(f'student 0 heeft {punten[0]} punten')
~~~

met als resultaat:

~~~
student 0 heeft 18 punten
~~~

#### Een element toevoegen

Toevoegen kan via de append-methode:

~~~python
punten = [20, 15, 16, 17]
print(punten)
punten.append(19)
print(punten)
~~~

met als resultaat

~~~
[18, 15, 16, 17]
[18, 15, 16, 17, 19]
~~~

#### for-loop

We konden ook gemakkelijk door een lijst "loopen" met een for-lus:

~~~python
punten = [18, 15, 16, 17]
for punt in studenten:
  print(f'student heeft {punt} punten')
~~~

met als resultaat:

~~~
student heeft 18 punten
student heeft 15 punten
student heeft 16 punten
student heeft 17 punten
~~~

#### for-loop met enumerate

Dikwijls wil je ook de index naast de waarde. Dat zou je als volgt kunnen doen:

~~~python
punten = [18, 15, 16, 17]
i = 0
for punt in punten:
  print(f'student {i} heeft {punt} punten')
  i = i + 1
~~~

met als resultaat:

~~~
student 0 heeft 18 punten
student 1 heeft 15 punten
student 2 heeft 16 punten
student 3 heeft 17 punten
~~~

Gezien dit veel gebruikt wordt, heeft Python de functie `enumerate` toegevoegd. Die kun je in een for-lus gebruiken om zowel door de index als het element te loopen:

~~~python
punten = [18, 15, 16, 17]
for i, punt in enumerate(punten):
  print(f'student {i} heeft {punt} punten')
~~~

### Dictionary's => sleutel en waarde

Bij een lijst gebruikten we de index als **sleutel** om een element op te vragen.
In het geval van studenten zou het misschien handiger zijn om de namen van de studenten te gebruiken om deze mapping uit te voeren. De volgnummers zijn immers betekenisloos voor ons.

Hiervoor bestaan in Python **dictionary's** (het equivalent in Java is de Map).  
Dit zijn - net zoals lijsten - **datastructuren** waar je **meerdere objecten of elementen** aan kan toevoegen.  

Het verschil echter is dat elke **waarde** die je toevoegt vergezeld moet zijn van een **sleutel**.  
Deze gebruik je dan nadien om deze waardes op te halen. Bijvoorbeeld:

~~~python
student_points = {
  "Jan": 12,
  "Piet": 15,
  "Joris": 16,
  "Korneel": 18
}

print(student_points["Jan"])
print(student_points)
~~~

De dictionary hierboven (`student_points`) mapt de namen van de studenten op punten, we spreken hier van **keys** (of sleutels) ten opzichte van de **values** of waardes.

Bij uitvoering van de code zie je:

* Je kan de **waarde opvragen** van een element door de **sleutel** te plaatsen **tussen vierkante haakjes**.
* De volgende regel print de volledige inhoud van de dictionary af.

~~~bash
$ python dictionary_demo.py
12
{'Jan': 12, 'Piet': 15, 'Joris': 16, 'Korneel': 18}
~~~

Zowel de sleutels als de waardes kunnen gelijk welk type zijn, er is **geen beperking van type**.  
In dit geval hebben we strings gebruikt als key maar dit kan ook een integer of zelfs een object zijn.

### Sleutels zijn uniek

Wat wel belangrijk is dat er **geen twee verschillende elementen** met dezelfde **sleutel** kunnen zijn **binnen** een **dictionary**.  
Onderstaande code demonstreert dit:

~~~python
student_points = {
  "Jan": 12,
  "Piet": 15,
  "Joris": 16,
  "Korneel": 18,
  "Jan": 14
}

print(student_points["Jan"])
print(student_points)
~~~

Jan is hier twee maal gedefinieerd als sleutel.  
In onderstaande uitvoer zie je dat de **laatste** waarde (14) de **eerste** waarde zal **overschrijven** (12).

~~~bash
$ python dictionary_demo.py
14
{'Jan': 14, 'Piet': 15, 'Joris': 16, 'Korneel': 18}
~~~

### Waarde toevoegen aan een dictionary

Een student **toevoegen** aan deze **dictionary** kan ook. Hieronder voegen we bijvoorbeeld een student met de sleutel (naam) "Bart" toe:

~~~python
student_points = {
  "Jan": 12,
  "Piet": 15,
  "Joris": 16,
  "Korneel": 18
}

print(student_points["Jan"])
print(student_points)

student_points["Bart"] = 12
print(student_points["Bart"])
print(student_points)
~~~

Met als resultaat dat er een extra studenten-punt is **toegevoegd**:

~~~bash
$ python3 dictionary_demo.py
12
{'Jan': 12, 'Piet': 15, 'Joris': 16, 'Korneel': 18}
12
{'Jan': 12, 'Piet': 15, 'Joris': 16, 'Korneel': 18, 'Bart': 12}
~~~

### Waarde updaten binnen een dictionary

Je kan naast het **toevoegen** ook dezelfde waarde **updaten**:

~~~python
student_points = {
  "Jan": 12,
  "Piet": 15,
  "Joris": 16,
  "Korneel": 18
}

print(student_points["Piet"])
student_points["Piet"] = 16
print(student_points["Piet"])
~~~

Hierbij gaat de **vorige waarde** vanzelfspekend **overschreven** worden:

~~~bash
$ python3 dictionary_demo.py
15
16
~~~

### Waarde verwijderen uit een dictionary

Naast **toevoegen** en **wijzigen** kan je zo'n paar van sleutel en waarde ook **verwijderen**.  
Hiervoor bestaat in Python het **keyword** `del`.

Stel dat we een student willen verwijderen. Dan kan dit als volgt:

~~~python
student_points = {
  "Jan": 12,
  "Piet": 15,
  "Joris": 16,
  "Korneel": 18
}
print ("Before: ", student_points)
del student_points["Piet"]
print ("After: ", student_points)
~~~

Na het verwijderen van het element met de sleutel "Piet" verdwijnt deze uit de dictionary:

~~~bash
$ python3 dictionary_demo.py
Before:  {'Jan': 12, 'Piet': 15, 'Joris': 16, 'Korneel': 18}
After:  {'Jan': 12, 'Joris': 16, 'Korneel': 18}
~~~

### Wat als de sleutel niet bestaat?

We hebben nu gezien hoe je een element verwijdert.
Wat als je een waarde opvraagt met een **onbestaande sleutel** zoals hieronder?

~~~python
student_points = {
  "Jan": 12,
  "Piet": 15,
  "Joris": 16,
  "Korneel": 18
}

print(student_points["Hans"])
~~~

Zoals je hieronder ziet, wordt er dan een exception geworpen:  

~~~bash
$ python3 dictionary_demo.py
Traceback (most recent call last):
  File "dictionary_demo.py", line 8, in <module>
    print(student_points["Hans"])
KeyError: 'Hans'
~~~

Python-dictionary's vereisen dat je bestaande sleutels doorgeeft. Als de sleutel niet bestaat, krijg je een exception.

Hoe kan je dit vermijden?

### Nakijken of een element binnen de dictionary bestaat

Om zulke situaties te vermijden heb je twee opties.  
Ofwel vang je deze exception op via een **try-except-constructie**:

~~~python
student_points = {
  "Jan": 12,
  "Piet": 15,
  "Joris": 16,
  "Korneel": 18
}

search_key = "Hans"

try:
    print(student_points[search_key])
except KeyError:
    print(search_key + " bestaat niet")
~~~

...met als resultaat...

~~~bash
$ python3 dictionary_demo.py
Hans bestaat niet
~~~

Ofwel **vermijd** je deze **exception** door gebruik te maken van de operator `in`:

~~~python
student_points = {
  "Jan": 12,
  "Piet": 15,
  "Joris": 16,
  "Korneel": 18
}

search_key = "Hans"

if search_key in student_points:
    print(student_points[search_key])
else:
    print(search_key + " bestaat niet")
~~~

...met hetzelfde resultaat.

~~~bash
$ python3 dictionary_demo.py
Hans bestaat niet
~~~

Welke moet je nu gebruiken? Dat is vooral een kwestie van stijl en hangt ook wat van het programma af. Doorgaans worden exceptions alleen gebruikt voor exceptionele gebeurtenissen. Als het dus in je programma heel normaal is dat een sleutel niet in een dictionary zit, vang je dit eerder op met de operator `in`.
