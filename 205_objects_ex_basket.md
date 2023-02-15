## Synthese-oefening: Winkelmandje

Dit hoofdstuk is opgebouwd rond een herhalingsoefening voor **klassen en objecten** gecombineerd met het gebruik van **lijsten**.

Naast het herhalen van het principe van **objectgeoriënteerd programmeren** willen we hier ook aantonen dat gelijk welk wat groter programma **stap voor stap** moet worden opgebouwd.  

De oefening gaat er van uit dat je reeds Git hebt geïnstalleerd.  
Indien dit nog niet het geval is, zie https://git-scm.com/ voor installatie-instructies.

Na de installatie van Git configureer je je e-mailadres en naam, zodat Git je identiteit aan je code koppelt. Dat kan met deze twee opdrachten:

~~~
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
~~~

Vul hier uiteraard je eigen gegevens in.

### Stap voor stap programmeren

Dit doe je door de volgende stappen constant te herhalen:

* **Code schrijven:** Een heel **klein onderdeel** van de **applicatie** opbouwen/toevoegen
* **Testen:**
    * Werkt je nieuwe functionaliteit?  
      Testen van je functie/code die je hebt gebouwd, ook soms **unit testing**
    * Werkt de functionaliteit die je tevoren had nog altijd even goed na je wijziging van de code?  
      Dit noemen we ook **testen op regressies**.
* **Revisioneren**
    * Na een succesvolle test slaan we onze wijziging (stap) op.
    * We voegen een duidelijke commentaar toe.
* ...en we starten opnieuw...

De laatste actie - revisioneren - is iets wat we nu voor de eerste maal introduceren.    
Het komt er op neer om (met behulp van een tool) de evolutie en verschillende versies van je programma bij te houden, waardoor je:

* Een historiek kan bijhouden van de wijzigingen van je code
* Kan terugkoppelen naar een vroegere specifieke revisie
* Code delen tussen verschillende personen (en zelfs organisaties)
* ...

De tool die we hiervoor gebruiken is **Git**.

### Git: Aanmaken van een project

In dit voorbeeld gaan we Git gebruiken om de wijzigingen van onze applicatie bij te houden.  

We starten met:

* We **maken** een **folder** aan (we noemen deze **basket**).
* We **navigeren** (in de command-line-prompt) naar deze folder.
* We voeren de opdracht `git init` uit binnen deze folder.

~~~
$ mkdir basket
$ cd basket
$ git init
Initialized empty Git repository in /home/koan/basket/.git/
~~~

Deze actie maakt - in deze folder basket - een lokale **git-repository**.  
Zo'n git-repository is:

* Een soort van database waar je **code-wijzigingen** kan **bijhouden/traceren**.
* Als een soort van **snapshot** (zoals je bijvoorbeeld een snapshot van een harde schijf zou nemen).
* Het stelt je in staat een historiek bij te houden en zelfs terug te keren naar een vorige **revisie**.

De files die deze database omvatten bevinden zich in een verborgen folder `.git`.

~~~
$ ls -a
.  ..  .git
$ ls -a .git/
.  ..  branches  config  description  HEAD  hooks  info  objects  refs
~~~

Met de opdracht `git status` kan je dan vervolgens controleren wat de status is.

~~~
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
~~~

De uitvoer van deze opdracht **suggereert** het gebruik van `git add`, we komen hier zo dadelijk nog op terug.

### Code schrijven: Basket-item-class

Alvorens met git verder te gaan, starten we met **code te schrijven**.  
Bedoeling is dat we een **applicatie** gaan maken die een **winkelmandje** (of **winkellijstje**) gaat beheren.

Zo'n winkelmandje bestaat uit verschillende **items** die je in dat **mandje** mag droppen.  
Laten we starten met er van uit te gaan dat zo'n **item** een **beschrijving** en een **prijs** bevat...

We hebben in het studenten-voorbeeld gezien hoe je zulke gestructureerde data kan **bijhouden/groeperen** in een **klasse**.

We starten met een file **basket.py** aan te maken en voorzien daarin een klasse **BasketItem**:

~~~python
from dataclasses import dataclass

@dataclass
class BasketItem:
    description: str = ""
    price: int = 0
~~~

Deze klasse bevat **twee attributen**: een beschrijving en een prijs van het item.

Om de code te controleren op correctheid, voeren we onze applicatie uit:

~~~
$ python basket.py 
$
~~~

