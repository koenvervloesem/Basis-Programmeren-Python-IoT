## Je code verbeteren

We hebben nu al een heel aantal basisconcepten gezien van programmeren in Python, inclusief wat richtlijnen en **best practices** om **kwalitatieve code** te schrijven.

Wat is kwalitatieve code? Daar is geen definitie van te geven, maar als je het ziet, weet je het :-) We kunnen wel een aantal eigenschappen van kwalitatieve code geven:

* Ze is correct **gedocumenteerd**.
* Ze is duidelijk **gestructureerd** en goed **leesbaar**.
* Ze is gemakkelijk **aan te passen en uit te breiden**.
* Ze bevat **tests** om te bepalen of de code correct werkt.
* Ze volgt de algemeen gangbare **conventies** voor de programmeertaal.

Je kunt perfect werkende Python-code schrijven die niet kwalitatief is. Maar dan zal iemand anders ze moeilijk begrijpen, zul je ze zelf later nog moeilijk begrijpen, is ze moeilijk aan te passen of uit te breiden, en bevat ze waarschijnlijk nog belangrijke fouten die je over het hoofd gezien hebt.

Gelukkig bestaan er heel wat hulpmiddelen waarmee je je Python-code kunt verbeteren. We bekijken er enkele en tonen hoe je ermee tot kwalitatieve code kunt komen.

### Docstrings

We hebben in deel 1 al beschreven hoe je code kunt documenteren. Enerzijds met commentaar op één regel (met `#`), anderzijds met commentaar op meerdere regels (tussen `"""`). Nu is er een speciaal type van dit laatste. Als we een multi-line comment als eerste statement in een module, functie, klasse of methode (die twee laatste zien we later) plaatsen, noemen we dit een **docstring**.

Een voorbeeld:

~~~python
def co2_description(co2_value):
    """Give a human-friendly description of a CO2 concentration."""
    if co2_value < 800:
        return "Good"
    elif co2_value < 1200:
        return "Acceptable"
    else:
        return "Bad"
~~~

Je ziet hier dat de docstring even veel ingesprongen is als de rest van de statements in de functie `co2_description`. Het is dan ook zelf een statement in de functie, maar een die de Python-interpreter niet uitvoert omdat het commentaar is.

Je kan ook een uitgebreidere docstring schrijven, die meerdere regels inneemt, bijvoorbeeld om uit te leggen wat voor argument je verwacht en wat de functie teruggeeft:

~~~python
def co2_description(co2_value):
    """Give a human-friendly description of a CO2 concentration.

    Args:
        co2_value: a concentration of CO2 gas in the air,
                   measured in ppm (parts per million)

    Returns:
        a string such as "Good", "Acceptable" or "Bad"
    """
    if co2_value < 800:
        return "Good"
    elif co2_value < 1200:
        return "Acceptable"
    else:
        return "Bad"
~~~

Het sluiten van de docstring doe je met een `"""` op de laatste regel.

Als je de docstring van deze functie nu leest, weet je alles wat je nodig hebt om de functie te gebruiken. Je weet wat voor argument je aan de functie moet doorgeven (een concentratie in ppm) en wat je van de functie als returnwaarde kunt verwachten. De drempelwaardes zelf die de functie gebruikt om die beschrijvingen "Good", "Acceptable" en "Bad" te berekenen, hoef je als gebruiker van de functie niet te kennen. Die staan dus niet in de docstring.

Tip: installeer in Visual Studio Code de extensie autoDocstring. Als je dan drie dubbele aanhalingstekens opent als eerste statement in een functie, toont de extensie je een knopje **Generate Docstring**. Als je hierop klikt, wordt al een sjabloon van een docstring toegevoegd, die je dan zelf verder invult.

Er bestaan ook tools zoals Sphinx en Read the Docs die automatisch html-pagina's kunnen genereren met documentatie over je Python-project. Die documentatie halen ze dan uit de docstrings van je Python-bestanden. Als je dan al de hele tijd consistent docstrings hebt gebruikt, is dit een eenvoudige manier om documentatie voor je Python-project te genereren voor anderen die niet in je code gaan duiken.

