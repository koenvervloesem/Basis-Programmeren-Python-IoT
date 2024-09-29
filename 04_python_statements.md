## Statements

### Eerste programma onder de loep

Ons (eerste) programma had dus als **functionaliteit** het **tonen** (`print` of afdrukken) van **een stuk tekst** naar de console.

Dit is min of meer het eenvoudigste programma dat we kunnen schrijven.

~~~python
print("Hello world")
~~~

### Enkelvoudige of simpele statements

Bovenstaande regel code is wat we noemen een **statement**. Eenvoudig gezegd: een statement is een stuk code dat op zichzelf staand een taak uitvoert.

Je kunt het bekijken als een **opdracht** die je aan je computer meegeeft, in dit geval aan de Python-interpreter.

Deze opdracht zal dan effectief **een actie uitvoeren**, zoals:
        
* een boodschap op de console tonen
* invoer van een gebruiker op de console opvragen

Een statement dat uit **één regel code** bestaat noemen we ook wel een **enkelvoudig statement**:

* De **kleinste eenheid van uitvoering** (of *unit of execution*)
* Staat **op zichzelf**
* Is altijd **één regel code**

> Nota:  
> Later zien we ook nog **complexe of meervoudige statements** (*block statements*).

### Sequentiële programmatie

Je kunt ook meerdere statements achter elkaar schrijven. Dit noemen we een **sequentie** of opeenvolging van statements.

~~~python
print("Hello world")
print("Welcome to Python")
print("Make sure to keep track")
~~~

De Python-interpreter zal deze opdrachten dan één voor één uitlezen en uitvoeren in de volgorde waarin jij ze hebt ingegeven.

### Functies aanroepen

Het **statement** dat we hier uitvoeren, is de **aanroep** van een **functie** (ofwel een *function call*).  

> Later zien we nog andere types van statements.

**Functies** zijn **herbruikbare** stukken **code** die je kunt **aanroepen** vanuit je **programma** onder de volgende vorm: 

~~~
<functie-naam>(<argument>)
~~~

Je schrijft eerst de naam van de functie, gevolgd - tussen haakjes - door het argument.  

Je kunt ook meerdere argumenten meegeven aan sommige functies. Dan moet je deze scheiden door komma's.  
Daar komen we zo dadelijk nog op terug.

### De functie print

Python voorziet **standaard** in een hele hoop **functies** die onmiddellijk te gebruiken zijn.
De functie die we hier gebruiken heeft als naam `print`.  

Bij deze functie geef je als argument een **tekst**.  

### String literal

Een stuk tekst dat je letterlijk wilt gebruiken in Python-code dient altijd omsloten te worden door aanhalingstekens.  
Dit mogen zowel **enkele** (`'`) als **dubbele** (`"`) aanhalingstekens zijn.  

Dit is een **string literal**:

* Het **datatype** voor stukken tekst heet **string**.
* We noemen dit een **literal** omdat de waarde **letterlijk **in de code wordt vermeld
  (in tegenstelling tot een string die door je programma wordt ingelezen).
