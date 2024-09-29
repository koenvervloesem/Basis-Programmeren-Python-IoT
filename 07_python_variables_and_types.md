## Variabelen

Samen met **variabelen** introduceren we ook **assignment statements**.

### Voorbeeld van gebruik van een variabele

We illustreren het gebruik van variabelen met de volgende voorbeeldcode:

~~~python
say_hello = "hello"
print(say_hello)
print(say_hello)
~~~

Als je deze code uitvoert, zal deze twee maal na elkaar **hello** tonen op de console.

~~~
hello
hello
~~~

Er zijn hier twee nieuwe elementen:

* Een **variabele** met de naam `say_hello`
* Een **assignment statement** dat wordt gebruikt om de variabele `say_hello` de waarde "hello" toe te kennen (*assign*)

Door het gebruik van een **variabele** kun je dus een **waarde** (hier de tekst "hello") in het geheugen steken en deze **hergebruiken** in latere **statements**.

### Een variabele is een stuk geheugen

Een **variabele** is dus:

* Een stuk **geheugen** dat je kunt hergebruiken
* Dat een **waarde** kan bevatten
* Waaraan een **naam** is gelinkt  
 (of ook wel **symbool** genoemd)

### Doel van een variabele

Het doel van zo'n variabele is:

* het **hergebruiken of delen van data** 
* tussen verschillende **statements** 
* binnen een **sequentiële uitvoering**

~~~
                        +------------------+
     +------write-------+ Statement 1:     |
     |                  | assignment       |
     |                  +-------+----------+           +------------------------+
     |                          |                      |      CONSOLE/OUTPUT    |
+----v-----+                    |                      | +--------------------+ |
| say_hello|            +-------v----------+           | |                    | |
+----------+            | Statement 2:     +---print---> | > hello            | |
| hello    +---read-----> function call    |           | |                    | |
+----+-----+            +-------+----------+           | |                    | |
     |                          |                      | |                    | |
     |                          |                      | |                    | |
     |                  +-------v----------+           | |                    | |
     |                  | Statement 3:     +---print---> | > hello            | |
     +---------read-----> function call    |           | +--------------------+ |
                        +------------------+           +------------------------+
~~~

### Assignment statement

Om met variabelen te kunnen werken, gebruiken we een nieuw soort statement, namelijk het **assignment statement**.   
Zo'n statement kent (of **"assigns"**) een **waarde** (**value**) toe aan een **naam**.  
Deze waarde kun je dan hergebruiken in latere **statements** (de functie-aanroepen die de waarde twee keer afdrukken).

### Vorm van een assignment statement

Zo'n assignment statement heeft altijd de vorm van :

~~~
<naam_variabele> = <waarde>
~~~

* Aan de **linkerkant** zet je de **naam** (het symbool) van de **variabele**.
* In het midden zet je de **assignment operator**. In Python is dit het symbool `=`.
* Aan de **rechterkant** zet je de waarde.

### Waarde van een variabele wijzigen

Een variabele zou niet "variabel" zijn als je deze niet kunt wijzigen. We beschouwen het volgende voorbeeld in de REPL:

~~~python
>>> say_hello = "hello"
>>> print(say_hello)
hello
>>> say_hello = "world"
>>> print(say_hello)
world
>>>
~~~

Wat verwachtte je dat deze instructies toonden?

Je kunt later in hetzelfde programma of dezelfde sequentie variabelen van **waarde wijzigen** (zo veel als je wilt) via deze **assignment statements**.

~~~
                        +------------------+
     +------write-------+ Statement 1:     |
     |                  | assignment       |
     |                  +-------+----------+           +------------------------+
     |                          |                      |      CONSOLE/OUTPUT    |
+----v-----+                    |                      | +--------------------+ |
| say_hello|            +-------v----------+           | |                    | |
+----------+            | Statement 2:     +--print----> | > hello            | |
| hello    +---read-----> function call    |           | |                    | |
+----------+            +-------+----------+           | |                    | |
                                |                      | |                    | |
                                |                      | |                    | |
                        +-------v----------+           | |                    | |
     +------write-------+ Statement 3:     |           | |                    | |
     |                  | assignment       |           | |                    | |
     |                  +-------+----------+           | |                    | |
     |                          |                      | |                    | |
+----v-----+                    |                      | |                    | |
| say_hello|            +-------v----------+           | |                    | |
+----------+            | Statement 2:     +--print----> | > world            | |
| world    +---read-----> function call    |           | +--------------------+ |
+----------+            +------------------+           +------------------------+


~~~

### Een variabele heeft een type

Een variabele heeft dus een naam, en aan de hand van deze naam kun je vanuit je code de waarde uitlezen en wijzigen.   
Daarnaast heeft een variabele ook een bepaald **type**.

~~~
+---------------------+           +---------------------+
| Variabele           |           | Type                |
+---------------------+           +---------------------+
|  Naam               |           |  Operaties          |
|  Waarde             +----------->  Grootte            |
|                     |           |  Beperkingen        |
|                     |           |  ...                |
+---------------------+           +---------------------+
~~~

Dit type bepaalt wat je met de waarde (een hoop bytes...) kunt doen.  
Python ondersteunt verschillende types. Tot nu toe hebben we gebruikgemaakt van het type **string** (in Python `str`).

