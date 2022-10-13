## Werken met tekstbestanden

### Bestanden

Een programma neemt doorgaans invoer aan, verwerkt die en toont een resultaat als uitvoer. Maar in veel gevallen zal een programma ook data willen **bewaren** voor **later gebruik**.  
De meest directe manier om data op te slaan is deze naar een **bestand** te schrijven.

Een bestand is in essentie:

* een **verzameling** van **bytes**
* opgeslagen op een **persistent medium** (harde schijf, solid state disk, usb-stick, ...)
* geadresseerd binnen **een bestandssysteem**
* met een specifiek **pad**

Er bestaan verschillende soorten bestanden:

* Tekstbestanden: Bevatten tekstkarakters die je kan lezen
* Uitvoerbare bestanden (*executables*): Bevatten programma's met code-instructies voor de processor
* Mediabestanden: Bevatten afspeelbare media (beelden, audio, video, ...)
* Andere binaire gegevensbestanden: Bevatten spreadsheets, tekstverwerkingsdocumenten, databases, printplaatontwerpen, ...
  
### Tekstbestanden

In dit deel gaan we in Python leren werken met tekstbestanden.

Tekstbestanden bevatten karakters, zoals we die ook kennen van strings in Python. Dit zijn bytes waarvan de waarde overeenstemt met een specifiek teken.

#### ASCII-encodering

We zagen eerder al ASCII, een veel gebruikte **encodering** die hiervoor gebruikt wordt: 