Vanzelfsprekend doet deze nog **niet** erg **veel**, daar komen we zo dadelijk **op** **terug**.  
Eerst gaan we echter onze eerste **code-wijzigen registreren/revisioneren**

### Git: Je eerste commit met git...

We hebben ons **eerste stuk code** geschreven. Als we via het git-commando `git status` de status opvragen...

~~~
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	basket.py

nothing added to commit but untracked files present (use "git add" to track)
~~~

...zien we dat git gezien heeft dat het bestand is gewijzigd en dat deze niet wordt getraceerd (tracked)   
We gebruiken het command `git add basket.py` om dit bestand basket.py toe te voegen aan de git-repository:

~~~
$ git add basket.py 
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   basket.py
~~~

Er staat **"Changes to be committed"**, dit betekent dat je nu aan de **git-repo** hebt meegedeeld dat er een **wijziging** is.  
De wijziging is echter **nog niet** in de git-repo **opgeslagen**, om dit te volbrengen moeten we een **commit** uitvoeren.

Dit kan je met het commando `git commit`.  
Zo'n **commit** moet altijd vergezeld gaan van een boodschap, in dit geval "Create class BasketItem".  

> Belangrijk:  
> Probeer deze **boodschap** kort maar toch **duidelijk** te maken.  
> Hoe **duidelijker** deze is, hoe **gemakkelijker achteraf** om na te gaan wat je wijzigingen waren.

~~~
$ git commit -m "Create class BasketItem"
[master (root-commit) 4ba8385] Create class BasketItem
 1 file changed, 6 insertions(+)
 create mode 100644 basket.py
$ git status
On branch master
nothing to commit, working tree clean
~~~

Als we de status oproepen, zien we dat er geen nieuwe wijziging meer is.  
Met het commando `git log` kan je dan zien dat je wijziging is geregistreerd:

~~~
$ git log
commit 4ba8385472e5dd3c74cd1f8954620a22cf0c67d8 (HEAD -> master)
Author: Koen Vervloesem <koen@vervloesem.eu>
Date:   Sat Nov 5 18:03:57 2022 +0100

    Create class BasketItem
~~~

Op de eerste regel van deze uitvoer zie je lange string van **hexadecimale tekens** (4ba8385472e5dd3c74cd1f8954620a22cf0c67d8), dit is wat we noemen de **commit id**, een unieke **identifier** voor je code-wijzigingen

Deze kan je dan gebruiken om achteraf je codewijzigingen te bekijken via het commando `git show`.  
Typ `git show` gevolgd door de commit-id (normaal gezien is het voldoende om de eerste vijf tekens te typen).

> **Belangrijk:**  
> Zo'n commit is het centrale begrip in git, het is een soort van snapshot van je code op een bepaald tijdstip.

~~~
$ git show 4ba83
commit 4ba8385472e5dd3c74cd1f8954620a22cf0c67d8 (HEAD -> master)
Author: Koen Vervloesem <koen@vervloesem.eu>
Date:   Sat Nov 5 18:03:57 2022 +0100

    Create class BasketItem

diff --git a/basket.py b/basket.py
new file mode 100644
index 0000000..dac7bce
--- /dev/null
+++ b/basket.py
@@ -0,0 +1,6 @@
+from dataclasses import dataclass
+
+@dataclass
+class BasketItem:
+    description: str = ""
+    price: int = 0
~~~

Met een druk op de toets q verlaat je de uitvoer van `git log` of `git show`.

### Code schrijven: event-loop

We hebben nu echter **nog geen applicatie** daarvoor zijn er nog twee belangrijke elementen nodig:

* Een **lijst** van BasketItem-objecten die kan worden bijgewerkt.
* Een **event loop** waar we de nodige acties implementeren.

#### Lijst van BasketItem-objecten

De **eerste stap** is een **lijst** aan te maken als globale variable waarin we BasketItems kunnen toevoegen en bewerken.

~~~python
from dataclasses import dataclass

@dataclass
class BasketItem:
    description: str = ""
    price: int = 0

items = []
~~~

#### Event loop (oneindige lus)

De applicatie moet natuurlijk nog wel iets doen, dus als **eerste stap** zetten we de **interactie met de gebruiker** op.  
Om met de gebruiker te interageren voegen we:

* Een **oneindige lus** toe
* Waar we telkens **input** gaan vragen aan de gebruiker
* Als deze gebruiker antwoordt (**event**)
* Voeren we een **actie** uit 
* En geven we **feedback** aan de gebruiker

