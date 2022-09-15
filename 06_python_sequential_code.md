## Sequentiële uitvoering

### Een tweede programma ... met meerdere statements

Ons eerste programma had **één statement**. Wat als je nu meerdere statements toevoegt?
Pas het vorige voorbeeld aan tot de volgende code:

~~~python
print("Hello")
print("world")
~~~

Deze **statements** worden **uitgevoerd** in de volgorde zoals jij ze in het bestand plaatst.

~~~bash
$ python hello.py
Hello
world
$
~~~

Zoals je ziet zal de Python-interpreter eerst de opdracht **print("Hello")** uitvoeren en dan pas **print("world")**.

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

### Basisblok "sequentiële uitvoering"

Wat we hiervoor zagen, noemen we **sequentiële uitvoering**  .
**Sequentieel** betekent hier dat alle **statements** die je in een Python-script plaatst:

* **één voor één** worden **uitgevoerd**
* in de **volgorde** dat jij ze hebt geplaatst

Dit is een **basisprincipe (fundering)** waarop (bijna) alle (imperatieve) **programmeertalen** gebaseerd zijn.  

### Maar het gaat verder...

Het principe van **sequentiële uitvoering** alleen is **niet voldoende** om een **volledig functionerend programma** te schrijven.  
Bovenop dit principe heb je een **aantal andere principes** die we de komende hoofdstukken gaan uitleggen, namelijk:

* **Conditionele** uitvoering  
  Statements alleen **uitvoeren onder bepaalde voorwaarden**.
  We zullen dit zien onder de vorm van **if-else-statements**.
* **Repetitieve** uitvoering  
  Statements blijven **uitvoeren zolang aan een bepaalde voorwaarde** is voldaan.  
  In het vakjargon wordt dit ook meestal **loops** (lussen) genoemd.
* **Hergebruik** van uitvoering
  Statements **groeperen** om **herhaaldelijk** te kunnen **uitvoeren**  

Het meest **elementaire hergebruik** vinden we bij **functies**, maar later gaan we ook zien dat we deze functies kunnen groeperen in **modules** en **objectgeoriënteerd programmeren**.

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
  Elementaire unit van functionaliteit in Python zoals:
    * Aanroepen van functies (*function call*)
    * Berekening uitvoeren en resultaat opslaan in variabele (*assignment statement*)
    * Beëindigen van een functie (*return statement*)
    * ...
* **Variabelen** (en **literals**)  
  Tussentijds opslaan van waardes in het geheugen om in een later statement te hergebruiken
* **Expressies**  
    * Gelijk welke **uitvoering van code** die een **resultaat** oplevert
    * Heeft **operatoren** en **operanden**