~~~
Dec Hex    Dec Hex    Dec Hex  Dec Hex  Dec Hex  Dec Hex   Dec Hex   Dec Hex  
  0 00 NUL  16 10 DLE  32 20    48 30 0  64 40 @  80 50 P   96 60 `  112 70 p
  1 01 SOH  17 11 DC1  33 21 !  49 31 1  65 41 A  81 51 Q   97 61 a  113 71 q
  2 02 STX  18 12 DC2  34 22 "  50 32 2  66 42 B  82 52 R   98 62 b  114 72 r
  3 03 ETX  19 13 DC3  35 23 #  51 33 3  67 43 C  83 53 S   99 63 c  115 73 s
  4 04 EOT  20 14 DC4  36 24 $  52 34 4  68 44 D  84 54 T  100 64 d  116 74 t
  5 05 ENQ  21 15 NAK  37 25 %  53 35 5  69 45 E  85 55 U  101 65 e  117 75 u
  6 06 ACK  22 16 SYN  38 26 &  54 36 6  70 46 F  86 56 V  102 66 f  118 76 v
  7 07 BEL  23 17 ETB  39 27 '  55 37 7  71 47 G  87 57 W  103 67 g  119 77 w
  8 08 BS   24 18 CAN  40 28 (  56 38 8  72 48 H  88 58 X  104 68 h  120 78 x
  9 09 HT   25 19 EM   41 29 )  57 39 9  73 49 I  89 59 Y  105 69 i  121 79 y
 10 0A LF   26 1A SUB  42 2A *  58 3A :  74 4A J  90 5A Z  106 6A j  122 7A z
 11 0B VT   27 1B ESC  43 2B +  59 3B ;  75 4B K  91 5B [  107 6B k  123 7B {
 12 0C FF   28 1C FS   44 2C ,  60 3C <  76 4C L  92 5C \  108 6C l  124 7C |
 13 0D CR   29 1D GS   45 2D -  61 3D =  77 4D M  93 5D ]  109 6D m  125 7D }
 14 0E SO   30 1E RS   46 2E .  62 3E >  78 4E N  94 5E ^  110 6E n  126 7E ~
 15 0F SI   31 1F US   47 2F /  63 3F ?  79 4F O  95 5F _  111 6F o  127 7F DEL
~~~

Stel dat we een tekstbestand aanmaken met de volgende inhoud:

~~~
hello
  world
~~~

Je kunt de inhoud als een tekstbestand bekijken. In Linux:

~~~bash
$ cat hello.txt 
hello
  world
~~~

En in Windows:

~~~
C:\Users\User\Documents>type hello.txt
hello
  world
~~~

Maar je kunt ook de waarde van de overeenkomende bytes bekijken. In Linux:

~~~bash
$ od -t x1z hello.txt 
0000000 68 65 6c 6c 6f 0a 20 20 77 6f 72 6c 64 0a        >hello.  world.<
0000016
~~~

In Windows:

~~~
C:\Users\User\Documents>PowerShell Format-Hex hello.txt


           Path: C:\Users\User\Documents\hello.txt

           00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F

00000000   68 65 6C 6C 6F 0D 0A 20 20 77 6F 72 6C 64        hello..  world
~~~

Je ziet hier dat beide programma's eerst de opeenvolgende bytes tonen, en daarna de overeenkomende leesbare tekens van de tekst volgens de ASCII-codering. Tekens zonder leesbare voorstelling worden met een punt aangeduid.

Je ziet hier ook een verschil tussen Linux en Windows als je tekstbestanden aanmaakt: in Linux wordt er voor de nieuwe regel het byte 0a (LF in de ASCII-tabel) toegevoegd, terwijl Windows daarvoor twee bytes gebruikt: 0d 0a (CRLF in de ASCII-tabel).

### Werken met tekstbestanden in Python

In de meeste gevallen moet je niets van deze encodering weten om met tekstbestanden te werken in Python. We tonen nu enkele standaardoperaties die je in Python met bestanden kunt uitvoeren.

### Lezen uit een tekst-file 

Om te demonstreren hoe we met een tekstbestand kunnen omgaan, starten we met het aanmaken van een bestand genaamd **demofile.txt** met de volgende (nietszeggende) inhoud:

~~~
Lorem ipsum dolor sit amet, consectetuer adipiscing elit. 
Aenean commodo ligula eget dolor. Aenean massa. 
Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. 
Donec quam felis, ultricies nec, pellentesque eu, pretium quis, sem. Nulla consequat massa quis enim. 
Donec pede justo, fringilla vel, aliquet nec, vulputate eget, arcu. In enim justo, rhoncus ut, imperdiet a, venenatis vitae, justo. Nullam dictum felis eu pede mollis pretium. 
Integer tincidunt. Cras dapibus. 
Vivamus elementum semper nisi. Aenean vulputate eleifend tellus. 
Aenean leo ligula, porttitor eu, consequat vitae, eleifend ac, enim. 
Aliquam lorem ante, dapibus in, viverra quis, feugiat a, tellus. Phasellus viverra nulla ut metus varius laoreet. 
Quisque rutrum. Aenean imperdiet. Etiam ultricies nisi vel augue. Curabitur ullamcorper ultricies nisi. Nam eget dui
~~~

Open dus Kladblok of een andere teksteditor, plak deze tekst daarin en sla het op onder de naam **demofile.txt**.

#### Open en close

Werken met een tekstbestand start met het aanmaken van een **file-object**.
Dat doe je met de functie **open()**. Daarna kun je bestandsoperaties op het file-object uitvoeren, zoals een leesoperatie, en daarna sluit je het file-object:

~~~python
>>> f = open("demofile.txt")
>>> print(f.read())
Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
Aenean commodo ligula eget dolor. Aenean massa.
Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.
Donec quam felis, ultricies nec, pellentesque eu, pretium quis, sem. Nulla consequat massa quis enim.
Donec pede justo, fringilla vel, aliquet nec, vulputate eget, arcu. In enim justo, rhoncus ut, imperdiet a, venenatis vitae, justo. Nullam dictum felis eu pede mollis pretium.
Integer tincidunt. Cras dapibus.
Vivamus elementum semper nisi. Aenean vulputate eleifend tellus.
Aenean leo ligula, porttitor eu, consequat vitae, eleifend ac, enim.
Aliquam lorem ante, dapibus in, viverra quis, feugiat a, tellus. Phasellus viverra nulla ut metus varius laoreet.
Quisque rutrum. Aenean imperdiet. Etiam ultricies nisi vel augue. Curabitur ullamcorper ultricies nisi. Nam eget dui

>>> f.close()
~~~

Na gebruik is het belangrijk dat je dit **file-object sluit** met de operatie `close()`.  
Het besturingssysteem kan namelijk het bestand blokkeren voor gebruik vanuit andere programma's zolang het file-object open staat.

Als je na het sluiten van het file-object nog een leesopdracht uitvoert, krijg je overigens een foutmelding:

~~~python
>>> f.close()
>>> print(f.read())
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: I/O operation on closed file.
~~~

#### Automatisch sluiten

Het probleem met bovenstaande code is: als er zich een exception voordoet na `open()` wordt het file-object mogelijk niet gesloten.
Om dit te vermijden bestaat er het with-statement:

~~~python
with open("demofile.txt") as f:
    print(f.read())
# Een aantal operaties...
print(f.closed)
~~~

Het with-statement zal er voor zorgen dat - na het uitvoeren van de code binnen dit blok - het file-object sowieso wordt gesloten, zelfs al treedt er een exception op.

**Bijna altijd is het aangeraden om `with open()` te gebruiken in plaats van `open()`.** In de rest van de cursus gebruiken we dan ook dit with-statement om met bestanden te werken.

#### Volledige inhoud uitlezen

Je kan zoals we zagen de hele inhoud van een tekstbestand opvragen via de functie `read()`:

~~~python
with open("demofile.txt") as f:
    print(f.read()) 
~~~

Het is interessant om in de REPL eens te kijken wat voor type data de functie read() teruggeeft:

~~~python
>>> with open("demofile.txt") as f:
...     content = f.read()
... 
>>> type(content)
<class 'str'>
>>> content
'Lorem ipsum dolor sit amet, consectetuer adipiscing elit.\nAenean commodo ligula eget dolor. Aenean massa.\nCum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.\nDonec quam felis, ultricies nec, pellentesque eu, pretium quis, sem. Nulla consequat massa quis enim.\nDonec pede justo, fringilla vel, aliquet nec, vulputate eget, arcu. In enim justo, rhoncus ut, imperdiet a, venenatis vitae, justo. Nullam dictum felis eu pede mollis pretium.\nInteger tincidunt. Cras dapibus.\nVivamus elementum semper nisi. Aenean vulputate eleifend tellus.\nAenean leo ligula, porttitor eu, consequat vitae, eleifend ac, enim.\nAliquam lorem ante, dapibus in, viverra quis, feugiat a, tellus. Phasellus viverra nulla ut metus varius laoreet.\nQuisque rutrum. Aenean imperdiet. Etiam ultricies nisi vel augue. Curabitur ullamcorper ultricies nisi. Nam eget dui\n'
~~~

Je ziet hier dat read() de volledige inhoud van het bestand als een string teruggeeft. En als je de inhoud van de string bekijkt, zie je dat elke nieuwe regel vervangen is door een teken `\n` (voor newline).

#### Aantal tekens uitlezen
Je kunt je ook beperken tot het uitlezen van een aantal bytes (in het geval van een tekstbestand het aantal tekens):

~~~python
with open("demofile.txt") as f:
    print(f.read(5)) 
    print(f.read(5)) 
~~~

Bemerk ook dat de positie tot waar je al hebt gelezen wordt bijgehouden in het file-object.

~~~
Lorem
 ipsu
~~~

#### Regel per regel lezen

Je kan ook kiezen om het bestand regel per regel uit te lezen.

~~~python
with open("demofile.txt") as f:
    print(f.readline())
    print(f.readline()) 
~~~

Bovenstaande code zal de eerste twee regels afdrukken:

~~~
Lorem ipsum dolor sit amet, consectetuer adipiscing elit. 
Aenean commodo ligula eget dolor. Aenean massa.
~~~

#### Alle regels lezen met een lus

Het file-object kan zich ook gedragen als een lijst van regels.  
Op dit object kan je dan een lus uitvoeren om het hele bestand regel per regel uit te lezen:

~~~python
with open("demofile.txt") as f:
    for x in f:
        print(x) 
~~~

Dan krijg je:

~~~
Lorem ipsum dolor sit amet, consectetuer adipiscing elit. 

Aenean commodo ligula eget dolor. Aenean massa. 

Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. 

Donec quam felis, ultricies nec, pellentesque eu, pretium quis, sem. Nulla consequat massa quis enim.

...
~~~

Doordenker: waarom toont het programma hier een lege regel tussen elke regel? En hoe zorg je dat dit niet gebeurt?

#### Modi

tot nu toe hebben we altijd bestanden gelezen. Dat is één van de mogelijke modi bij bij het openen van een bestand:

* "r" - Read   - Opent een bestand aan om dit te lezen.
* "w" - Write  - Maakt een bestand aan als dit nog niet bestaat, overschrijft een bestaand bestand. Opent het bestand om ernaar te schrijven.
* "a" - Append - Maakt een bestand aan als dit nog niet bestaat. Alle schrijfoperaties worden aan het einde van het bestand toegevoegd.
* "x" - Create - Maakt een bestand aan als dit nog niet bestaat en opent het om ernaar te schrijven. Geeft een exception FileExistsError als het bestand al bestaat.

Deze mode kan je meegeven als een tweede (optioneel) argument aan de functie open:

~~~python
with open("demofile.txt", "r") as f:
    print(f.read()) 
~~~

Als je dit tweede argument niet toevoegt, is de standaardmodus r (read-only).

### Schrijven naar een bestand

Voor het schrijven naar een bestand zijn er drie belangrijke modi die we moeten begrijpen:

* Een nieuw bestand schrijven => x (geeft een FileExistsError als het bestand al bestaat)
* Een bestaand bestand overschrijven => w
* Aan het einde van een bestaand bestand toevoegen => a

#### Een nieuw bestand aanmaken

Het eerste scenario is dat we een nieuw bestand willen aanmaken.  
In dit geval gebruiken we de modus **x**:

~~~python
with open("hello.txt", "x") as f:
    f.write("Dit is een nieuw bestand")
with open("hello.txt", "r") as f:
    print(f.read()) 
~~~

Als het bestand nog niet bestaat, zal er een nieuw bestand hello.txt worden aangemaakt.

~~~bash
$ python create_new_file.py
Dit is een nieuw bestand
~~~

Mocht het bestand al bestaan, bijvoorbeeld door het programma een tweede keer uit te voeren, zal er een exception worden opgeworpen door de open-functie:

~~~
$ python create_new_file.py
Traceback (most recent call last):
  File "/home/koan/create_new_file.py", line 1, in <module>
    with open("hello.txt", "x") as f:
FileExistsError: [Errno 17] File exists: 'hello.txt'
~~~

#### Nieuw of bestaand bestand overschrijven

Als we dezelfde code wijzigen en de modus **w** gebruiken:

* zal er **geen exception** worden opgeworpen als het **bestand reeds bestaat**
* maar wordt dit **overschreven**
* als het bestand **niet bestaat**, wordt het **aangemaakt**

Onderstaande code:

~~~python
with open("hello.txt", "w") as f:
    f.write("Dit is een nieuw bestand")
with open("hello.txt", "r") as f:
    print(f.read()) 

with open("hello.txt", "w") as f:
    f.write("Het bestand is overschreven")
with open("hello.txt", "r") as f:
    print(f.read()) 
~~~

* Zal het bestand eerst aanmaken (als dit nog niet bestaat)
* En vervolgens de inhoud overschrijven

~~~
$ python create_new_file.py
Dit is een nieuw bestand

Het bestand is overschreven
~~~

#### Toevoegen aan het einde van een tekstbestand

~~~python
with open("hello.txt", "w") as f:
    f.write("Dit is een nieuw bestand")
with open("hello.txt", "r") as f:
    print(f.read()) 

with open("hello.txt", "a") as f:
    f.write("Deze regel is toegevoegd")
with open("hello.txt", "r") as f:
    print(f.read()) 
~~~

~~~
$ python append_to_file.py
Dit is een nieuw bestand

Dit is een nieuw bestand
Deze regel is toegevoegd
~~~

#### Relatief of absoluut pad

Vorige voorbeelden openden een bestand dat in dezelfde directory staat als van waaruit je het Python-programma uitvoert.

We gaven als pad om te openen dan ook gewoon de bestandsnaam door. Dit pad is **relatief** ten opzichte van de toepassing die je uitvoert.

Als je een bestand wilt inlezen dat in een subdirectory staat ten opzichte van de directory vanwaar je programma wordt uitgevoerd, beschrijf je het pad als volgt:

~~~python
with open("subdirectory/demofile.txt") as f:
    print(f.read())
~~~

Zowel in Linux als Windows kun je een forward slash (/) gebruiken om directory's af te scheiden, ook al gebruikt Windows intern een backslash (\\). Een backslash wordt in Python-strings immers als escape character gebruikt (zie voorheen).

Daarnaast kan je ook een exact pad opgeven, dit wordt ook een **absoluut** pad genoemd. Een voorbeeld in Linux:

~~~python
with open("/home/koan/demofile.txt") as f:
    print(f.read())
~~~

En in Windows:

~~~python
with open("C:/Users/User/Documents/demofile.txt") as f:
    print(f.read())
~~~

### Verdere info

Meer info over het werken met bestanden vind je op https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files.