Zo'n lus is wat we noemen een **event loop**. Telkens als de gebruiker invoer geeft, zal deze event loop een reactie geven.

Een eerste versie van de loop...

~~~python
from dataclasses import dataclass

@dataclass
class BasketItem:
    description: str = ""
    price: int = 0

items = []

while True:
    pass
~~~

Hier ontbreekt nog een stuk om van een event loop te spreken...

> Het statement `pass` gebruiken we hier slechts tijdelijk omdat we hier nog geen invulling hebben gegeven aan onze lus.

#### Event loop (menu)

Een **oneindige lus** is natuurlijk **niet voldoende**, de gebruiker moet een **menu** hebben.
We printen hier het menu telkens en vragen gebruikersinvoer op via de functie `input()`.

~~~python
from dataclasses import dataclass

@dataclass
class BasketItem:
    description: str = ""
    price: int = 0

items = []

menu = """
1> Voeg item toe
2> Toon items
3> Sluit af
"""

while True:
    menu_input = input(menu)
~~~

De **gebruikersinvoer** wordt opgevraagd en **in een variabele opgeslagen**, maar er is nog **meer nodig**...

#### Event loop (if-else)

We moeten er natuurlijk voor zorgen dat deze **gebruikersinvoer verwerkt** wordt.  
Hiervoor voegen we een **if/elif-structuur** toe voor elke **menu-optie**.

~~~python
from dataclasses import dataclass

@dataclass
class BasketItem:
    description: str = ""
    price: int = 0

items = []

menu = """
1> Voeg item toe
2> Toon items
3> Sluit af
"""

while True:
    menu_input = input(menu)

    if menu_input == "1":
        print("TODO: Voeg item toe")
    elif menu_input == "2":
        print("TODO: Toon items")
    elif menu_input == "3":
        print("TODO: Sluit de applicatie af")
    else:
        print("Foutieve keuze")
~~~

Voorlopig tonen we een **tijdelijke TODO-boodschap** waarmee we kunnen **testen** of onze event loop correct werkt.   
We voeren even een **korte test** uit...

~~~
$ python basket.py

1> Voeg item toe
2> Toon items
3> Sluit af
1
TODO: Voeg item toe

1> Voeg item toe
2> Toon items
3> Sluit af
4
Foutieve keuze

1> Voeg item toe
2> Toon items
3> Sluit af
~~~

...en zien dat de boodschappen worden getoond overeenkomstig de keuzes die je maakt.  
Het **basisskelet** van onze **event loop** lijkt **ok** te zijn.  

Sluit het programma voorlopig af met de toetsencombinatie Ctrl+c.

### Git: Een tweede commit...

Een nieuw onderdeel van onze applicatie is **ontwikkeld** en **getest**. We gaan nu deze **codewijziging** toevoegen aan onze **git-repository**.

Als we de status opvragen, geeft git aan dat er een nieuwe wijziging is:  

~~~
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   basket.py

no changes added to commit (use "git add" and/or "git commit -a")
~~~

Wij hebben deze **wijziging** echter **nog niet klaargezet** voor de commit.  
Hier voor gebruiken we opnieuw het commando `git add` met de bestandsnaam.

~~~
$ git add basket.py
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   basket.py
~~~

De status geeft nu aan dat deze kan **gecommit** worden.  
We doen dit opnieuw met een korte maar duidelijke boodschap.

~~~
$ git commit -m "Add basic event loop and menu"
[master 759d627] Add basic event loop and menu
 1 file changed, 20 insertions(+)
~~~

Merk op dat onze **git-historiek** nu ook **twee commits** bevat:

~~~
$ git log
commit 759d627a9ed718f1ef88430dc159754ad3b591a8 (HEAD -> master)
Author: Koen Vervloesem <koen@vervloesem.eu>
Date:   Sat Nov 5 18:11:16 2022 +0100

    Add basic event loop and menu

commit 4ba8385472e5dd3c74cd1f8954620a22cf0c67d8
Author: Koen Vervloesem <koen@vervloesem.eu>
Date:   Sat Nov 5 18:03:57 2022 +0100

    Create class BasketItem
~~~

> **Opdracht voor de student:**  
> Probeer nu even de **wijzigingen** te **bestuderen** van elke commit via `git show <commit-id>`.  
> Normaal gezien is het voldoende van enkel de eerste vijf tekens van je commit-id te typen.

