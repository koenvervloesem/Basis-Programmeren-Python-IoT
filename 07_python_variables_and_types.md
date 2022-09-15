## Variabelen

Samen met **variabelen** introduceren we ook **assignment statements**.

### Voorbeeld van gebruik van een variabele

We illustreren het gebruik van variabelen met de volgende voorbeeldcode:

~~~python
say_hello = "hello"
print(say_hello)
print(say_hello)
~~~

Als je deze code uitvoert, zal deze **twee maal na elkaar hello tonen** op de console.

~~~
hello
hello
~~~

Er zijn hier twee nieuwe elementen:

* Een **variabele** met de naam **say_hello**
* Een **assignment statement** dat wordt gebruikt om de variabele say_hello de waarde "hello" toe te kennen (*assign*)

Door het gebruik van een **variabele** kan je dus een **waarde** (hier de string "hello") in het geheugen steken en deze **hergebruiken** in latere **statements**.

### Een variabele is een stuk geheugen

Een **variabele** is dus:

* Een stuk **geheugen** dat je kan hergebruiken
* Dat een **waarde** kan bevatten
* Waaraan een **naam** is gelinkt  
 (of ook wel **symbool** genoemd)

### Doel van een variabele

De **essentie/doel** van zo'n **variabele** is 

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
Deze waarde kan je dan hergebruiken in latere **statements** (de functie-aanroepen die de waarde twee maal afdrukken).

### Vorm van een assignment statement

Zo'n assignment statement heeft altijd de vorm van :

~~~
<naam_variabele> = <waarde>
~~~

* Aan de **linkerkant** zet je de **naam** (het symbool) van de **variabele**.
* In het midden zet je de **assignment operator**. In Python is dit het symbool **=**.
* Aan de **rechterkant** zet je de waarde.

### Waarde van een variabele wijzigen

Een variabele zou niet "variabel" zijn als je deze niet kan wijzigen. We beschouwen het volgende voorbeeld:

~~~python
say_hello = "hello"
print(say_hello)
say_hello = "world"
print(say_hello)
~~~

Wat verwachte je dat deze code toont? Probeer het eens uit?

Je kan later in hetzelfde programma of dezelfde sequentie variabelen van **waarde wijzigen** (zo veel als je wilt) via deze **assignment statements**.

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

Een variabele heeft dus een naam, en aan de hand van deze naam kan je vanuit je code de waarde uitlezen en wijzigen.   
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

Dit type bepaalt wat je met de waarde (een hoop bytes...) kan doen.  
Python ondersteunt verschillende types. Tot nog toe hebben we gebruik gemaakt van het type **string** (in Python **str**).

In onderstaand stuk code maken we drie variabelen aan van verschillende types.  
Met de functie **type** vragen we het type van de variabele op.

~~~python
a = "hello"
b = 10
c = 0.5
print(type(a))
print(type(b))
print(type(c))
~~~

Python leidt direct het type af bij creatie van de variabele. Dit principe wordt **type inference** genoemd.
In dit geval kan de Python-interpreter vanuit de **literal** afleiden tot **welk type** deze variabele is:

* "hello" is omgeven door aanhalingstekens
* 10 is niet omgeven door aanhalingstekens en bevat alleen cijfers
* 0.5 is een getal met een decimaal punt (kommagetal)

Als we dan bovenstaand programma uitvoeren, krijgen we de volgende uitvoer:

~~~
<class 'str'>
<class 'int'>
<class 'float'>
~~~

Het eerste type hier is **str**, dat we al een aantal keren hebben gebruikt.  

### Numerieke types

De twee andere - **int** en **float** - zijn **numerieke types** die je kan gebruiken om berekeningen met getallen uit te voeren.

### Integers

**Integers** (type **int**) kennen we als de groep van **gehele getallen**. Bijvoorbeeld:

~~~
—1
–3
42
355
888888888888888
–7777777777
~~~

In **Python 3** is er **geen limiet** op hoe lang een **geheel getal** kan zijn.  
Natuurlijk wordt het **beperkt** door de **hoeveelheid geheugen** die je **systeem** heeft, zoals alle dingen.  

Maar verder kan een geheel getal zo lang zijn als je nodig hebt, zoals hieronder:

~~~python
a = 154646546465465465465465465464
print(a)
~~~

Uitgevoerd wordt dit:

~~~
154646546465465465465465465464
~~~

Vanzelfsprekend kan je hier ook berekeningen mee uitvoeren:

~~~python
a = 3
b = 4
c = a + b
print(c)
~~~

Dit geeft als uitvoer:

~~~
7
~~~

Naast het decimale talstelsel is het ook mogelijk om deze getallen binair, octaal of hexadecimaal te noteren.  
Dat doe je door dit getal te typen startende met 0 gevolgd door:

* **o** of O voor **octaal** (grondgetal 8)
* **x** of X voor **hexadecimaal** (grondgetal 16)
* **b** of B voor **binair** (grondgetal 2)

Onderstaand voorbeeld illustreert dit:

~~~
print(0o10)  # 0o = octal notation
print(0x10)  # 0x = hexadecimal notation
print(0b1110)  # 0b = binary notation
~~~

Met als uitvoer:

~~~
8
16
14
~~~

### Floats

Python gebruikt ook een floating-point type (**float**).

Dit wordt gebruikt voor kommagetallen, dus getallen waar je precisie nodig hebt na de komma.

~~~
a = -3.0
b = 24.75
print(a+b)
~~~

Dit geeft als uitvoer:

~~~
21.75
~~~