In onderstaand stuk code maken we drie variabelen aan van verschillende types.  
Met de functie `type` vragen we het type van de variabele op.

~~~
>>> a = "hello"
>>> b = 10
>>> c = 0.5
>>> type(a)
<class 'str'>
>>> type(b)
<class 'int'>
>>> type(c)
<class 'float'>
>>>
~~~

Python leidt direct het type af bij creatie van de variabele. Dit principe wordt **type inference** genoemd.
In dit geval kan de Python-interpreter vanuit de **literal** afleiden tot **welk type** deze variabele is:

* "hello" is omgeven door aanhalingstekens
* 10 is niet omgeven door aanhalingstekens en bevat alleen cijfers
* 0.5 is een getal met een decimaal punt (kommagetal)

Het eerste type hier is `str`, dat we al een aantal keren hebben gebruikt.  

### Numerieke types

De twee andere - `int` en `float` - zijn **numerieke types** die je kunt gebruiken om berekeningen met getallen uit te voeren.

### Integers

**Integers** (type `int`) kennen we als de groep van **gehele getallen**. Bijvoorbeeld:

~~~
—1
–3
42
355
888888888888888
–7777777777
~~~

In Python 3 is er **geen limiet** op hoe groot een **geheel getal** kan zijn.  
Natuurlijk wordt het in de praktijk **beperkt** door de **hoeveelheid geheugen** die je **systeem** heeft, zoals alle dingen.  

Maar verder kan een geheel getal zo lang zijn als je nodig hebt, zoals hieronder:

~~~python
>>> a = 154646546465465465465465465464
>>> print(a)
154646546465465465465465465464
~~~

Vanzelfsprekend kun je hier ook berekeningen mee uitvoeren:

~~~
>>> a = 3
>>> b = 4
>>> c = a + b
>>> print(c)
7
~~~

Naast het decimale talstelsel is het ook mogelijk om deze getallen binair, octaal of hexadecimaal te noteren.  
Dat doe je door dit getal te typen startende met 0 gevolgd door:

* **o** of O voor **octaal** (grondgetal 8)
* **x** of X voor **hexadecimaal** (grondgetal 16)
* **b** of B voor **binair** (grondgetal 2)

Onderstaand voorbeeld illustreert dit:

~~~
>>> print(0o10)
8
>>> print(0x10)
16
>>> print(0b1110)
14
~~~

### Floats

Python gebruikt ook een floating-point type (`float`).

Dit wordt gebruikt voor kommagetallen, dus getallen waar je precisie nodig hebt na de komma.

~~~
>>> a = -3.0
>>> b = 24.75
>>> print(a+b)
21.75
~~~

Let op: werken met floats heeft zo zijn beperkingen. Zo worden floats intern als breuken met grondtal 2 (binair) voorgesteld, terwijl niet alle decimaal voorgestelde getallen een exacte binaire voorstelling hebben.

Vergelijk dit met ons decimale talstelsel: we kunnen de breuk 1/3 niet exact voorstellen. We kunnen dit wel benaderen als 0,3, 0,33, 0,333, enzovoort, maar dit is nooit volledig correct, het is een zich oneindig herhalende waarde 0,33333... Op welk punt je stopt, bepaalt hoe goed je benadering van de exacte waarde 1/3 is.

Op dezelfde manier kun je de breuk 1/10, dus de decimale waarde 0,1, niet exact voorstellen in grondtal 2. Het is de oneindig herhalende waarde 0,00011001100110011... Op welk punt je stopt, bepaalt hier weer hoe goed je benadering van de exacte waarde 1/10 is.

Als je dus met floats werkt, gebruikt Python altijd een benaderende waarde, maar toont het niet de waarde die het opslaat, maar een benadering door de waarde af te ronden.

Dat kun je eenvoudig testen in de REPL:

```python
>>> 0.1*3
0.30000000000000004
```

Hoe komt dit? 0.1 wordt door Python intern voorgesteld als 0.1000000000000000055511151231257827021181583404541015625. Door dit met 3 te vermenigvuldigen, kom je aan die 4 helemaal achteraan.

Hou hier ook rekening mee als je floats gaat vergelijken met elkaar. Door dit soort afrondingen kunnen de waardes dan onverwacht niet overeenkomen.

Als je met floats werkt, is het daarom meestal aan te raden om zelf resultaten altijd af te ronden tot op het aantal decimalen dat je nodig hebt. Dat doe je met de ingebouwde functie `round`:

```python
>>> round(0.1*3, 2)
0.3
>>> round(0.1*3, 17)
0.30000000000000004
>>> round(0.1*3, 16)
0.3
>>> round(4/3, 2)
1.33
>>> round(4/3, 5)
1.33333
>>> round(0.11)
0
```

Het tweede argument dat je aan de functie round doorgeeft, is het aantal cijfers na de komma dat je wilt weergeven. Laat je dit weg, dan rondt de functie de waarde af tot op een geheel getal (dus zonder cijfers na de komma).

Wil je meer weten over de uitdagingen van met floats te werken, lees dan <https://docs.python.org/3/tutorial/floatingpoint.html>.

In de zeldzame gevallen dat je toch exacte berekeningen nodig hebt, gebruik dan een van de volgende modules:

* <https://docs.python.org/3/library/decimal.html>
* <https://docs.python.org/3/library/fractions.html>