### Code schrijven: programma/event-loop beëindigen

Tot nog toe moesten we ons eerste programma afsluiten met **Ctrl+c**  
Het eerste event dat we al kunnen afwerken is **"3> Sluit af"**, zodat we het programma op een elegante manier kunnen afsluiten.

Hiervoor gebruiken we de Python-functie `exit()` die het programma afsluit.

~~~python
from dataclasses import dataclass

@dataclass
class BasketItem:
    description: str = ""
    price: int = 0

items = []

menu = """
1> Voeg item toe
2> Toon items
3> Sluit af
"""

while True:
    menu_input = input(menu)

    if menu_input == "1":
        print("TODO: Voeg item toe")
    elif menu_input == "2":
        print("TODO: Toon items")
    elif menu_input == "3":
        print("Programma wordt afgesloten")
        exit()
    else:
        print("Foutieve keuze")
~~~

Probeer dit uit:

~~~
$ python basket.py

1> Voeg item toe
2> Toon items
3> Sluit af
1
TODO: Voeg item toe

1> Voeg item toe
2> Toon items
3> Sluit af
3
Programma wordt afgesloten
~~~

### git: diff en nog commits

We gaan deze wijziging/vooruitgang ook **bewaren** in de git-repository.  
Alvorens dit te doen, gebruiken we de opdracht `git diff` om te inspecteren wat we hebben gewijzigd ten opzichte van de vorige commit.

~~~
$ git diff
diff --git a/basket.py b/basket.py
index 680966f..9071881 100644
--- a/basket.py
+++ b/basket.py
@@ -21,6 +21,7 @@ while True:
     elif menu_input == "2":
         print("TODO: Toon items")
     elif menu_input == "3":
-        print("TODO: Sluit de applicatie af")
+        print("Programma wordt afgesloten")
+        exit()
     else:
         print("Foutieve keuze")
~~~

Dit geeft een vergelijkbaar rapport zoals we dit eerder hebben gezien voor `git show`.  
Het toont ons dat ten opzichte van de vorige commit:

* **1 regel** is **verwijderd** (aangeduid met **-**)

~~~
-        print("TODO: Sluit de applicatie af")
~~~

* **2 regels** zijn **toegevoegd** (aangeduid met **+**)

~~~
+        print("Programma wordt afgesloten")
+        exit()
~~~

* Ook wordt er aangeduid **waar** in de code **deze wijzigingen** te vinden zijn:
    * het bestand basket.py
    * ergens vanaf regel 21

~~~
--- a/basket.py
+++ b/basket.py
@@ -21,6 +21,7 @@ while True:
~~~

We **voegen** deze **commit toe**...

~~~
$ git add basket.py 
$ git commit -m "Implement exit"
[master e50e407] Implement exit
 1 file changed, 2 insertions(+), 1 deletion(-)
$ git log
commit e50e4072028a1a3cca2b4b4a70abfb972bb35d70 (HEAD -> master)
Author: Koen Vervloesem <koen@vervloesem.eu>
Date:   Sat Nov 5 18:15:24 2022 +0100

    Implement exit

commit 759d627a9ed718f1ef88430dc159754ad3b591a8
Author: Koen Vervloesem <koen@vervloesem.eu>
Date:   Sat Nov 5 18:11:16 2022 +0100

    Add basic event loop and menu

commit 4ba8385472e5dd3c74cd1f8954620a22cf0c67d8
Author: Koen Vervloesem <koen@vervloesem.eu>
Date:   Sat Nov 5 18:03:57 2022 +0100

    Create class BasketItem
~~~

...en we zien dat de **git-historiek** zich langzaam maar zeker begint **op te bouwen**

### Code schrijven: items tonen

We hebben bij een vorige commit een (lege) **lijst** gemaakt die wordt gebruikt om de **items** bij te houden.  

In een nieuwe **wijziging/commit** gaan we deze **lijst tonen**. Aangezien we echter de invulling van die lijst nog niet hebben uitgewerkt, maken we voorlopig een *hardcoded* lijst aan met voorbeelddata.

> **Belangrijk:**  
> Deze lijst met voorbeelddata dient achteraf verwijderd te worden 
> maar het laat ons toe om onze code-wijziging te testen.

~~~python
from dataclasses import dataclass

@dataclass
class BasketItem:
    description: str = ""
    price: int = 0

