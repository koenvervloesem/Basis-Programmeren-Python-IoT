## Interactief code uitvoeren (of REPL)

Voor sommige programmeertalen -en/of omgevingen is een **REPL** voorzien.
Dit staat voor **R**ead **E**val **P**rint **L**oop:

* **R**ead: lezen van een instructie van de gebruiker
* **E**val: evaluatie van deze instructie en uiteindelijk uitvoering hiervan
* **P**rint: tonen van het resultaat van de instructie (als er één is)
* **L**oop: wacht (continu) opnieuw op de volgende instructie

Je krijgt als het ware onmiddellijk **feedback op je instructies**.

### Interactieve "Hello World"

De REPL van Python open je gewoon door de opdracht `python` te typen op de opdrachtregel:

~~~bash
$ python
Python 3.12.3 (main, Sep 11 2024, 14:17:37) [GCC 13.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
~~~

Vanaf het moment dat er `>>>` verschijnt (voorafgegaan door wat systeeminformatie) kun je aan de slag en Python-instructies uitvoeren.

De REPL is een ongelooflijk handige tool die je toelaat om snel iets uit te testen zonder dat je er een Python-script voor hoeft aan te maken. Het is aan te raden om altijd een REPL-sessie open te hebben staan wanneer je in Python aan het programmeren bent.

### Eenvoudige berekeningen

De Python-interpreter is handig om eenvoudige berekeningen uit te voeren:

~~~
$ python
Python 3.12.3 (main, Sep 11 2024, 14:17:37) [GCC 13.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 37+5
42
>>> 5/2
2.5
>>>
~~~

### Geldige code

Vanzelfsprekend moet je geldige instructies invoeren, anders zal er een foutboodschap verschijnen:

~~~
>>> 3+
  File "<stdin>", line 1
    3+
      ^
SyntaxError: invalid syntax
>>>
~~~

Interessant is wel dat de interpreter bij een fout niet wordt afgesloten en je je code opnieuw kunt proberen.   
Bij een script dat een fout bevat zou het programma beëindigd worden...

### Tab completion

De Python REPL vult code aan waar mogelijk als je de toets Tab indrukt. Als er meerdere mogelijke vervolledigingen zijn, moet je twee keer op Tab duwen. Dan worden ze allemaal getoond. Een voorbeeld als je p intypt en dan twee keer op Tab drukt: 

~~~
>>> p
pass       pow(       print(     property( 
~~~

Als je nu na de p een r typt en dan twee keer op Tab drukt, toont de REPL:

~~~
>>> pr
print(     property(
~~~

En als je nu de i intypt en één keer op Tab drukt, vervolledigt de REPL dit tot:

~~~
>>> print(
~~~

Dit kun je nu zelf verder vervolledigen tot een geldig statement:

~~~
>>> print("Hello")
Hello
~~~

De REPL vervolledigt overigens niet alleen namen die door Python zelf gedefinieerd zijn, maar ook namen in modules die je geïmporteerd hebt en namen die je zelf gedefinieerd hebt (zie later in de cursus). Dat maakt tab completion een handig hulpmiddel om nog efficiënter met de REPL te werken.

### Geschiedenis

De REPL onthoudt de voorgaande opdrachten die je ingetypt hebt. Door met de pijltjestoetsen naar boven en onder te werken, kun je de eerder ingetypte opdrachten weer oproepen. Nadat je zo een eerder ingetypte opdracht op de opdrachtprompt van de REPL ziet, druk je op Enter om deze weer uit te voeren.

### Help!

De REPL heeft een handige ingebouwde helpfunctie waarmee je informatie kunt opvragen over allerlei onderdelen van Python.

Wil je bijvoorbeeld meer informatie over de functie `print` die we al enkele keren gebruikt hebben, typ dan het volgende in de REPL:

~~~
>>> help(print)
~~~

Je krijgt dan het volgende te zien:

~~~
Help on built-in function print in module builtins:

print(...)
    print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)

    Prints the values to a stream, or to sys.stdout by default.
    Optional keyword arguments:
    file:  a file-like object (stream); defaults to the current sys.stdout.
    sep:   string inserted between values, default a space.
    end:   string appended after the last value, default a newline.
    flush: whether to forcibly flush the stream.
~~~

Als de helptekst langer dan een scherm is, kun je erdoor scrollen met de pijltjestoetsen naar boven en beneden of met PgUp en PgDown.

Typ de toets Q in om het helpscherm te verlaten en weer de opdrachtprompt van de REPL te zien.

Maak er een gewoonte van om als je vragen hebt over gelijk wat in Python de helpfunctie van de REPL te gebruiken. Je vindt zo vaak sneller het antwoord op je vraag dan de online documentatie (<https://docs.python.org>) of andere websites te raadplegen.

### De REPL verlaten

Om de REPL te verlaten, roep je de functie `exit()` of `quit()` aan:

~~~
$ python
Python 3.12.3 (main, Sep 11 2024, 14:17:37) [GCC 13.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> exit()
$
~~~

De toetsencombinatie **Ctrl+D** doet hetzelfde.

## Sequentiële uitvoering

### Een tweede programma ... met meerdere statements

Ons eerste programma had **één statement**. We hebben daarna al een keer getoond wat er gebeurt als je meerdere statements toevoegt.
Pas het voorbeeldprogramma aan tot de volgende code:

~~~python
print("Hello")
print("world")
~~~

Deze **statements** worden **uitgevoerd** in de volgorde zoals je ze in het bestand plaatst.

~~~bash
$ python hello.py
Hello
world
$
~~~

Zoals je ziet, zal de Python-interpreter eerst de opdracht `print("Hello")` uitvoeren en dan pas `print("world")`.

### Python-interpreter voert statement per statement uit

De statements uit je bestand worden **één voor één** uitgevoerd in de **volgorde zoals ze gegeven** zijn.  
Het komt dus neer op:

~~~
                                                               +--------------------------+
    +-----------------+        +----------------------+        |     CONSOLE/OUTPUT       |
    | hello.py        +--(1)-->+                      |        | +----------------------+ |
    +-----------------+        |                      |        | |                      | |
    | print("Hello")  +--(2)-->+  Python Interpreter  +--(2)-->+ | > Hello              | |
    | print("world")  +--(3)-->+                      +--(3)-->+ | > world              | |
    | ...             | (...)  |                      | (...)  | | > ...                | |
    +-----------------+        +----------------------+        | |                      | |
                                                               | +----------------------+ |
                                                               +--------------------------+
~~~

* *(1)* De Python-interpreter leest **hello.py**  
  (doordat het script als argument wordt meegegeven)
* *(2)* Uitvoeren van het **eerste statement**
* *(3)* Uitvoeren van het **tweede statement**
* *(...)* Tot er geen statements meer zijn

De REPL werkt hetzelfde, maar leest de opeenvolgende statements niet uit een bestand uit maar wacht telkens op een nieuw statement dat je in de console intypt.

### Basisblok "sequentiële uitvoering"

Wat we hiervoor zagen, noemen we **sequentiële uitvoering**  .
**Sequentieel** betekent hier dat alle **statements** die je in een Python-script of de REPL plaatst:

* **één voor één** worden **uitgevoerd**
* in de **volgorde** waarin je ze hebt geplaatst

Dit is een **basisprincipe (fundering)** waarop (bijna) alle (imperatieve) **programmeertalen** gebaseerd zijn.  

### Maar het gaat verder...

Het principe van **sequentiële uitvoering** alleen is **niet voldoende** om een **volledig functionerend programma** te schrijven.  
Naast dit principe heb je enkele **andere principes** die we de komende hoofdstukken gaan uitleggen, namelijk:

* **Conditionele** uitvoering  
  Statements alleen **uitvoeren onder bepaalde voorwaarden**.
  We zullen dit zien onder de vorm van **if-else-statements**.
* **Repetitieve** uitvoering  
  Statements blijven **uitvoeren zolang aan een bepaalde voorwaarde** is voldaan.  
  In het vakjargon wordt dit ook meestal **loops** (lussen) genoemd.
* **Hergebruik** van uitvoering  
  Statements **groeperen** om **herhaaldelijk** te kunnen **uitvoeren**. Het meest **elementaire hergebruik** vinden we bij **functies**, maar later gaan we ook zien dat we deze functies kunnen groeperen in **modules** en **objectgeoriënteerd programmeren**.

### Verschillende basisuitvoeringsprincipes

Al deze **basisprincipes** steunen op elkaar.

Om **conditionele uitvoering** te kunnen doen heb je **sequentiële uitvoering** nodig.  
**Repetitieve uitvoering** heeft **condities** nodig om te weten of een statement verder mag worden **uitgevoerd**.  

**Hergebruik van uitvoering** staat hier een beetje **los/onafhankelijk** van. Meestal begin je dit pas te leren als je de andere basisprincipes onder de knie hebt.

~~~
                            +------------------------------+
                            |                              |
                            |     Hergebruik uitvoering    |
                            |                              |
                        +---+------------------------------+---+
                        |                                      |
                        |        Repetitieve uitvoering        |
                        |                                      |
                    +---+--------------------------------------+---+
                    |                                              |
                    |           Conditionele uitvoering            |
                    |                                              |
                +---+----------------------------------------------+---+
                |                                                      |
                |                Sequentiële uitvoering                |
                |                                                      |
                |   +------------+   +------------+   +------------+   |
                |   | Statements |   | Variabelen |   | Expressies |   |
                +---+------------+---+------------+---+------------+---+
~~~

### First things first...

We moeten **eerst leren kruipen alvorens te fietsen**, dus we beginnen bij de **drie basiselementen** van **sequentiële uitvoering**

~~~
                +------------------------------------------------------+
                |                                                      |
                |                Sequentiële uitvoering                |
                |                                                      |
                |   +------------+   +------------+   +------------+   |
                |   | Statements |   | Variabelen |   | Expressies |   |
                +---+------------+---+------------+---+------------+---+
~~~

* **(Enkelvoudige) Statements**  
  De elementaire eenheid van functionaliteit in Python zoals:
    * Aanroepen van functies (*function call*)
    * Berekening uitvoeren en resultaat opslaan in een variabele (*assignment statement*)
    * Beëindigen van een functie (*return statement*)
    * ...
* **Variabelen** (en **literals**)  
  Tussentijds opslaan van waardes in het geheugen om in een later statement te hergebruiken
* **Expressies**  
    * Gelijk welke **uitvoering van code** die een **resultaat** oplevert
    * Heeft **operatoren** en **operanden**