### Voorbeelden in een docstring opnemen

Om gebruikers van de functie concreet te laten zien hoe ze met de functie kunnen werken, kun je voorbeelden van het aanroepen van de functie in de docstring opnemen, alsof je de functie in de REPL gedefinieerd hebt en ze daar uitvoert:

~~~python
def co2_description(co2_value):
    """Give a human-friendly description of a CO2 concentration.

    Args:
        co2_value: a concentration of CO2 gas in the air,
                   measured in ppm (parts per million)

    Returns:
        a string such as "Good", "Acceptable" or "Bad"

    Examples:
    >>> co2_description(521)
    'Good'
    >>> co2_description(908.1)
    'Acceptable'
    >>> co2_description(3259)
    'Bad'
    """
    if co2_value < 800:
        return "Good"
    elif co2_value < 1200:
        return "Acceptable"
    else:
        return "Bad"
~~~

Merk op: de REPL toont strings altijd met enkele aanhalingstekens (`'`), dus we tonen de resultaten van de functie hier ook op die manier in de voorbeelden.

### Code testen met doctest

We hebben nu enkele voorbeelden van het gebruik van de functie in de docstring opgenomen, zodat we weten wat de functie voor die drie waardes als resultaat moet geven. Wat als we nu ook automatisch zouden kunnen testen of de functie wel degelijk de resultaten uit de voorbeelden teruggeeft? Dat kan, namelijk met de Python-module doctest (https://docs.python.org/3/library/doctest.html).

Hoe werkt dat? Je voert op de opdrachtregel de Python-module doctest uit en plaatst erachter de naam van je Python-bestand, bijvoorbeeld co2.py:

~~~shell
python -m doctest co2.py
~~~

Als je dit doet, krijg je niets te zien. Maar wat als je nu in de code van je functie een typefout hebt gemaakt? Stel dat je in de volgende regel:

~~~python
    if co2_value < 800:
~~~

per ongeluk dit hebt getypt:

~~~python
    if co2_value < 8000:
~~~

Daardoor krijgen alle CO2-waardes kleiner dan 8000 als beschrijving "Good". De functie zal de voorbeelden dan ook niet meer correct uitvoeren. En dat krijg je ook te zien als je nu opnieuw de doctest-module uitvoert op je programma:

```shell
$ python -m doctest co2.py
**********************************************************************
File "co2.py", line 13, in co2.co2_description
Failed example:
    co2_description(908.1)
Expected:
    'Acceptable'
Got:
    'Good'
**********************************************************************
File "co2.py", line 15, in co2.co2_description
Failed example:
    co2_description(3259)
Expected:
    'Bad'
Got:
    'Good'
**********************************************************************
1 items had failures:
   2 of   3 in co2.co2_description
***Test Failed*** 2 failures.
```

De laatste regel zegt dat er twee falingen zijn. Als je in de uitvoer ervoor gaat kijken, zie je bijvoorbeeld:

```
File "co2.py", line 13, in co2.co2_description
Failed example:
    co2_description(908.1)
Expected:
    'Acceptable'
Got:
    'Good'
```

Dat zegt dus dat op regel 13 in je bestand een voorbeeld staat dat door je code fout wordt uitgevoerd. De aanroep `co2_description(908.1)` zou immers als resultaat 'Acceptable' moeten geven volgens je voorbeeld, terwijl de functie 'Good' teruggeeft. En hetzelfde voor het andere voorbeeld.

Hoe weet doctest dit? De module haalt die voorbeelden uit alle docstrings van je bestand en voert deze als Python-code uit. Het resultaat van de uitgevoerde code wordt vergeleken met het resultaat in het voorbeeld. Indien dit verschilt, wordt dit getoond.

Merk op: dit is ook de reden waarom we de strings van de voorbeelden in de docstring met enkele aanhalingstekens weergeven, omdat doctest bij een string enkele aanhalingstekens verwacht.

Op deze manier kun je dus eenvoudig je code testen. Je schrijft je code en je schrijft dan ook tegelijk enkele voorbeelden van het gebruik ervan in een docstring. Je test je code met doctest en als dit een probleem oplevert, weet je dat je code niet werkt en los je het probleem op, tot doctest geen foutmeldingen geeft.

Uiteraard kan je code daarna nog altijd fouten bevatten: doctest test immers alleen de voorbeelden die je opgeeft. Het is daarom aan te raden om ook ongeldige waardes als voorbeelden in de docstring op te nemen en te kijken of het resultaat van je code wel is wat je verwacht.

Stel dat we door het schrijven van die voorbeelden beseft hebben dat negatieve CO2-concentraties niet kunnen, en dat onze functie dan eigenlijk de waarde "Invalid" moet teruggeven (nu geeft ze gewoon "Good" terug). Dan voegen we het volgende voorbeeld in de docstring toe:

```python
    >>> co2_description(-100)
    'Invalid'
```

Als we nu hierop weer doctest uitvoeren, krijgen we als resultaat:

```shell
$ python -m doctest co2.py 
**********************************************************************
File "co2.py", line 17, in co2.co2_description
Failed example:
    co2_description(-100)
Expected:
    'Invalid'
Got:
    'Good'
**********************************************************************
1 items had failures:
   1 of   4 in co2.co2_description
***Test Failed*** 1 failures.
```

Dus nu zien we dat we de functie nog moeten aanpassen om voor negatieve waardes "Invalid" terug te geven. Stel dat we dit als volgt doen:

```python
    if co2_value < 800:
        return "Good"
    elif co2_value < 1200:
        return "Acceptable"
    elif co2_value < 0:
        return "Invalid"
    else:
        return "Bad"
```

Dan krijgen we nog altijd dezelfde foutmelding van doctest. Zie je wat we verkeerd hebben gedaan?

We controleren op het negatief zijn van de CO2-waarde na de controle of de waarde kleiner dan 800 of 1200 is. Maar als de waarde kleiner dan 0 is, is ze sowieso kleiner dan 800, dus is aan de voorwaarde in de eerste `if` al voldaan en wordt er "Good" teruggegeven. Een correcte implementatie van onze functie moet dan ook als eerste controleren of de waarde kleiner dan 0 is en dan pas de rest:

```python
    if co2_value < 0:
        return "Invalid"
    elif co2_value < 800:
        return "Good"
    elif co2_value < 1200:
        return "Acceptable"
    else:
        return "Bad"
```

Als we nu doctest weer uitvoeren, krijgen we geen foutmeldingen meer.

De praktijk om eerst een test (hier in de docstring) te schrijven en dan pas de code die de test doet slagen, heet **test-driven development** (TDD). Het is een manier om heel robuuste code te schrijven.

Als je **Visual Studio Code** als code-editor gebruikt, bestaat er een handige extensie om doctest op je codebestanden uit te voeren: **Python DoctestBtn**. Daarna kun je voor een codebestand dat je open hebt in de editor rechts bovenaan op het knopje met de drie groterdantekens (`>>>`) klikken om doctest op je code uit te voeren in de terminal onderaan. Overigens toont dit ook de resultaten van de tests die geslaagd zijn. Dat kun je ook zelf op de terminal verkrijgen door de optie `-v` voor je bestandsnaam toe te voegen:

```shell
python -m doctest -v co2.py
```

Doctest is niet de enige manier om Python-code te testen, maar wel een laagdrempelige. Voor complexere tests kun je een externe module zoals pytest (https://docs.pytest.org) gebruiken.

Sowieso zal doctest je pas helpen als je je code in functies hebt opgedeeld. Als je één groot bestand zonder functies schrijft, heb je immers geen functie om uit te testen met doctest. Ook als je één grote functie maakt, is het testen wat lastig. Door je code op te splitsen in allemaal kleinere functies, kun je ze individueel testen met doctest en zo sneller fouten ontdekken.

### Types controleren

Je kan in de definitie van een functie de types van de argumenten definiëren die je verwacht:

~~~python
def times(a: int, b: int):
    return a * b

print(times(2, 5))
print(times("2", 5))
~~~

We verwachten hier dus dat de argumenten die je aan de functie `times` doorgeeft twee ints zijn.

Als je dit programma uitvoert, krijg je het volgende te zien:

~~~
10
22222
~~~

Python 'vermenigvuldigt' dus gewoon de string "2" met 5, ook al hebben we aangegeven dat dit argument het type int moet hebben. Python houdt hier dus geen rekening mee: dit type is gewoon een aanduiding om het onszelf duidelijk te maken wat voor type we verwachten.

Maar als we `times("2", 5))` aanroepen, weet Python toch dat "2" een string is en geen int zoals we in de functiedefinitie aangeduid hebben? Zouden we dit dan toch niet op een of andere manier automatisch kunnen testen dat onze code niet correct is?

Dat kan, namelijk met de externe module mypy (http://mypy-lang.org). Je kunt deze op de opdrachtregel installeren met pip:

```shell
pip install mypy
```

Als je nu mypy uitvoert met erna de bestandsnaam van je code, krijg je deze foutmelding:

```shell
$ mypy times.py
times.py:5: error: Argument 1 to "times" has incompatible type "str"; expected "int"
Found 1 error in 1 file (checked 1 source file)
```

Wat zegt dit? Op regel 5 roepen we de funtie `times` aan met als eerste argument een string, terwijl de functie een int verwacht. Dat klopt: het eerste argument is de string "2", terwijl in de functiedefinitie staat `a: int`. Mypy meldt je dat dit niet klopt.

Maar het type van een variabele is niet altijd even duidelijk, zoals we al gezien hebben. Stel dat je deze code schrijft:

```python
def times(a: int, b: int):
    return a * b

getal = input("Geef getal in: ")
print(times(getal, 5))
```

Het is je bedoeling dat je het door de gebruiker ingevoerde getal met 5 vermenigvuldigt. Maar als je dit uittest, krijg je het volgende:

```shell
$ python times.py
Geef getal in: 2
22222
```

Je weet ondertussen dat de functie `input` een string teruggeeft, en dat een string vermenigvuldigen met een getal die string dat aantal keren aan elkaar plakt. Maar dat is niet wat we hier willen, en als we deze code niet getest hadden, hadden we de fout misschien niet ontdekt.

Maar wat als we mypy op dit bestand hadden uitgevoerd? Dan kreeg je dezelfde foutmelding over het incompatibele type als in de eerste voorbeeldcode die we toonden. Mypy weet immers dat de functie `input` een string teruggeeft, en dat de variabele `getal` dus een string bevat en geen int, zoals de functie `times` verwacht, omdat we in de functiedefinitie de types van de argumenten hebben opgegeven.

We hebben al gezien dat je dit oplost door het resultaat van `input` naar een int om te zetten:

```python
def times(a: int, b: int):
    return a * b

getal = int(input("Geef getal in: "))
print(times(getal, 5))
```

Als we nu mypy op dit bestand uitvoeren, krijgen we als resultaat:

```shell
$ mypy times.py
Success: no issues found in 1 source file
```

In Visual Studio Code kun je ook mypy als onderdeel van Microsofts Python-extensie inschakelen. Druk op Ctrl+Shift+P en kies **Python: Enable/Disable Linting**. Kies daarna **Enable**. Druk dan nog eens op Ctrl+Shift+P en kies **Python: Select Linter** en dan **mypy**. Vanaf nu toont Visual Studio Code onderaan in het tabblad **Problems** problemen met incompatibele types elke keer dat je je bestand opslaat. Er wordt ook een rode lijn onder de probleemvariabele getoond die dezelfde melding toont als je er met de muiscursor over blijft hangen.

Overigens kun je ook het returntype van een functie in Python aangeven:

```python
def times(a: int, b: int) -> int:
    return a * b
```

Mypy controleert dit ook. Stel dat je in je functie per ongeluk de twee argumenten deelt door elkaar in plaats van vermenigvuldigt:

```python
def times(a: int, b: int) -> int:
    return a / b
```

Als je nu mypy hierop uitvoert, dan krijg je als foutmelding:

```shell
$ mypy times.py
times.py:2: error: Incompatible return value type (got "float", expected "int")
Found 1 error in 1 file (checked 1 source file)
```

Mypy weet immers dat het resultaat van de deling van twee ints door elkaar geen int maar een float is, maar ziet dat je in de functiedefinitie hebt opgegeven dat het resultaat een int moet zijn.

Kortom, als je zoveel mogelijk in je code expliciet de types van argumenten, variabelen of returnwaardes opgeeft en mypy zelf uitvoert of door je code-editor automatisch laat uitvoeren, heb je een extra hulpmiddel om je te waarschuwen voor fouten.

### Code linters

Met tests zoals in doctest en met controles op types zoals met mypy kunnen we allerlei fouten ontdekken, maar zeker nog niet alle. Je code kan bovendien ook regels bevatten die niet zozeer fout zijn, maar stilistisch niet in orde, of niet volgens de Python-conventies, of mogelijk fout. Om dit soort zaken in je code te ontdekken, bestaan er zognenoemde **code linters**.

De bekendste linter voor Python is Pylint (https://www.pylint.org). Je installeert dit als externe Python-module op de opdrachtregel met:

```shell
pip install pylint
```

In Visual Studio Code installeer je eenvoudig de extensie Pylint van Microsoft. Daarna toont Visual Studio Code automatisch in het tabblad **Problems** onderaan meldingen van Pylint.

Stel dat we Pylint op het bestand times.py van hierboven toepassen, dan krijgen we de volgende meldingen:

```shell
$ pylint times.py 
************* Module times
times.py:1:0: C0114: Missing module docstring (missing-module-docstring)
times.py:1:10: C0103: Argument name "a" doesn't conform to snake_case naming style (invalid-name)
times.py:1:18: C0103: Argument name "b" doesn't conform to snake_case naming style (invalid-name)
times.py:1:0: C0116: Missing function or method docstring (missing-function-docstring)

-----------------------------------
Your code has been rated at 0.00/10
```

Van de rating op het einde hoef je je niet te veel aan te trekken, maar de meldingen ervoor wel. Voor elke melding zie je de locatie van het probleem (bestand, regel, kolom), een code voor het type melding en een korte beschrijving, met ook een naam voor het type melding.

Als je meer uitleg over een melding wilt, kun je die code of naam van het type melding opzoeken op de website van Pylint, op https://pylint.pycqa.org/en/latest/user_guide/messages/messages_overview.html. Zoek daar bijvoorbeeld op C0114 of missing-module-docstring. Klik je op de link, dan krijg je een korte uitleg met altijd ook een voorbeeld van problematische code en code die het probleem oplost.

Zo zien we twee meldingen over een ontbrekende docstring. Het is inderdaad aan te raden om altijd zowel in het begin van je module als in het begin van elke functie een docstring met uitleg toe te voegen. Stel dat we dat toevoegen:

```python
"""Some helpful utilities."""

def times(a: int, b: int) -> int:
    """Multiply the first argument by the second.

    Args:
        a (int): The first operand to multiply.
        b (int): The second operand to multiply.

    Returns:
        int: The product of the two arguments.
    """
    return a * b

getal = int(input("Geef getal in: "))
print(times(getal, 5))
```

Nu geeft Pylint hier geen meldingen meer over. We zien wel nog twee meldingen over de namen van de argumenten. Het probleem is dat de argumenten maar één letter lang zijn, wat Pylint als niet beschrijvend genoeg ziet. We kunnen deze namen dus aanpassen tot:

```python
"""Some helpful utilities."""

def times(first: int, second: int) -> int:
    """Multiply the first argument by the second.

    Args:
        first (int): The first operand to multiply.
        second (int): The second operand to multiply.

    Returns:
        int: The product of the two arguments.
    """
    return first * second

getal = int(input("Geef getal in: "))
print(times(getal, 5))
```

Daarna klaagt Pylint niet meer:

```shell
$ pylint times.py 

--------------------------------------------------------------------
Your code has been rated at 10.00/10 (previous run: 0.00/10, +10.00)
```

Een andere code linter voor Python is Flake8 (https://flake8.pycqa.org). Installeer die met pip in de opdrachtregel:

```shell
pip install flake8
```

Of installeer de extensie Flake8 van Microsoft in Visual Studio Code.

Flake8 geeft doorgaans meer stilistische waarschuwingen. Voor ons al na de meldingen van Pylint opgekuiste Python-bestand geeft Flake8 bijvoorbeeld de volgende meldingen:

```shell
$ flake8 times.py 
times.py:3:1: E302 expected 2 blank lines, found 1
times.py:15:1: E305 expected 2 blank lines after class or function definition, found 1
```

Die codes zoals E302 en E305 kun je opzoeken op https://www.flake8rules.com. Daar vinden we dat we tussen functies twee lege regels moeten houden. Passen we onze code nu aan:

```python
"""Some helpful utilities."""


def times(first: int, second: int) -> int:
    """Multiply the first argument by the second.

    Args:
        first (int): The first operand to multiply.
        second (int): The second operand to multiply.

    Returns:
        int: The product of the two arguments.
    """
    return first * second


getal = int(input("Geef getal in: "))
print(times(getal, 5))
```

Dan krijgen we geen foutmelding meer.

Het aantal lege regels of spaties of andere stilistische zaken lijken misschien wat overbodig, maar het zorgt er wel voor dat je code er consistent uitziet en dat het in één oogopslag duidelijker leest. Probeer dus de code linters Pylint en Flake8 te gebruiken terwijl je aan het programmeren bent om de kwaliteit van je code hoog te houden.

### Code formatters

Terwijl code linters je wijzen op problemen in je code, gaan **code formatters** je code automatisch aanpassen om een bepaalde stijl aan te houden, bijvoorbeeld met consistente spaties, lege regels enzovoort. Zo hoef je zelf niet aan die stilistische elementen te denken, en kun je je focussen op wat je code doet.

De bekendste code formatter voor Python is Black (https://black.readthedocs.io). Installeer dit als Python-module met:

```
pip3 install black
```

Of installeer in Visual Studio Code de extensie Black Formatter van Microsoft.

Stel dat we in onze code nog niet de richtlijnen van Flake8 hadden gevolgd om twee lege regels voor en na onze functie op te nemen. Dan hadden we gewoon Black op onze code moeten uitvoeren, en die had automatisch het juiste aantal lege regels toegevoegd en zo Flake8 tevreden gemaakt:

```shell
$ black times.py 
reformatted times.py
All done! ✨ 🍰 ✨
1 file reformatted.
```

In Visual Studio Code rechtsklik je in het editorvenster van je bestand en kies je **Format Document**. Klik dan op **Configure...** en kies **Black Formatter**. Vanaf nu zal Visual Studio Code je code met Black aanpassen als je rechtsklikt en **Format Document** kiest.

Je kunt ook instellen dat Visual Studio Code automatisch Black op je codebestanden toepast elke keer dat je ze opslaat. Open daarvoor **File / Preferences / Settings**, klik op **Text Editor / Formatting** en vink **Format On Save** aan.

### Oefening

Probeer deze methodes om de codekwaliteit te verbeteren eens op een van de opdrachten die je ingediend hebt uit:

* Documenteer je code met docstrings.
* Voeg voorbeelden in de docstrings toe die je met doctest test om te controleren of je code wel correct werkt.
* Voeg types aan je code toe en test met mypy of je wel overal correct met die types omgaat.
* Voer de suggesties van Pylint en Flake8 uit.
* Formatteer je code met Black.

Bekijk het resultaat en vergelijk dit met je initiële code. Zie je het verschil in kwaliteit?