items = []
# TODO: remove temporary list later
items.append(BasketItem("Laptop", 1000))
items.append(BasketItem("USB-stick", 10))

menu = """
1> Voeg item toe
2> Toon items
3> Sluit af
"""

while True:
    menu_input = input(menu)

    if menu_input == "1":
        print("TODO: Voeg item toe")
    elif menu_input == "2":
        for item in items:
            print(item.description, "kost", item.price, "euro")
    elif menu_input == "3":
        print("Programma wordt afgesloten")
        exit()
    else:
        print("Foutieve keuze")
~~~

Onder **optie 2** voegen we vervolgens een **loop** toe die deze **items toont**.  

~~~
$ python basket.py

1> Voeg item toe
2> Toon items
3> Sluit af
2
Laptop kost 1000 euro
USB-stick kost 10 euro

1> Voeg item toe
2> Toon items
3> Sluit af
3
Programma wordt afgesloten
~~~

Als we dit testen, zien we dat de **hardcoded lijst** correct wordt **getoond**. We kunnen de code dus **revisioneren**.


### git: ... volgende commit

We voegen onze **wijzigingen** toe aan de git-repository met een commit:

~~~
$ git add basket.py
$ git commit -m "Print available items"
[master 914635a] Print available items
 1 file changed, 5 insertions(+), 1 deletion(-)
~~~ 

### Code schrijven: items toevoegen

Om er een **interactieve applicatie** van te maken, moeten we het **winkelmandje** aanvullen op basis van **gebruikersinvoer**.  
We doen dit door **event 1** te **implementeren**, dat bestaat uit de volgende drie taken:

* Vraag **gebruikersinvoer** op (beschrijving en prijs)
* **Maak BasketItem-object** aan op basis van invoer
* Voeg BasketItem-object toe aan **lijst**

> Let op: vergeet niet om hierbij ook de eerder aangemaakte **hardcoded** lijst te verwijderen!

~~~python
from dataclasses import dataclass

@dataclass
class BasketItem:
    description: str = ""
    price: int = 0 

items = []

menu = """
1> Voeg item toe
2> Toon items
3> Sluit af
"""

while True:
    menu_input = input(menu)

    if menu_input == "1":
        # Vraag gebruiker om invoer
        description = input("Beschrijving: ")
        price = int(input("Prijs: "))
        # Creëer item
        item = BasketItem(description, price)
        # Voeg nieuw item toe
        items.append(item)
    elif menu_input == "2":
        for item in items:
            print(item.description, "kost", item.price, "euro")
    elif menu_input == "3":
        print("Programma wordt afgesloten")
        exit()
    else:
        print("Foutieve keuze")
~~~

Als we dit uittesten:

~~~
python basket.py

1> Voeg item toe
2> Toon items
3> Sluit af
1
Beschrijving: Laptop
Prijs: 1000

1> Voeg item toe
2> Toon items
3> Sluit af
1
Beschrijving: Harde schijf
Prijs: 200

1> Voeg item toe
2> Toon items
3> Sluit af
2
Laptop kost 1000 euro
Harde schijf kost 200 euro

1> Voeg item toe
2> Toon items
3> Sluit af
3
Programma wordt afgesloten
~~~

De test is correct uitgevoerd:

* We kunnen meerdere items **toevoegen**
* En deze nog altijd **tonen**

### git: ... volgende commit

We **voegen** onze wijzigingen **toe** aan de git-repository:

~~~
$ git add basket.py
$ git commit -m "Add items to basket"
[master 2f87f19] Add items to basket
 1 file changed, 7 insertions(+), 4 deletions(-)
~~~ 

### Code schrijven: vereenvoudiging
We hebben in onze vorige code eerst een item aangemaakt om dit daarna dan aan de lijst met items toe te voegen. We kunnen dat ook in één keer, zonder daarvoor een variabele aan te maken:

~~~python
from dataclasses import dataclass

@dataclass
class BasketItem:
    description: str = ""
    price: int = 0

items = []

menu = """
1> Voeg item toe
2> Toon items
3> Sluit af
"""

while True:
    menu_input = input(menu)

    if menu_input == "1":
        # Vraag gebruiker om invoer
        description = input("Beschrijving: ")
        price = int(input("Prijs: "))
        # Voeg nieuw item toe
        items.append(BasketItem(description, price))
    elif menu_input == "2":
        for item in items:
            print(item.description, "kost", item.price, "euro")
    elif menu_input == "3":
        print("Programma wordt afgesloten")
        exit()
    else:
        print("Foutieve keuze")
~~~

> De code wordt hierdoor gecondenseerd zonder aan leesbaarheid in te winnen.  

We voeren opnieuw dezelfde test uit die we bij de vorige commit hadden uitgevoerd en zien dat onze code nog altijd naar behoren werkt (geen regressie).

### git: ...volgende commit

We kunnen dus opnieuw een commit toevoegen hiervoor, met een korte duidelijke boodschap:

~~~
$ git add basket.py
$ git commit -m "Don't create variable when adding item to basket"
[master c9f6ea0] Don't create variable when adding item to basket
 1 file changed, 1 insertion(+), 3 deletions(-)
~~~

### Code schrijven: Voeg een attribuut quantity toe

In een winkelmandje dien je soms **meerdere items** van **hetzelfde product** bij te houden.  
Om dit op te lossen, voegen we aan onze klasse BasketItem een **attribuut quantity** toe, en vragen we dit attribuut ook aan de gebruiker.

We willen dit attribuut optioneel maken en de standaardwaarde 1 geven. Tegelijk willen we de bestaande attributen die momenteel een standaardwaarde hebben (en dus optioneel zijn) verplicht maken.

Daarnaast voegen we een methode `total_price()` aan de klasse BasketItem toe om de totale prijs op basis van de itemprijs en de hoeveelheid te berekenen.

Binnen de event loop passen we dan ook de `print()` aan zodat die niet de prijs van één item toont, maar van de gegeven hoeveelheid items.

Het resultaat ziet er als volgt uit:

~~~python
from dataclasses import dataclass

@dataclass
class BasketItem:
    description: str
    price: int
    quantity: int = 1

    def total_price(self):
        return self.price * self.quantity

items = []

menu = """
1> Voeg item toe
2> Toon items
3> Sluit af
"""

while True:
    menu_input = input(menu)

    if menu_input == "1":
        # Vraag gebruiker om invoer
        description = input("Beschrijving: ")
        price = int(input("Prijs: "))
        quantity = int(input("Hoeveelheid: "))
        # Voeg nieuw item toe
        items.append(BasketItem(description, price, quantity))
    elif menu_input == "2":
        for item in items:
            print(item.quantity, "*", item.description, "=", item.total_price(), "euro")
    elif menu_input == "3":
        print("Programma wordt afgesloten")
        exit()
    else:
        print("Foutieve keuze")
~~~

Als we dit dan **uittesten**, krijgen we volgende output

~~~
$ python basket.py

1> Voeg item toe
2> Toon items
3> Sluit af
1
Beschrijving: Laptop
Prijs: 1000
Hoeveelheid: 2

1> Voeg item toe
2> Toon items
3> Sluit af
2
2 * Laptop = 2000 euro

1> Voeg item toe
2> Toon items
3> Sluit af
3
Programma wordt afgesloten
~~~

Test OK, we kunnen vervolgen met onze commit...

### git: commit

~~~
$ git add basket.py 
$ git commit -m "Add quantity to BasketItem"
[master 30a88ef] Add quantity to BasketItem
 1 file changed, 9 insertions(+), 4 deletions(-)
~~~

### Totaal van de items

Interessant om te weten als gebruiker is de **totale prijs** van deze items.  
Hiervoor voegen we een loop toe binnen **event 2**

~~~python
from dataclasses import dataclass

@dataclass
class BasketItem:
    description: str
    price: int
    quantity: int = 1

    def total_price(self):
        return self.price * self.quantity

items = []

menu = """
1> Voeg item toe
2> Toon items
3> Sluit af
"""

while True:
    menu_input = input(menu)

    if menu_input == "1":
        # Vraag gebruiker om invoer
        description = input("Beschrijving: ")
        price = int(input("Prijs: "))
        quantity = int(input("Hoeveelheid: "))
        # Voeg nieuw item toe
        items.append(BasketItem(description, price, quantity))
    elif menu_input == "2":
        total_of_items = 0
        for item in items:
            print(item.quantity, "*", item.description, "=", item.total_price(), "euro")
            total_of_items = total_of_items + item.total_price()
        print("Totaalprijs:", total_of_items)
    elif menu_input == "3":
        print("Programma wordt afgesloten")
        exit()
    else:
        print("Foutieve keuze")
~~~

En we testen dit:

~~~
$ python basket.py

1> Voeg item toe
2> Toon items
3> Sluit af
1
Beschrijving: Laptop
Prijs: 1000
Hoeveelheid: 2

1> Voeg item toe
2> Toon items
3> Sluit af
1
Beschrijving: Harde schijf
Prijs: 250
Hoeveelheid: 3

1> Voeg item toe
2> Toon items
3> Sluit af
2
2 * Laptop = 2000 euro
3 * Harde schijf = 750 euro
Totaalprijs: 2750

1> Voeg item toe
2> Toon items
3> Sluit af
3
Programma wordt afgesloten
~~~

De test is geslaagd...

### git: volgend commit...

...We kunnen dit **committen**

~~~
$ git add basket.py 
$ git commit -m "Add total price of all items"
[master 8bc2470] Add total price of all items
 1 file changed, 3 insertions(+)
~~~

### Basket-klasse

We kunnen het het beheer van verschillende items in een afzonderlijke klasse onderbrengen.  We noemen deze klasse **Basket**.

Dit kan ook interessant zijn als we ooit de applicatie uitbreiden om meerdere winkelmandjes bij te houden (bijvoorbeeld voor verschillende gebruikers).

Deze klasse bevat:

* Een **lijst** met **items**
* Een methode om een **item toe te voegen**
* Een methode om de **totale prijs** te berekenen

~~~python
from dataclasses import dataclass

@dataclass
class BasketItem:
    description: str
    price: int
    quantity: int = 1

    def total_price(self):
        return self.price * self.quantity

class Basket:
    def __init__(self, items=[]):
        self.items = items

    def add(self, item):
        self.items.append(item)

    def get_items(self):
        return self.items

    def total_price(self):
        total_of_items = 0
        for item in self.items:
            total_of_items = total_of_items + item.total_price()
        return total_of_items

basket = Basket()

menu = """
1> Voeg item toe
2> Toon items
3> Sluit af
"""

while True:
    menu_input = input(menu)

    if menu_input == "1":
        # Vraag gebruiker om invoer
        description = input("Beschrijving: ")
        price = int(input("Prijs: "))
        quantity = int(input("Hoeveelheid: "))
        # Voeg nieuw item toe
        basket.add(BasketItem(description, price, quantity))
    elif menu_input == "2":
        total_of_items = 0
        for item in basket.get_items():
            print(item.quantity, "*", item.description, "=", item.total_price(), "euro")
            total_of_items = total_of_items + item.total_price()
        print("Totaalprijs:", basket.total_price())
    elif menu_input == "3":
        print("Programma wordt afgesloten")
        exit()
    else:
        print("Foutieve keuze")
~~~

We **herhalen** de **test** van het voorgaande deel en zien dat de code nog altijd werkt (geen regressie).

### git: committen maar...

~~~
$ git add basket.py 
$ git commit -m "Add Basket class to manage group of items"
[master 3b08c94] Add Basket class to manage group of items
 1 file changed, 20 insertions(+), 6 deletions(-)
~~~

### Error handling: negatieve invoer 

Tot nu toe hebben we geen controles uitgevoerd op negatieve waardes.  
We passen de volgende regels toe:

* Enkel een hoeveelheid toelaten > 0
* Enkel een prijs toelaten >= 0

We gaan deze controles toevoegen in de klasse BasketItem.  

Het idee is om vanuit de constructor een exception op te werpen om te vermijden dat er een item met ongeldige data wordt aangemaakt.

Om deze exceptions op type te kunnen opvangen maken we twee specifieke exception-klassen aan
 (één voor elke fout).  
Deze exceptions worden dan opgevangen binnen de event loop, waarna een boodschap wordt getoond.

~~~python
class InvalidQuantityException(Exception):
    pass

class InvalidPriceException(Exception):
    pass

class BasketItem:
    def __init__(self, description, price, quantity = 1):
        if quantity <= 0:
            raise InvalidQuantityException()

        if price < 0:
            raise InvalidPriceException()

        self.description = description
        self.price = price
        self.quantity = quantity

    def total_price(self):
        return self.price * self.quantity

class Basket:
    def __init__(self, items=[]):
        self.items = items

    def add(self, item):
        self.items.append(item)

    def get_items(self):
        return self.items

    def total_price(self):
        total_of_items = 0
        for item in self.items:
            total_of_items = total_of_items + item.total_price()
        return total_of_items

basket = Basket()

menu = """
1> Voeg item toe
2> Toon items
3> Sluit af
"""

while True:
    menu_input = input(menu)

    if menu_input == "1":
        try:
            # Vraag gebruiker om invoer
            description = input("Beschrijving: ")
            price = int(input("Prijs: "))
            quantity = int(input("Hoeveelheid: "))
            # Voeg nieuw item toe
            basket.add(BasketItem(description, price, quantity))
        except InvalidQuantityException:
            print(quantity, "is geen geldige hoeveelheid")
        except InvalidPriceException:
            print(price, "is geen geldige prijs") 
    elif menu_input == "2":
        total_of_items = 0
        for item in basket.get_items():
            print(item.quantity, "*", item.description, "=", item.total_price(), "euro")
            total_of_items = total_of_items + item.total_price()
        print("Totaalprijs:", basket.total_price())
    elif menu_input == "3":
        print("Programma wordt afgesloten")
        exit()
    else:
        print("Foutieve keuze")
~~~

Test dit uit:

~~~
$ python basket.py

1> Voeg item toe
2> Toon items
3> Sluit af
1
Beschrijving: Laptop
Prijs: -1000
Hoeveelheid: 10
-1000 is geen geldige prijs

1> Voeg item toe
2> Toon items
3> Sluit af
1
Beschrijving: Laptop
Prijs: 1000
Hoeveelheid: -10
-10 is geen geldige hoeveelheid

1> Voeg item toe
2> Toon items
3> Sluit af
3
Programma wordt afgesloten
~~~

Dit werkt!

~~~
$ git add basket.py 
$ git commit -m "Exception handling for negative values"
[master 85c956d] Exception handling for negative values
 1 file changed, 26 insertions(+), 11 deletions(-)
~~~

### Error handling: niet-gehele getallen

Als we momenteel geen getal maar een string invoeren voor de hoeveelheid of de prijs, zal de applicatie crashen.  
We zullen dit opvangen door ook de exception `ValueError` op te vangen binnen de event loop.

~~~python
class InvalidQuantityException(Exception):
    pass

class InvalidPriceException(Exception):
    pass

class BasketItem:
    def __init__(self, description, price, quantity = 1):
        if quantity <= 0:
            raise InvalidQuantityException()

        if price < 0:
            raise InvalidPriceException()

        self.description = description
        self.price = price
        self.quantity = quantity

    def total_price(self):
        return self.price * self.quantity

class Basket:
    def __init__(self, items=[]):
        self.items = items

    def add(self, item):
        self.items.append(item)

    def get_items(self):
        return self.items

    def total_price(self):
        total_of_items = 0
        for item in self.items:
            total_of_items = total_of_items + item.total_price()
        return total_of_items

basket = Basket()

menu = """
1> Voeg item toe
2> Toon items
3> Sluit af
"""

while True:
    menu_input = input(menu)

    if menu_input == "1":
        try:
            # Vraag gebruiker om invoer
            description = input("Beschrijving: ")
            price = int(input("Prijs: "))
            quantity = int(input("Hoeveelheid: "))
            # Voeg nieuw item toe
            basket.add(BasketItem(description, price, quantity))
        except InvalidQuantityException:
            print(quantity, "is geen geldige hoeveelheid")
        except InvalidPriceException:
            print(price, "is geen geldige prijs") 
        except ValueError:
            print("Geef een geldige waarde in")
    elif menu_input == "2":
        total_of_items = 0
        for item in basket.get_items():
            print(item.quantity, "*", item.description, "=", item.total_price(), "euro")
            total_of_items = total_of_items + item.total_price()
        print("Totaalprijs:", basket.total_price())
    elif menu_input == "3":
        print("Programma wordt afgesloten")
        exit()
    else:
        print("Foutieve keuze")
~~~

Test dit nu...

### git: ...nog een commit...

Vanzelfsprekend voeren we een nieuwe commit uit:

~~~
$ git add basket.py 
$ git commit -m "Intercept non-numeric values in user input"
[master 6e3f7fe] Intercept non-numeric values in user input
 1 file changed, 3 insertions(+), 1 deletion(-)
~~~

#### Uitbreidingsoefeningen

Er kunnen nog veel verbeteringen worden toegevoegd aan deze applicatie:

* Verwijderen van een item
* Wijzigen van de hoeveelheid
* Meerdere baskets aanmaken
* Toevoegen van klantendata
* ...

Probeer deze stapsgewijs toe te voegen op de manier zoals we dit hier hebben geïllustreerd.
